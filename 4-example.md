## 4. Run the Example
The example is placed in the hls_tools/tutorial_example directory

1. Specify the vitis_hls path in run.sh. You could also modify the project path and export destination in run.sh according to different projects.
```bash
cd tutorial_example/hls_tools_script
nano run.sh
# Edit the following variable to specify the executable path:
# xilinx_vitis=/mnt/d/Xilinx/Vitis_HLS/2023.2/bin/vitis_hls
```

1. Synthesis all HLS functions
```bash
./run.sh xhls all
```

2. (Optional) Capture the test data
```bash
./run.sh capture vector_add
```

The default is to capture 10,000 times, you can manually capture a specific count by adding the desired number before the function
```bash
./run.sh capture 1000 vector_add
```

3. (Optional) Run the Vitis C simulation
```bash
./run.sh csim vector_add
```

4. Connect all HLS to common bus interface
```bash
./run.sh xgen all
```

After finishing, relevant RTL and SystemVerilog testbench will be generated to the export destination. Simply copy the rtl code to a dedicated directory to run in the FPGA.

#### 4.1 Other useful option:
* Specify the desired HLS function list in the file, separated by new lines. The parser will ignore the line starts with #
```bash
./run.sh xhls hls_func_list.txt
```

* Specify which HLS functions should be synthesized, accept multiple function name
```bash
./run.sh xhls vector_add
./run.sh xhls vector_add array_xor
```

* Specify the HLS function list that connect to the riscv
```bash
./run.sh xgen hls_func_list.txt
```