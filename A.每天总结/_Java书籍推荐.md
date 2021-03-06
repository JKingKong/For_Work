作者：黄小斜
https://www.nowcoder.com/discuss/162234?type=post&order=time&pos=&page=1来源：牛客网

  先声明一点，文章里面不会详细到每一步怎么操作，只会提供大致的思路和方向，给大家以启发，如果真的要一步一步指导操作的话，那至少需要一本书的厚度啦。 

  因为笔者还只是一名在校生，所以写的内容主要还是针对Java初学者或者接触Java后端不久的朋友，不适用于已经工作多年的Java大佬们。所以本文中的方法不一定适合所有人，如有错误还请谅解。 

  本期的内容是系列文章的最后一部分内容了。这个系列可能还有很多东西没有说清楚，也有很多内容被忽略了。但是这些内容也确实是笔者结合自己经验总结而成的，希望能对大家有用 ~ 当然如果有什么建议也可以随时和笔者交流。 

  上期回顾 

  上期我们重点介绍了Java工程师进阶所需要掌握的一些技术内容。特别对于即将参加校招的同学来说，最重要的也是这部分内容，你需要了解JVM虚拟机原理，Java并发原理，并且熟悉JDK的部分源码，了解这些API的底层实现。 

  之所以把这部分放在Java Web项目之后来讲，是因为我觉得，一开始做项目的时候你不可能已经掌握好上述内容了，所以你完全可以带着问题去做项目，再花时间去学习底层原理，这样你可以很好地结合你之前实践过的代码去理解那些底层技术了。 

  本期主题 

  本期主要介绍的是Java后端技术比较“高端”的一些内容，也就是我们经常聊的分布式，架构，缓存，消息队列等内容，另外我们也会介绍一些大后端相关的技术，比如云计算（OpenStack和docker），大数据（hadoop生态），以及一些常用的后端技术。 

  这些内容其实离我们并不远，只不过在平时的项目中可能用的比较少，所以作为学生党一般也只能通过一些文章或者书本去学习理论知识。那么基于这么一个情况，我们来谈谈怎么学习这部分的内容吧。 

#   01 

#   **Web后端架构**  

  Web后端架构 

  后端进阶第一步，先把Web架构相关的技术学好吧，因为之前大家都做过Java Web项目，想必对这块内容还是比较熟悉的吧。我们需要了解Web架构演化的历史，了解为什么要做服务器集群，为什么要用缓存，为什么要做拆分，做主从，以及为什么要有分布式。 

####   **推荐资源：《深入分析Java Web技术内幕》，《大型网站技术架构》**  

  两本都是阿里大佬出的书，两位都是淘宝系的技术大牛。前一本书主要讲述的Java Web的一些技术基础，关于Web架构的内容比较少。 

  后一本则是李智慧大佬写的架构科普书籍，用非常简单易懂的语言写出了大型Web项目架构之美，分别着眼于高可用，高性能，高扩展等方面讲解了很多设计结构的原则和方法。这本书应该是Web架构小白最好的入门书籍了。 

#   02 

#   **分布式理论基础**  

  由于下面的内容或多或少都会涉及到分布式相关的知识，所以这一部分我们主要介绍一下有关分布式的基础知识。笔者对分布式的学习主要也停留在理论上，所以这里讲的也是一些理论的东西。 

####   **推荐资源：《从Paxos到zookeeper分布式一致性原理与实践》，我的技术博客专栏“分布式系统理论与实践”**  

  这本书比较好地科普了分布式基础知识，也介绍了zookeeper的原理和使用。了解zookeeper是了解分布式技术很重要的一个环节。 

  1 CAP 和 BASE 

  谈分布式就要谈CAP，一致性，高可用，网络分区容忍性为何只能三选二，为什么网络分区容忍性必须要被考虑。CAP在实际应用中真的可靠么？ 

  BASE出现的原因，为什么BASE更容易实现，更适合实际应用，BASE可以通过哪些技术去实现呢？ 

  2 一致性协议和算法 

  一致性协议也是分布式理论的一个重点，2PC，3PC，分别指的是什么，其中分别有什么问题。3PC解决了2PC的一个问题，却仍然不完美。 

  Paxos和Raft两种一致性算法，显然前者比后者复杂得多，但是Raft可能更加实用。为什么我们需要一致性算法，它们又有什么用呢。 

  3 分布式事务和最终一致性 

  分布式事务是一个复杂的概念，主要指分布式系统中需要强一致场景时所用到的事务。理解和实现它都不是简单的事情。 

  如果我们退而求其次，不要求强一致性，而选择最终一致性，则可以用更加灵活的方案，比如事务消息。 

#   03 

#   **常见分布式技术**  

