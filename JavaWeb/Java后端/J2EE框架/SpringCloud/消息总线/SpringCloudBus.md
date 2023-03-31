# 1、概念

- 代理AMQP协议（RabbitMQ）或Kafka两种消息中间件，提供启动器作为消息的传输。
- 通过SpringCloudBus和SpringCloudConfig配合使用即可达到向Config配置中心发送刷新命令时，广播到所有由配置中心管理的实例中，即配置中心刷新，处处刷新。
- 同时SpringCloudBus也可以发送自定义的消息给其所管理的所有服务实例进行消费。

# 2、服务刷新主题（以AMQP为例）

- 引入依赖：
    - 注意：所有需要发送或传递消息的实例都需要引入依赖和配置。
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-bus-amqp</artifactId>
  <version></version>
</dependency>
```
- application配置消息中间件代理：

    - 注意：所有需要发送或传递消息的实例都需要引入依赖和配置。

```yaml
spring:
 rabbitmq:
  host: ip
  port: 端口
  username: 用户名
  password: 密码
```
- 在服务端暴露监控刷新端点：
    - 因为只需要往服务端实例发送请求即可进行消息的广播，所以客户端无需暴露刷新端点。

```properties
management-endpoints-web-exposure-include: "*"
```
- 在引入配置文件依赖的Bean对象中增加@RefreshScope刷新注解。

- 修改github-config后，向服务总线端点发送请求刷新即可一次向所有实例进行重启。

    - 请求URL：

```http
POST http://<总线实例ip:port>/actuator/bus-refresh
```
- 此时MQ代理会向MQ实例建立一个主题模型（主题默认名为SpringCloudBus），在此模型内进行消息的生产消费，从而达到广播服务刷新效果。


# 3、服务刷新定点通知

- 某时，只想刷新某些实例服务，而不想广播后一次性刷新全部服务，所以需要定点通知某个服务进行刷新。与直接向客户端配置监控刷新端点不同，定点通知是向消息总线通知时的具体指定。

- 请求URL：

```http
POST http://<总线实例ip:port>/actuator/bus-refresh/{destination}
// destination：微服务名:端口号
// http://localhost:3344/actuator/bus-refresh/cloud-config-client:3355
```





