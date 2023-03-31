## 一、Dcoker File

### 1、概述

- Dockerfile是用来构建Docker镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。类似Linux下的Shell脚本。通过脚本构建属于自己的新镜像。

- Dockerfile官方文档：https://docs.docker.com/engine/reference/builder/


### 2、脚本编写基本原则

- 每条保留字指令都必须为大写字母且后面要跟随至少一个参数。

- 指令按照从上到下，顺序执行。

- #表示注释。
- 每条指令都会创建一个新的镜像层并对镜像进行提交。即执行一条指令，则进行一次容器的commit操作。


### 3、Docker执行Dockerfile的流程

（1）docker从基础镜像运行一个容器

（2）执行一条指令并对容器作出修改

（3）执行类似docker commit的操作提交一个新的镜像层

（4）docker再基于刚提交的镜像运行一个新容器

（5）执行dockerfile中的下一条指令直到所有指令都执行完成

![clipboard.png](Docker%20File%E8%AF%A6%E8%A7%A3.assets/clip_image002.gif)

 

## 二、保留字指令

### 1、FROM

- 基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，必须在文件代码的第一行。


### 2、MAINTAINER

- 镜像维护者的姓名和邮箱


### 3、RUN

- 构建容器时需要运行的命令，RUN是在docker build时运行。

- 一共有两种格式

- shell格式

```shell
RUN <命令行命令>  # <命令行命令>等同于，在终端操作的shell命令
```
- n exec格式：

```shell
RUN ["可执行文件", "参数1", "参数2" ,...]
# eg. RUN ["./test.php", "dev", "offline"] 等价于 RUN ./text.php dev offline
```
### 4、EXPOSE

- 当前容器对外暴露的端口


### 5、WORKDIR

- 进入容器后，终端默认登录的进来的工作目录，即默认的容器目录落脚点。

- 例如，进入终端后，默认在/usr/local/tomcat目录，而不是根目录。


### 6、USER

- 指定容器权限


### 7、ENV

- 定义一个环境变量，就是类似string a = "abc"。这个环境变皇可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；也可以在其它指令中直接使用这些环境变量。

```dockerfile
ENV <变量名> <变量值>  # 使用 $<变量名> 引用
```
### 8、ADD

- 将宿主机目录下的文件拷贝进镜像，且会自动处理URL和解压tar压缩包


### 9、COPY

- 类似ADD，拷贝文件和目录到镜像中。

- 将从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置

    - 注：目标路径无需提前建好。

```dockerfile
COPY <宿主机源路径> <容器内目标路径>
```
### **10****、VOLUME**

- 指定容器卷挂载


### 11、CMD

- 指定容器启动后要干的事情。同RUN命令两种使用格式一致。

- 区别：Dockerfile 中可以有多个CMD指令，但只有最后一个CMD生效，CMD会被docker run命令之后的参数替换。

    - RUN命令是在docker build的时候运行的

    - CMD是在docker run的时候运行的

- 如，docker run -it /bin/bash。则Dockerfile的CMD命令被替换成CMD /bin/bash


### 12、ENTRYPOINT

- 也是用来指定一个容器启动时要运行的命令。类似于CMD指令。

- 但是ENTRYPOINT不会被docker run后面的命令覆盖，而且这些命令行参数会被当作参数送给ENTRYPOINT指令指定的程序。

```dockerfile
ENTRYPOINT ["可执行文件", "参数1", "参数2" ,...]
```
- 注意：ENTRYPOINT可以和CMD一起用。当指定了ENTRYPOINT后，CMD的含义就发生了变化，不再是直接运行其命令而是将CMD的内容作为参数传递给ENTRYPOINT指令。


```dockerfile
ENTRYPOINT ["nginx", "-c"]  # 定参

CMD ["etc/nginx/nginx.conf"]  # 变参，作为参数加到ENTRYPOINT后
# 最后执行结果为 nginx -c /etc/nginx/nginx.conf
```
## 三、Docker File构建运行

### 1、编写Dockerfile

- Dockerfile文件注意D为大写。官方规范。

- 新建Dockerfile文件

```shell
vim Dockerfile
```
- 模板（以安装Java8为例）

```dockerfile
FROM centos
MAINTAINER 作者名<邮箱地址@126.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH

# 安装vim编辑器
RUN yum -y install vim

# 安装ifconfig命令查看网络IP
RUN yum -y install net- tools

# 安装java8及lib库
RUN yum -y install glibt.i686
RUN mkdir /usr/local/java

# ADD是相对路径jar，把宿主机jdk-8u171-linux-x64.tar.gz添加到容器中
# 安装包必须要和Dockerfile文件在同一位置
ADD jdk-8u171-linux-x64.tar.gz /usr/local/java/

#配置java环境变量
ENV JAVA_HOME /usr/local/java/jdk1.8.0_171
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
ENV PATH $JAVA_HOME/bin:$PATH

EXPOSE 80
CMD echo $MYPATH
CMD echo "success----------ok"
CMD /bin/bash
```
### 2、构建Dockerfile

- 执行docker build命令，即可将编写好的dockerfile文件构建成一个自定义的新的镜像。

```shell
docker build -t <新镜像名>:<TAG版本> .  # *注意tag后面有一个空格和一个点
# eg. docker build -t mycentos:2.3 .
```



