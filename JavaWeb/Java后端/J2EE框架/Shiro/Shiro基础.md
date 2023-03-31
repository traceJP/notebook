# 一、概述

- Apache Shiro是一个功能强大且易于使用的Java安全框架，它为开发人员提供了一种直观，全面的身份验证，授权，加密和会话管理解决方案。并且，Shiro不必运行在Web应用上就可以使用或者管理Session。
- Shiro的Web应用是基于Filter过滤器而实现的。
- 高层类图：

![clipboard.png](Shiro%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

- Shiro详细架构

![clipboard.png](Shiro%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

- 名词介绍：
    - 主题（Subject）：特定域安全性的视图（页面）
    - SecurityManager：Shiro框架核心
    - 认证器（Authenticator）：用于执行用户操作的组件，如登录退出等
        - 认证策略（AuthenticationStategy）：多种认证方式下的策略（一个认证成功是否其他的都成功，还是都需要认证）
    - SessionManager：Shiro框架提供的Session管理机制
    - CacheManager：缓存
    - 领域（Realm）：Shiro框架与应用种的安全数据之间的桥梁（连接器），即向数据库拿取安全数据以提供安全验证。

# 二、依赖引入

- Shiro依赖：
```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-core</artifactId>
  <version></version>
</dependency>
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-web</artifactId>
  <version></version>
</dependency>
```
- Spring整合Shiro依赖：（包含Shiro依赖）

```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-spring</artifactId>
  <version></version>
</dependency>
```


# 三、整合Shiro

- 具体配置细节可以参考官方文档

    - http://shiro.apache.org/spring-xml.html

    - http://shiro.apache.org/spring-boot.html


## 1、在web.xml中配置shiro过滤器
```xml
<!-- 配置shiro过滤器: 注意:filter-name必须要和ShiroFilterFactoryBean的id一致 -->
<filter>
  <filter-name>shiroFilter</filter-name>
  <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
  <init-param>
      <param-name>targetFilterLifecycle</param-name>
      <param-value>true</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>shiroFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```
## 2、在springContext中配置自定义的领域
```xml
<!-- 自定义Realm -->
<bean id="shiroRealm" class="com.tracejp.testshiro.config.ShiroRealm"/>
```

```java
// 编写自定义的Realm并继承AuthorizingRealm抽象类
public class ShiroRealm extends AuthorizingRealm {
  // 重写doGetAuthorizationInfo(PrincipalCollection principals)授权
  // 重写doGetAuthenticationInfo(AuthenticationToken token)认证
}
```
## 3、在springContext中配置shiro安全管理器
```xml
<!-- 配置shiro安全管理器 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
  <!-- 整合自定义域 -->
  <property name="realm" ref="shiroRealm"/>
</bean>
```
## 4、在springContext中配置shiro过滤器
```xml
<!-- 配置shiro过滤器: 注意-id必须要和web.xml中的shiro过滤器filter-name一致 -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
  <!-- 整合shiro安全管理器 -->
  <property name="securityManager" ref="securityManager"/>

  <!-- 程序登录页面:即shiro在认证失败时的重定向页面 -->
  <property name="loginUrl" value="/login.jsp"/>

  <!-- 登录成功页面:即shiro在认证成功后重定向的页面 -->
  <property name="successUrl" value="/success.jsp"/>

  <!-- 未经授权的页面:即shiro在权限检查后不够权限跳转的页面 -->
  <property name="unauthorizedUrl" value="/unauthorized.jsp"/>

  <!-- 配置需要受保护的页面,以及访问这些页面的权限 -->
      <!-- anon可以被匿名访问 / authc必须被认证才可以访问 -->
      <!-- 可以使用#行注释; 页面url映射到web工程下的/; 支持通配符 -->
  <property name="filterChainDefinitions">
      <value>
        /private.jsp = authc
        /index.jsp = anon
      </value>
  </property>
</bean>
```
## 5、HelloWorld

- 启动tomcat后，访问private.jsp页面将自动重定向到login.jsp页面，这是因为当前未经过认证，所以为匿名用户，无法访问已经配置的保护页面。

- 执行认证后即可访问private.jsp页面


## 6、简单认证
```java
@GetMapping("/rz")
@ResponseBody
public String certification(String name, String pwd) {

  // 1 将用户名密码封装成shiro提供的pojo
      // 参数：用户名，密码，是否记住用户，登录位置
  UsernamePasswordToken token = new UsernamePasswordToken(name, pwd);

  // 2 获取Subject
  Subject subject = SecurityUtils.getSubject();

  // 3 执行登录操作
  try {
      subject.login(token);
  } catch (UnknownAccountException e) {
      return "不存在用户名";
  } catch (IncorrectCredentialsException e) {
      return "密码错误";
  } catch (LockedAccountException e) {
      return "用户被锁定";
  } catch (ExcessiveAttemptsException e) {
      return "用户登录次数过多，并且可能被永久锁定";
  } catch (AuthenticationException e) {
      return "所有异常的父类";
  }
  return "登录成功";
}
```
## 7、简单清除认证
```java
@GetMapping("/out")
public String logout() {

  // 1 获取subject
  Subject subject = SecurityUtils.getSubject();

  // 2 执行登出操作
  subject.logout();

  // 3 重定向页面到首页以清理所有cookie
  return "forward:/index/jsp";
}
```





