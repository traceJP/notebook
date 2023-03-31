# 一、概念

- SpringCloudConfig分为服务端和客户端两部分。

- 服务端也称为分布式配置中心，它是一个独立的微服务引用，用于连接配置服务器（默认GitHub）并为客户端提供获取配置信息，加密、解密信息等访问接口。

- 客户端则是通过指定的配置中心管理应用资源，并在启动的时候从配置中心获取和加载配置信息，并将这些配置信息直接注入到spring容器之中，如同直接配置到application中一样。而且实时感知服务端配置信息的变化进行配置的热部署。


# 二、创建github仓库

- 在github上创建仓库，并提交对应的配置文件

- 可以选择配置ssh密钥接入方式，即配置ssh密钥

- 配置文件命名建议：

```
application配置文件名;profile文件后缀;label分支名;
eg: /master/config-dev.yml -> application=config,profile=dev,label=master
/{application}/{profile}[/{label}]
/{application}-{profile}.yml
/{label}/{application}-{profile}.yml
/{application}-{profile}.properties
/{label}/{application}-{profile}.properties
```


# 三、配置服务端

## 1、引入依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-config-server</artifactId>
  <version></version>
</dependency>
```
## 2、在主类上开启功能
```java
@EnableConfigServer
public class AppConfigCenterMain3344 {
  public static void main(String[] args) {
      SpringApplication.run(AppConfigCenterMain3344.class, args);
  }
}
```
## 3、配置application

spring-cloud-config
```yaml
server：
  git:
    # 超时时间 
    timeout: <单位秒，默认5秒>
    # github仓库-https地址 或 ssh地址 （密码登录方式即将被废弃，建议使用ssh）
    uri: https://github.com/traceJP/springcloud-config-demo.git
    # 如果是https地址需要配置账号-密码
    username: <用户名>
    password: <密码>
    # 搜索目录（集合）：将会在这些目录下搜索所有合格的配置文件
    # 如果为项目根目录文件，则填写项目名作为文件名即可
    search-paths:
      - <目录名>
    # 如果使用ssh地址连接则需要配置host-key密钥
    host-key: <ssh密钥>
# 读取分支
label: master
```


# 四、配置客户端

## 1、引入依赖
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-config</artifactId>
  <version></version>
</dependency>
```
## 2、新增配置文件

- 在resources下新增一个bootstrap.yml的配置文件，在idea中bootstrap文件呈云图标。

```yaml
spring:
  cloud:
    config:
      # 读取分支
      label: master
      # 配置文件名
      name: config
      # 读取后缀名
      profile: dev
      # 配置中心服务地址
      uri: http://localhost:3344
```


## 3、bootstrap与application

- bootstrap：是application的父上下文，即加载优先于application，主要用于从额外的资源来加载配置信息。

- 属性覆盖：

    - 与springboot的多个配置文件一样，按照加载顺序进行相同属性名的覆盖。

    - 即后加载 覆盖 前加载的。

    - 配置文件覆盖顺序：远程配置文件 > application > bootstrap


# 五、动态配置刷新

- 当github配置更新时，已运行的Config客户端无法察觉到变化，必须重启服务才会刷新。

- 原因：客户端启动时才会对服务端进行请求，然后服务端再克隆github项目并把配置文件数据发还给客户端。所以客户端无法主动察觉变化。

- 动态配置刷新的最佳实现可以参考SpringCloudBus。


## 1、通过actove健康检查刷新（请求刷新）

- 通过给具体的服务实例发送一个请求使其重新加载配置文件动态获取，可以使用shell脚本编写批量发送请求。

- 需要引入spring-actuator依赖：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
- 在引用配置文件的类中开启刷新功能：

    - 当实例进行刷新时，所有注释了@RefreshScope的Bean对象都会进行刷新，从而注入配置文件的新值。

```java
@Component
@RefreshScope  // 刷新此bean对象
public class ConfigClientController {
  @Value("${config.info}")
  private String configInfo;
}
```
- 在application配置文件中暴露监控刷新端点：

```properties
management-endpoint-web-exposure-include: "*"
```
- 发送请求给实例进行刷新：

```http
POST http://<实例ip>:<实例端口>/actuator/refresh
```





