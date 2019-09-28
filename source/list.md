# 双端列表:list

[源码](https://github.com/antirez/redis/blob/unstable/src/adlist.h)

```c
// 节点
typedef struct listNode {
    struct listNode *prev;  // 前驱节点
    struct listNode *next;  // 后续节点
    void *value;            // 值
} listNode;
      
// 链表
typedef struct list {
    listNode *head;                     // 表头指针
    listNode *tail;                     // 表尾指针
    void *(*dup)(void *ptr);            // 复制函数
    void (*free)(void *ptr);            // 释放函数
    int (*match)(void *ptr, void *key); // 比对函数
    unsigned long len;
} list;

// 迭代器
typedef struct listIter {
    listNode *next; // 下一节点
    int direction;  // 迭代方向
} listIter;  

```