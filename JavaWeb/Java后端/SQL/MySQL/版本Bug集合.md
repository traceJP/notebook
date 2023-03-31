### 一、5.X版本权限问题：（2021.7.4）

- 此版本下linux服务对于用户密码设置需要符合mysql制定的标准。

- 密码标准查看：

```mysql
SHOW VARIABLES LIKE 'validate_password%';
```
- 使用全局变量可修改密码标准：

    - 修改密码标准后重新进入命令提示符窗口或使用FLUSH PRIVILEGES;刷新。

```mysql
set global <Variable_name>=<value>;
```
- 在linux下此版本mysql无法被外部访问，只允许localhost。需要手动向账号添加ip地址的授权进行访问。注意此密码也需要符合mysql密码标准。

```mysql
GRANT ALL PRIVILEGES ON *.*
TO <用户名>@"<ip:port> | <% 百分号通配符无ip限制>"
IDENTIFIED BY "<用户密码>"
WITH GRANT OPTION;
```





