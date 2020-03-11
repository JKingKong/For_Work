1. @EnableDubbo
2. @Service   （Dubbo自带的注解   声名类为服务）
3. 消费者  与 提供者实现相同的接口
   1. 提供者需要实现接口
   2. 消费者不需要实现接口，需要使用@Reference引用
4. 配置原理：一般配置在提供者
5. Dubbo高可用：
   1. 本地缓存通讯
   2. 注册中心对等集群
   3. 内置了几种负载均衡算法
   4. 服务提供者无状态
   5. Hystrix来实现服务降级
6. Dubbo直连
7. 负载均衡
   1. RandomLoadBalance		随机 权重
   2. LeastActiveLoadBalance   最小活跃数负载均衡
   3. ConsistentHashLoadBalance
   4. RoundRobinLoadBalance  轮询 权重
8. 均衡机制选择
   1. 消费者： `@Reference(loadbalance = RoundRobinLoadBalance.NAME)`
   2. 提供者：`@Service(interfaceClass = HelloService.class, loadbalance = RoundRobinLoadBalance.NAME)`
9. 权重分配
10. 容错
    1. 集群容错
    2. 服务降级
       1. 服务提供者模拟出错
       2. 两种模式
          1. 不调用服务，直接返回null  `mock=force:return+null` 
          2. 容错按钮   `mock=fail:return+null`
    3. 整合Hystrix
    4. 导入依赖
    5. 开启*@EnableHystrix*
    6. server-provider的入口类上使用`@EnableHystrix`注解开启Hystrix功能
    7. `    @HystrixCommand(fallbackMethod = "defaultHello")`

## 基础知识



## Dubbo配置

1. 配置优先级：命令行启动配置  > XML > properties
2. 重试次数
3. 超时时间
   1. 消费端
   2. 服务端
4. 配置原则：reference method > service method > reference > service > consumer > provider
5. 版本号
   

## 高可用

1. 整合Hystrix
2. 服务熔断
3. 服务降级

## Dubbo原理

### RPC原理

1. 一次完整的RPC调用流程：

   1. **服务消费方（client）调用以本地调用方式调用服务**；
   2. client stub 接收到调用后负责将方法、参数等组装成能够进行网络传输的消息体； 
   3. client stub 找到服务地址，并将消息发送到服务端； 
   4. server stub 收到消息后进行解码； 
   5. server stub 根据解码结果调用本地的服务； 
   6. 本地服务执行并将结果返回给 server stub； 
   7. server stub 将返回结果打包成消息并发送至消费方；
   8. client stub 接收到消息，并进行解码； 
   9. **服务消费方得到最终结果**

   **RPC 框架**的目标就是要 **2~8 这些步骤都封装起来**，这些细节对用户来说是透明的

   1. 消费：调用远程、cs打包、cs发送
   2. 提供：接收解码、调用本地、结果给ss、ss打包、发送
   3. 消费：接收解码、得到结果

   

### netty通信原理

Selector 一般称 为**选择器** ，也可以翻译为 **多路复用器，** 

Connect（连接就绪）、Accept（接受就绪）、Read（读就绪）、Write（写就绪）

Netty基本原理：

1. 初始化channel
2. 注册channel到Selector
3. 轮询accept事件
4. 处理accept建立的channel
5. 注册channel到selector
6. 轮询读写事件
7. 处理读写事件

### Dubbo原理（重要）

#### 1、dubbo 原理 -框架设计

#### 2、dubbo 原理 -启动解析、加载配置信息

#### 3、dubbo 原理 -服务暴露

#### 4、dubbo 原理 -服务引用

#### 5、dubbo 原理 -服务调用