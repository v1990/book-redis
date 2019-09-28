# hash

## 内部编码
- zaplist: 压缩列表
- hashtable： 哈希表

当元素个数小于`hash-max-zpilist-entries`配置（默认512个），同时所有值都小于`hash-max-ziplist-value`配置（默认64字节），redis会使用`ziplist`作为hash的内部实现。
ziplist使用更加紧凑的结构实现多个元素的连续存储，所以在节省内存方面比hashtable更加优秀。
当不满足上述两个条件时，ziplist的读写效率会下降，而hashtable的读写时间复杂度为O(1)