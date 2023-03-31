- MyBatis框架官方文档 https://mybatis.org/mybatis-3/zh/index.html 支持中文

# 一、MyBatis框架概念

## （1）JDBC工具与框架的差距

- JDBC：

![clipboard.png](MyBatis%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

 

- MyBatis：（**半自动**框架**，轻量级**）

![clipboard.png](MyBatis%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

## （2）MyBatis依赖

- 需要引入mybatis-版本.jar和引入mysql数据库驱动jar包即可

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.6</version>
</dependency>

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.22</version>
</dependency>
```


## （2.1）MyBatis-IDEA插件
```
Free MyBatis plugin  // 标记名提示，绑定跳转功能
```


## （3）HelloMyBatis

### 1、编写mybatis-config.xml全局配置文件

- mybatis-config.xml：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
 <environments default="development">
  <environment id="development">
   <transactionManager type="JDBC"/>
      
   <dataSource type="POOLED">  <!-- 数据源 -->
     <!-- 值替换${}即可 -->
     <property name="driver" value="${driver}"/>  <!-- 驱动 -->
     <property name="url" value="${url}"/>    <!-- 协议 -->
     <property name="username" value="${username}"/>    <!-- 数据库用户名 -->
     <property name="password" value="${password}"/>  <!-- 数据库密码 -->
   </dataSource>
  </environment>
 </environments>
    
 <mappers>
  <!-- 注册sql映射文件：mapper-resource属性值映射在resources资源目录下 --> 
  <mapper resource="<EmployeeMapper.xml映射Sql语句配置文件路径>"/>
 </mappers>
</configuration>
```

### 2、编写EmployeeMapper.xml映射Sql语句配置文件

- EmployeeMapper.xml：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace名称空间：可以自定义取值 -->
<mapper namespace="org.mybatis.example.BlogMapper"> 
 <!-- id：唯一标识；resultType：返回值类型；#{id}：从传递过来的参数中取出id值 -->   
 <select id="<唯一标识ID>" resultType="<返回的记录类型-JavaBean>">
  select * from Blog where id = #{id}  <!-- select-sql语句 -->
 </select>
</mapper>
```


### 3、编写HelloMyBatis
```java
public static void main(String[] args) throws IOException {

  // 1 根据xml配置文件创建一个SqlSessionFactory对象：Factory工厂
  String resource = "<全局变量配置文件>";
  InputStream inputStream = Resources.getResourceAsStream(resource);
  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

  // 2 通过SqlSessionFactory制作一个SqlSession实例
  SqlSession openSession = sqlSessionFactory.openSession();

  // 3 执行一个已经映射的sql语句
     // 参数s：sql的唯一标识；参数o：传入的参数
     // 注意：JavaBean中的每个键需要与sql语句中查询出来的键相同，不同可以使用sql-as设置别名 
     //   #{id}为传入的参数，类型为Object
  JavaBean javaBean = openSession.selectOne("<名称空间>.<唯一标识ID>", #{id});

  // 4 操作数据javaBean
  System.out.println(javaBean);

  // 5 关闭Session
  openSession.close();  // 需要写在异常捕获中
}
```


## （4）MyBatis接口式编程（推荐使用）

- 概念：MyBatis中sql映射文件可以与一个Java接口进行绑定，通过Java接口中的方法可以准确的制定sql中的返回值、传入参数类型和参数值；

- 优势：可以避免sql的直接映射导致传入的参数类型不一致，如selectOne中传入的参数类型为Object类型。

- 步骤：


### 1、制作一个Java接口，在其中制作相应的方法：
```java
public interface MyBatisInterface {
  // 返回UserJavaBean类型，传入一个Integer类型的id参数
  UserJavaBean selectMyBatis(Integer id);
}
```
### 2、在sql映射.xml文件中绑定接口
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
     PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
     "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 注意：此时namespace命名空间需要填写接口名 -->    
<mapper namespace="com.TraceJP.TestMybatis.MyBatisInterface">
  <!-- 注意：此时id唯一标识需要填写接口中的方法名，以绑定对应的方法 -->   
  <select id="selectMyBatis" resultType="com.TraceJP.TestMybatis.UserJavaBean">
     SELECT uid,username as userName,email FROM `user` WHERE uid = #{id};
  </select>
</mapper>
```
### 3、在MyBatis全局配置文件中注册sql映射文件
```xml
<mapper resource="MyBatisInterfaceMapper.xml"/>
```
### 4、编写测试用例
```java
String resource = "MyBatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
SqlSession openSession = sqlSessionFactory.openSession();

// 1 通过反射获取接口的实现类对象
MyBatisInterface myBatisInterface = openSession.getMapper(MyBatisInterface.class);

// 2 调用接口对象方法，并传入参数，返回得到结果集
UserJavaBean userJavaBean = myBatisInterface.selectMyBatis(<接口方法中指定的参数>);

// 3 操作返回的数据
System.out.println(userJavaBean);

openSession.close();  // 需要写在异常捕获中
```


## （4.1）通过注解直接映射接口

- 在Mybatis中可以通过注解直接绑定接口方法，只需要在全局配置文件中注册此接口即可使用。

- 优势：不需要使用sql映射文件绑定接口即可使用

- 劣势：会造成代码书写不规范


### 1、在接口方法中使用注解绑定
```java
import org.apache.ibatis.annotations.Select;
public interface MyBatisInterface {
  @Select("SELECT uid,username as userName,email FROM `user` WHERE uid = #{id}")
  UserJavaBean selectMyBatis(Integer id);
}
```
### 2、在全局配置文件中注册此接口
```xml
// 注意：此时mapper标签中的resource属性变成了class属性
<mapper class="com.TraceJP.TestMybatis.MyBatisInterface"/>
```
### 3、编写测试用例

- 该测试用例与 MyBatis接口式编程 测试用例一致


 

 

 