# 一、Consul

## 1、概念

- Consul是一套分布式服务发现和配置管理系统，由Go语言开发。提供微服务系统中的服务治理、配置中心、控制总线等功能，每个功能可用单独使用。

## 2、安装运行

- 官方下载地址https://www.consul.io/downloads
- 启动ConsulServer服务
- ConsulServer默认运行在8500端口。只需要通过http://ip:8500即可访问Consul管理页面。
```shell
// linux或windows下使用开发者模式启动服务
consul agent -dev

// 后台启动方式 - 先后台运行，再指定输出路径
nohup consul agent -dev -ui -node=consul-dev -client=0.0.0.0 &
nohup consul agent -dev -ui -node=consul-dev -client=0.0.0.0 &> <日志路径>
```


# 二、服务注册

## 1、引入ConsulClient依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-consul-discovery</artifactId>
  <version></version>
</dependency>
```
## 2、配置Application
```yaml
spring:
 application:
  name: consul-provider-payment
 cloud:
  consul:
   host: <consul-server-ip>
   port: <consul-server-端口>
   discovery:
     # 注册服务名称
     service-name: ${spring.application.name} 
     # 采用cp原则，所以需要手动设置服务超时注销时间:m分
     # 如果不设置超时时间，服务信息将一直存留在consul上
     health-check-critical-timeout: 1m 
     # consul采用server向client发送心跳，所以要保证client能被ping通
     ip-address: <本地公网ip>
     instance-id: ${spring.application.name}:${spring.cloud.consul.discovery.ip-address}
     health-check-url: http://${spring.cloud.cosuldiscovery.ip-address}:${server.port}/actuator/health
```
## 3、在主启动类上添加客户端标识

```java
@SpringBootApplication
// 该注解用于向consul或zookeeper作为注册中心时注册服务
@EnableDiscoveryClient
public class PaymentMain8006 {
  public static void main(String[] args) {
     SpringApplication.run(PaymentMain.class, args);
  }
}
```





