## 一、概述

- Portainer是一款轻量级的应用，它提供了图形化界面，用于方便地管理Docker环境，包括单机环境和集群环境。利用Portainer，几乎可以代替Docker的常规命令，对镜像，容器等进行操作和监控。

- 官网：https://www.portainer.io/


### 1、安装

- 使用Docker镜像直接安装

```shell
docker search 
```
- 启动portainer

```shell
docker run -d -p 8000:8000 -p 9000:9000 \
    --name portainer --restat=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer
```
### 2、配置portainer

- 第一次登录需创建admin，访问地址: xxx.xXx.XXX.xxx:9000

- 设置admin用户和密码后首次登陆

- 选择local选项卡后本地docker详细信息展示


### 3、官网最新端口配置

- 为了访问 UI 和 API，以及 Portainer 服务器实例和 Portainer 代理进行通信，某些端口需要可访问。Portainer 服务器在端口9443上侦听UI 和 API（或在30779 上侦听带有NodePort的 Kubernetes），并在端口8000上公开 TCP 隧道服务器（第二个端口是可选的，仅在使用边缘计算功能和边缘代理时才需要）。Portainer 代理侦听端口9001（对于带有NodePort的 Kubernetes，则为 30778）。


 
