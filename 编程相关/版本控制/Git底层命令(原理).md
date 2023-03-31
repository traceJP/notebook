 

# 一、初始化新仓库：

- 定义：要对现有的某个项目开始用git进行管理，需要定位到此项目所在的目录。

- git命令：
```shell
$ git init  // 创建一个仓库
```
- 如图，在一个新建文件夹下输入命令：

![clipboard.png](Git%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4(%E5%8E%9F%E7%90%86).assets/clip_image002.gif)

- 此时该文件下出现隐藏的git文件：

![clipboard.png](Git%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4(%E5%8E%9F%E7%90%86).assets/clip_image004.gif)

## 1、 向git仓库中存入git对象：（git仓库也叫git版本库）

- 向git仓库中存入控制台输入内容:
```shell
echo "test content" | git hash -object -w --stdin
```
    - -w 迭项指示 hash-object 命令存储数据象：若不指定此迭项，则该命令仅返回对应的哈希值
    
    - --stdin (standard input) 选项则指示该命令从标准输入读取内容;若不指定此选项，则须在命令尾部给出待存储文件的路径

- 向git仓库中存入一个文件：
```shell
git hash-object -w <文件路径>  // 存文件

git hash-object <文件路径>  // 返回对应文件的哈希值
````
- 返回：以上命令输出一个长度为40个字符的按验和。这是一个SHA-1 哈希值，如d6704 60b4 b4aece5915caf5c68d12f5 60a9fe3e4

![clipboard.png](Git%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4(%E5%8E%9F%E7%90%86).assets/clip_image006.gif)

- 如图：在隐藏文件git中，头两位被命名新建为一个文件，剩下的以哈希码的形式保存数据。

![clipboard.png](Git%E5%BA%95%E5%B1%82%E5%91%BD%E4%BB%A4(%E5%8E%9F%E7%90%86).assets/clip_image008.gif)

- 根据哈希码返回对应的git对象数据内容：
```shell
git cat-file -p <git对象哈希码>
// 不能使用cat命令进行查看，因为存储的数据是被压缩过的
 -p选项可指示该命令自动判断内容的数据类型，以便于返回友好的格式
```

## 2、构建树对象：

- 定义：将多个git对象集中管理生成一个树对象

    - 过程：通过update-index; write-tree; read-tree 等命令构建树对象，并将树对象存放至 暂存区（并未存放在git的版本数据库中）

    - 利用update-index命令将一个git对象加入暂存区——创建一个暂存区：
```shell
git update-index --add --cacheinfo <文件模式> <git对象哈希码> <git对象名>

文件模式：100644表示这是一个普通文件，100755表示这是一个可执行文件...

--add选项：当此前该文件并不在暂存区中时，因此首次添加需要使用--add

--cacheinfo选项：从git数据库中拿取git对象

***当不加--cacheinfo选项时:他将自动进行两步操作，先将文件打包成git对象（放入版本控制库中），再将文件加入暂存区

git update-index --add <文件对象名>  

暂存区中对象名是唯一标识，即当对象名相同时，暂存区对象添加会覆盖掉原来的对象
```

- 为暂存区中的树对象生成一个快照：
```shell
git write-tree  // 该树对象返回的依然是 哈希码
- 多个git对象集中存放在暂存区中后，将其统一生成一个树对象
- 例如：将多个git对象放入池子中，当池子到达一定量的时候将整个池子打包起来生成一个总的对象。
```

- 将第一个树对象加入到第二个树对象中，使其成为新的树对象：
```shell
git read-tree --prefix=bak <树对象哈希>  // 将一个树对象加入暂存区

加入暂存区之后再次使用git write-tree进行快照生成（生成新的树）
```

- 根据哈希码查看树对象的数据内容：
```shell
git cat-file -p <树对象哈希码>
```

## 3、提交对象

- 过程：通过commit-tree命令创建一个提交对象，为此对象指定一个树对象的哈希码，以及该提交对象的父提交对象（第一次生成暂存区的快照没有父对象）

- 创建提交对象：
```shell
echo '<注释信息>' | git commit-tree <树对象哈希码>

// 返回一个提交对象的哈希码

创建提交对象时指定该对象的父对象:

echo '<注释信息>' | git commit-tree <树对象哈希码> -p <父对象哈希码>
```
- 查看提交对象：
```shell
git cat-file -p <提交对象哈希码>
```

