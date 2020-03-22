### 1. Java 篇

**(1). Java基础知识**

- java中==和equals和hashCode的区别
- int与integer的区别
- 抽象类的意义
- 接口和抽象类的区别
- 能否创建一个包含可变对象的不可变对象?
- 谈谈对java多态的理解
- **String、StringBuffer、StringBuilder区别**
- 泛型中extends和super的区别
- 进程和线程的区别
- final，finally，finalize的区别
- 序列化的方式
- **string 转换成 integer的方式及原理**
- 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？
- 成员内部类、静态内部类、局部内部类和匿名内部类的理解，以及项目中的应用
- 讲一下常见编码方式？
- 如何格式化日期?
- Java的异常体系
- 什么是异常链
- throw和throws的区别
- **反射的原理，反射创建类实例的三种方式是什么。**
- java当中的四种引用
- 深拷贝和浅拷贝的区别是什么?
- **什么是编译器常量？使用它有什么风险？**
- **你对String对象的intern()熟悉么?**
- **a=a+b与a+=b有什么区别吗?**
- 静态代理和动态代理的区别，什么场景使用？
- Java中实现多态的机制是什么？
- 如何将一个Java对象序列化到文件里？
- 说说你对Java反射的理解
- 说说你对Java注解的理解
- 说说你对依赖注入的理解
- 说一下泛型原理，并举例说明
- Java中String的了解
- String为什么要设计成不可变的？
- Object类的equal和hashCode方法重写，为什么？

**(2).多线程**

- 开启线程的三种方式？
- 说说进程，线程，协程之间的区别
- 线程之间是如何通信的？
- 什么是Daemon线程？它有什么意义？
- 在java中守护线程和本地线程区别？
- 为什么要有线程，而不是仅仅用进程？
- 什么是可重入锁（ReentrantLock）？
- 什么是线程组，为什么在Java中不推荐使用？
- 乐观锁和悲观锁的理解及如何实现，有哪些实现方式？
- Java中用到的线程调度算法是什么？
- 同步方法和同步块，哪个是更好的选择？
- run()和start()方法区别
- 如何控制某个方法允许并发访问线程的个数？
- 在Java中wait和seelp方法的不同；
- Thread类中的yield方法有什么作用？
- 什么是不可变对象，它对写并发应用有什么帮助？
- 谈谈wait/notify关键字的理解
- 为什么wait, notify 和 notifyAll这些方法不在thread类里面？
- 什么导致线程阻塞？
- 讲一下java中的同步的方法
- 谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
- static synchronized 方法的多线程访问和作用
- 同一个类里面两个synchronized方法，两个线程同时访问的问题
- 你如何确保main()方法所在的线程是Java程序最后结束的线程？
- 谈谈volatile关键字的作用
- 谈谈ThreadLocal关键字的作用
- 谈谈NIO的理解
- 什么是Callable和Future?
- ThreadLocal、synchronized 和volatile 关键字的区别
- synchronized与Lock的区别
- ReentrantLock 、synchronized和volatile比较
- 在Java中CycliBarriar和CountdownLatch有什么区别？
- CopyOnWriteArrayList可以用于什么应用场景？
- ReentrantLock的内部实现
- lock原理
- Java中Semaphore是什么？
- Java中invokeAndWait 和 invokeLater有什么区别？
- 多线程中的忙循环是什么?
- 怎么检测一个线程是否拥有锁？
- 死锁的四个必要条件？
- 对象锁和类锁是否会互相影响？
- 什么是线程池，如何使用?
- Java线程池中submit() 和 execute()方法有什么区别？
- Java中interrupted 和 isInterruptedd方法的区别？
- 用Java实现阻塞队列
- BlockingQueue介绍：
- 多线程有什么要注意的问题？
- 如何保证多线程读写文件的安全？
- 多线程断点续传原理
- 断点续传的实现
- 实现生产者消费者模式
- Java中的ReadWriteLock是什么？
- 用Java写一个会导致死锁的程序，你将怎么解决？
- SimpleDateFormat是线程安全的吗?
- Java中的同步集合与并发集合有什么区别？
- Java中ConcurrentHashMap的并发度是什么？
- 什么是Java Timer类？如何创建一个有特定时间间隔的任务？

