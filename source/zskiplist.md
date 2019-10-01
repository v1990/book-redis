# 跳跃表:zskiplist

## 应用
- zset

## 节点：zskiplistNode
[源码](https://github.com/antirez/redis/blob/unstable/src/server.h#L923)
```c
typedef struct zskiplistNode {
    // member 对象
    sds ele;
    // source
    double score;
    // 后退指针:
    struct zskiplistNode *backward;
    // 层
    struct zskiplistLevel {
        // 前进指针
        struct zskiplistNode *forward;
        // 这个层跨越的节点数量
        unsigned long span;
    } level[];
} zskiplistNode;
```
## 跳跃表: zskiplist
[源码](https://github.com/antirez/redis/blob/unstable/src/server.h#L933-L937)
```c
typedef struct zskiplist {
    // 头/尾节点
    struct zskiplistNode *header, *tail;
    // 节点数量
    unsigned long length;
    // 目前表内所有节点的最大层数
    int level;
} zskiplist;
```

