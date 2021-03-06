一个是自己的学习情况

二是项目经历

三是为什么应聘这个岗位

# 算法

1. **LRU缓存设计**
2. **反转链表：迭代和递归**
3. TopN问题
   1. bitmap
   2. 堆
4. 快速排序
   1. 实现
   2. 最好和最坏（划分：每次左0 右 n）
   3. 最坏情况的优化
      1. 随机打乱数组
      2. 主元是随机选择的，（选择后主元与a[left]交换）
5. **复习笔试题（第三题   城市扩张那个）**
6. 跳台阶问题
7. **二叉树是如何序列化存入文件，如何恢复**
   1. 使用数组存储：先序遍历输出（null节点也要输出）
   2. 层序遍历输入
8. **排序数组，查找目标数字出现最后一次的位置**
9. **最小栈问题**
10. 大根堆手撕
11. **一堆数怎么排序拼起来能得到一个最大的数**
12. 不用中序遍历查二叉搜索树第K大数
13. 并查集与朋友圈

# 数据结构

1. 讲讲哈夫曼编码
2. 链表与数组区别
3. 数组和链表的对比
4. Hash冲突的解决
5. Hash算法设计
6. 跳表查第K大数流程
7. 并查集

# 计算机网络

### 基础知识

1. TCP
   1. TCP三次握手
      1. TCP三次握手过程及其状态变化
      2. TCP如何保证可靠传输
      3. TCP如何保证顺序交付
   2. TCP四次挥手
      1. TCP四次挥手的过程及其状态变化
      2. TCP四次挥手中的TIME_WAIT状态
   3. 针对TCP三次握手的攻击（SYN攻击、DDOS攻击原理）
   4. TCP传输过程
      1. tcp每一个字节都返回ack吗
      2. 累计确认、快速重传
      3. 流量控制（发送窗口、拥塞窗口、接受窗口）
      4. 拥塞控制算法
2. TCP和UDP的优缺点比较
3. websocket，为什么浪费资源
4. session和cookie区别
5. 在浏览器地址栏输入一个URL会发生什么
   1. DNS本地缓存和本地Host
   2. 本地DNS服务器缓存
   3. 根DNS服务器
   4. 顶级域名服务器
6. 长连接和短连接
7. 七层结构和五层结构
8. TCP和UDP在程序设计的时候应该注意的点
9. 客户端断了，服务端知道吗
10. 如何优雅关闭连接
11. 什么是MTU
12. 客户端服务端tcp建立连接接口函数
13. **ip数据包和流有什么区别**
14. http状态码

### 场景题

1. 下载一个超大文件为什么网速会越来越快,解释背后的技术（TCP滑动窗口、拥塞控制算法）
2. 

# 数据库

### Mysql

1. 事务
2. 事务隔离级别
3. 锁
   1. 乐观锁和悲观锁
4. 索引
   1. 最左前缀匹配原则是什么
   2. Mysql复合索引
   3. 联合索引
   4. 多列索引
   5. **覆盖索引**
   6. 聚集索引和非聚集索引
   7. B树、B+树对比，为什么用B+树作为索引
   8. Hash索引和B+树索引对比
   9. 一个B+树的节点中到底存多少个元素最合适你有了解过么？
   10. InnoDB引擎主键为什么设成自增（B+树，插入最后否则插入中间页分裂等问题）
   11. 索引失效条件
5. 引擎
   1. myisam和innodb的区别
   2. innodb引擎深度解析
6. 聚集索引和非聚集索引
7. 关系型数据库三范式
8. mvcc如何实现
9. 写一个多表查询题目
10. 查询优化：explain，慢查询，show profile
11. 分布式：分库分表，读写分离
12. 数据库的锁：行锁，表锁，页级锁，意向锁，读锁，写锁，悲观锁，乐观锁等等

回表查询

![1584631326617](images/1584631326617.png)

### Redis



# 操作系统

### 基础知识

1. 进程和线程之间的区别
2. 操作系统的调度算法
3. 进程间通信的方式和区别
4. 虚拟内存机制的作用
5. 缓存的作用以及缓存替换算法
6. 线程的实现方式
7. 虚拟文件系统
8. fork一个子进程，和父进程共享什么**
9. 银行家算法
10. Linux的指令
11. **缺页中断**
12. **cache和内存一致性**
13. 进程调度算法
14. 同步io和异步io区别
15. **epoll底层实现**
16. 哈希表根据键值进行查询时候CPU底层会如何工作，产生什么指令

### Linux命令

1. 常用命令：awk、ps、top、netstat
2. 进程相关命令
   1. ps
   2. top
3. 网络相关命令
   1. netstat
4. linux的目录
5. linux查看本机ip

# 项目问题

### Redis

1. 单点故障怎么办（高可用）
2. 超卖

RabbitMQ

1. 消息丢失
   1. 发送端到RabbitMQ
   2. RabbitMQ到接收端
2. 消息持久化