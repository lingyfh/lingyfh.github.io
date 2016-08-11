---
title: redis sentinel 简单配置
date: 2016-08-11 21:15:28
tags: redis sentinel
categories: redis
---

备注:
配置不要少于2个sentinel
sentinel表决不少于2个
超时间根据需求设置,为了高可用,时间不宜过长。


### 启动三个redis-server


**Master** redis_custom_6380.conf
``` bash
port 6380
```

**Slave** redis_custom_6381.conf
``` bash
port 6381
slaveof 127.0.0.1 6380
```

**Slave** redis_custom_6382.conf
``` bash
port 6382
slaveof 127.0.0.1 6380
```

#### 测试redis的Master Slace

其中6380作为Master,6381、6382作为slave
启动三个
``` bash
redis-cli -p 6380
redis-cli -p 6381
redis-cli -p 6382
```

其中只有6380可写,另个两个可读

### 配置sentinel

sentinel_26380.conf
``` bash
port 26380
dir ./
sentinel monitor mymaster 127.0.0.1 6380 2
sentinel down-after-milliseconds mymaster 10000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 10000
```
sentinel_26381.conf
``` bash
port 26381
dir ./
sentinel monitor mymaster 127.0.0.1 6380 2
sentinel down-after-milliseconds mymaster 10000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 10000
```
sentinel_26382.conf
``` bash
port 26382
dir ./
sentinel monitor mymaster 127.0.0.1 6380 2
sentinel down-after-milliseconds mymaster 10000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 10000
```

#### 启动三个sentinel
redis-sentinel sentinel_26380.conf
redis-sentinel sentinel_26381.conf
redis-sentinel sentinel_26382.conf


此时,存在1个Master、2个slave,三个sentinel
开启三个redis-cli,分别连上6380-6382
只有Master(6380)能执行写入操作

kill 6380的redis
过了超时时间(配置文件中的10s),通过sentinel的日志,某一个slave将会切换成master

**sentinel切换master日志**
``` bash
33920:X 11 Aug 23:18:03.104 # +sdown master mymaster 127.0.0.1 6380
33920:X 11 Aug 23:18:03.208 # +new-epoch 1
33920:X 11 Aug 23:18:03.208 # +vote-for-leader 8e2ce7686142bf9853270a96b2c6d84ea634a36b 1
33920:X 11 Aug 23:18:04.243 # +odown master mymaster 127.0.0.1 6380 #quorum 3/2
33920:X 11 Aug 23:18:04.243 # Next failover delay: I will not start a failover before Thu Aug 11 23:18:23 2016
33920:X 11 Aug 23:18:04.298 # +config-update-from sentinel 8e2ce7686142bf9853270a96b2c6d84ea634a36b 127.0.0.1 26381 @ mymaster 127.0.0.1 6380
33920:X 11 Aug 23:18:04.298 # +switch-master mymaster 127.0.0.1 6380 127.0.0.1 6382
33920:X 11 Aug 23:18:04.299 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6382
33920:X 11 Aug 23:18:04.299 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6382
33920:X 11 Aug 23:18:14.319 # +sdown slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6382
```

#### [redis 配置详解](http://yijiebuyi.com/blog/bc2b3d3e010bf87ba55267f95ab3aa71.html)
#### [Redis官方文档sentinel](http://ifeve.com/redis-sentinel/)
