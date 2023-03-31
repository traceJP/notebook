- 使用JMS接口需要引入实现JMS接口规范的消息中间件依赖：（以ActiveMQ为例）
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-activemq</artifactId>
  <version></version>
</dependency>
```
# 一、JMS队列模型

## 1、生产消息

- 使用JMS规范API发送消息至ActiveMQ服务。

- ACTIVEMQ_SERVER_URL（tcp://localhost:61616）：服务器地址常量（指定协议为tcp），若改变了服务器默认密码admin则需要在连接工厂中设置对应账号和密码。

- QUEUE_NAME（queue01）：常量队列名。

```java
public void sendMessageToQueue() throws JMSException {

  // 1 创建连接工厂
  ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_SERVER_URL);

  // 2 获取连接，启动访问mq服务端
     // 抛出异常javax.jms.JMSException
  Connection connection = activeMQConnectionFactory.createConnection();
  connection.start();

  // 3 创建会话session
     // 指定参数 - 事物,签收
  Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

  // 4 创建目的地（指定具体为队列还是主题）
     // 指定参数 - 目的地名
  Queue queue = session.createQueue(QUEUE_NAME);

  // 5 创建消息的生产者
  MessageProducer producer = session.createProducer(queue);

  // 6 创建消息 - 子类实例为字符串消息
  Message message = session.createTextMessage("message111");

  // 7 通过生产者发布消息
  producer.send(message);

  // 8 关闭资源
  producer.close();
  session.close();
  connection.close();
}
```
## 2、消费消息

- 消费消息ACTIVEMQ_SERVER_URL、QUEUE_NAME常量与生产消息常量一致，即生产消费者实例。

```java
public void receiveMessageByQueue() throws JMSException {

  // 1 创建连接工厂
  ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_SERVER_URL);

  // 2 获取连接，启动访问mq服务端
  Connection connection = activeMQConnectionFactory.createConnection();
  connection.start();

  // 3 创建会话session
  Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
    
  // 4 创建目的地（指定具体为队列还是主题）
  Queue queue = session.createQueue(QUEUE_NAME);

  // 5 创建消息的消费者
  MessageConsumer consumer = session.createConsumer(queue);

  // 6 消费消息 - 消息实例与生产出来的实例匹配 - （可以使用while循环持续消费队列中所有消息）
  TextMessage textMessage = (TextMessage) consumer.receive();
  System.out.println("消费者接收到消息为---> " + textMessage.getText());

  // 7 关闭资源
  consumer.close();
  session.close();
  connection.close();
}
```
## 3、消费模式

- 具体可以参考jvax.jms.MessageConsumer接口，该接口用于管理消费者做出的行为。


### （1）同步阻塞式消费

- 使用MessageConsumer接口方法调用消费即可。

```java
// ...
MessageConsumer consumer = session.createConsumer(queue);

// 持续阻塞直到消费到一条消息
consumer.receive();

// 等待timeout毫秒后仍然没有消费到消息（队列无消息），则停止阻塞
consumer.receive(Long timeout);

