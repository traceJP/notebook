# 一、Subjects详解

## 1、Subject概述

- Subject（主题 / 用户），Subject表示单个应用程序用户的状态和安全操作。 这些操作包括身份验证（登录/注销），授权（访问控制）和会话访问。 它是Shiro的单用户安全功能的主要机制；Subject实例都绑定到（并要求）SecurityManager。当您与Subject互动时，这些互动会转化为与主题相关的互动SecurityManager。

## 2、Subject获取

- 为了获得当前正在执行的Subject ，应用程序开发人员几乎总是使用SecurityUtils.getSubject()，几乎所有安全操作都应使用此方法返回的Subject进行。

## 3、基本API

- 为此主题执行认证尝试
    - 如果不成功，则抛出异常，异常的子类有具体的原因；如果成功，则与提交的委托人/凭证关联的帐户数据将与此Subject关联。
```java
void login(AuthenticationToken token) throws AuthenticationException;
```
- 查看当前主题是否通过认证尝试，通过了则返回true

    - 注意：使用了记住我功能通过的认证结果依旧返回false

```java
boolean isAuthenticated();
```
- 查看当前主题是否通过“记住我功能”认证，通过则返回true

```java
boolean isRemembered();
```
- 返回与此主题关联的应用程序Session。 如果在调用此方法时不存在会话，则将创建一个新的会话，并与此主题相关联，然后返回该会话。

```java
Session getSession();
// 注意：这不是HttpSession，但拥有和HttpSession几乎一样的功能
// 并且和HttpSession有一定的联系，如共享域空间等。
// 这是shiro提供的企业级的会话管理功能。
```
- 获取当前主题的唯一标识，如果当前主题为匿名的，则返回null

```java
Object getPrincipal();
PrincipalCollection getPrincipals();
```
- 查看当前主题是否拥有某个权限，存在权限则返回true

```java
boolean isPermitted(String permission);
// ...其他重载方法包括传入对象参数，传入可变形参，传入list参数等
```
- 保证当前主题拥有某个权限，如果没有对应权限则抛出异常

```java
void checkPermission(String permission) throws AuthorizationException;
// ...其他重载方法包括传入对象参数，传入可变形参，传入list参数等
```
- 查看当前主题是否拥有某个角色，存在角色则返回true

```java
boolean hasRole(String roleIdentifier);
// ...其他重载方法包括传入对象参数，传入可变形参，传入list参数等
```
- 保证当前主题拥有某个角色，如果没有则抛出异常

```java
void checkRole(String roleIdentifier) throws AuthorizationException;
// ...其他重载方法包括传入对象参数，传入可变形参，传入list参数等
```


# 二、realm详解

## 1、realm概述

- 领域充当Shiro与应用程序的安全数据之间的“桥梁”或“连接器”。当真正需要与安全性相关的数据（例如用户帐户）进行交互以执行身份验证（登录）（与Subject对象交互）和授权（访问控制）时，Shiro会从一个或多个为应用程序配置的领域中查找许多此类内容。


## 2、自定义realm继承树

- 对于认证：shiro提供了AuthenticatingRealm抽象类，自定义realm继承该类并重写认证doGetAuthenticationInfo方法即可。

- 对于授权：shiro提供了AuthorizingRealm抽象类，该抽象类继承了AuthenticatingRealm类，自定义realm继承该类并重写认证和授权两个doGetAuthenticationInfo方法即可。


# 三、shiro过滤器

## 1、shiro过滤器概述

- shiro为WEB应用的前端页面提供了认证和授权内的过滤器。通过此过滤器配置则可以实现页面之间权限重定向问题。

- 配置相关可以参考shiro基础笔记。

- shiro所有过滤器的枚举类

```java
// 通过此枚举类可以查看到对应拦截器的实现
org.apache.shiro.web.filter.mgt.DefaultFilter
```
## 2、认证相关过滤器

- **anon**
    - 匿名拦截器，无需执行任何类型的安全性检查即可直接访问路径的拦截器。

```xml
/index.html = anon
```
- **authc**
    - 要求对请求用户进行身份验证以使请求继续，如果未通过，则通过将用户重定向到您配置的loginUrl来强制用户登录。

    - 此过滤器构造一个UsernamePasswordToken ，当其中的isLoginSubmission为true时，将自定调用Subject.login(usernamePasswordToken) 进行自动登录。

```xml
/index.html = authc
```
- **authcBasic**
    - 要求对请求用户进行authenticated以使请求继续，否则，要求用户通过特定于HTTP Basic协议的质询登录。 成功登录后，允许他们继续访问请求的资源/ URL。

```xml
/index.html = authcBasic
```
- **authcBearer**
    - 要求对请求用户进行authenticated以使请求继续，如果未通过，则要求用户通过特定于HTTP Bearer协议的质询登录。 成功登录后，允许他们继续访问请求的资源/ URL。

```xml
/index.html = authcBearer
```
- **logout**
    - 简单过滤器，在接收到请求后，将立即注销当前正在执行的subject ，然后将其重定向到已配置的redirectUrl 

```xml
/index.html = logout
```
- **user**
    - 如果访问者是已知用户（定义为具有已知主题），则允许访问资源的过滤器。 这意味着将允许通过“记住我”功能进行身份验证或记住的任何用户访问此过滤器。如果访问者不是已知用户，那么他们将被重定向到loginUrl

```xml
/index.html = user
```
## 3、授权相关过滤器

- **roles**
    - 如果当前用户具有映射值指定的角色，则允许访问的过滤器；如果用户没有指定所有角色，则拒绝访问的过滤器。

```xml
/index.html = roles[<角色字符串>]
```
- **perms**
    - 如果当前用户具有映射值指定的权限，则允许访问的过滤器；如果用户没有指定所有权限，则拒绝访问的过滤器。

```xml
/index.html = perms[<权限字符串>]
```
- **rest**
    - 用于对rest请求风格进行权限过滤。会根据请求方法构建相应的权限字符串。

        - get -> read

        - post -> create

        - put -> update

        - delete -> delete

```xml
/users=rest[user]
// 假设此时有个put请求，则将需要"user:update"权限匹配
```
- **ssl**
    - SSL过滤器，只有请求协议是HTTPS才能通过，否则自动跳转到https端口（443）

- **invalidRequest**
    - 阻止恶意请求的请求过滤器。 无效的请求将以400响应码进行响应。如果在请求URI中找到以下字符，此过滤器将检查并阻止请求：

        - 分号-可以通过设置blockSemicolon = false来禁用

        - 反斜杠-可以通过设置blockBackslash = false来禁用

        - 非ASCII字符-可以通过设置blockNonAscii = false禁用，禁用此检查的功能将在以后的版本中删除。


## 4、其他相关过滤器

- **port**
    - 要求请求位于特定端口上的过滤器，如果没有，则重定向到该端口上的相同URL。

- **noSessionCreation**
    - 不创建会话拦截器。


## 5、具体权限配置

- 参考官网了解shiro权限http://shiro.apache.org/permissions.html


 

 


