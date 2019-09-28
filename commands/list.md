# list

## 内部编码
- ziplist
- linkedlist

当列表元素个数小于`list-max-ziplist-enties`配置（默认512个），同时列表中每个元素的值都小于`list-max-ziplist-value`配置（默认64字节），redis会使用ziplist来实现以减少内存的使用