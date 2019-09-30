

# Introduction


- we work in an hpc office and get to play with a lot of new hardware
- one essential and unusual feature of this setup is that every resource is shared between many users
- this is not a lab - e.g. our cluster users don't know each other, don't know who other users are, don't even know how to contact them. Coordinating jobs via human communication in this situation is impossible.
- sharing resources without knowing works well with a number of shedulers, e.g. slurm
- involves solving the following Problem: how to restrict access of a particular user to the shared resource so that their job will not be interrupted by other people's jobs; and vice versa?

## Our setup and problem

- 4 FPGA cards on a server on ERN
- used by a class of 15 groups - too many to keep humanly manageable
- alternative solution would be very manual (signup groups for time slots; or slack channel where a group can ask "can I run it here"?)


## Slurm details

This is a paragraph that introduces slurm to people who are not familiar with it. 
- generalities about Slurm - adoption, history, codebase size, company behind it
- slurm cgroups - control permissions to resources and limit the user to a subset of resources so that they can be shared
- how it works for GPUs


## FPGA details

- explain how opencl enables/disables cards for a user. There is permissions file at `inteldevstack/a10_gx_pac_ias_1_1_pv/opencl/opencl_bsp/linux64/libexec/setup_permissions.sh` that controls two sets of files
  * `intel-fpga-port.*` gives you access to MMIO region for the workload that is loaded into the card
  * `intel-fpga-fme.*` portion allows you to program the card (along with giving access to the FME MMIO region that handles card management) 

The relevant part of the `setup_permissions.sh` controls two groups of files

### Devices

```
sudo chmod 666 /dev/intel-fpga-port.*
sudo chmod 666 /dev/intel-fpga-fme.*
```

### /sys part

```
sudo chmod 666 /sys/class/fpga/intel-fpga-dev.*/intel-fpga-port.*/userclk_freqcmd
sudo chmod 666 /sys/class/fpga/intel-fpga-dev.*/intel-fpga-port.*/userclk_freqcntrcmd
sudo chmod 666 /sys/class/fpga/intel-fpga-dev.*/intel-fpga-port.*/errors/clear
```

## Problem why it can't work with slurm as is

- slurm `init()` only allows controlling a single file. That's good enough for gpus, but as we have seen, there are several /dev files to be controlled... 

# Solution

## Best solution - slurm source code change

Not applicable in this situation. 
- we need a solution fast
- any change to complex codebase needs to go through thorough review
- lack of manpower at SchedMD

## Failed solution - slurm prolog script

slurm jobs can have prolog and epilog scripts which get executed before and after a job. However, these scripts are executed before cgroups are set. To slurm users, these are the 
most familiar injection points for customization. 

## Our workaround

We will use mic types and the possibility to controll mic devices to actually repurpose that for fpgas. There is support in slurm for mic types, but there is no support for
fpgas. Since mic types are essentially an abandonded technology, and if you know you don't need those, you can repurpose that support for new purpose - namely, FPGAs. 
In particular, slurm has the following variable in its source code `SLURM_STEP_MICS`, which we will co-opt to indicate to be 0,1,2 or 3, depending on the FPGA device number. 

*The logic behind the workaround is*: 
- `SLURM_STEP_MICS` is slurm variable that's unused, but is indicative of the FPGA card we are trying to control  TODO: explain
- at the appropriate point of the slurm job-launch workflow, namely, just before elevated privileges are dropped, we get the value of `SLURM_STEP_MICS`. If we got fpga with device number 2, `SLURM_STEP_MICS=2`.
- spank plugin modifies `slurm_spank_task_init_privileged` callback to perform two things: 
  * get value of slurm variable `SLURM_STEP_MICS` via `spank_getenv` function - e.g. 2
  * call a python script that changes permissions to a number of files for that device number: e.g. 

