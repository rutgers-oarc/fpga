
# Quickstart - only commands

```
#you need to get permissions to all the files, so copy the example to somewhere
cp -r /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/hw/samples/hello_afu /tmp/  

# build .gbs file
cd /tmp/hello_afu
afu_synth_setup --source hw/rtl/filelist.txt build_synth  # will create a build_synth directory
cd build_synth
${OPAE_PLATFORM_ROOT}/bin/run.sh    # this is the longest portion with lots of output

# configure fpga
fpgainfo port     # get info on bus etc
fpgaconf --help   # usage
fpgaconf  -B 59 -S 0 hello_afu.gbs  #convert hexadecimal bus to decimal to specify 1 of 4 boards

# run software
cd ../sw/
make clean
make
./hello_afu
```

# Big tip: read the README

All these commands (with the exception of specifying one of 4 cards) come from the README file in the home directory of the example. 

# Outputs of commands

## Getting your path correct

```
export QUARTUS_HOME="/scratch/inteldevstack_v1.2/intelFPGA_pro/quartus"
export INTELFPGAOCLSDKROOT="/scratch/inteldevstack_v1.2/intelFPGA_pro/hld"
export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3
QUARTUS_BIN="/scratch/inteldevstack_v1.2/intelFPGA_pro/quartus/bin"
export PATH="${QUARTUS_BIN}":"${PATH}"
export OPAE_PLATFORM_ROOT="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv"
export AOCL_BOARD_PACKAGE_ROOT="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp"
source $AOCL_BOARD_PACKAGE_ROOT/linux64/libexec/setup_permissions.sh >> /dev/null   # need to check this one
OPAE_PLATFORM_BIN="/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/bin"
export PATH="${PATH}":"${OPAE_PLATFORM_BIN}"

export  QSYS_ROOTDIR="/scratch/inteldevstack_v1.2/intelFPGA_pro/qsys/bin"
export ALTERAOCLSDKROOT="/scratch/inteldevstack_v1.2/intelFPGA_pro/hld"
```

## Setup
```
[kp807@fpga1 hello_afu]$ afu_synth_setup --source hw/rtl/filelist.txt build_synth
Copying build from /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/hw/lib/build...
Configuring Quartus build directory: build_synth/build
Loading platform database: /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/hw/lib/platform/platform_db/a10_gx_pac_hssi.json
Loading platform-params database: /usr/share/opae/platform/platform_db/platform_defaults.json
Loading AFU database: /usr/share/opae/platform/afu_top_ifc_db/ccip_std_afu.json
Writing platform/platform_afu_top_config.vh
Writing platform/platform_if_addenda.qsf
Writing ../hw/afu_json_info.vh
[kp807@fpga1 hello_afu]$ 
```

## Building the .gbs file

This is going to take a while and produce a lot of output (debug flag is set to INFO). The following is the end of this long output: 

```
Info: Quartus Prime TimeQuest Timing Analyzer was successful. 0 errors, 188 warnings
    Info: Peak virtual memory: 4633 megabytes
    Info: Processing ended: Fri Feb  1 18:38:18 2019
    Info: Elapsed time: 00:00:41
    Info: Total CPU time (on all processors): 00:01:14
Info (19538): Reading SDC files took 00:00:07 cumulatively in this process.
Wrote hello_afu.gbs

===========================================================================
 PR AFU compilation complete
 AFU gbs file is 'hello_afu.gbs'
 Design meets timing
===========================================================================
```

## Getting info about a specific FPGA

Look at the PCIe bus part. In this case D8 is the bus number in hexadecimal. You have to convert it to decimal and use as the argument to give to the fpgaconf command. 

