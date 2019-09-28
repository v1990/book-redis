# 原子操作

原子操作的命令：

### MSET key value [key value …]
MSET 是一个原子性(atomic)操作， 所有给定键都会在同一时间内被设置， 不会出现某些键被设置了但是另一些键没有被设置的情况。
### MSETNX key value [key value …]
当且仅当所有给定键都不存在时， 为所有给定键设置值。
即使只有一个给定键已经存在， MSETNX 命令也会拒绝执行对所有键的设置操作

### SETEX key seconds value
将键 key 的值设置为 value ， 并将键 key 的生存时间设置为 seconds 秒钟

### EVAL script numkeys key [key …] arg [arg …]
<http://redisdoc.com/script/eval.html>