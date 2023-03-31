# 一、Git：

- 定义：分布式代码版本控制工具

- 是Linux作者创作的

# 二、Git和svn的对比：

- 集中式(svn)

    - svn因为每次存的都足差异需要的硬盘空间会相对的小一点可是回滚的速度会 很慢
    
    - 优点：
        - 代码存放在单一的服务器上便于F项目的管理
    
    
    - 缺点：
    
        - 服务器宕机：员工写的代码得不到保障
    
    
        - 服务器炸了：整个项目的历史记录都会丢失
    

- 分布式(git)
    - git每次存的都是项目的完整快照需要的硬盘空间会相对大一点(Git团队对代码做了极致的压缩最终需要的实际空间比svn多不了太多可是Git的回滚速度极快)
    - 优点：

        - 完全的分布式

    - 缺点：

        - 学习难度比svn高



- 本地仓库和远程仓库形成的闭环：（面向区域编程）

![clipboard.png](Git%E5%9F%BA%E7%A1%80.assets/clip_image002.gif)

- 安装git后出现的变化：

![clipboard.png](Git%E5%9F%BA%E7%A1%80.assets/clip_image004.gif)

- Git图形化管理工具：

![clipboard.png](Git%E5%9F%BA%E7%A1%80.assets/clip_image006.gif)

## Git命令窗口：

- 基于此窗口可以输入Linux命令以及Git命令。

- 与windowsCMD区别：cmd输入的是dos命令，而git窗口输入的是Linux命令。

![clipboard.png](Git%E5%9F%BA%E7%A1%80.assets/clip_image008.gif)

- 环境配置完成后可以使用git命令查看版本信息：
```shell
git --version
```


## git用户配置：

- 定义：git中配置的是个人的用户名和电子邮箱，这两条配置很重要，当每次git提交时都会引用这两条信息，说明是谁提交了更新，并记录下更新的历史。

- 原因：一般在开发过程中，开发团队和测试团队是分开的，当你写的代码出现bug之后，将由测试团队指出，并找到相应的人的邮箱给你指出bug并要求修改

- 配置命令：
```shell
git config --global user.name "<用户名>"
git config --global user.email <邮箱地址>
 
// 其中--global可以替换为 --system 或者 直接不写。如：
git config --system user.name "<用户名>"  // git配置系统级别信息
git config user.name "<用户名>"  // git配置项目级别信息
```

- 优先级： 项目>用户>系统（注意：每一个级别的配置都会覆盖上一级别的配置）

    - 删除配置信息：
```shell
git config --global --unset user.name | user.email
```

- 查看配置后的信息：
    - 查看已有的配置信息，可以使用命令 git config --list 

![clipboard.png](Git%E5%9F%BA%E7%A1%80.assets/clip_image010.gif)

 

 

 