####   **推荐资源：《从Paxos到zookeeper分布式一致性原理与实践》，我的技术博客专栏“分布式系统理论与实践”，《深入理解Spring Cloud与微服务构建》，《分布式服务框架原理与实践》。**  

  1 zookeeper 

  上文说到zookeeper是分布式技术很重要的一块内容，这是因为zookeeper用于管理和协调分布式组件，虽然它出自hadoop生态，却用于很多应用当中，基本上有分布式的地方就有zk的存在。 

  简单说来，zk可以提供全局统一的节点树结构，通过节点来管理资源，同时zk自身是使用集群方式部署的，所以保证自己是高可用的。根据这一特点，它可以作为服务注册中心，还可以实现分布式锁等功能。 

  2 分布式服务 

  分布式服务是一个挺有意思的东西，也很常用，简单来说，就是把服务组件部署在不同节点上，通过rpc的方式访问，为了实现这一功能，我们需要考虑通信协议，序列化方式，进一步来说，我们还要了解如何做服务注册和发现，以及如何做限流，做服务熔断和降级，等等等等。 

  常见的分布式服务框架有dubbo，以及Spring Cloud这类产品，学会使用他们，然后了解它们的底层实现原理，相信会是一个很有趣的过程。 

  3 负载均衡 

  关于负载均衡，说起来其实很简单，就是把一组请求分成多组，按照某种规则分发到多台服务器上。 

  但是负载均衡也涉及很多内容，包括负载均衡的算法，负载均衡的实现方式，我们需要了解它到底是在哪一层实现的。 

  一般来说，常用的负载均衡方式有nginx和lvs两种，分别是7层和4层的负载均衡，一个基于域名进行负载均衡，一个基于端口号做负载均衡。了解它们的实现原理，会让你更好地理解这部分内容。 

  4 分布式session和分布式锁 

  这两个组件也是分布式项目中经常要用到的，了解它们的使用和实现原理，有助于以后在项目中的实践。 

  分布式session一般有多种实现方式，可以存数据库或者缓存，也可以单独部署成一个服务，总之最重要的一点就是，性能要好，并且要高可用。 

  分布式锁则用于一些需要一致性的场景中，比如订单生成这种全局唯一的功能，分布式锁通常可以用缓存或者数据库来实现，但为了保证高性能，并且避免死锁，我们一般采用Redis或者zookeeper来实现。 

#   04 

#   **缓存**  

  讲到缓存，我们说的最多的就是Redis，所以我们要讲的也是Redis。学习Redis，除了学会使用简单的api之外，最好还要了解它的实现原理。 

####   **推荐资源：我的技术博客专栏“重新学习MySQL和Redis”，《Redis设计与实现》**  

  这里我们主要介绍三部分内容，也是我个人认为比较重要的三块内容。 

  1 数据结构和底层实现 

  Redis的数据结构比较丰富，但更有意思的是这些数据结构背后的底层实现，也就是作者如何用c语言来实现这些结构的。其中会有你熟悉的数组，链表，还有一些有意思的结构比如跳表，哈希表。 

  2 持久化方式 

  持久化方式主要分两种，aof和rdb，前者基于追加日志的方式来实现日志持久化，后者则是使用备份数据的方式来实现持久化。 

  3 分布式方案 

  这是Redis最有趣也最复杂的部分。 

  首先，Redis可以使用主从的方式部署，其中“哨兵”这一组件用于故障切换。 

  基于哨兵的主从部署后来发展为Redis cluster的部署方式，也就是Redis集群，通过分片的方式来部署Redis集群，并且集群中任一节点都可以用来对外提供服务。 

  当然，除了Redis集群之外，还有codis的分布式方案，codis基于***的方式来实现，表面上还是使用原来的Redis API，但实际上访问的却是一个Redis集群。 

#   05 

#   **消息队列**  

  消息队列的作用一般来说就是削峰，控流，解耦合，目前业界也有很多的消息队列产品，在很多公司都会使用，当然，它们各有各的优缺点，我们也不必全都了解，这里我们大概介绍3种消息队列，它们各自的特点都比较鲜明，值得大家去了解一番。 

  1 RabbitMQ 

  笔者刚开始接触的消息队列是rabbitmq，它的使用方法比较简单。 RabbitMQ是一个由erlang开发的AMQP（Advanced Message Queue ）的开源实现，主要有以下特点： 

  安装部署简单，上手门槛低，功能丰富，符合AMQP标准； 

  企业级消息队列，经过大量实践考验的高可靠； 

  集群易扩展，可以轻松的增减集群节点； 

  有强大的WEB管理页面。 

  2 Kafka 

  与其他MQ相比较，Kafka有一些优缺点，主要如下 

  优点： 

  可扩展。Kafka集群可以透明的扩展，增加新的服务器进集群。 

  高性能。Kafka性能远超过传统的ActiveMQ、RabbitMQ等，Kafka支持Batch操作。 

  容错性。Kafka每个Partition数据会复制到几台服务器，当某个Broker失效时，Zookeeper将通知生产者和消费者从而使用其他的Broker。 

  缺点： 

  重复消息。Kafka保证每条消息至少送达一次，虽然几率很小，但一条消息可能被送达多次。 

  消息乱序。Kafka某一个固定的Partition内部的消息是保证有序的，如果一个Topic有多个Partition，partition之间的消息送达不保证有序。 

  复杂性。Kafka需要Zookeeper的支持，Topic一般需要人工创建，部署和维护比一般MQ成本更高。 

  RocketMQ 

  RocketMQ是一个纯java、分布式、队列模型的开源消息中间件，前身是Metaq，当 Metaq 3.0发布时，产品名称改为 RocketMQ。 

  具有以下特点： 

  1、能够保证严格的消息顺序 

  2、提供丰富的消息拉取模式 

  3、高效的订阅者水平扩展能力 

  4、实时的消息订阅机制 

  5、亿级消息堆积能力 

  除此之外，它还有一个优点，就是支持事务消息，让分布式事务的实现变得简单 

