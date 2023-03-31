# 一、概念

- 封装AMQP协议的API抽象方法，就类似SpringJMS封装简化JMS接口，可用于简化以AMQP为协议的通信方法调用，如Rabbitmq通信调用。
- SpringAMQP文档：https://docs.spring.io/spring-amqp/docs/current/reference/html/
- RabbitMQ-SpringAMQP：https://www.rabbitmq.com/getstarted.html
- 依赖：
```xml
<dependency>
     <groupId>org.springframework.amqp</groupId>
     <artifactId>spring-rabbit</artifactId>
     <version>2.3.9</version>
</dependency>
```


# 二、配置与使用

## 1、application配置

- autoconfig：

```yaml
spring:
 rabbitmq:
  # 账号密码端口等
  username: admin
  password:
  # 默认发送模板
  template:
   exchange:
   routing-key:
   default-receive-queue:
```
- 开启注解使用：

```java
@EnableRabbit
public class RabbitmqDemoApplication {
  public static void main(String[] args) {
      SpringApplication.run(RabbitmqDemoApplication.class, args);
  }
}
```
## 2、使用@Bean管理队列、交换机、绑定对象

- 在Configuration中管理bean对象即可。

```java
// 队列 - 子类AnonymousQueue可声明临时队列
@Bean
public Queue myQueue() {
  return new Queue("hello");
}

// 交换机 - 父类AbstractExchange的所有字类对应各种交换机类型
@Bean  // 声明扇出交换机
public FanoutExchange fanout() {
  return new FanoutExchange("tut.fanout");
}

// 绑定
@Bean
public Binding myBinding(Queue queue, FanoutExchange exchange) {
  // return new Binding();
  // 使用绑定builder构建
  return BindingBuilder.bind(queue).to(exchange);
}
```
## 3、使用RabbitTemplate

- 直接使用AutoWired注入即可

    - RabbitTemplate类设计模式与SpringJMS大体类似，都是以doXXX方法为主的封装重载方法。

```java
@Autowired
private RabbitTemplate rabbitTemplate;
```
## 4、生产者实例

- 使用rabbitTemplate#doSend相关方法和重载即可，send相关方法实现org.springframework.amqp.core.AmqpTemplate接口。

```java
// 发送消息
// 其他重载可以发送在application中配置的默认地址
rabbitTemplate.convertAndSend("交换机名", "路由key", "消息内容");
```
- 快速构建消息对象（通过MessageBuilder类）：

```java
Message message = MessageBuilder.withBody("foo".getBytes())
  .setContentType(MessageProperties.CONTENT_TYPE_TEXT_PLAIN)
  .setMessageId("123")
  .setHeader("bar", "baz")
  .build();
```
## 5、消费者实例

- 注解异步监听消费：

```java
// 直接在方法上声明
@RabbitListener(queues = "queue1")
public void consumer(Message msg) {...}
```
- 使用@SendTo做消息传递（与spring-jms一致）：

```java
@RabbitListener(queues = "queue1")
@SendTo("queue2")
public void consumer(Message msg) {...}
```
- 类与多方法监听：

```java
@RabbitListener(queues = "queue2")
public class RabbitListenerClazz {
    
  // RabbitListener放类上时指定处理方法
  // 多RabbitHandler时格局消息类型选择
  // isDefault=true标识默认方法，找不到时选择
  @RabbitHandler(isDefault = true)
  public void consumerHandler(String msg) {...}

  @RabbitHandler
  public void consumerHandler(int msg) {...}

  // ...
}
```





