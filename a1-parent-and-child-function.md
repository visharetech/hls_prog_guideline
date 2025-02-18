## Parent and Child Function
You could use HLS_CHILD_CALL() to call a HLS child function in the parent HLS function.

```C
HLS_CHILD_INIT();
HLS_CHILD_CALL(hls_id, arg0, arg1, arg2, arg3, arg4, arg5, arg6, arg7);
```

In the parent HLS function, HLS_CHILD_INIT() should be invoked first before any HLS child function.

If the variable is modified by a child function, the volatile keyword should be added to the parent function's argument so that Vitis HLS can identify the data that might change after HLS_CHILD_CALL is involved.

Using the flow detection feature in asim can identify the variable dependency between the parent function and the child function, making it easier to determine which arguments should add the volatile keyword.

### Prerequisite
1. The flow detect option should be enabled in asim
2. The riscv ELF should be in single-thread mode (without pthread_create) because asim only supports running flow detection in single-thread mode.

### Procedure
1. Run the elf with asim, ensure that the flow detection option is enabled.
```c
#./asim --elfFileName program.elf program {additional argument}
./asim --elfFileName example.elf example
```

2. After asim executed, the report will be exported to out/dataflow.csv
3. In libreoffice, open the csv file and apply the standard filter with below expression:
* XmemOffset <> None
* Read Function contains [parent function]

Check the field column. If it shows child write parent read, it means that the dedicated xmem variable is altered by the child function and subsequently read by the parent function. Therefore, volatile keywords should be added to the associated argument.