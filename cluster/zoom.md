# 集群伸缩

## 集群伸缩
### 扩容集群
1. 启动新节点: `redis-server conf/redis_6385.conf`
2. 加入集群： `redis-cli h 127.0.0.1 -p 6379 cluster meet 127.0.0.1 6385`
    新节点刚开始都是主节点状态，但是由于没有负责的槽，所以不能接受任何的读写操作
    所以它一般有两种选择：
    -  为它迁移槽和数据实现扩容
    -  作为其他主节点的从节点负责故障转移
    
    可以用`redis-trib.rb`工具添加节点：
    usage: `redis-trib.rb add-node new_host:now_port existing_host:existing_port --slave --master-id <arg>`
    example: `redis-trib.rb add-node 127.0.0.1:6385 127.0.0.1:6379`

3. 迁移槽和数据
`redis-trib.rb`工具提供了槽重分片的功能:
usage:`redis-trib.rb reshared host:port --from <arg> -- to <arg> --slots <arg> --yes --timeout <arg> --pipeline <arg>`
`host:port`：必传，集群内任意节点地址
`--from`:源节点的id；如果有多个源节点，逗号分隔；如果是`all`表示集群内所有主节点
`--to`: 需要迁移的目标节点id，目标节点只能写一个
`--slots`:需要迁移的槽的总数量
`--yes`:当打印出reshared执行计划时，是否需要用户输入yes确认后再执行reshared
`--timeout`:控制每次migrate操作的超时时间，默认`60,000`毫秒
`--pipeline`:控制每次批量迁移键的数量，默认10
### 收缩集群
1. 下线迁移槽
如果下线的节点持有槽，则下线节点需要把自己负责的槽迁移到其他节点；通过`redis-trib.rb reshared`命令来完成
2. 忘记节点
使用`redis-trib.rb del-node`工具来完成
