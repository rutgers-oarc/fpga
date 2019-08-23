# FPGA at Rutgers

## Hardware Specs

fpga1: PowerEdge R740XD Server: 

- 32 Xeon 2.6Ghz cores 
- 24 slots of 32GB RDIMMs (total 768Gb RAM) 
- 4 FPGA cards [Intel FPGA Programmable Acceleration Card](https://www.intel.com/content/www/us/en/programmable/products/boards_and_kits/dev-kits/altera/acceleration-card-arria-10-gx.html)

fpga2: 

- has 16 cores, faking 32 with hyperthreading; 
- 187G of RAM; 
- model Xeon(R) Silver 4110
- one fpga card

Licensing: 5 floating licenses for Quartus

## How to get access

- FPGA box is part of ERN (Eastern Regional Network), and hosted at Rutgers. Requests for access should fill out [this form for general Amarel access](https://oarc.rutgers.edu/access/) and specify that you need an account on ERN and that hardware you would like to use is FPGA. 

## Directions to get to the box (from Amarel)

- ssh to amarel (your netid will be your username, and password is your RU password). e.g. `ssh kp807@amarel.hpc.rutgers.edu`. 
- ssh to a machine called mace. This is a login node for Rutgers part of the ERN slurm federation. e.g. `ssh mace`. This is passwordless because your ssh keys have been set up when you got an ERN account. 
- FOR now: ssh to fpga1, e.g. `ssh fpga1`. (For the future: run this command: `salloc -p fpga ...TBD... `  (in order for slurm to allocate fpga resources to you) )
- alltogether, list of commands: 
```
ssh kp807@amarel.hpc.rutgers.edu
ssh mace
ssh fpga1
```
- for instructions how to run vnc server (remote desktop to run Quartus): see vncserver.md in this repo

## Join the community 

- We have a [user forum](https://ask.oarc.rutgers.edu/questions/) - login with your Rutgers credentials
- We have a slack space: fpga-dev.slack.com - join us! 

## Resources

- [Parallel Programming for FPGAs book](http://kastner.ucsd.edu/hlsbook/)
- [Intel Acceleration Stack](https://www.intel.com/content/www/us/en/programmable/solutions/acceleration-hub/acceleration-stack.html)
- [Intel FPGA training](https://www.intel.com/content/www/us/en/programmable/solutions/acceleration-hub/knowledge-center.html)
- [List of reference designs](https://www.intel.com/content/www/us/en/programmable/products/design-software/embedded-software-developers/opencl/support.html#ref-designs)
- [5-lecture course material for students](https://software.intel.com/en-us/ai-academy/students/kits/dl-inference-fpga)
- [Intel fpga channel on youtube](https://www.youtube.com/channel/UC0wEPiFb0J6AZZ3oPXRoRpw)
- [Catalog of many FPGA classes](https://www.intel.com/content/www/us/en/programmable/support/training/catalog.html)
- [Intel's open source website/community](https://01.org/)  - you can search for FPGA
- [Open Programmable Acceleration Engine](https://01.org/opae) - has link to github
- [Intel's developer community](https://devmesh.intel.com/)
- [Some interesting projects (not hardware, but was pointed to it at the OpenVINO workshop](https://github.com/IntelLabs) 
- If you need to use FPGAs from Matlab check out [this 32-minute online course](https://www.intel.com/content/www/us/en/programmable/support/training/course/odspintro.html) on DSP Builder. 

## Past Events

- May 2-5, 2019 - Intel OpenCL training attended by 5 people
- Jan 18, 2019 - FPGA brown bag lunch
- Nov xx, 2018 - AI on PC workshop (for engineering undergraduate students)
- Oct 8, 2018 - FPGA Workshop by Bill Jenkins

## Directory organization

The software is in `/scratch/inteldevstack*`  directory: 

- `intelFPGA_pro`  - this is where you will find HLS and Quartus
- `a10_gx_pac_ias_1_2_pv` - this is where you will find OpenCL tools. 

### Instructions to run helloworld in OpenCL

See a separate example in the repo. 

## Sharing model - TO BE IMPLEMENTED

ERN federation runs a scheduler called Slurm. Slurm basically uses cgroups to limit users' resources on a shared machine. What we have in mind is the following setup: 
- `-p fpga` is a slurm partition on ERN that contains the fpga1 machine
- `--gres=fpga:1` is to specify how many boards you want to use at a time (here, 1)


# Set your path correctly 

```
export QUARTUS_HOME="/scratch/inteldevstack_v1.2/intelFPGA_pro/quartus"
export INTELFPGAOCLSDKROOT="/scratch/inteldevstack_v1.2/intelFPGA_pro/hld"
export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3
QUARTUS_BIN="/scratch/inteldevstack_v1.2/intelFPGA_pro/quartus/bin"
export PATH="${QUARTUS_BIN}":"${PATH}"
export OPAE_PLATFORM_ROOT="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv"
export AOCL_BOARD_PACKAGE_ROOT="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp"
OPAE_PLATFORM_BIN="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/bin"
export PATH="${PATH}":"${OPAE_PLATFORM_BIN}"

export  QSYS_ROOTDIR="/scratch/inteldevstack_v1.2/intelFPGA_pro/qsys/bin"
export ALTERAOCLSDKROOT="/scratch/inteldevstack_v1.2/intelFPGA_pro/hld"

source $INTELFPGAOCLSDKROOT/init_opencl.sh #sets LD_LIBRARY_PATH correctly - for people who want to use OpenCL
```

# Open graphical programs

See the file vncserver.md in this directory for the instructions how to install vnc client (on a Linux machine). Not sure about Mac or Windows. (TODO)
