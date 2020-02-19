# 一、缓存

## 1、搭建基本环境

#### 1、导入数据库文件 创建出department和employee表

#### 2、创建javaBean封装数据

#### 3、整合MyBatis操作数据库

1.配置数据源信息

2.使用注解版的MyBatis
		1）、@MapperScan指定需要扫描的mapper接口所在的包

## 2、快速体验缓存

### 步骤：

#### 1、开启基于注解的缓存 @EnableCaching

#### 2、标注缓存注解即可

- @Cacheable
- @CacheEvict
- @CachePut
- 默认使用的是ConcurrentMapCacheManager == ConcurrentMapCache；将数据保存在	ConcurrentMap<Object, Object>中
  开发中使用缓存中间件；redis、memcached、ehcache；



## 3、自动配置原理

 将方法的运行结果进行缓存；以后再要相同的数据，直接从缓存中获取，不用调用方法；
 CacheManager管理多个Cache组件的，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字；

### 1、原理

1. 自动配置类；CacheAutoConfiguration
2. 缓存的配置类
   org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
   org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration
   org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration
   org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration
   org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration
   org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration
   org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration
   org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration
   org.springframework.boot.autoconfigure.cache.GuavaCacheConfiguration
   org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration【默认】
   org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration
3. 哪个配置类默认生效：SimpleCacheConfiguration
4. 给容器中注册了一个CacheManager：**ConcurrentMapCacheManager**
5. 可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在**ConcurrentHashMap**中；

### 2、几个注解的运行流程

   **@Cacheable**

1. 方法运行之前，先去查询Cache（缓存组件），按照cacheNames指定的名字获取；
       （CacheManager先获取相应的缓存），第一次获取缓存如果没有Cache组件会自动创建。

2. 去Cache中查找缓存的内容，使用一个key，默认就是方法的参数；
       key是按照某种策略生成的；默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key；
           SimpleKeyGenerator生成key的默认策略；
                   如果没有参数；key=new SimpleKey()；
                   如果有一个参数：key=参数的值
                   如果有多个参数：key=new SimpleKey(params)；

3. 没有查到缓存就调用目标方法；

4. 将目标方法返回的结果，放进缓存中

   @Cacheable标注的方法执行之前先来检查缓存中有没有这个数据，默认按照参数的值作为key去查询缓存，如果没有就运行方法并将结果放入缓存；以后再来调用就可以直接使用缓存中的数据；

   **核心：**

5. 使用CacheManager【ConcurrentMapCacheManager】按照名字得到Cache【ConcurrentMapCache】组件

6. key使用keyGenerator生成的，默认是SimpleKeyGenerator

   **几个属性：**
      cacheNames/value：指定缓存组件的名字;将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；

7. **key**：缓存数据使用的key；可以用它来指定。默认是使用方法参数的值  1-方法的返回值
       编写SpEL； #i d;参数id的值   #a0  #p0  #root.args[0]
       getEmp[2]

8. **keyGenerator：**key的生成器；可以自己指定key的生成器的组件id
       **key/keyGenerator**：二选一使用;

9. **cacheManager**：指定缓存管理器；或者cacheResolver指定获取解析器

10. **condition**：指定符合条件的情况下才缓存；
        ,condition = "#id>0"
    condition = "#a0>1"：第一个参数的值>1的时候才进行缓

11. **unless**:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断
      		unless = "#result == null"
      		unless = "#a0==2":如果第一个参数的值是2，结果不缓存；

12. **sync**：是否使用异步模式

------

**@CachePut**

既调用方法，又更新缓存数据；同步更新缓存;只要加了这个注解方法就一定会执行
 修改了数据库的某个数据，同时更新缓存；
 **运行时机**：
  1、先调用目标方法
  2、将目标方法的结果缓存起来

1. **测试步骤**：
2. 查询1号员工；查到的结果会放在缓存中；
       key：1  value：lastName：张三
3. 以后查询还是之前的结果
4. 更新1号员工；【lastName:zhangsan；gender:0】
       将方法的返回值也放进缓存了；
       key：传入的employee对象  值：返回的employee对象；
5. 查询1号员工？
       应该是更新后的员工；
           key = "#employee.id":使用传入的参数的员工id；
           key = "#result.id"：使用返回后的id
              @Cacheable的key是不能用#result
       为什么是没更新前的？【1号员工没有在缓存中更新】

