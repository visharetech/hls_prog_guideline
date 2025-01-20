## 1. Introduction
hls_tools are primarily used to simplify the HLS synthesis and connection process. It can also be used to automatically generate the test cases and verify the results in ModelSim.

Features:
1. Scan and extract the functions surrounded with IMPL() macro from the source 'hls.cpp'
2. Automatic generate testcases from the extracted functions
3. Run Vitis HLS to batch synthesis the HLS function
4. Capture the test data
5. Run the testcases by Vitis C simulation
6. Generate RTL function and testbench by autonet to run in ModelSim
7. Export the rtl code could be copy and placed to RTL code to simplify the process and minimze the typo error.


IMPL() macro should be added for each HLS function so that the hls_tools can identify and automatic generate additional functions for capturing data / perform co-simulation. Additional, several memory interfaces are provided for HLS functions to share the data with RISC-V. They are XMEM, DCACHE and APCALL arguments.
                                                                     |
