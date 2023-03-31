# 一、SpringJMS

- spring通过封装原生的javax.jms接口规范形参一套自己的api体系。
- 官网文档：https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#jms

#  二、基本配置和开启

- 在主类上开启注解支持：

```java
@EnableJms  // 开启Jms注解支持
public class ActiveMqDemoApplication {
  public static void main(String[] args) {
     SpringApplication.run(ActiveMqDemoApplication.class, args);
  }
}
```
- 配置：

```yaml
spring:
 activemq:
  # ActiveMQ服务地址
  broker-url: tcp://localhost:61616
  # 用户名-密码：默认都为admin
  user: admin
  password: admin
 # * 直接配置jms默认的一些规则，可以避免在代码中重复指定规则
 jms:
  # 默认目的地类型；false队列，true主题，默认false队列
  pub-sub-domain: false
  template:
   # 默认目的地名：未指定则使用这个 
   default-destination: <string>
```


# 三、使用JmsTemplate（以Queue为例）

- 使用@Autowired直接注入JmsTemplate对象即可使用。并且使用JmsTemplate对象进行消息操作简化了连接创建，生产者/消费者创建等步骤。只需直接调用方法生产消费即可。

- 基本发送消费消息：

```java
public void simpleSend() {
  // 发送消息到队列 - myqueue为Queue对象，可以通过单独一个类统一管理
     // 本质是传入Destination对象，而Queue、Topic等类继承Destination类
     // 其他重载方法也可以直接传入目的地名（默认都使用队列模型）
  jmsTemplate.send(myqueue, session -> session.createTextMessage("hello queue world"));

  // 消息转换器加发送到队列-使用默认目的地名
  jmsTemplate.convertAndSend("hello");

  // 消费消息到队列，返回javax.jms.Message对象
     // 其他重载方法可以传入消费名-不指定则使用默认目的地名
  Message message = jmsTemplate.receive();

  // 便捷消费消息到队列，直接返回Object类型
  Object message = jmsTemplate.receiveAndConvert();

  // 消费选择
  jmsTemplate.receiveSelected();

  // 发送消息并阻塞直到有一条消息可以被消费
  jmsTemplate.sendAndReceive();
}
```
- JmsTemplate-API封装规则：

    - 其内部方法大体包括获取默认值方法、事物执行方法、发送消息和便捷发送消息方法、消费和选择消费和便捷消费方法、浏览消息方法（浏览指接收后不消费）。

    - 其中发送消息和消费消息都有一个主要的方法以doXXX命名，如doSend方法。


# 四、使用消费监听器

- 使用Spring实现异步消费。主要通过配置DefaultMessageListenerContainer对象或SimpleMessageListenerContainer对象实现。

- 配置实例（以DefaultMessageListenerContainer为例）：

    - 定义自定义监听器：

```java
// 自定义监听器-与JMS接口实现类似
public class MyConsumerListener implements MessageListener {
  @Override
  public void onMessage(Message message) {...}
}

// 或者使用SessionAwareMessageListener接口
// 此接口是spring对JMS-MessageListener接口的增强
// 与其效果一致，监听消息时多一个session参数
public class MySessionConsumerListener implements SessionAwareMessageListener<TextMessage> {
  @Override
  public void onMessage(TextMessage message, Session session) throws JMSException {...}
}
```
- 定义配置容器并注册监听器

```java
@Bean
public DefaultMessageListenerContainer defaultMessageListenerContainer(ConnectionFactory connectionFactory) {

  DefaultMessageListenerContainer defaultMessageListenerContainer = new DefaultMessageListenerContainer();

  // 将spring容器中的jms连接工厂注册到监听器容器中
  defaultMessageListenerContainer.setConnectionFactory(connectionFactory);

  // 配置目的地
  defaultMessageListenerContainer.setDestinationName("queue");

  // 注册监听器 - 注册自定义的监听器
  defaultMessageListenerContainer.setMessageListener(new MyConsumerListener());
  return defaultMessageListenerContainer;
}
```
- DefaultMessageListenerContainer与SimpleMessageListenerContainer：

    - SimpleMessageListenerContainer是使用普通的JMS-API实现的，其监听过程是调用MessageConsumer.setMessageListener()方法实现的并发监听。

    - DefaultMessageListenerContainer是spring自己实现的对象。通过spring的定时任务TaskExecutor实现，内部则是使用MessageConsumer.receive()进行循环消费达到的监听效果。即容器启动时会使用TaskExecutor创建一个额外的线程，在此线程中循环调用receive进行监听。并且Default比传统JMS新增了缓存功能，也可以在运行过程中动态的改变监听目标。并且也实现了代理宕机的容器恢复功能。


# 五、注解配置

## 1、定义监听器

- 使用注解直接定义监听器并开始监听：

    - 只需要在方法上加入@JmsListener注解即可将此方法注册为消费者监听器。也可以使用@JmsListeners注解将多个JmsListener合并成一个容器，使用同一个方法监听。

    - 缺点：自定义配置低，无法实现事物签收等操作。

```java
@JmsListener(destination = "<目的地名>")
public void processOrder(Message message) throws JMSException {...}
```
## 2、定义响应发送

- 响应发送可以使得一个方法的返回值直接作为消息生产发送出去，使用@SendTo注解到方法上即可使用。并且必须与@JmsListener配合使用。类似一个消息中转站。

    - 如果指定自己会造成死循环。即接收消息,再响应发送给自己,自己又接收,然后又响应发送给自己。

```java
@JmsListener(destination = "<目的地名>")
@SendTo("<目的地名>")  // 可配置多个目的地名
public String processOrder(TextMessage textMessage) throws JMSException {
  return "响应发送的消息";
}
```





