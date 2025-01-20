## 4. Detail Operation
when execute ./run.sh command, it will

1. Scan and extract the function list from the hls source file hls.cpp (`${hls_src_file}`)
2. Generate hls_enum.h (`enum_header_file`) to assign unique ID per dedicated HLS function
3. Automatic generate the source code used to capture the test data and c testbench for Vitis (hls_capture.cpp and hls_tb.cpp) (`${hls_src_file}_capture.cpp`, `${hls_src_file}_tb.cpp`)
4. Depends on the current input argument to perform below operation
   | argument   | Description                                                                                                                                       |
   | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
   | xhls       | batch synthesized all HLS functions by calling \vitis_batch_generate_rtl\gen_hls script                                                           |
   | capture    | rebuilt the source file with macro `CAPTURE_COSIM=1`, the wrapper function CAPTURE_(hls_func) will be invoked to dump the input / output data.    |
   | csim       | perform Vitis C simulation                                                                                                                        |
   | cosim      | perform Vitis cosimulation                                                                                                                        |
   | cosimwave  | perform Vitis cosimulation with waveform shown                                                                                                    |
   | xgen       | Reorder the xmem element in xmem.h (Classified as scalar type, array type, cyclic type) </br> Generate xmem </br> Connect the HLS functions to the bus</br> Generate verilog testbench and C function to invoke the hardware function |