------

   **@CacheEvict**

缓存清除

1. key：指定要清除的数据

2. allEntries = true：指定清除这个缓存中所有的数据

3. beforeInvocation = false/true：缓存的清除是否在方法之前执行
       默认为**false**代表缓存清除操作是在方法执行之后执行;如果出现异常缓存就不会清除

   ```
   beforeInvocation = true：
   代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除 
   ```

------

**@Caching** 

定义复杂的缓存规则

```java
@Caching(
     cacheable = {
         @Cacheable(/*value="emp",*/key = "#lastName")
     },
     put = {
         @CachePut(/*value="emp",*/key = "#result.id"),
         @CachePut(/*value="emp",*/key = "#result.email")
    }
)
```

   **抽取缓存的公共配置**           

```java
   @CacheConfig(cacheNames="emp"/*,cacheManager = "employeeCacheManager"*/) 
   public class EmployeeService{}
```

## 4、整合redis作为缓存

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。

1. 安装redis：使用docker
2. 引入redis的starter
3. 配置文件里配置redis相关配置
4. 测试缓存

### 原理

CacheManager === Cache 缓存组件来实际给缓存中存取数据

1. 引入redis的starter，容器中保存的是 RedisCacheManager；
2. RedisCacheManager 帮我们创建 RedisCache 来作为缓存组件；RedisCache通过操作redis缓存数据的
3. 默认保存数据 k-v 都是Object；利用序列化保存；如何保存为json
   1. 引入了redis的starter，cacheManager变为 RedisCacheManager；
   2. 默认创建的 RedisCacheManager 操作redis的时候使用的是 RedisTemplate<Object, Object>
   3. RedisTemplate<Object, Object> 是 默认使用jdk的序列化机制
   4.  改为jackson序列化机制  ---- RedisTemplate<Object, Employee> 
4. 自定义CacheManager:

```java
import com.atguigu.cache.bean.Department;
import com.atguigu.cache.bean.Employee;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;

import java.net.UnknownHostException;

@Configuration
public class MyRedisConfig {

    // 自定义 RedisTemplate：empRedisTemplate RedisTemplate<Object, Employee>
    @Bean
    public RedisTemplate<Object, Employee> empRedisTemplate(
            RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        RedisTemplate<Object, Employee> template = new RedisTemplate<Object, Employee>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Employee> ser = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(ser);
        return template;
    }
    @Bean
    public RedisTemplate<Object, Department> deptRedisTemplate(
            RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        RedisTemplate<Object, Department> template = new RedisTemplate<Object, Department>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Department> ser = new Jackson2JsonRedisSerializer<Department>(Department.class);
        template.setDefaultSerializer(ser);
        return template;
    }



    //CacheManagerCustomizers可以来定制缓存的一些规则(较少使用)
    @Primary  //将某个缓存管理器作为默认的
    @Bean
    public RedisCacheManager employeeCacheManager(RedisTemplate<Object, Employee> empRedisTemplate){
        RedisCacheManager cacheManager = new RedisCacheManager(empRedisTemplate);
        //key多了一个前缀

        //使用前缀，默认会将CacheName作为key的前缀
        cacheManager.setUsePrefix(true);

        return cacheManager;
    }

    @Bean
    public RedisCacheManager deptCacheManager(RedisTemplate<Object, Department> deptRedisTemplate){
        RedisCacheManager cacheManager = new RedisCacheManager(deptRedisTemplate);
        //key多了一个前缀

        //使用前缀，默认会将CacheName作为key的前缀
        cacheManager.setUsePrefix(true);

        return cacheManager;
    }
}

```

## 5、总结

1. 几个关于缓存的注解
2. 生成Key的策略
3. 自定义 RedisTemplate
4. 自定义RedisCacheManager---将默认缓存管理器替换成Redis的

# 二、消息

## 1、消息队列

1. 消息队列的作用

   异步处理、应用解耦、流量削峰

2. 重要概念

   - 消息代理
   - 目的地 
     - 队列
     - 主题

3. 实现方式

   - 点对点式
   - 发布订阅式
   - JMS
   - AMQP

## 2、RabbitMQ简介

1. RabbitMQ简介
2. 核心概念
   - Message
   - Publisher
   - Exchange
   - Queue
   - Binding
   - Connection
   - Channel
   - Consumer
   - Virtual Host
   - Broker

