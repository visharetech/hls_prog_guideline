## 2. System Synthesis Steps
* Identify a list of accelerator functions 
* Determine the data structured to be mapped to xcache 
* Based on the bandwidth and storage requirements of accelerated functions, define the number of cache banks, widths of different banks and storage capacity of each bank 
* Rewrite the C code to make use of the hardware accelerator and xcache 
  * define the functions 
  * declare the data in xcache data structure 
* Synthesis with accelerator hardware with HLS 
* Synthesis the system with 
  * decide the parameter of standard components 
    * RISC core number 
    * L1 cache size 
    * L2 cache size 
* Synthesis the RTL code 
* Perform simulation
* Check the correctness and evaluate the performance on the FPGA board
