

# Quickstart

Reference: [Intel guide](Following: https://www.intel.com/content/www/us/en/programmable/documentation/fvf1521490619217.html#gji1513206065498)
```
#instead of manually adding the path as in the commented lines below, source this file:  
source $INTELFPGAOCLSDKROOT/init_opencl.sh #sets 

#in order to put aocl in your path:
#export PATH="${PATH}":"${INTELFPGAOCLSDKROOT}/bin"
#in order to find libintel_opae_mmd.so library
#export LD_LIBRARY_PATH=/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp/linux64/lib/

#execute compiled program
aocl program acl0  $OPAE_PLATFORM_ROOT/opencl/hello_world.aocx

#run diagnostics
aocl diagnose acl0   # can be replaced with acl1, acl2, acl3, depending on the card
```

## Add these to your path: 

This is not in the guide, and errors were appearing because of it: 
```
export PATH="${PATH}":"${INTELFPGAOCLSDKROOT}/bin"
export LD_LIBRARY_PATH=/scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp/linux64/lib/
```

## Run a compiled program

```
[root@fpga1 opencl]# aocl program acl0  $OPAE_PLATFORM_ROOT/opencl/hello_world.aocx
aocl program: Running program from /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp/linux64/libexec
Program succeed. 
```

## Run diagnostics on a card: 

```
[root@fpga1 opencl]# aocl diagnose acl0
Using platform: Intel(R) FPGA SDK for OpenCL(TM)
Using Device with name: pac_a10 : PAC Arria 10 Platform (pac_a10_ec00003)
Using Device from vendor: Intel Corp
clGetDeviceInfo CL_DEVICE_GLOBAL_MEM_SIZE = 8589934592
clGetDeviceInfo CL_DEVICE_MAX_MEM_ALLOC_SIZE = 8588886016
Memory consumed for internal use = 1048576
Actual maximum buffer size = 8588886016 bytes
Writing 8191 MB to global memory ...
Allocated 1073741824 Bytes host buffer for large transfers
Write speed: 6847.94 MB/s [6631.20 -> 6884.52]
Reading and verifying 8191 MB from global memory ...
Read speed: 6049.14 MB/s [6033.20 -> 6070.95]
Successfully wrote and readback 8191 MB buffer

Transferring 262144 KBs in 512 512 KB blocks ... 3111.15 MB/s
Transferring 262144 KBs in 256 1024 KB blocks ... 3275.55 MB/s
Transferring 262144 KBs in 128 2048 KB blocks ... 4463.07 MB/s
Transferring 262144 KBs in 64 4096 KB blocks ... 5399.42 MB/s
Transferring 262144 KBs in 32 8192 KB blocks ... 6041.82 MB/s
Transferring 262144 KBs in 16 16384 KB blocks ... 6441.45 MB/s
Transferring 262144 KBs in 8 32768 KB blocks ... 6627.33 MB/s
Transferring 262144 KBs in 4 65536 KB blocks ... 6769.88 MB/s
Transferring 262144 KBs in 2 131072 KB blocks ... 6828.36 MB/s
Transferring 262144 KBs in 1 262144 KB blocks ... 6822.85 MB/s

As a reference:
PCIe Gen1 peak speed: 250MB/s/lane
PCIe Gen2 peak speed: 500MB/s/lane
PCIe Gen3 peak speed: 985MB/s/lane

Writing 262144 KBs with block size (in bytes) below:

Block_Size Avg    Max    Min    End-End (MB/s)
  524288 2058.97 3042.24 856.16 1872.51
 1048576 2607.50 3275.55 2333.58 2395.04
 2097152 4085.93 4463.07 3237.24 3972.24
 4194304 4746.49 5399.42 3954.22 4665.09
 8388608 5627.90 6041.82 4877.88 5579.72
16777216 6196.91 6441.45 5707.22 6172.12
33554432 6523.24 6627.33 6314.47 6508.68
67108864 6708.29 6769.88 6645.83 6701.22
134217728 6803.87 6828.36 6779.55 6801.69
268435456 6822.85 6822.85 6822.85 6822.85

Reading 262144 KBs with block size (in bytes) below:

Block_Size Avg    Max    Min    End-End (MB/s)
  524288 2519.59 3111.15 861.00 2272.84
 1048576 2611.26 3118.65 1238.56 2441.12
 2097152 2788.76 3206.86 2334.09 2715.91
 4194304 2813.12 3144.83 2620.39 2786.86
 8388608 3873.45 4182.43 3554.50 3854.40
16777216 4723.26 4940.43 4431.28 4709.61
33554432 4736.41 5404.73 3067.08 4728.58
67108864 5610.48 5624.99 5583.36 5606.82
134217728 5868.80 5889.10 5848.65 5867.27
268435456 5982.05 5982.05 5982.05 5982.05

Write top speed = 6828.36 MB/s
Read top speed = 5982.05 MB/s
Throughput = 6405.21 MB/s

DIAGNOSTIC_PASSED
[root@fpga1 opencl]# 
```

## Everything together

```
[kp807@fpga1 tmp]$ mkdir opencl_vector_add
[kp807@fpga1 tmp]$ cd opencl_vector_add/
[kp807@fpga1 opencl_vector_add]$ cp /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/exm_opencl_vector_add_x64_linux.tgz .
[kp807@fpga1 opencl_vector_add]$ ls
exm_opencl_vector_add_x64_linux.tgz
[kp807@fpga1 opencl_vector_add]$ tar xzvf exm_opencl_vector_add_x64_linux.tgz 
vector_add/README.html
vector_add/Makefile
vector_add/device/vector_add.cl
vector_add/host/src/main.cpp
common/readme.css
common/inc/AOCLUtils/scoped_ptrs.h
common/inc/AOCLUtils/aocl_utils.h
common/inc/AOCLUtils/opencl.h
common/inc/AOCLUtils/options.h
common/src/AOCLUtils/opencl.cpp
common/src/AOCLUtils/options.cpp
[kp807@fpga1 opencl_vector_add]$ ls
common  exm_opencl_vector_add_x64_linux.tgz  vector_add
[kp807@fpga1 opencl_vector_add]$ cd vector_add/
[kp807@fpga1 vector_add]$ ls
device  host  Makefile  README.html
[kp807@fpga1 vector_add]$ make
[kp807@fpga1 vector_add]$ ls
bin  device  host  Makefile  README.html
[kp807@fpga1 vector_add]$ ./bin/host 
Initializing OpenCL
Platform: Intel(R) FPGA SDK for OpenCL(TM)
Using 1 device(s)
  pac_a10 : PAC Arria 10 Platform (pac_a10_ec00003)
Using AOCX: vector_add.aocx
AOCX file 'vector_add.aocx' does not exist.
ERROR: CL_INVALID_PROGRAM 
Location: ../common/src/AOCLUtils/opencl.cpp:370
Failed to load binary file
[kp807@fpga1 vector_add]$ cp $OPAE_PLATFORM_ROOT/opencl/vector_add.aocx ./bin
[kp807@fpga1 vector_add]$ ./bin/host 
Initializing OpenCL
Platform: Intel(R) FPGA SDK for OpenCL(TM)
Using 1 device(s)
  pac_a10 : PAC Arria 10 Platform (pac_a10_ec00003)
Using AOCX: vector_add.aocx
Context callback: Specified kernel was not built for any devices
ERROR: CL_INVALID_KERNEL_NAME 
Location: host/src/main.cpp:169
Failed to create kernel
[kp807@fpga1 vector_add]$ aocl program acl0 bin/vector_add.aocx 
aocl program: Running program from /scratch/inteldevstack_v1.2/a10_gx_pac_ias_1_2_pv/opencl/opencl_bsp/linux64/libexec
Program succeed. 
[kp807@fpga1 vector_add]$ ./bin/host 
Initializing OpenCL
Platform: Intel(R) FPGA SDK for OpenCL(TM)
Using 1 device(s)
  pac_a10 : PAC Arria 10 Platform (pac_a10_ec00003)
Using AOCX: vector_add.aocx
Launching for device 0 (1000000 elements)

Time: 10.705 ms
Kernel time (device 0): 3.756 ms

Verification: PASS
[kp807@fpga1 vector_add]$ 
```