## 3、docker安装带web界面的RabbitMQ

1. 安装docker

2. 安装带web界面的RabbitMQ

   ```shell
   docker pull rabbitmq:3-management
   ```

3. 启动镜像并且暴露访问端口和管理端口

   ```shell
   docker run -d -p 5672:5672 -p 15672:15672 --name myrabbitmq 镜像id
   #默认管理密码 guest guest
   
   docker ps  #查看是否安装成功
   ```

## 4、通过Web界面简单使用RabbitMQ

队列exchange、routerKey、Queue关系图

![](E:/JKingKong/For_Work_BigFile/Spring%20Boot-Advance%E7%AC%94%E8%AE%B0/images/QQ%E6%88%AA%E5%9B%BE20200217162204.png)

1. 创建exchange
2. 创建Queue
3. exchange绑定Queue和routerKey
4. 发送消息
5. 接收消息

## 5、使用Java操作RabbitMQ

### 1、发送消息

```java
	@Autowired
	RabbitTemplate rabbitTemplate;

	/**
	 * 1、单播（点对点）
	 */
	@Test
	public void contextLoads() {
		//Message需要自己构造一个;定义消息体内容和消息头
		//rabbitTemplate.send(exchage,routeKey,message);

		//object默认当成消息体，只需要传入要发送的对象，自动序列化发送给rabbitmq；
		//rabbitTemplate.convertAndSend(exchage,routeKey,object);
		Map<String,Object> map = new HashMap<>();
		map.put("msg","这是第一个消息");
		map.put("data", Arrays.asList("helloworld",123,true));
		//对象被默认序列化以后发送出去
		rabbitTemplate.convertAndSend("exchange.direct","atguigu.news",new Book("西游记","吴承恩"));
	}


	/**
	 * 广播
	 */
	@Test
	public void sendMsg(){
		rabbitTemplate.convertAndSend("exchange.fanout","",new Book("红楼梦","曹雪芹"));
	}

```

### 2、接收消息

```java
	//接受数据,如何将数据自动的转为json发送出去
	@Test
	public void receive(){
		Object o = rabbitTemplate.receiveAndConvert("atguigu.emps");
		System.out.println(o.toString());
	}
```

1. #### jackson数据序列化

   ```java
   @Configuration
   public class MyAMQPConfig {
   
   
       // jackson序列化器转成jackson字符，默认使用java序列化器
       @Bean
       public MessageConverter messageConverter(){
           return new Jackson2JsonMessageConverter();
       }
   }
   
   ```

2. #### 使用注解监听消息

   ```java
   @EnableRabbit  //开启基于注解的RabbitMQ模式
   @SpringBootApplication
   public class Springboot02AmqpApplication {
       ....
   }
   
   @Service
   public class BookService {
   
       // 接收消息监听器注解，要在SpringBoot总配置类上加上 @EnableRabbit  //开启基于注解的RabbitMQ模式
       @RabbitListener(queues = "atguigu.news")
       public void receive(Book book){  // (Book book)消息的类型
           System.out.println("收到消息："+book);
       }
   
       // (Message message)参数，接收消息头
       @RabbitListener(queues = "atguigu")
       public void receive02(Message message){
           System.out.println(message.getBody());
           System.out.println(message.getMessageProperties());
       }
   }
   ```

### 3.AmqpAdmin---管理RabbitMQ（exchange、routerKey、Queue等）

```java
	@Autowired
	AmqpAdmin amqpAdmin;

	// AmqpAdmin测试
	@Test
	public void createExchange(){
		// 创建exchange
		amqpAdmin.declareExchange(new DirectExchange("amqpadmin.exchange"));
		System.out.println("创建完成");

		// 创建队列
		amqpAdmin.declareQueue(new Queue("amqpadmin.queue",true));

		//创建绑定规则
		amqpAdmin.declareBinding(new Binding("amqpadmin.queue", Binding.DestinationType.QUEUE,"amqpadmin.exchange","amqp.hah",null));

	}
```

## 三、检索

### 1、elasticsearch简介

### 2、使用docker安装elasticsearch

```shell
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name es01  镜像id
```

### 3、elasticsearch的文档

[elasticsearch的文档](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_retrieving_a_document.html)

### 4、简单使用elasticsearch