#   05 

#   **分布式数据库**  

  这里说的分布式”数据库“，其实指的是数据库的分布式方案，更具体来说，主要指的是数据库的主从部署，以及分库，分表。 

  1 主从复制和读写分离 

  这是数据库高可用的基础。MySQL数据库会使用日志来完成主从复制，先写主库，然后再同步到从库。读写分离则一般是指的是：从库负责读，主库负责写。 

  2 分库分表方案 

  分库分表是解决大表性能瓶颈的一种方法，当然也分为横向拆分和纵向拆分，横向拆分指的就是减少单表的数据量，放到其他表或者其他库中。纵向拆分则一般指按照业务来拆分，把不必要的字段放到其他表中。 

  分库分表可以在应用层做，通过对id或者其他字段进行hash以便映射到对应的表中。当然也可以通过数据库中间件来完成，例如mycat这种中间件，通过***的方式实现分库分表，非常方便。 

#   06 

#   **大后端相关技术**  

  这部分的内容笔者也只是略知一二，所以这里只是抛砖引玉，做一个简单的科普罢了。毕竟咱们学技术的人都是先讲深度再来谈广度。当你对之前的内容掌握得比较好的时候，再去看看大后端的一些其他技术，也会感觉挺有意思的。 

  下面这些技术主要是我自己学习路上接触过的一些内容，所以比较熟悉，才拿出来分享，至于适不适合大家的口味，可能就见仁见智了。 

  Hadoop生态 

  笔者之前参与过数据仓库相关的项目，所以稍微了解了这方面的内容，感觉hadoop生态还是挺有意思的。 

  大家不妨去了解一下其中的基本组件，然后打一个集群自己玩玩看。 

  常见的组件有：hdfs，hbase，hive，zookeeper，flume，sqoop，yarn。 

####   **推荐资源：我的技术博客-个人分类-hadoop，《大数据技术原理与应用》**  

  对于入门hadoop生态来说，这本书完全足够了，如果你要做大数据平台开发或者是数据研发工程师，可能需要非常全面地了解这些组件的底层原理。 

  云计算初探 

  笔者之前参与过私有云相关的项目，所以稍微了解了这方面的内容，感觉这方面的内容也蛮有趣的。 

  我在项目中主要接触到的是OpenStack，docker以及kubenetes，OpenStack是一个私有云生态，内部结构对于我们来说还是比较复杂的，不过最根本的虚拟化技术还是基于kvm虚拟化来实现的。 

  docker则是现在非常流行的一种容器，用于快速部署应用。 

  kubenetes也借着docker的东风火了起来，可以理解为是基于容器的分布式调度系统。 

  这些技术在企业中也是比较常用的，只不过对于研发同学来说，更多时候扮演的是工具的角色。 

####   **推荐资源：《Docker技术入门与实战》，《kubenetes权威指南》**  

  其他常见后端技术 

  除此之外，想必大家还了解过很多其他的技术，只不过不同的业务用到的组件往往不一样，所以并不是每个东西你都需要去了解。 

  比如搜索引擎技术Lucene，基于它的两款产品solr和elasticsearch，通常出现在需要搜索功能的项目中。 

  再比如流式计算技术，如storm和spark streaming等等，通常都用于大数据部门，用作实时数据采集。 

  又如ELK实现的分布式日志系统，多用于分析和定位系统问题，经常会出现在一些比较重要的应用当中。 

  当然，也有现在大火的人工智能，还有太多的技术我们没机会去了解和使用，我们能做的也就是在自己能力范围内把需要做的东西做到最好了。 

  所以，这些内容并不是每一样你都需要知道，但是如果有时间去了解一下的话，还是建议多了解一点的。 

#   07 

#   **总结**  

  总结 

  今天码的字有点多，所以难免有些写的不太好的地方，希望大家见谅。纵观全文，我们主要讲了这些内容： 

  1 Web架构 

  2 分布式基础理论 

  3 常见分布式技术 

  4 缓存 

  5 消息队列 

  6 数据的分布式方案 

  7 大后端相关技术 

  至此本系列文章就已经结束了，不知道大家有什么问题或者建议想和笔者交流吗~赶紧加我的微信来聊聊吧。 

  写本系列文章也是因为有很多朋友想要了解更加清晰的Java后端学习路线，所以我总结了之前自己的学习历程，才创作出这四篇文章，希望能够对大家有所帮助~ 

  — END —