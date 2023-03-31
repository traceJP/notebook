**依赖信息查询网站：**[**http://mvnrepository.com/**](http://mvnrepository.com/)

# 一、Maven的由来：

1、目前开发中存在的问题：

- 一个项目就是一个工程（模块）

    -  而当项目越来越大，就不适合用package划分模块。最好是将一个项目分化成多个模块，一个模块对应一个工程，然后每个工程互相联系。

- 项目中需要的jar包必须手动复制黏贴到lib目录下并导入依赖

    -  同样的jar包出现在不同的项目工程中，浪费储存空间且让工程臃肿。

- jar包必须要去官网上下载

    -  下载的jar包各式各样，各种版本

 

2、Maven简介：

- Maven是一款服务于Java平台的**自动化构建工具**。

- 构建：

    -  概念：以“Java源文件”、“框架配置文件”、“"JSP”、“HTML”、“图片”等资源为“原材料”，去“生产”—个可以运行的项目的过程。

    -  编译、部署、搭建

- 构建过程中的各个环节：

    -  [1]清理：将以前编译得到的旧的class字节码文件删除，为下一次编译做准备

    -  [2]编译：将Java源程序编程或class字节码文件

    -  [3]测试：自动测试，自动调用junit程序

    -  [4]报告：测试程序执行的结果

    -  [5]打包：动态Web工程 n打war包，Java工程打jar包

    -  [6]安装：Maven特定的概念——将打包得到的文件复制到“仓库”中的指定位置

    -  [7]部署：将动态Web工程生成的war包复制到Servlet容器的指定目录下，使其可以运行


3、Maven安装：

- 注意：需要JDK才可以安装

- 在官网上下载maven并解压

http://maven.apache.org/download.cgi

// 下载后的文件目录中带有bin、boot、conf、lib等

- 配置Maven环境变量

- 1 新建一个Maven系统变量，变量值指向Maven解压文件目录

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

- 2 在Path变量中新增一个Maven值：该值指向解压Maven目录的bin目录中

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

- 3 验证Maven是否成功配置完毕
```shell
cmd > mvn -v
```
出现以下字样即表示安装成功
```shell
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)

Maven home: D:\Maven\apache-maven-3.6.3\bin\..

Java version: 1.8.0_241, vendor: Oracle Corporation, runtime: D:\JDK\JDK1.8\jre

Default locale: zh_CN, platform encoding: GBK

OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

# 二、Maven

- Maven的核心概念：

    -  约定的目录结构、POM、坐标、依赖、仓库、生命周期/插件/目标、继承、聚合


1、Maven常用命令：

- 执行命令原理：

    -  Maven的核心程序中仅仅定义了抽象的生命周期，但是具体的工作必须由特定的插件来完成。而插件本身并不包含在Maven的核心程序中。

    -  当我们执行的Maven命令需要用到某些插件时，Maven核心程序会首先到本地仓库中查找。

    - 本地仓库的默认位置︰[系统中当前用户的家目录]\.m2\repository

    - Maven核心程序如果在本地仓库中找不到需要的插件，那么它会自动连接外网，到中央仓库下载。

    -  如果此时无法连接外网，则构建失败。
```shell
// 命令需要进入到pom.xml目录中输入

mvn clean  // 清理

mvn compile  // 编译主程序

mvn test-compile  // 编译测试程序

mvn test  // 执行测试

mvn package  // 打包

mvn instal-  // 安装

mvn site  // 生成站点

mvn deploy  // 部署
```

1.1、生命周期：

（1）概念：

- 生命周期在Maven中指各个构建环节执行的顺序，如先编译才能打包，之后才能允许。不能打乱顺序，必须按照既定的正确顺序来执行。

- Maven的核心程序中定义了抽象的生命周期，生命周期中各个阶段的具体任务是由插件来完成的。

- Maven核心程序为了更好的实现自动化构建，按照这一的特点执行生命周期中的各个阶段︰不论现在要执行生命周期中的哪一个阶段，都是从这个生命周期最初的位置开始执行。

（2）三套独立的Maven生命周期以及对应执行的顺序：

- Clean Lifecycle：在进行真正的构建之前进行一些清理工作。

- pre-clean  // 执行一些需要在clean之前完成的工作

- clean  // 移除所有上一次构建生成的文件·

- post-clean  // 执行一些需要在clean之后立刻完成的工作。

- Default Lifecycle：构建的核心部分，编译，测试，打包，安装，部署等等。
```shell
validate

generate-sources

process-sources

generate-resources

process-resources  // 复制并处理资源文件，至目标目录，准备打包

compile  // 编译项目的源代码

process-classes

generate-test-sources

process-test-sources

generate-test-resources

process-test-resources  // 复制并处理资源文件，至目标测试目录

test-compile  // 编译测试源代码

process-test-classese

test  // 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署

prepare-package

package  // 接受编译好的代码，打包成可发布的格式，如 JAR

pre-integration-test

integration-test

post-integration-test

verify

install  // 将包安装至本地仓库，以让其它项目依赖

deploy  // 将最终的包复制到远程的仓库，以让其它开发人员与项目共享或部署到服务器上运行
```
-  deploy自动部署：
```shell
<build>  // 通过配置build标签进行指定自动部署的配置
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <configuration>
        <source>8</source>
        <target>8</target>
      </configuration>
    </plugin>
  </plugins>
</build>
```
- Site Lifecycle：生成项目报告，站点，发布站点。
    - pre-site  // 执行一些需要在生成站点文档之前完成的工作
    - site  // 生成项目的站点文档
    - post-site  // 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
    - site-deploy  // 将生成的站点文档部署到特定的服务器上

1.2、插件与目标：

- 生命周期的各个阶段仅仅定义了要执行的任务是什么

- 各个阶段和插件的目标是对应的。

- 相似的目标由特定的插件来完成。

 

2、约定的目录结构：

- 新建一个Maven工程必须要指定的Package结构

    -  Maven要负责我们这个项目的自动化构建，以编译为例，Maven要想自动进行编译，那么它必须知道Java源文件保存在哪里。

工程名（模块名）
```shell
|---src

|---|---main

|---|---|---java

|---|---|---resources

|---|---test

|---|---|---java

|---|---|---resources

|---pom.xml
```

- 根目录：工程名

- src目录：源码

- main目录：存放主程序

- test目录：存放测试程序

- java目录：存放Java源文件

- resources目录：存放框架或其他工具的配置文件

- pom.xml文件：Maven工程的核心配置文件




3、POM：

- 含义：（Project Object Model）项目对象模型

- pom.xml对于Maven工程是核心配置文件，与构建过程相关的一切设置都在这个文件中进行配置。其重要程度相当于web.xml对于动态工程的配置。

 

4、坐标：

- 数学上的坐标：在空间中使用XYZ三个向量可以唯一的定位到空间中任意一个点

- Maven上的坐标：

    -  使用三个向量在仓库中唯一定位一个Maven工程（GAV）
```shell
// g 公司或组织域名倒序+项目名

  // 如com.baidu.maven，com.baidu为域名www.baidu.com,maven为项目工程名

<groupId></groupId>

// a 模块名

<artifactId></artifactId>

// v 版本

<version></version>

- 定位路径：

C:\Users\15250\.m2\repository\

<groupId>\<artifactId>\<version>\<artifactId>-<version>.jar
```


5、仓库：

- [1]本地仓库：为当前本机上的所有Maven工程服务。

- [2]远程仓库：

    -  私服：架设在当前局域网环境下，为局域网范围内所有的Maven工程使用

        - Nexus是架设Maven私服的一个产品

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image006.gif)

- 中央仓库：架设在Internet上，为全世界所有Maven工程服务

- 中央仓库镜像：分担中央仓库流量，提升访问速度


- 仓库中保存的内容：

    -  Maven自身所需要的插件

    -  第三方框架或工具的jar包

    -  我们自己开发的Maven工程


6、依赖：

（1）依赖代码：
```shell
pom.xml:

<dependencies>
  <dependency>
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <scope>...</scope>
  </dependency>
  <dependency>...
</dependencies>
```

- Maven解析依赖信息时会到本地仓库中查找被依赖的jar包。

    -  对于我们自己开发的Maven工程，使用install命令安装后就可以进入仓库。|


（2）依赖的范围：
```shell
<scope></scope>  // scope 范围
```
- scope取值：

[1]compile范围依赖

  对主程序是否有效：有效

  对测试程序是否有效：有效

  是否参与打包：参与

[2]provided范围依赖

  对主程序是否有效：有效

  对测试程序是否有效：有效

  是否参与打包：不参与  // 不参与部署

[3]test范围依赖

  对主程序是否有效：无效

  对测试程序是否有效：有效

  是否参与打包：不参与

- compile：

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image008.gif)

- provided：

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image010.gif)

 

（3）依赖的传递：

- 概念：

    -  假设现在有两个工程：A、B；

    -  A工程->依赖Spring框架

    -  B工程->依赖A工程

    -  那么：当B工程添加依赖A工程的同时，也会自动添加Spring框架的依赖

    -  当A工程的安装变更时，B工程也会同时变更其对应的依赖

- 好处：可以避免代码冗余，只需添加一次依赖，不必再每个模块中重复声明。

- 注意：只有compile范围的依赖才可以传递。而其他范围的依赖需要自己手动重复添加。


（4）依赖的排除：

- 概念：

    -  假设现在有两个工程：A、B；

    -  A工程->依赖Spring框架

    -  B工程->依赖A工程

    -  那么：现在假设B工程依赖A工程后，但不想要同时依赖Spring框架（该框架为依赖传递过来的）。

    -  此时可以通过在B工程中配置排除这个Spring框架
```shell
// 该标记需要放在<dependency>标记中，以确定是排除谁带来的传递性依赖
<exclusions>
  <exclusion>
    <groupId>...</groupId>
    <artifactId>...</artifactId>
  </exclusion>
  <exclusion>...
</exclusions>
```

-  假设此时有一个工程C；他依赖工程B；

-  那么：此时Spring框架是在B工程中被排除的，所以C工程不会有Spring框架的传递。


（5）依赖原则：

- 就近优先原则：

    -  假设Hello依赖log4j.1.2.17；HelloFriend依赖log4j.1.2.14和Hello；MakeFriends依赖HelloFriend；

    -  那么此时根据就近优先原则：MakeFriends到log4j.1.2.14的路径为2个箭头，到log4j.1.2.17的路径为3个箭头。所以此时MakeFriends被传递的依赖将选择log4j.1.2.14。

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image012.gif)

-  那么此时我就需要用log4j.1.2.17：解决方案：直接在MakeFriends中声明使用log4j.1.2.17即可。

- 先声明者优先：

    -  假设此时两个不同版本的jar包路径都相同时

![clipboard.png](Maven%E5%9F%BA%E7%A1%80.assets/clip_image014.gif)

-  先声明：指dependency标签的声明顺序


（6）统一管理依赖版本号：

- 概念：

    -  假设此时项目依赖了Spring框架全家桶-4.0.0版本

    -  那么：现在我想将其全部修改为5.0.0版本

    -  此时：手动逐一修改并不可靠

- 使用properties标签内使用自定义标签声明版本号，在需要统一版本的位置，使用${自定义标签名}引用声明的版本号

- 该properties标签可以声明任意的配置数据，凡是需要声明引用的场合都可以使用，相当与一个键值对。

```shell
声明：
<properties>
  <自定义标签名>版本号</自定义标签名>
</properties>

使用：
<dependency>
  ...
  <version>${自定义标签名}</version>
  ...
</dependency>
```


7、继承：

- 概念：

    -  假设此时有两个工程A、B；A工程依赖junit4.0；B工程依赖junit4.9；

    -  由于test范围的依赖不能传递，所以很容易会造成junit的版本不一致。

    -  那么现在：需要统一管理各个模块工程中对junit依赖的版本

- 解决：将junit依赖统一提取到“父”工程中，在子工程中声明junit依赖时不指定版本，以父工程中统—设定的为准。

- 步骤：

    -  创建一个Maven工程作为父工程

- 注意：打包方式为pom <packaging>pom</packaging>

    -  在子工程中声明对父工程的依赖：认爹
```shell
<parent>

  <groupId>...</groupId>

  <artifactId>...</artifactId>

  <version>...</version>

  <!-- 以当前文件为基准的父工程pom.xml文件的相对路径 -->

  <relativePath>当前工程相对于父工程的相对路径</relativePath>

</parent>
```
- 将子工程的坐标中与父工程坐标中重复的内容删除

   - 父工程与子工程有对其他工程有相同的依赖

   - 在父工程中统一junit的依赖
```shell
<dependencyManagement>  // 统一管理标签
  <dependencies>
    <dependency>
      <groupId>...</groupId>
      <artifactId>...</artifactId>
      <version>...</version>
      <scope>...</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```
-  在子工程中删除junit依赖的版本号部分

- 删除子工程的dependency依赖即可，如果保留依赖，填写不同的版本号即可脱离对父工程的管理

    - 注意：做了继承后，需要先安装父工程，才能安装子工程。

8、聚合：

- 作用：一键安装各个模块工程。用于继承后直接通过安装父工程一键安装所有子工程。

- 配置方式：在一个“总的聚合工程”中配置各个参与聚合的模块。
```shell
<modules>

  <module>子工程相对路径</module>

  ...

</modules>
```
- 使用：在聚合工程的pom.xml中执行安装生命周期即可。

