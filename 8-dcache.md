## 8. DCACHE

If the variable size is too large / not suitable to fit into xmem, you could use the DCACHE interface to access the variable content in the risc-v.
Usage:

```C
void IMPL(dcache_example_hls)(DCACHE_ARG(uint8_t, table, 38400),
uint32_t dcache[DCACHE_SIZE]){
    uint8_t *table = (uint8_t*)DCACHE_GET(table);
    //... access the variable. for example,
    uint8_t lookup_value = table[N];
}
```

Please note that the access time depends on the cache miss rate, if the data is not inside the cache, the access time will be longer.