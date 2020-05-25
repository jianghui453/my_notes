# intset

```
typedef struct intset {

    // 保存元素所使用的类型的长度
    uint32_t encoding;

    // 元素个数
    uint32_t length;

    // 保存元素的数组
    int8_t contents[];

} intset;
```