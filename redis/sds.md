# sds

```
struct sdshdr {
    long len;
    long free;
    char buf[];
};
```

The buf character array stores the actual string.

The len field stores the length of buf. This makes obtaining the length of a Redis string an O(1) operation.

The free field stores the number of additional bytes available for use.

Together the len and free field can be thought of as holding the metadata of the buf character array.

## references

1. [Hacking Strings](https://redis.io/topics/internals-sds)