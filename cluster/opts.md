# 集群运维


## 集群运维
### 集群完成性
为了保证集群完整性，默认情况下集群中16384个槽任何一个没有指派到节点时整个集群都不可用，执行任何命令都会返回操作
但是当持有槽的主节点下线时，从故障发现到自动完成转移期间整个集群时不可用状态，对于大多数业务无法容忍这种情况，因此建议将`cluster-require-full-coverage`设置为no，当主节点故障时只影响它负责槽的相关命令，不影响其他主节点的可用性
### 带宽消耗
集群内Gossip消息通信本身会消耗带宽，因此官方建议集群最大规模在1000以内；
在满足业务需要的情况下尽量避免大集群
### pub/sub广播问题
在集群模式下publish命令会向所有节点进行广播，造成每条publish数据都会在集群内所有节点传播一次，加重带宽负担；
针对这种情况建议使用`sentinel`结构专门用于pub/sub功能
### 集群倾斜
- 数据倾斜
如：节点和槽分配不均，不同槽对应键数量差异过大等；
使用`redis-trib.rb info {host:ip}`进行定位
可以使用`redis-trib.rb rebalance`进行平衡
- 请求倾斜
### 集群读写分离
#### 只读连接
#### 读写分离
### 手动故障转移