*The steps to perform for the workaround*: 
- modify `slurm.conf` (add GresTypes a mic type) - this is related to `SLURM_STEP_MICS`. Requesting `--gres=mic:2` will now grant access to 2 fpga cards in slurm
- create or modify `/etc/slurm/gres.conf` with the following line: 
```Name=mic Count=4 File=/dev/intel-fpga-port[0-3]```
- install spank plugin that will call a python script during the initialization (see next section)
- place the python script in the appropriate location on the fpga machine (see following section)

### spank plugin system for slurm

We will use the [spank plugin system for slurm](https://slurm.schedmd.com/spank.html). This system exists - like hooks in web development - so that you can inject your own behavior in 
a system that executes a number of steps in sequence, at predetermined points in the code. It provides customization framework. There are several "contexts" in which hooks are available, 
like local (as in, when the job is already on the local machine), _remote_, allocator, slurmd (slurm_init or slurm_exit) or job_script (prolog and epilog). 

Spank plugin examples include oom-detect (if jobs is killed by OOM, leave some trace of reasons why in user's error file), tmp (write job output in temporarily mounted private tmp directory 
for easy cleanup)

In this case we need to located the appropriate point in which to inject our behavior, i.e. the restricting the permissions on some files, that depend on the device number (0,1,2,3 in case of the 4 fpga cards). 
This will be in the _remote_ context. In particular, the hook we will modify is `slurm_spank_task_init_privileged`, which is described in the spank documentation as 
"Called for each task just after fork, but before all elevated privileges are dropped". 

The relevant part of `intelfpga_spank.c` will be: 

```
(..other code..)
#define FPGA_VAR "SLURM_STEP_MICS"
int slurm_spank_task_init_privileged(spank_t sp, int ac, char **av)
{
    char val[1024]={0};
    char cmd[2048]={0};
    spank_getenv(sp,FPGA_VAR,val,512);
    slurm_info("Intelfpga spank plugin devices=%s", val);
    sprintf(cmd, "/usr/local/sbin/setupfpga %s", val);
    system(cmd);

    return (0);
}
```

Finally, you will want to: 

- declare the compiled shared object's path to the plugin's configuration file at `/etc/slurm/plugstack.conf.d/intelfpga.conf`. 
- Restart slurmctld/slurmd. Test.

### Python script
 
- location of the script: `/usr/local/sbin/setupfpga`
- purpose of the script: given an argument (for us, a number 0,1,2, or 3; let's say 2), modify permissions to /dev/intelfpga*.2 files. 
- usage of the script: `/usr/local/sbin/setupfpga 3,4` to set exclusive access to fpga cards 3 and 4 

```
#!/usr/bin/python
import os
import sys
import glob
from stat import *

DEVICE = '/dev/intel-fpga-fme' 

def set_access(c_path, device, allow):
  f_name = c_path + ('/devices.allow' if allow else '/devices.deny')
  # get device info we need to allow/deny access
  s = os.stat(device)
  devtype = 'c' if S_ISCHR(s[ST_MODE]) else 'b'
  # c 195:3 rwm
  attr = '%s %s:%s rwm' % (devtype, os.major(s.st_rdev), os.minor(s.st_rdev))
  try:
    with open(f_name, 'w') as f:
      f.write(attr)
  except:
    return

# Get our devices cgroup
c_devices = None
with open("/proc/%d/cgroup" % os.getpid(), 'r') as f:
  for line in f:
    if ":devices:/slurm" in line:
      c_devices = '/sys/fs/cgroup/devices' + line.split(':')[2].strip()

if c_devices == None or not os.path.isdir(c_devices):
  sys.exit(0)

alloc_devs = []
if len(sys.argv) > 1:
  alloc_devs = [int(x) for x in sys.argv[1].split(',')]

for d in glob.glob(DEVICE + '[0-9]*'):
  d_num = int(d[len(DEVICE):])
  set_access(c_devices, d, d_num in alloc_devs)
```