1. #### 使用postman发送请求（restful风格）

   - put请求存放、更新数据

   - get请求获取数据

   - head检查有无数据，返回404表示没有

     ![1581937919075](E:/JKingKong/For_Work_BigFile/Spring%20Boot-Advance%E7%AC%94%E8%AE%B0/images/1581937919075.png)

## 四、任务

引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

### 1、异步任务

1. 注解
   - @EnableAysnc
   - @Aysnc

### 2、定时任务

1. 注解
   - @EnableScheduling //开启基于注解的定时任务
   - @Scheduled(cron = "0/4 * * * * MON-SAT")

![1581950971201](E:/JKingKong/For_Work_BigFile/Spring%20Boot-Advance%E7%AC%94%E8%AE%B0/images/1581950971201.png)

```java
    /**
     * second(秒), minute（分）, hour（时）, day of month（日）, month（月）, day of week（周几）.
     * 0 * * * * MON-FRI
     *  【0 0/5 14,18 * * ?】 每天14点整，和18点整，每隔5分钟执行一次
     *  【0 15 10 ? * 1-6】 每个月的周一至周六10:15分执行一次
     *  【0 0 2 ? * 6L】每个月的最后一个周六凌晨2点执行一次
     *  【0 0 2 LW * ?】每个月的最后一个工作日凌晨2点执行一次
     *  【0 0 2-4 ? * 1#1】每个月的第一个周一凌晨2点到4点期间，每个整点都执行一次；
     */
```

### 3、邮件任务

1. 简单邮件任务

   ```java
   	@Test
   	public void contextLoads() {
   		SimpleMailMessage message = new SimpleMailMessage();
   		//邮件设置
   		message.setSubject("通知-今晚开会");
   		message.setText("今晚7:30开会");
   
   		message.setTo("icu.996@qq.com");
   		message.setFrom("1339064067@qq.com");
   
   		mailSender.send(message);
   	}
   ```

2. 复杂邮件任务

   ```java
   	@Autowired
   	JavaMailSenderImpl mailSender;  // 传入
   	// 复杂邮件任务
   	@Test
   	public void test02() throws  Exception{
   		//1、创建一个复杂的消息邮件
   		MimeMessage mimeMessage = mailSender.createMimeMessage();
   		MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
   
   		//邮件设置
   		helper.setSubject("通知-今晚开会");
   		helper.setText("<b style='color:red'>今天 7:30 开会</b>",true);
   
   		helper.setTo("icu.996@qq.com");
   		helper.setFrom("1339064067@qq.com");
   
   		//上传文件
   		helper.addAttachment("1581927818484.png",new File("E:\\JKingKong\\For_Work_BigFile\\Spring Boot-Advance笔记\\images\\1581927818484.png"));
   
   		mailSender.send(mimeMessage);
   
   	}
   ```

   

## 五、安全

**spring security功能：**

* 可以进行配置用户身份，让其能访问什么连接（java里写代码），
* 按照身份显示什么界面（配合thymeleaf）
* 自定义登录页面

### 引入依赖

```xml
thymeleaf模板引擎 整合 spring security
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity4</artifactId>
</dependency>

thymeleaf模板引擎：
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

spring security:
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 注解

```
@EnableWebSecurity
```

### 安全配置类

```java
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@EnableWebSecurity	//开启安全注解
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //super.configure(http);
        //定制请求的授权规则，不同url下需要的身份不同
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("VIP1")    //  /level1/下的所有请求需要VIP1才行
                .antMatchers("/level2/**").hasRole("VIP2")
                .antMatchers("/level3/**").hasRole("VIP3");

        //开启自动配置的登陆功能，效果，如果没有登陆，没有权限就会来到登陆页面
        http.formLogin()
                .usernameParameter("user").passwordParameter("pwd")  //配合前端登录表单的name属性  默认如果不更改是 username password  (看文档)
                .loginPage("/userlogin");    //loginPage("/userlogin")转到自己的页面 /userlogin Controller里有配置
        //1、/login来到登陆页
        //2、重定向到/login?error表示登陆失败
        //3、更多详细规定
        //4、默认post形式的 /login代表处理登陆
        //5、一但定制loginPage；那么 loginPage的post请求就是登陆


        //开启自动配置的注销功能。
        http.logout().logoutSuccessUrl("/");//注销成功以后来到首页
        //1、访问 /logout 表示用户注销，清空session
        //2、注销成功会返回 /login?logout 页面；

        //开启记住我功能
        http.rememberMe().rememberMeParameter("remeber");
        //登陆成功以后，将cookie发给浏览器保存，以后访问页面带上这个cookie，只要通过检查就可以免登录
        //点击注销会删除cookie

    }

    //定义认证规则  账号密码，也可以从数据库访问
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
        auth.inMemoryAuthentication()
                .withUser("zhangsan").password("123456").roles("VIP1","VIP2")
                .and()
                .withUser("lisi").password("123456").roles("VIP2","VIP3")
                .and()
                .withUser("wangwu").password("123456").roles("VIP1","VIP3");

    }
}