**(3).集合**

- Collection 和Collections 的区别？
- 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？
- List,Set,Map的区别
- List和Map的实现方式以及存储方式
- HashMap的实现原理
- HashMap如何put数据（从HashMap源码角度讲解）？
- HashMap的扩容操作是怎么实现的？
- HashMap在JDK1.7和JDK1.8中有哪些不同？
- ConcurrentHashMap的实现原理
- HashTable实现原理
- ArrayMap和HashMap的对比
- HashMap和HashTable的区别
- HashMap与HashSet的区别
- 集合Set实现Hash怎么防止碰撞
- 数组和链表的区别
- Array和ArrayList有何区别？什么时候更适合用Array
- .EnumSet是什么？
- Comparable和Comparator接口有何区别？
- **Java集合的快速失败机制 “fail-fast”？**
- **fail-fast 与 fail-safe 之间的区别？**
- BlockingQueue是什么？
- Iterator类有什么作用
- poll()方法和remove()方法区别？
- JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，你如何设计。

**(4).JVM**

- 谈谈你对解析与分派的认识。
- 你知道哪些或者你们线上使⽤什么GC策略？它有什么优势，适⽤于什么场景？
- Java类加载器包括⼏种？它们之间的⽗⼦关系是怎么样的？双亲委派机制是什么意思？有什么好处？
- 如何⾃定义⼀个类加载器？你使⽤过哪些或者你在什么场景下需要⼀个⾃定义的类加载器吗？
- 堆内存设置的参数是什么？
- Perm Space中保存什么数据？会引起OutOfMemory吗？
- 做GC时，⼀个对象在内存各个Space中被移动的顺序是什么？
- 你有没有遇到过OutOfMemory问题？你是怎么来处理这个问题的？处理 过程中有哪些收获？
- StackOverflow异常有没有遇到过？⼀般你猜测会在什么情况下被触发？如何指定⼀个线程的堆栈⼤⼩？⼀般你们写多少？
- 内存模型以及分区，需要详细到每个区放什么。
- 分派：静态分派与动态分派。
- 虚拟机在运行时有哪些优化策略
- 请解释StackOverflowError和OutOfMemeryError的区别？
- .在JVM中，如何判断一个对象是否死亡？

### 计算机网络

- 从网络加载一个10M的图片，说下注意事项
- OSI网络体系结构与TCP/IP协议模型
- TCP的3次握手和四次挥手
- 为什么TCP链接需要三次握手，两次不可以么，为什么？
- TCP协议如何来保证传输的可靠性
- TCP与UDP的区别
- TCP与UDP的有哪些应用
- HTTP1.0与2.0的区别
- HTTP报文结构
- HTTP的长连接和短连接?
- HTTP与HTTPS的区别以及如何实现安全性
- 如何验证证书的合法性
- Get与POST的区别
- TCP的拥塞处理
- TCP是如何进行流量控制
- TCP和UDP分别对应的常见应用层协议
- IP地址的分类
- 有了唯一的Mac地址为啥还需要IP地址？
- 交换机、集线器与路由器有什么区别？
- 网桥的作用
- ARP是地址解析协议，简单语言解释一下工作原理。
- 网络接口卡（网卡）的功能？
- IO中同步与异步，阻塞与非阻塞区别
- URI和URL的区别
- GET请求中URL编码的意义
- 常见状态码及原因短语
- 说说Session、Cookie 与 Application
- 如何避免浏览器缓存
- 什么是分块传送。
- 谈谈SQL 注入
- DDos 攻击
- DDos攻击有那些预防方法？
- 什么是XSS 攻击
- 从输入网址到获得页面的过程

### 数据结构与算法

这部分要会手动实现一些数据结构，我总结了以下一些重要的数据结构

**数据结构**

- 链表(增删查操作)

- - 单向链表
  - 双向链表

- 队列(增删查操作)

- - 普通队列
  - 优先队列

- 树

- - 二叉树(前序、中序、后序)
  - 平衡树(尽量会实现代码)
  - 堆
  - 红黑树(了解性质、应用场景)
  - B树(了解性质、应用场景)

