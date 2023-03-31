- 将sh开启服务注册到systemctl中

```properties
\# 在system目录下新建文件
\# vim /usr/lib/systemd/system/<调用的服务名>.service

[Unit]
Description=<服务名> service
After=network.target

[Service]
Type=forking
WorkingDirectory=<工作目录>
ExecStart=<启动路径> start
ExecStop=<启动路径> stop
User=root
Group=root

[Install]
WantedBy=multi-user.target
```

 