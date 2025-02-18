## 4. Detail Operation
When execute ./run.sh command, it will

1. Scan and extract the function list from the hls source file hls.cpp
2. Generate hls_enum.h (enum_header_file) to assign unique ID for each HLS function
3. Automatic generate the source code used to capture the test data and c testbench for Vitis (hls_capture.cpp and hls_tb.cpp) (${hls_src_file}_capture.cpp, ${hls_src_file}_tb.cpp) which could be used in below capture, csim, cosim and cosimwave argument.
4. Depends on the current input argument to perform below operation
   | argument   | Description                                                                                                                                       |
   | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
   | xhls       | batch synthesized all HLS functions by calling \vitis_batch_generate_rtl\gen_hls script                                                           |
   | capture    | rebuilt the source file with macro `CAPTURE_COSIM=1`, the wrapper function CAPTURE_(hls_func) will be invoked to dump the input / output data.    |
   | csim       | perform Vitis C simulation by using hls_tb.cpp                                                                                                    |
   | cosim      | perform Vitis cosimulation by using hls_tb.cpp                                                                                                    |
   | cosimwave  | perform Vitis cosimulation with waveform shown by using hls_tb.cpp                                                                                |
   | xgen       | It involves several operation: </br> 1. Reorder the xmem element in xmem.h (Classified as scalar type, array type, cyclic type) </br> 2. Generate XCACHE verilog connection </br> 3. Connect the HLS functions to the bus </br> 4. Generate verilog testbench in the autonet/export/tutorial_example</br> 5. Generate C routine in hls_apcall.h and hls_apcall.cpp to invoke the hardware function |

