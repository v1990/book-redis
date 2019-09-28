# 搭建集群

## 搭建集群
### 1. 准备节点
至少6个；
conf开启集群模式
```conf
# 节点端口
port xxxx
# 开启集群模式
cluster-enabled yes
# 节点超时时间，毫秒
cluster-node-timeout 15000
# 集群内部配置文件（不存在会自动创建）
cluster-config-file "conf/nodes-xxx.conf"
```
启动所有节点
```shell
redis-server conf/redis-6379.conf
redis-server conf/redis-6380.conf
redis-server conf/redis-6381.conf
...
```
### 2. 节点握手
节点握手是指一批运行在集群模式下的节点通过Gossip协议彼此通信，达到感知对方的过程
1. 客户端发起命令:
客户端向节点6379发起命令：`cluster meet 127.0.0.1 6380`,让节点6379与6380节点进行握手通信，`cluster meet`是一条异步命令，执行后立刻返回，然后内部发起与目标节点进行握手通信，过程如下：
    - 节点6379本地创建6380节点信息对象，并发送meet消息
    - 节点6380收到meet消息后，保存6379节点信息并回复pong消息
    - 之后节点6379和6380彼此定期通过ping/pong消息进程正常的节点通信
    
客户点继续向6379节点发起`meet`命令让其他节点也加入到集群中

向各个节点分别执行`cluster nodes`命令，可以看到它们彼此已经感知到对方的存在

节点建立握手后集群还不能正常工作，这时集群处于下线状态，所有的数据读写都被禁止；
可以通过`cluster info`命令获取集群的当前状态
### 3. 分配槽
通过cluster addslots命令为节点分配槽
```bash
redis-cli h 127.0.0.1 -p 6379 cluster addslots {0...5461}
redis-cli h 127.0.0.1 -p 6380 cluster addslots {5462...10922}
redis-cli h 127.0.0.1 -p 6381 cluster addslots {10923...16383}
```
此时集群进入在线状态，所有槽都已经分配给节点
执行`cluster info`命令查看集群状态
执行`cluster nodes`查看节点和槽的分配关系

### 4. 配置从节点
首次启动的节点和被分配槽的节点都是主节点
从节点负责复制主节点槽信息和相关数据
使用`cluster replicate [nodeId]`命令让一个节点成为从节点；nodeId为要复制的主节点id（使用`cluster nodes`查看主节点id）
```bash
redis-cli h 127.0.0.1 -p 6382 cluster replicate "$node_id_6379"
redis-cli h 127.0.0.1 -p 6383 cluster replicate "$node_id_6380"
redis-cli h 127.0.0.1 -p 6384 cluster replicate "$node_id_6381"
```
再次通过`cluster nodes`查看集群状态和复制关系；

目前，此集群由6个节点构成，
3个主节点负责处理槽和相关数据，
3个从节点负责故障转移

### 官方集群搭建工具：`redis-trib.rb`
