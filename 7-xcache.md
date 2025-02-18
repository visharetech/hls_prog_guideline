## 7. XCACHE

Generally, the data could be shared by XCACHE for fast and convenient access. Each core has its own XCACHE region.

```C
typedef struct {
    int xm_a _ALIGN;
    int xm_y _ALIGN;
    int xm_arr[10] _ALIGN;
    struct vector_2d xm_v _ALIGN;
}xmem_t;
```

Support datatype:
* Native var type (int, unsigned short, uint8_t, uint16_t, bool, enum....etc..)
* Structure var type
* Nested structure
* Array (complete array partition)
* Array (single port ap_memory interface)
* Array (cyclic partition)
Currently, it doesn't support arrays of structure (struct person[10]..etc..).

### 7.1 Attention
1. The width of the variable in the XCACHE must match the memory fetch size of the load / store instruction. For example, a 1-byte data (uint8_t) declared in XCACHE should be accessed using dedicated LB (Load Byte) / SB (Store Byte) instructions, rather than the 2-byte or 4 byte instructions like SW (Store Word), LW(Load Word), LH (Load Halfword),SH (Store Halfword) and so on. To ensure that the correct instructions are used, you could utilize the XMEM_SET, XMEM_ASSIGN, XMEM_COPY related macros or xmem_assign(), xmem_set(), xmem_copy() C++ template functions.

2. To access an array starting from a specific index IDX, we must pass the offset as an additional argument. For example, if xm_dst and xm_src are the array types, and you want to perform an XOR operation starting from IDX, you should call the HLS function like this
```c
xor(xmem->xm_dst, xmem->xm_src, IDX);
```

You should not use the following statement:
```c
//wrong
xor(xmem->xm_dst, &xmem->xm_src[IDX]);
```