```

## 六、分布式

### **zookeeper + Dubbo**

导入依赖

```xml
<dependency>
    <groupId>com.alibaba.boot</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>0.1.0</version>
</dependency>

<!--引入zookeeper的客户端工具-->
<!-- https://mvnrepository.com/artifact/com.github.sgroschupf/zkclient -->
<dependency>
    <groupId>com.github.sgroschupf</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.1</version>
</dependency>
```

1. docker安装zookeeper并配置暴露端口

   ```shell
   docker pull zookeeper
   docker images
   docker run --name zk01 -p 2181:2181 --restart always -d 镜像id
   ```

2. 新建空工程

3. provider-ticket工程

   1. 配置文件

      ```properties
      dubbo.application.name=provider-ticket
      #zookeeper注册中心地址
      dubbo.registry.address=zookeeper://114.55.102.208:2181
      #包扫描路径
      dubbo.scan.base-packages=com.atguigu.ticket.service
      ```

   2. 发布服务注解

      - ```
        //包是这个：不是Spring的
        import com.alibaba.dubbo.config.annotation.Service;
        //将服务发布出去 是Dubbo的Service注解 
        @Service 
        ```

4. consumer-ticket工程

   1. 配置文件

      ```properties
      dubbo.application.name=consumer-user
      
      dubbo.registry.address=zookeeper://114.55.102.208:2181
      ```

   2. 接口全限定类名要与服务提供类的相同

   3. 调用服务类 @Reference注解

      ```java
      import com.alibaba.dubbo.config.annotation.Reference;
      import com.atguigu.ticket.service.TicketService;
      import org.springframework.stereotype.Service;
      
      @Service // 这个是Spring的注解
      public class UserService{
          
          // Dubbo按照zookeeper里注册的(按照全类名)远程调用  提供者和消费者全类名要一致
          @Reference
          TicketService ticketService;
      
          public void hello(){
              String ticket = ticketService.getTicket(); // 远程调用实现的方法
              System.out.println("买到票了："+ticket);
          }
      }
      ```

### **Spring Cloud**

## 七、热部署

可以在容器启动的时候，做出更改，使用CTRL+ F9重新编译之后，实时体现到页面

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

## 八、监控管理

### 1、监控和管理端点（通过url访问）

![1581950622234](E:/JKingKong/For_Work_BigFile/Spring%20Boot-Advance%E7%AC%94%E8%AE%B0/images/1581950622234.png)

### 2、定制端点

![1581951451196](E:/JKingKong/For_Work_BigFile/Spring%20Boot-Advance%E7%AC%94%E8%AE%B0/images/1581951451196.png)

```shell
management.security.enabled=false

spring.redis.host=118.24.44.169

info.app.id=hello
info.app.version=1.0.0

#endpoints.metrics.enabled=false
endpoints.shutdown.enabled=true

#将原来的beans端点访问：/beans ---> /mybean
#endpoints.beans.id=mybean

#将原来的beans端点访问：/beans ---> /bean/b
#endpoints.beans.path=/bean/b

# 关闭beans端点
#endpoints.beans.enabled=false

#将原来的dump端点访问：/dump ---> /du
#endpoints.dump.path=/du


# 关闭所有端点访问
#endpoints.enabled=false
#endpoints.beans.enabled=true

management.context-path=/manage

management.port=8181
```

### 3、 自定义健康状态指示器

1. 编写一个指示器 实现 HealthIndicator 接口

2. 指示器的名字 xxxxHealthIndicator

3. 加入容器中

   ```java
   @Component
   public class MyAppHealthIndicator implements HealthIndicator {
   
       @Override
       public Health health() {
   
           //自定义的检查方法
           //Health.up().build()代表健康
           return Health.down().withDetail("msg","服务异常").build();
       }
   }
   
   ```

   