```
[kp807@fpga1 build_synth]$ fpgainfo port
Board Management Controller, microcontroller FW version 26889
Last Power Down Cause: POK_CORE
Last Reset Cause: None
//****** PORT ******//
Object Id                     : 0xEC00003
PCIe s:b:d:f                  : 0000:D8:00:0
Device Id                     : 0x09C4
Socket Id                     : 0x00
Ports Num                     : 01
Bitstream Id                  : 0x123000200000185
Bitstream Version             : 0x55A500030201
Pr Interface Id               : 69528db6-eb31-577a-8c36-68f9faa081f6
Accelerator Id                : f7df405c-bd7a-cf72-22f1-44b0b93acd18
Board Management Controller, microcontroller FW version 26889
Last Power Down Cause: POK_CORE
Last Reset Cause: None
//****** PORT ******//
Object Id                     : 0xEC00002
PCIe s:b:d:f                  : 0000:87:00:0
Device Id                     : 0x09C4
Socket Id                     : 0x00
Ports Num                     : 01
Bitstream Id                  : 0x123000200000185
Bitstream Version             : 0x55A500030201
Pr Interface Id               : 69528db6-eb31-577a-8c36-68f9faa081f6
Accelerator Id                : f7df405c-bd7a-cf72-22f1-44b0b93acd18
Board Management Controller, microcontroller FW version 26889
Last Power Down Cause: POK_CORE
Last Reset Cause: None
//****** PORT ******//
Object Id                     : 0xEC00001
PCIe s:b:d:f                  : 0000:5F:00:0
Device Id                     : 0x09C4
Socket Id                     : 0x00
Ports Num                     : 01
Bitstream Id                  : 0x123000200000185
Bitstream Version             : 0x55A500030201
Pr Interface Id               : 69528db6-eb31-577a-8c36-68f9faa081f6
Accelerator Id                : f7df405c-bd7a-cf72-22f1-44b0b93acd18
Board Management Controller, microcontroller FW version 26889
Last Power Down Cause: POK_CORE
Last Reset Cause: None
//****** PORT ******//
Object Id                     : 0xEC00000
PCIe s:b:d:f                  : 0000:3B:00:0
Device Id                     : 0x09C4
Socket Id                     : 0x00
Ports Num                     : 01
Bitstream Id                  : 0x123000200000185
Bitstream Version             : 0x55A500030201
Pr Interface Id               : 69528db6-eb31-577a-8c36-68f9faa081f6
Accelerator Id                : f7df405c-bd7a-cf72-22f1-44b0b93acd18
```

## Configuring the board

```
[kp807@fpga1 build_synth]$ fpgaconf --help

fpgaconf
FPGA configuration utility

Usage:
        fpgaconf [-hvn] [-B <bus>] [-D <device>] [-F <function>] [-S <socket-id>] <gbs>

                -h,--help           Print this help
                -v,--verbose        Increase verbosity
                -n,--dry-run        Don't actually perform actions
                --force             Don't try to open accelerator resource
                -B,--bus            Set target bus number
                -D,--device         Set target device number
                -F,--function       Set target function number
                -S,--socket-id      Set target socket number
```


Now, 3B in hex = 59 in decimal, so we specify this bus. If you notice, `PCI s:b:d:f`  is `socket:bus:device:function`, so socket is 0000, bus is 3B, device is 0 and function is 0. 

```
[kp807@fpga1 build_synth]$ fpgaconf  -B 59 -S 0 hello_afu.gbs 
[kp807@fpga1 build_synth]$ ls
build  design  hello_afu.gbs  hw
[kp807@fpga1 build_synth]$ cd ..
[kp807@fpga1 hello_afu]$ ls
bin  build.sim  build_synth  hw  README  sw
[kp807@fpga1 hello_afu]$ cd sw/
[kp807@fpga1 sw]$ make clean
rm -f hello_afu hello_afu.o afu_json_info.h
[kp807@fpga1 sw]$ make
afu_json_mgr json-info --afu-json=../hw/rtl/hello_afu.json --c-hdr=afu_json_info.h
Writing afu_json_info.h
gcc -fstack-protector -fPIE -fPIC -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror -g -O2 -std=c99 -Wall -Wno-unknown-pragmas -c -o hello_afu.o hello_afu.c
gcc -fstack-protector -fPIE -fPIC -O2 -D_FORTIFY_SOURCE=2 -Wformat -Wformat-security -Werror -g -O2 -std=c99 -Wall -Wno-unknown-pragmas -o hello_afu hello_afu.o -z noexecstack -z relro -z now -pie -luuid -lpthread -lopae-c
[kp807@fpga1 sw]$ ls 
afu_json_info.h  hello_afu  hello_afu.c  hello_afu.o  Makefile
```

## now let’s run the software

```
[kp807@fpga1 sw]$ ./hello_afu 
Running Test
AFU DFH REG = 1000010000000000
AFU ID LO = 9722d43375b61c66
AFU ID HI = 850adcc26ceb4b22
AFU NEXT = 00000000
AFU RESERVED = 00000000
Reading Scratch Register (Byte Offset=00000080) = 00000000
MMIO Write to Scratch Register (Byte Offset=00000080) = 123456789abcdef
Reading Scratch Register (Byte Offset=00000080) = 123456789abcdef
Setting Scratch Register (Byte Offset=00000080) = 00000000
Reading Scratch Register (Byte Offset=00000080) = 00000000
Done Running Test
```


