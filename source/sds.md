# 简单动态字符串:sds

[源码](https://github.com/antirez/redis/blob/unstable/src/sds.h)

- redis字符串表示为sds，而不是C字符串（char*）
- 可以高效地获取字符串长度
- 可以高效地执行追加
- 二进制安全

```c
struct sdshdr{
    int len;    // buf已占用长度
    int free;   // buf剩余可用长度
    char buf[]; // 实际保存字符串数据的地方
}
```