## T.1 Tutorial Example

### 1.1 Port C code to HLS

In hls.cpp, suppose below cnn_hls function which consists of 3 inputs and 2 arrays type. According to the vitis HLS user guide, the output argument in C should be declared as a pointer / reference type. And the array type must explicitly specify the array size, such as uint8_t filter_map[8x8] rather than uint8_t *filter_map.

```c
void cnn_hls(int width, int height, int filter, char *pixel, char
*filter_map, int *sum)
```

The IMPL() and HLS_COMMON_ARG macro should be added to these HLS functions so that it can be recognized by hls_tools and generate valid hardware modules.<br/>
IMPL(<function_name>)(HLS_COMMON_ARG <arguments…>)
<br/>

Hence, the cnn function argument rewrite as below where pixel and sum are array type, width, height and filter are input type:

```c
void IMPL(cnn_hls)(HLS_COMMON_ARG int width, int height, int filter, char pixel[(MAX_WIDTH_SIZE + MAX_FILTER_SIZE -1 ) * (MAX_HEIGHT_SIZE + MAX_FILTER_SIZE - 1)], char filter_map[MAX_FILTER_SIZE * MAX_FILTER_SIZE], int sum[MAX_WIDTH_SIZE * MAX_HEIGHT_SIZE])
```

pixel and sum should also be declared in XCACHE.
```c
typedef struct xmem_t{
    //...
    char filter_map[MAX_FILTER_SIZE * MAX_FILTER_SIZE] _ALIGN;
    char pixel[(MAX_WIDTH_SIZE + MAX_FILTER_SIZE -1 ) * (MAX_HEIGHT_SIZE + MAX_FILTER_SIZE - 1)] _ALIGN;
    //...
}xmem_t;
```

In HLS, the array type could be customized by using different pragma options to implement one of the following memory type:
* generic ap_memory interface (default)
* complete array partitioning
* cyclic array partitioning

ap_memory means that during synthesis, the array will be treated as single / dual port memory which consists of ce (enable), q (read data port) and we (write enable), d (write data port) signal.

Complete partitioning decomposes the array into individual elements.In other words, a[5] will be decomposed into a_0, a_1, a_2, a_3, a_4 individual variable during synthesis.

Cyclic partitioning creates smaller arrays by interleaving elements from the original array. The array is partitioned cyclically by putting one element into each new array before coming back to the first array to repeat the cycle until the array is fully partitioned. Suppose the pragma cyclic factor = 4, it will create 4 ap_memory with each memory size divided by 4.

Please refer to AMD Vitis High Level Synthesis User Guide (UG1399)[https://docs.amd.com/r/en-US/ug1399-vitis-hls/HLS-Pragmas] for detailed description.

Both native and array types are supported in XCACHE as below.
| Argument Type                                                | Region                   | #HLS pragma should be added                                           |
| ------------------------------------------------------------ | ------------------------ | --------------------------------------------------------------------- |
| input native type <= 32-bit (e.g. uint32_t, uint8_t, int)    | XCACHE / APCALL argument | (none)                                                                |
| input native type > 32bit (e.g. uint64_t, long long, double) | XCACHE                   | (none)                                                                |
| output native type (pointer or reference)                    | XCACHE                   | (none)                                                                |
| array                                                        | XCACHE                   | (none)                                                                |
| array (complete partition)                                   | XCACHE                   | #pragma HLS array_partition variable=<name> type=complete_partition   |
| array (cyclic)                                               | XCACHE                   | #pragma HLS array_partition variable=<name> type=cyclic factor=<int>  |
| struct                                                       | XCACHE                   | #pragma HLS disaggregate variable=<variable>                          |
Table 2: Argument Type in XCACHE region

After executing the ./run.sh xgen command, the script will parse the Verilog code and the XCACHE structure, reordering the element in xmem.h if necessary. It will classify and reorganize the xmem into three categories: scalar, array, and cyclic.


### 1.2 hls_tools usage
Basic example is provided to demonstrate how to use the hls_tools
1. Please follow the installation section to install required components.
2. Configure the Xilinx Vitis path

```console
cd tutorial_example
cd hls_tools_script
```

Please edit $xilinx_vitis in ./run.sh to point to Vitis HLS program
```console
xilinx_vitis=/mnt/d/Xilinx/Vitis_HLS/2023.2/bin/vitis_hls
```

Ensure the script has executable permission

```
chmod +x ./run.sh
```

3. Synthesis HLS functions
To synthesis all HLS functions, type:
```console
./run.sh xhls all
```

To synthesis individual function, for example, cnn_hls, type:
```console
./run.sh xhls cnn_hls
```

4. (Optional) Capture the test data
The default is to capture the input/output argument of specify function 10000 times
``` console
./run.sh capture vector_add
```

You can customize the capture times by specify the number:
```console
./run.sh capture 10 vector_add
```

5. (Optional) Run the C simulation of the HLS function
```console
./run.sh csim cnn_hls
```

6. (Optional) Run the co-simulation of the HLS function
```console
./run.sh cosim cnn_hls
```

7. (Optional) Run the co-simulation with the waveform generated by the HLS function
```console
./run.sh cosimwave cnn_hls
```

8. Link up the HLS functions and generate the relevant systemVerilog testbench by tying:
```console
./run.sh xgen all
```

Relevant Verilog files will be generated under the autonet/export/tutorial_example directory The hls_apcall.cpp will also be updated in the tutorial_example project, which served as the hardware function call interface that can be checked on the RISC-V simulator (asim)

9. Run on RISC-V simulator (asim)
```console
cd ../build/riscv-sim
source ./make-Makefiles.sh
source ./run.sh
```