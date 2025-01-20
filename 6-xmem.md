## 6. xmem

Generally, the data could be shared by xmem for fast and convenient access.

```C
typedef struct {
    int xm_a        _ALIGN;
    int xm_y        _ALIGN;
    int xm_arr[10]  _ALIGN;
    struct vector v _ALIGN;
}xmem_t;
```

Support datatype:
* Native var type (int, unsigned short, uint8_t, uint16_t, bool, enum....etc..)
* Structure var type
* Nested structure
* Array (complete array partition)
* Array (single port ap_memory interface)
* Array (cyclic partition)

Currently, it doesn't support array of structure (struct person[10]..etc..).