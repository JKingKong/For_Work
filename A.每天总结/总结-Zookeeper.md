## 特点

1. Zookeeper:一个领导者(Leader) ，多个跟随者(Follower) 组成的集群。
2. 集群中只要有半数以上节点存活，Zookeeper集群就能正常服务。
3. 全局数据一致:每个Server保存一份相同的数据副本，Client无论连接到哪Server，数据都是一致的。
4. 更新请求顺序进行，来自同一个Client的更新请求按其发送顺序依次执行。
5. 数据更新原子性，一次数据更新要么成功，要么失败。
6. 实时性，在一定时间范围内，Client能读到最新数据。

## 作用

1. 统一命名服务
2. 统一配置管理
3. 统一集群管理
4. 服务器节点动态上下线
5. 软负载均衡

## 内部原理

### 选举机制（面试重点）

1. 半数机制
2. Leader 、Follow
3. 选举过程
   1. ID与未选举状态
   2. ID与选举状态

### 节点类型

1. 持久型、短暂型
2. 顺序编号

### Stat结构体？？？

### 监听器原理（面试重点）



1. 首先要有一个main()线程.
2. 在main线程中创建Zookeeper客户端, 这时就会创建两个线程
   1. 通信（connect）：网络连接通信
   2. 监听（listener ）：监听
3. 通过connect线程将注册的监听事件发送给Zookeeper。
4. Zookeeper的注册监听器列表中将注册的监听事件添加到列表中。
5. Zookeeper监听到有数据或路径变化,就会将这个消息发送给listener线程
6. listener线程内部调用了process()方法

### 写数据流程





## Zookeeper实战