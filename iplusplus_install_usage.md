
# Quickstart

- path to i++ compiler: `/scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++`

# Installing i++ - done by root

## Install/configure command and output: 
```
[root@fpga1 hls]# source init_hls.sh
Assuming current directory (/scratch/inteldevstack_v1.2/intelFPGA_pro/hls) is root of i++

Will use Quartus at /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/../quartus for internal i++ use
     Quartus at /scratch/inteldevstack_v1.2/intelFPGA_pro/quartus/bin/quartus_sh will be used for any direct use
Adding /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin to PATH
Adding /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/host/linux64/lib to LD_LIBRARY_PATH
```

## veryfing it's here
```
[root@fpga1 hls]# which i++
/scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++
[root@fpga1 hls]# i++ --version
Intel(R) HLS Compiler
Version 17.1.0 Build 240
Copyright (C) 2017 Intel Corporation
[root@fpga1 hls]# 
```

## try to compile 

```
root@fpga1 hls]# cd examples/counter
[root@fpga1 counter]# ls
counter.cpp  Makefile  README
[root@fpga1 counter]# make
No target specified, defaulting to test-x86-64
Available targets: test-x86-64, test-fpga, test-gpp, clean
i++ counter.cpp   -march=x86-64 -o test-x86-64
+----------------------------------------+
| Run ./test-x86-64 to execute the test. |
+----------------------------------------+
[root@fpga1 counter]# ./test-x86-64 
PASSED
[root@fpga1 counter]# 
```

## try to compile test-fpga

```
[root@fpga1 counter]# make test-fpga
i++ counter.cpp  -v  -march=Arria10 -o test-fpga
Error: Error accessing ModelSim.  Please ensure you have a valid ModelSim installation on your path.
       Check your ModelSim installation with "vsim -version" 

make: *** [test-fpga] Error 1
[root@fpga1 counter]# 
```

the line in Makefile with this target (we are not sure what HLS_CXX_FLAGS are, i.e. which options can be used): 

```
test-fpga: CXXFLAGS := $(CXXFLAGS) $(HLS_CXX_FLAGS) -march=Arria10 -o test-fpga
```



