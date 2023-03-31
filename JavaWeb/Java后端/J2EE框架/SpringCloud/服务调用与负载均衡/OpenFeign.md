# 一、概念

## 1、Feign

- 在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方去。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，只需创建一个接口并使用注解的方式来配置它（如Mybatis中Dao接口上面标注Mapper注解，即可发送JDBC请求），即可完成对服务提供方的接口绑定。并集成了Ribbon，简化服务调用客户端的开发量。

## 2、Feign与OpenFeign

- Feign是Spring Cloud组件中的一个轻量级RESTful的HTTP服务客户端Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务。
- OpenFeign是SpringCloud在Feign的基础上支持了SpringMVC的注解，如@RequesMapping等等。OpenFeign的@FeignClient可以解析SpringMvc的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。 

# 二、服务调用

## 1、依赖

- Feign（已过时）：
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-feign</artifactId>
  <version></version>
</dependency>
```
- OpenFeign：

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
  <version></version>
</dependency>
```
## 2、启用Feign
```java
@SpringBootApplication
// 注解启动Feign
@EnableFeignClients
public class Application {
  public static void main(String[] args) {
      SpringApplication.run(Application.class, args);
  }
}
```
## 3、映射生产者接口

- 创建与生产者相映射的接口和指定映射的方法，指定映射到哪个微服务。

    - 其中映射指定也可以直接指定url

    - 映射规则：参数传递时必须标注@RequestParam注解或@RequestBody注解，否则Feign无法识别并报错。

```java
// 生产者：提供get接口，响应返回端口
@GetMapping("/get")
public String baseRequest() {
  return port;
}

// 消费者
@Component
@FeignClient("<注册服务名>")  // 1 映射服务
public interface ProviderPayment {

  // 2 映射接口方法
  @GetMapping("/get")
  String baseRequest();
}
```
## 4、调用发送请求

- 通过第三步实现生产者消费者接口映射后，消费者直接调用接口方法即可实现远程调用。


# 三、超时控制

- 由于Feign的底层使用Ribbon控制请求的时间以及负载均衡，所以Feign的客户端（消费者）调整了每个请求默认只等待一秒钟，如果服务端处理需要超过一秒钟，将会导致Feign客户端直接报超时异常。

- application配置Feign超时时间：

```yaml
ribbon:
  # 建立连接所用的时间 
  Read-timeout: <毫秒>
  # 建立连接后从服务器读取到可用资源所用的时间  
  Connect-timeout: <毫秒> 
```


# 四、日志增强

## 1、概念

- Feign提供日志打印功能，并与传统的日志框架如slf4j整合，从而可用通过配置来调整输出日志和级别，从而了解Feign中发送Http请求的细节。实现对Feign接口的调用情况进行监控和输出。


## 2、开启日志

- application配置：

    - 注意：配置时可用分别指定开启监控哪个接口的监控。并且logging只能配置debug级别，即只能控制开启关闭。

```yaml
logging:
  level:
    <接口全类名>: debug
```
## 3、Feign日志级别

- Feign提供的日志级别：

    - NONE：无日志记录（默认）。

    - BASIC：只记录请求方法和 URL 以及响应状态码和执行时间。

    - HEADERS：除了BASIC外，还记录基本信息以及请求和响应标头。

    - FULL：除了HEADERS外，还记录请求和响应的标头、正文和元数据。

- 配置具体日志级别：

    - 只需要将Feign控制日志级别Bean添加到容器即可。

```java
@Configuration
public class FeignConfig {
  // 配置Feign的日志级别bean
  @Bean
  Logger.Level feignLoggerLevel() {
      return Logger.Level.FULL;
  }
}
```





