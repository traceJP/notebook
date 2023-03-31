# 一、Linux：
```shell
clear  // 清除屏慕

echo 'testcontent'  // 往控制台输出信息

ll  // 将当前目录下的子文件&子目录平铺在控制台

find 目录名  // 将对应目录下的子孙文件&子孙目录平铺在控制台

find 目录名 -typef  // 将对应目录下的文件平铺在控制台

echo '文件内容' > 文件名.文件类型  // 创建文件,即将控制台输出的信息输入到文件中

rm 文件名  // 删除文件

mv 源文件 重命名文件  // 重命名

cat 文件的url  // 查看对应文件的内容

exit  // 退出git命令界面

\------

vim 文件的url(在英文模式下)

  按i进插入模式进行文件的编辑

  按esc键&按:键 进行命令的执行  // 先进入vim命令行的执行，在操作以下命令

    q!  // 强制退出(不保存)

    wq  // 保存退出

    set nu  // 设置行号
```

# 二、Git低层命令：
```shell
git对象：

  git hash-object -w <文件路径>  // 生成一个key(hash值):val(压缩后的文件内容)键值对存到git/objects

tree对象：

  git update-index --add --cacheinfo <文件类型编号> <git哈希值> <对象名>  // 往暂存区添加一条记录(让git对象 对应 上文件名)存到.git/index

  git write-tree  // 生成树对象存到.git/objects

commit对象：

  echo 'first commit' / git commit-tree <tree哈希值>  // 生成一个提交对 象存到git/objects

对以上对象的查询：

   git cat-file -p <哈希值>  // 显示对应对象的内容

  git cat-file -t <哈希值>  // 显示对应对象的类型

$$$ 查看暂存区：

  git ls-files -s

reset对象：

  git reset --soft <HEAD~ or 提交对象哈希>  // 将head指针以及head所指向的分支一起移动到上一个或其他的提交对象上

  git reset [--mixed] HEAD~  // 将head和分支移动到其他提交对象，且暂存区也变动到其他提交对象

  git reset --hard HEAD~  // 将head和分支移动到其他提交对象，且暂存区和工作目录也变动到其他提交对象

  git reset [--mixed] HEAD <文件名>  // 工作区目录覆盖暂存区

  git reset <文件名>  // 默认用head指向的提交对象覆盖暂存区
```

# 三、Git高层命令（配置）：
```shell
git --version  // 查看git版本信息

git config --global user.name "<用户名>"  // 配置用户级用户名

git config --global user.email <邮箱地址>  // 配置用户级邮箱地址

git config --global user.name|user.email  // 删除已有的name或者email配置信息

git config --list  // 查看已有的配置信息
```

# 四、Git高层命令（CRUD）：
```shell
git init  // 初始化仓库

git status  // 查看文件的状态

git diff  // 查看哪些修改还没有暂存

git diff --cached（staged）  // 查看哪些修改以及被暂存了但还没提交

git add <文件URL or ./>  // 将修改添加到暂存区（跟踪文件）

git rm <文件名>  // 删除工作区文件并将修改自动添加到暂存区

git mv <原文件名> <新文件名>  // 将工作区目录重命名，再将修改添加到暂存区

git log  // 查看历史记录（当前单独分支的日志）

git reflog  // 查看所有操作的日志（所有分支的日志，只要是命令就会被记录） （按q键退出）

git commit  // 进入vim编辑注释信息：用于书写多段注释信息，退出vim后自动提交到版本库

git commit -m "<注释信息>"  // 书写一段注释并将暂存区提交到版本库

git commit -a  // 自动将所有已跟踪的文件暂存起来一并提交（从而跳过add步骤）

git commit -a -m "<注释信息>"  // 上述功能整合
```

# 五、Git高层命令（分支）：
```shell
git branch  // 显示分支列表

git branch <分支名>  // 创建分支

git branch -d <分支名>  // 删除空的以及被合并的分支（当分支未被合并的时候必须使用大写字母-D进行强制删除）

git branch -v  // 查看每一个分支的最后一次提交对象

git branch <分支名> <提交对象哈希值>  // 新建一个分支并且使分支指向对应的提交对象

git checkout <分支名>  // 切换分支

git checkout -b <分支名>  // 新建一个分支并切换到该分支

git merge <分支名>  // 将<分支名>的分支合并到当前分支下

git log --oneline --decorate --graph --all  // 查看整个项目的所有分支历史

git stash  // 将文件保存在栈中

git stash apply  // 应用栈中文件元素（应用即该元素的复制被取出，栈内并没有被删除）

git stash pop  // 弹栈操作，应用并删除元素

git stash drop stash@{n}   // stash@{n}为栈内元素的名称，具体可以通过查看命令查看名称

git stash list  // 查看存储


**Git****高层命令（撤回）：**

git checkout --<文件名>  // 撤回文件的修改

git reset HEAD <文件名>  // 将文件从暂存区中撤回

git commit --amend  // 重新书写日志，并暂存区的文件重新提交（撤回提交）
```

# 六、Git同步命令（远程仓库）：
```shell
git remote add <远程仓库别名> <url>  // 对远程仓库url取别名

git remote -v  // 显示远程仓库使用的Git别名以及它对应的URL

git push [<远程仓库别名>] [<分支名>]  // 推送本地项目到远程仓库

git clone <克隆url>  // 从github上下载仓库到本地

git fetch <远程仓库别名>  // 获取远程仓库分支数据到远程跟踪分支

\------

git branch -u <远程跟踪分支名> <本地分支名>  // 将本地分支和远程分支建立联系

git checkout -b <本地分支名> <远程跟踪分支名>  // 在新建分支时指定想要跟踪的远程跟踪分支

git checkout --track <远程跟踪分支名>  // 新建一个与远程跟踪分支名相同的本地分支（上个命令简写）

git branch -vv  // 查看设置的所有跟踪分支

git push  // 直接上传数据

git pull  // 直接下载数据

\------

git push origin --delete serverfix  // 删除远程分支

git remote prune origin --dry-run  // 列出仍在远程跟踪但是远程已被删除的无用分支

git remote prune origin  // 清除上面命令列出的远程跟踪分支
```

 

 