// 不阻塞消费一条消息，如果没有消息则返回null
consumer.receiveNoWait();
```
### （2）异步非阻塞式（监听器）消费

- 注册一个监听器到MessageConsumer中，该监听器将会持续异步非阻塞的消费队列中存在的消息。即有消息就消费，无消息就阻塞等待消息。

- 监听器为继承MessageListener接口实现onMessage方法的实例，当队列存在消息时则会触发此方法，传入一个消息实例。

- 需要注意：此方法是异步的，并且如果资源被关闭close后，监听器将不会继续监听。可以使用System.in.read()用户输入从而阻塞一下程序关闭。

```java
// 可以使用lambda表达式匿名类
consumer.setMessageListener(new MessageListener() {
  @Override
  public void onMessage(Message message) {
     // ...对消息的操作
  }
});
```
### （3）持续消费的分配

- 当队列中出现多个消费者同时消费消息时，消息将按照轮询规则平均分配给所有消费者。

- 如两个持续监听消费者对消息进行监听。此时生产者发布6条消息，那么此时首次注册的消费者将消费135，其次注册的将消费246。


# 二、JMS发布订阅模型

## 1、生产消息

- 与队列模型生产消息类似，只需要指定消息的目的地为主题即可。

```java
public void sendMessageToTopic() throws JMSException {

  // 1 与队列创建创建连接和session一致
  ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_SERVER_URL);
  Connection connection = activeMQConnectionFactory.createConnection();
  connection.start();
  Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

  // 2* 创建目的地 - 指定为主题（createTopic）
  Topic topic = session.createTopic(TOPIC_NAME);

  // 3 创建消息的生产者并发送消息-与队列一致
  MessageProducer producer = session.createProducer(topic);
  Message message = session.createTextMessage("message111");
  producer.send(message);

  // 4 关闭资源
  producer.close();
  session.close();
  connection.close();
}
```
## 2、消费消息

- 也与队列模型消费消息类似，改变目的地即可。

```java
public void receiveMessageByTopic() throws JMSException {

  // 1 与队列创建创建连接和session一致
  ActiveMQConnectionFactory activeMQConnectionFactory = new ActiveMQConnectionFactory(ACTIVEMQ_SERVER_URL);
  Connection connection = activeMQConnectionFactory.createConnection();
  connection.start();
  Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

  // 2* 创建目的地 - 指定为主题
  Topic topic = session.createTopic(TOPIC_NAME);

  // 3 创建消息的消费者
  MessageConsumer consumer = session.createConsumer(topic);

  // 4 监听或直接消费
  consumer.setMessageListener(...);

  // 5 关闭资源
  consumer.close();
  session.close();
  connection.close();
}
```


# 三、持久性消息

## 1、队列模型

- 只需要设置消息头JMSDeliveryMode或消息生产者MessageProducer对象持久化模式即可。默认为消息持久模式。

```java
// 枚举类javax.jms.DeliveryMode
void Message#setJMSDeliveryMode(int <枚举值>);
void MessageProducer#setDeliveryMode(int <枚举值>);
```
## 2、发布订阅模型

- 在发布中通过设定MessageProducer对象持久化模式即可，但必须要保证连接开启在此设置之后。

```java
// ...设置连接
// 设置持久化
producer.setDeliveryMode(DeliveryMode.PERSISTENT);

// *** 需要在指定持久化后 再开启连接
connection.start();

// ... 创建发送消息等
```
- 在订阅消息中需要指定当前客户端id并且创建一个持久化订阅主题javax.jwt.TopicSubscriber。使用该对象进行消息的消费和订阅。并且连接开启必须在创建该对象之后。

```java
// 设置客户端id：即谁要订阅公众号
connection.setClientID(String clientID);

// 目的地
Topic topic = session.createTopic(TOPIC_NAME);

  // Topic topic1 = ... 多个Topic对象
// 创建持久化订阅主题 - 传入参数：目的地, 订阅名,（唯一）
  // 通过指定多个topic对象不同名，订阅多个发布者
session.createDurableSubscriber(Topic topic, String name);

// *** 需要在指定持久化后 再开启连接
connection.start();

// 取消订阅，name：订阅名
session.unsubscribe(String name);
```


# 四、事物和签收

## 1、事物

- 事物开启：

```java
// 第一个参数为Boolean transacted; true开启,false关闭
Session session = connection.createSession(true, Session.AUTO_ACKNOWLEDGE);
```
- 批处理提交：

```java
session.commit();
```
- 事物回滚：

```java
session.rollback();
```


## 2、签收

- 签收模式修改：

```java
// 第二个参数为int acknowledgeMode; 枚举java.jms.Session
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
```
- 手动签收：

```java
// 消费的每条消息对象都需要签收
message.acknowledge();
```