- 图

- - Prim算法
  - Kruskal算法
  - 深度优先搜索
  - 广度优先搜索
  - 最短路径
  - 最小生成树
  - 拓扑

- 字符串

- - Knuth-Morris-Pratt算法
  - Boyer-Moore算法

- 散列

**几种算法思想**

- 递归
- 递推
- 贪心
- 枚举
- 动态规划
- 回溯法
- 分治

**必学十大排序算法**

- 选择排序
- 插入排序
- 冒泡排序
- 希尔排序
- 归并排序
- 快速排序
- 堆排序
- 计数排序
- 桶排序
- 基数排序

**刷题**

牛客网剑指offer六七十到题

leetcode

### 数据库

- 请简洁描述Mysql中InnoDB支持的四种事务隔离级别名称，以及逐级之间的区别？
- 在Mysql中ENUM的用法是什么？
- CHAR和VARCHAR的区别？
- 事务是如何通过日志来实现的，说得越深入越好
- drop,delete与truncate的区别
- 局部性原理与磁盘预读
- 数据库范式
- 存储过程与触发器的区别
- 锁的优化策略
- 什么情况下设置了索引但无法使用
- 什么情况下不宜建立索引？
- 解释MySQL外连接、内连接与自连接的区别
- 完整性约束包括哪些？
- Mysql 的存储引擎,myisam和innodb的区别。
- 如何进行SQL优化
- 乐观锁和悲观锁是什么，INNODB的标准行级锁有哪2种，解释其含义。
- MVCC的含义，如何实现的
- MYSQL的主从延迟怎么解决。

### spring

**1. spring概述**

- 使用Spring框架的好处是什么？
- Spring由哪些模块组成?
- 解释AOP模块
- 解释WEB 模块
- 核心容器（应用上下文) 模块。
- 什么是Spring IOC 容器？
- IOC的优点是什么？
- ApplicationContext通常的实现是什么?
- Bean 工厂和 Application contexts  有什么区别？
- Bean 工厂和 Application contexts  有什么区别？

**2. spring依赖注入**

- 什么是Spring的依赖注入？
- 有哪些不同类型的IOC（依赖注入）方式？
- 什么是Spring beans?
- 一个 Spring Bean 定义 包含什么？
- 解释Spring支持的几种bean的作用域。
- Spring框架中的单例bean是线程安全的吗?
- 解释Spring框架中bean的生命周期
- 哪些是重要的bean生命周期方法？ 你能重载它们吗？
- 什么是bean装配?  
- 什么是bean的自动装配？
- 解释不同方式的自动装配 。
- 自动装配有哪些局限性 ?

**3. spring 注解**

- 怎样开启注解装配？
- 谈谈@Required、 @Autowired、 @Qualifier注解。

**4， spring 数据访问**

- 在Spring框架中如何更有效地使用JDBC?
- 使用Spring通过什么方式访问Hibernate?
- Spring框架的事务管理有哪些优点？

**5. Spring面向切面编程（AOP）**

- 解释AOP
- Aspect 切面
- 在Spring AOP 中，关注点和横切关注的区别是什么？
- 通知
- 有几种不同类型的自动代理？
- 什么是织入。什么是织入应用的不同点？

**6. springMVC**

- 什么是Spring的MVC框架？
- DispatcherServlet
- WebApplicationContext
- 什么是Spring MVC框架的控制器？
- @Controller 注解
- @RequestMapping 注解

### JavaWeb

**servlet与Tomcat**

- Servlet生命周期
- forward和redirect的区别
- tomcat容器是如何创建servlet类实例？用到了什么原理？
- 什么是cookie？Session和cookie有什么区别？
- Servlet安全性问题
- Tomcat 有哪几种Connector 运行模式(优化)？

**JSP**

- jsp静态包含和动态包含的区别
- jsp有哪些内置对象?作用分别是什么?
- jsp和servlet的区别、共同点、各自应用的范围？
- 写出5种JSTL常用标签
- JSP是如何被执行的？执行效率比SERVLET低吗？
- 说出Servlet和CGI的区别？
- 简述JSP的设计模式。