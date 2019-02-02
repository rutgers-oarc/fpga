
# Quickstart

- path to i++ compiler: `/scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++`
```
cd /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/examples/counter
i++ counter.cpp  -v  -march=Arria10 -o test-fpga --simulator none  #default is modelsim, we don't have it yet
```

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

## veryfing it's here and visible to all
```
[root@fpga1 hls]# which i++
/scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++
[root@fpga1 hls]# 
[root@fpga1 hls]# 
[root@fpga1 hls]# i++ --version
Intel(R) HLS Compiler
Version 17.1.0 Build 240
Copyright (C) 2017 Intel Corporation
[root@fpga1 hls]# 
[root@fpga1 hls]# 
[root@fpga1 hls]# ls -ltra /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++
-rwxr-xr-x. 1 root root 1842396 Oct 26  2017 /scratch/inteldevstack_v1.2/intelFPGA_pro/hls/bin/i++
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


## try to compile test-fpga without simulator

You have to specify simulator none, if there is no simulator. 

```
[root@fpga1 counter]# i++ counter.cpp  -v  -march=Arria10 -o test-fpga --simulator none
Target FPGA part name:   10AX115U1F45I1SG
Target FPGA family name: Arria 10
Target FPGA speed grade: -2
Analyzing counter.cpp for hardware generation
Optimizing component(s) and generating Verilog files
```

There is a lot of reports generated in the reports directory: 

```
[root@fpga1 counter]# tree
.
├── counter.cpp
├── Makefile
├── README
└── test-fpga.prj
    ├── components
    │   └── count
    │       ├── count
    │       │   ├── count_bb.v
    │       │   ├── count.cmp
    │       │   ├── count_generation.rpt
    │       │   ├── count.html
    │       │   ├── count_inst.v
    │       │   ├── count_inst.vhd
    │       │   ├── count_internal_10
    │       │   │   └── synth
    │       │   │       ├── acl_data_fifo.v
    │       │   │       ├── acl_dspba_buffer.v
    │       │   │       ├── acl_dspba_valid_fifo_counter.v
    │       │   │       ├── acl_enable_sink.v
    │       │   │       ├── acl_fanout_pipeline.sv
    │       │   │       ├── acl_fifo.v
    │       │   │       ├── acl_high_speed_fifo.sv
    │       │   │       ├── acl_lfsr.sv
    │       │   │       ├── acl_ll_fifo.v
    │       │   │       ├── acl_ll_ram_fifo.v
    │       │   │       ├── acl_low_latency_fifo.sv
    │       │   │       ├── acl_pipeline.v
    │       │   │       ├── acl_pop.v
    │       │   │       ├── acl_push.v
    │       │   │       ├── acl_reset_handler.sv
    │       │   │       ├── acl_reset_wire.v
    │       │   │       ├── acl_staging_reg.v
    │       │   │       ├── acl_std_synchronizer_nocut.v
    │       │   │       ├── acl_tessellated_incr_decr_threshold.sv
    │       │   │       ├── acl_tessellated_incr_lookahead.sv
    │       │   │       ├── acl_token_fifo_counter.v
    │       │   │       ├── acl_valid_fifo_counter.v
    │       │   │       ├── acl_zero_latency_fifo.sv
    │       │   │       ├── bb_count_B0_runOnce_stall_region.vhd
    │       │   │       ├── bb_count_B0_runOnce.vhd
    │       │   │       ├── bb_count_B1_start_stall_region.vhd
    │       │   │       ├── bb_count_B1_start.vhd
    │       │   │       ├── count_B0_runOnce_branch.vhd
    │       │   │       ├── count_B0_runOnce_merge_reg.vhd
    │       │   │       ├── count_B0_runOnce_merge.vhd
    │       │   │       ├── count_B1_start_branch.vhd
    │       │   │       ├── count_B1_start_merge_reg.vhd
    │       │   │       ├── count_B1_start_merge.vhd
    │       │   │       ├── count_function.vhd
    │       │   │       ├── count_function_wrapper.vhd
    │       │   │       ├── count_internal.v
    │       │   │       ├── dspba_library_package.vhd
    │       │   │       ├── dspba_library.vhd
    │       │   │       ├── hld_fifo.sv
    │       │   │       ├── hld_fifo_zero_width.sv
    │       │   │       ├── i_acl_pipeline_keep_going_count6.vhd
    │       │   │       ├── i_acl_pipeline_keep_going_count_sr.vhd
    │       │   │       ├── i_acl_pipeline_keep_going_count_valid_fifo.vhd
    │       │   │       ├── i_acl_pop_i1_wt_limpop_count0.vhd
    │       │   │       ├── i_acl_pop_i1_wt_limpop_count_reg.vhd
    │       │   │       ├── i_acl_pop_i32_zz5counte3cnt_addr_0_pop3_count15.vhd
    │       │   │       ├── i_acl_push_i1_notexitcond_count8.vhd
    │       │   │       ├── i_acl_push_i1_wt_limpush_count2.vhd
    │       │   │       ├── i_acl_push_i1_wt_limpush_count_reg.vhd
    │       │   │       ├── i_acl_push_i32_zz5counte3cnt_addr_0_push3_count17.vhd
    │       │   │       ├── i_acl_sfc_exit_c0_wt_entry_count_c0_exit_count10.vhd
    │       │   │       ├── i_acl_sfc_exit_c1_wt_entry_count_c1_exit_count19.vhd
    │       │   │       ├── i_iord_bl_do_unnamed_count1_count12.vhd
    │       │   │       ├── i_iowr_bl_return_unnamed_count2_count21.vhd
    │       │   │       ├── i_sfc_c0_wt_entry_count_c0_enter_count.vhd
    │       │   │       ├── i_sfc_c1_wt_entry_count_c1_enter_count.vhd
    │       │   │       ├── i_sfc_logic_c0_wt_entry_count_c0_enter_count4.vhd
    │       │   │       ├── i_sfc_logic_c1_wt_entry_count_c1_enter_count13.vhd
    │       │   │       └── st_top.v
    │       │   ├── count.ipxact
    │       │   ├── count.qgsynthc
    │       │   ├── count.qip
    │       │   ├── count.sopcinfo
    │       │   ├── count.xml
    │       │   └── synth
    │       │       └── count.v
    │       ├── count_inst.v
    │       ├── count_internal_hw.tcl
    │       ├── count_internal.v
    │       ├── count.ip
    │       ├── interface_structs.v
    │       ├── ip
    │       │   ├── acl_data_fifo.v
    │       │   ├── acl_dspba_buffer.v
    │       │   ├── acl_dspba_valid_fifo_counter.v
    │       │   ├── acl_enable_sink.v
    │       │   ├── acl_fanout_pipeline.sv
    │       │   ├── acl_fifo.v
    │       │   ├── acl_high_speed_fifo.sv
    │       │   ├── acl_lfsr.sv
    │       │   ├── acl_ll_fifo.v
    │       │   ├── acl_ll_ram_fifo.v
    │       │   ├── acl_low_latency_fifo.sv
    │       │   ├── acl_pipeline.v
    │       │   ├── acl_pop.v
    │       │   ├── acl_push.v
    │       │   ├── acl_reset_handler.sv
    │       │   ├── acl_reset_wire.v
    │       │   ├── acl_staging_reg.v
    │       │   ├── acl_std_synchronizer_nocut.v
    │       │   ├── acl_tessellated_incr_decr_threshold.sv
    │       │   ├── acl_tessellated_incr_lookahead.sv
    │       │   ├── acl_token_fifo_counter.v
    │       │   ├── acl_valid_fifo_counter.v
    │       │   ├── acl_zero_latency_fifo.sv
    │       │   ├── bb_count_B0_runOnce_stall_region.vhd
    │       │   ├── bb_count_B0_runOnce.vhd
    │       │   ├── bb_count_B1_start_stall_region.vhd
    │       │   ├── bb_count_B1_start.vhd
    │       │   ├── count_B0_runOnce_branch.vhd
    │       │   ├── count_B0_runOnce_merge_reg.vhd
    │       │   ├── count_B0_runOnce_merge.vhd
    │       │   ├── count_B1_start_branch.vhd
    │       │   ├── count_B1_start_merge_reg.vhd
    │       │   ├── count_B1_start_merge.vhd
    │       │   ├── count_function.vhd
    │       │   ├── count_function_wrapper.vhd
    │       │   ├── hld_fifo.sv
    │       │   ├── hld_fifo_zero_width.sv
    │       │   ├── i_acl_pipeline_keep_going_count6.vhd
    │       │   ├── i_acl_pipeline_keep_going_count_sr.vhd
    │       │   ├── i_acl_pipeline_keep_going_count_valid_fifo.vhd
    │       │   ├── i_acl_pop_i1_wt_limpop_count0.vhd
    │       │   ├── i_acl_pop_i1_wt_limpop_count_reg.vhd
    │       │   ├── i_acl_pop_i32_zz5counte3cnt_addr_0_pop3_count15.vhd
    │       │   ├── i_acl_push_i1_notexitcond_count8.vhd
    │       │   ├── i_acl_push_i1_wt_limpush_count2.vhd
    │       │   ├── i_acl_push_i1_wt_limpush_count_reg.vhd
    │       │   ├── i_acl_push_i32_zz5counte3cnt_addr_0_push3_count17.vhd
    │       │   ├── i_acl_sfc_exit_c0_wt_entry_count_c0_exit_count10.vhd
    │       │   ├── i_acl_sfc_exit_c1_wt_entry_count_c1_exit_count19.vhd
    │       │   ├── i_iord_bl_do_unnamed_count1_count12.vhd
    │       │   ├── i_iowr_bl_return_unnamed_count2_count21.vhd
    │       │   ├── i_sfc_c0_wt_entry_count_c0_enter_count.vhd
    │       │   ├── i_sfc_c1_wt_entry_count_c1_enter_count.vhd
    │       │   ├── i_sfc_logic_c0_wt_entry_count_c0_enter_count4.vhd
    │       │   ├── i_sfc_logic_c1_wt_entry_count_c1_enter_count13.vhd
    │       │   └── st_top.v
    │       └── linux64
    │           └── lib
    │               └── dspba
    │                   └── Libraries
    │                       └── vhdl
    │                           └── base
    │                               ├── dspba_library_package.vhd
    │                               └── dspba_library.vhd
    ├── quartus
    │   ├── generate_report.tcl
    │   ├── quartus_compile.qpf
    │   ├── quartus_compile.qsf
    │   ├── quartus_compile.sdc
    │   ├── quartus_compile.v
    │   └── quartus.ini
    └── reports
        ├── lib
        │   ├── ace
        │   │   ├── Ace.foss
        │   │   ├── LICENSE
        │   │   └── src
        │   │       ├── ace.js
        │   │       ├── ext-searchbox.js
        │   │       ├── mode-c_cpp.js
        │   │       ├── mode-verilog.js
        │   │       ├── mode-vhdl.js
        │   │       └── theme-xcode.js
        │   ├── bootstrap
        │   │   ├── Bootstrap.foss
        │   │   ├── css
        │   │   │   ├── bootstrap.min.css
        │   │   │   └── bootstrap-theme.min.css
        │   │   ├── fonts
        │   │   │   ├── glyphicons-halflings-regular.eot
        │   │   │   ├── glyphicons-halflings-regular.svg
        │   │   │   ├── glyphicons-halflings-regular.ttf
        │   │   │   └── glyphicons-halflings-regular.woff
        │   │   └── js
        │   │       └── bootstrap.min.js
        │   ├── d3
        │   │   ├── D3.foss
        │   │   ├── d3.min.js
        │   │   ├── dagre-d3.foss
        │   │   └── dagre-d3.js
        │   ├── fancytree
        │   │   ├── fancytree.foss
        │   │   ├── jquery.fancytree-all.min.js
        │   │   └── skin-win8
        │   │       ├── icons.gif
        │   │       ├── kernelicon.png
        │   │       ├── memicon.png
        │   │       ├── ui.fancytree.css
        │   │       └── ui.fancytree.less
        │   ├── favicon.ico
        │   ├── fonts
        │   │   ├── IntelClear_Bd.ttf
        │   │   ├── IntelClear_It.ttf
        │   │   ├── IntelClear_Lt.ttf
        │   │   └── IntelClear_Rg.ttf
        │   ├── graph.js
        │   ├── jquery
        │   │   ├── jquery-2.1.1.min.js
        │   │   ├── jquery.foss
        │   │   ├── jquery-ui.min.css
        │   │   └── jquery-ui.min.js
        │   ├── json
        │   │   ├── area.json
        │   │   ├── area_src.json
        │   │   ├── info.json
        │   │   ├── lmv.json
        │   │   ├── loops.json
        │   │   ├── mav.json
        │   │   ├── quartus.json
        │   │   ├── summary.json
        │   │   └── warnings.json
        │   ├── main.css
        │   ├── main.js
        │   ├── report_data.js
        │   ├── table.js
        │   └── verification_data.js
        ├── report.html
        └── test_fpga.aoco
```

