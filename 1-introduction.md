## 1. Introduction
Vishare hls_tools are primarily used to simplify the HLS synthesis and connection process. It can automatically generate relevant SystemVerilog test cases and verify the results in ModelSim. Additionally, it generates an appropriate cpp file which contains the hardware function call interface, which can be run on riscv simulator (asim).

Features:
1. Identify and extract the functions that are surrounded by the IMPL() macro from the source file 'hls.cpp'
2. Automatic generate code to capture test data and create test cases for the above functions
3. Run Vitis HLS to batch synthesis the HLS functions
4. Capture the test data
5. Batch verify the test cases in Vitis co-simulation
6. Generate RTL function and testbench by autonet to run in ModelSim
7. Export the rtl code could be copied and placed to RTL code to simplify the process and minimize the typo error
8. Generate the hardware function call interface for RISC-V

IMPL() macro should be added for each HLS function so that the hls_tools can identify and automatically generate additional functions for capturing data and performing co-simulation. Additionally, several interfaces are provided for HLS functions to share the data with RISC-V. They are XCACHE, DCACHE and APCALL arguments, which will be described in a  later section.
