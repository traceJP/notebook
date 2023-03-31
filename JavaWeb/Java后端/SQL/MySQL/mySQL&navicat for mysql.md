2020.2.24

 

 

***/服务的启动和后台界面的登陆登出均需要使用管理员权限启动cmd

 

mySQL数据库服务的启动与停止：

启动命令：  net start mysql

停止命令：  net stop mysql

 

mySQL后台界面的登陆与登出：

登陆命令：mysql -uroot -p

登出命令：exit;

*(其中-p为输入密码，当前电脑密码为空)

 

 

关于mysql中的cls指令：mysql中未提供cls指令，没有方法可以使历史命令进行清除

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image002.gif)

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image004.gif)

 

\* mysql＝关系型数据库

\* 关系＝二维表

 

 

 

mySQL数据库管理系统>数据库>数据

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image006.gif)

 

sql语言大部分都写在java等之中

通过java前台调用mysql后台

 

 

E-R图

E代表实体  R代表联系

 

//联系也可以有属性

 

 

ER图概念设计：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image008.gif)

 

可以通过er图知道我应当建立什么表，表中应该建立什么字段。

 

 

 

逻辑数据模型：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image010.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image012.gif)

 

唯一约束：不能出现重复值，可以出现空值

主键：不能出现重复值也不能出现空值

 

 

 

物理设计：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image014.gif)

 

​    

 

需求分析，概念设计，逻辑设计，物理设计

 

然后将物理设计转换为mysql中的表

 

 

中国国家编码：gbk （双字节，适合中文

 

 

~在数据库管理系统中按f6可以打开命令界面

 

 

 

 

 

2020.2.25

 

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image016.gif)

为什么要设置字符集：因为要让他支持中文，不然就是一堆乱码

 

通常字符集采用的是国际通用字符集utf8

 

而排序规则中的utf8_general_ci是和utf8配套使用的

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image018.gif)

如果不用命令设置编码方式等则会自动为默认字符集utf8.

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image020.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image022.gif)

 

 

数据库的一些指令：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image024.gif)

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image026.gif)

存储引擎的不同，则创建的数据库文件不同

两大最重要的存储引擎：

innoDB   MyISAM  

 

（其中innoDB也叫事务引擎，是使用最广的引擎，创建数据库也要优先考虑innoDB引擎）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image028.gif)

不支持外键时，即使输入代码成功创建外键，也不会创建成功。

 

 

 

默认存储引擎为 MyISAM

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image030.gif)

创建表时选择引擎。

 

 

 

系统数据库：（不能乱动

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image032.gif)

 

此命令可以查看系统都有什么数据引擎：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image034.gif)

 

单词：engines(引擎）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image036.gif)

 

 

 

 

2020.3.2

 

 

 

 

 

mysql支持的整数类型:

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image038.gif)

 

 

 

c语言数据类型补充：

declmal类型（又叫货币类型）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image040.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image042.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image044.gif)

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image046.gif)

char（固定）  varchar(可变)

 

 

枚举类型：（与c一样）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image048.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image050.gif)

（set和enum差不多）

 

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image052.gif)

（文本类型，用于存储内容多的）

 

 

 

 

*********************

 

新的数据库命令语句：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image054.gif)

 

其中可以通过usc切换数据库；

usc <数据库名>;

 

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image056.gif)

 

 

 

 

 

枚举类型的枚举添加：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image058.gif)

 

 

 

关于建表命令：

 

表修改的命令前缀基本关于alter table (修改);

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image060.gif)

*其中describe 可以简写为desc

 

学习内容概括：

1、创建表有两种方式，第一种是直接在Navicat图形界面创建，第二种是通过代码创建

2、打开数据库：use 数据库名

3、创建表 CREATE [TEMPORARY] TABLE 表名

( 字段定义1， 

  字段定义2， 

  ……

字段定义n

);

4、查看数据库中有多少张表：SHOW TABLES

5、查看数据库中某张表的结构：DESCRIBE 表名

6、重命名表名：ALTER TABLE 旧表名 RENAME [TO] 新表名;

7、重命名字段名：ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新数据类型；

8、修改字段类型：ALTER TABLE 表名 MODIFY 字段名 新数据类型;

 

 

 

 

 

2020.3.3

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image062.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image064.gif)

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image066.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image068.gif)

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image070.gif)

（其中*表示用于查询表中所有的字段，星号又叫通配符）

 

 

 

**可以通过复制表类比，可推导出查询表中所有字段的命令为：

**select \* from** **表名;**

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image072.gif)

 

其中where与c语言中的if相似，指定false为条件。

（false是条件）

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image074.gif)

（只复制结构）

 

 

 

我只想要部分字段复制：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image076.gif)

 

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image078.gif)

如果想同时删除两个表，可在表名后加个逗号（,）隔开再输表名即可

 

 

 

1.关于主键：(主键约束)

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image080.gif)

（primary key）

 

 

 

 复合主键：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image082.gif)

 

 

eg：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image084.gif)

（此时primary key 写在下方，并且将字段括起来）

 

 

书本扩充：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image086.gif)

 

alter table 表名 drop primary key;

（通过此命令删除所有主键）

 

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image088.gif)

 

alter table 表名 add primary key(字段1,...,字段n);

（通过此命令添加主键）

 

 

 

\~~~~~~~~~~

 

2.（非空约束）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image090.gif)

 

 

3.（默认值约束）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image092.gif)

\~~~~~~~~~！

 

（新增加的字段默认放在最后一个）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image094.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image096.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image098.gif)

\* after 字段名  ：  放在某某字段的后面

\* first    :  放在表的第一个 

 

 

\~~~

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image100.gif)

alter table 表名 modify 字段名 字段属性 default 默认值；

 

*如果删去默认值则

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image102.gif)

则上述命令不需要输入 default 默认值 则表示删去默认值

~

 

3月3日的内容如下：

1、修改字段位置：ALTER TABLE 表名 MODIFY 字段名1 数据类型 FIRST|AFTER 字段名2

2、添加字段：ALTER TABLE 表名 ADD 字段名 数据类型

3、删除字段：ALTER TABLE 表名 DROP 字段名;

4、复制表结构及记录：CREATE TABLE 新表名 SELECT * FROM 源表名;

5、只复制表结构：CREATE TABLE 新表名 SELECT * FROM 源表名 WHERE FALSE ;

6、复制表的部分字段到新表：复制表的部分字段和数据到新表：CREATE TABLE 新表名 AS(SELECT 字段1,字段2,...... FROM 源表名);

7、删除一个表时，表的结构定义、数据、约束等都将被启示删除：DROP TABLE 表名;

8、设置主键约束：CREATE TABLE Goods

(  gdID INT PRIMARY KEY,  -- 标识该字段为主键

  gdName VARCHAR(30) NOT NULL,

  gdPrice DECIMAL(8,2)

);

9、设置复合主键：

CREATE TABLE SCarInfo 

( gdID INT,

uID INT,

scNum INT,

  PRIMARY KEY(gdID,uID)     -- 定义复合主键

);

10、设置非空约束：属性名 数据类型 NOT NULL

11、设置默认值约束

CREATE TABLE SCarInfo 

( gdID INT,

uID INT,

scNum INT DEFAULT 1,        -- 设置默认值为1

​       PRIMARY KEY(gdID,uID) 

);

 

 

\~~~~~~~~~~~~

 

 

 

 

 2020/03/09 

 

 

 

4.unique约束（唯一约束）：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image104.gif)

（该约束指令放在最后面）（与复合主键约束类似使用）

 

 

***5.foreign key约束（外键约束）：

**外键约束不能只在一张表中设置（又可理解为：为两张表实现关联，显然，为两张表不可单独为一张表设置，只能两张表同时进行）**

 

图形界面设置外键：

1）先建立一张主键表，设置一个你想要关联的字段

2）再建立一张从表，设置与主表相关联的字段

其中，从表必须要有一项字段与主表相同

3）在从表中设置外键，如图

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image106.gif)

*栏位与参考栏位：两张表中相关联的字段（该表中为tid）

*参考数据库，参考表，参考栏位：主表的数据库路径

*其中删除时和更新时中CASCADE（级联）用处为将两个表中的数据进行联系关联，（你删除，我也删除。你增加，我也增加。跟随更新）

4）此时在主键表中进行添加记录后，在从表中相同字段则会自动出现。

*当一张从表被设置为外键后，不允许在从表中优先填写外键相同项目

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image108.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image110.gif)

 

 

 

代码界面设置外键：

 

主键表创建：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image112.gif)

从表的创建：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image114.gif)

其中最后一行为外键的创建：

-> constraint 外键名 foreign key(关联字段名) references 主表名(关联字段名)  

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image116.gif)

on update cascade  (添加级联 更新时) 

on delete cascade （添加级联 删除时）

*/将两个指令放在创建外键指令的后面即可添加级联

 

以下为外键设置中（删除时）（更新时）的4个选项解释：

restrict(拒绝主表)：假设从表设置了主表的关联字段1，则主表不能更新和删除从表的关联字段。

no action(无操作)：即不进行任何操作，主表或从表删除更新时，关联表不会有任何改变

***///cascade(****级联)**：你变我也变

set null(设置为空)：当主表删除或更新时，从表字段关联变为空值null

 

 

 

\~~~~~~~~~~~~~~~~~~~~~~~

 

 

 

 2020/03/10 （从今天开始所有命令都是写在端口，不能使用图形界面）

 

 

 

 

数据的添加更新和删除：

 

 

添加记录：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image118.gif)

*其中字段列表如果省略：则为给所有字段赋值

*values：中的值列表一定要与字段列表的顺序相对应，如果省略了字段列表，则要与原本的字段顺序相对应。否者会报错

*字段列表的顺序可以不和原表顺序不一致，但值列表一定要与字段列表一致

eg.

 

mysql> use qwq;

Database changed

mysql> desc dept;

+-------------+--------------+------+-----+---------+-------+

| Field    | Type     | Null | Key | Default | Extra |

+-------------+--------------+------+-----+---------+-------+

| dept_id   | int     | NO  | PRI | NULL  |    |

| name    | varchar(22) | YES |   | NULL  |    |

| location  | varchar(50) | YES |   | NULL  |    |

| description | varchar(200) | YES |   | NULL  |    |

+-------------+--------------+------+-----+---------+-------+

4 rows in set

 

mysql> insert into dept values(666,'qaq','qwq','qnq');

Query OK, 1 row affected

注意：该例题insert into 表名后没有字段列表

 

成功添加记录：

mysql> select * from dept;

+---------+------+----------+-------------+

| dept_id | name | location | description |

+---------+------+----------+-------------+

|   666 | qaq  | qwq   | qnq     |

+---------+------+----------+-------------+

1 row in set

 

 

 

 

replace(插入或替换记录)：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image120.gif)

 

使用机制：他会在你使用命令后检查表中是否有你输入的相同的值

如果有：删除原来的记录，使用命令中添加的新记录

如果没有：直接添加一个命令中的记录

 

判断机制：判断该字段是否是唯一索引或者主键索引。

 

如：

 

此时表中是没有233该行记录：

 

（1）

mysql> replace into dept(dept_id,name,location) values('233','zxc','vbn');

Query OK, 1 row affected

 

mysql> select * from dept;

+---------+------+----------+-------------+

| dept_id | name | location | description |

+---------+------+----------+-------------+

|   233 | zxc | vbn   | NULL    |

|   666 | qaq | qwq   | qnq     |

+---------+------+----------+-------------+

2 rows in set

 

 

之后：

 

（2）

mysql> replace into dept(dept_id,name,location) values('233','tyu','ghj');

Query OK, 2 rows affected

mysql> select * from dept;

+---------+------+----------+-------------+

| dept_id | name | location | description |

+---------+------+----------+-------------+

|   233 | tyu | ghj   | NULL    |

|   666 | qaq | qwq   | qnq     |

+---------+------+----------+-------------+

2 rows in set

 

实现替换

 

 

连续添加多条记录：（用逗号将记录之间隔开）

mysql> insert into dept(dept_id,name,location) values(

  -> '110','nun','num'),

  -> ('111','zvd','zxg');

Query OK, 2 rows affected

Records: 2 Duplicates: 0 Warnings: 0

 

*//其中replace也可同时替换多条记录：使用方法与上述类似

 

 

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image122.gif)

eg.

insert into dept(...) select(...) from dept where dept_id>3;

 

 

 

添加记录的其他方式：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image124.gif)

 

 

修改数据：（给表里的字段重新赋值）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image126.gif)

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image128.gif)

*若where条件省略，则变成修改所有记录

 

 

 

删除记录：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image130.gif)

*where条件：把满足条件的删除

例如where uID=4   :  将uid为4的人的记录删除

警告：如果不添加where条件，则将会把表中所有记录全部删除

 

 

或直接把表中记录删除：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image132.gif)

 

区别 ：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image134.gif)

可以理解为：delete会记录下你的操作过程，truncate直接就全部从零开始

delete是针对标志列设计的

 

 

 

 

3月10日学习内容：

1、添加数据：（1） INSERT INTO GoodsType VALUES(1,’学习用品’);

（2）INSERT INTO user(uName,uPwd) VALUES(‘张小山’,’123’);

（3）REPLACE INTO user(uID,uName,uPwd) VALUES(‘3’,‘乐天天’,’111’);

（4）INSERT INTO user(uName,uSex,uPwd) VALUES

(‘郑霞’,’女’,’asd’),

(‘李竞’,’男’,’555’),

(‘朱小兰’,’女’,’123’) ;

（5）INSERT INTO users(uname,upwd,uSex)

SELECT uname,upwd,usex

FROM user

WHERE uID>5

2、修改记录：UPDATE users

SET uPwd = '888'

WHERE uName= '朱小兰‘

3、删除记录

（1）DELETE FROM users

WHERE uID = 4;

（2）TRUNCATE users

 

 

 

 

 

 

 2020/03/16

 

数据库的查询（select命令）：

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image136.gif)

 

星号* ：（查询所有列）

在SELECT子句中，关键字“*"表示选择指定表中所有列。查询结果集中的排列顺序与源表中列的顺序相同。如：select * from 表名;(查询表中的所有字段)

 

 

 

只查询表中的一个字段：

select 字段1,字段2,,,字段n from 表名;

eg.

mysql> select name from dept;

+------+

| name |

+------+

| nun |

| zvd |

| tyu |

| qaq |

+------+

4 rows in set (0.00 sec)

 

 

 

如何用函数获取当前系统时间：

now();函数

使用方法：select 字段1,字段2,字段n,now() from 表名;

eg.(此时查询列表增加了一列)

mysql> select name,now() from dept;

+------+---------------------+

| name | now()        |

+------+---------------------+

| nun | 2020-03-16 10:57:12 |

| zvd | 2020-03-16 10:57:12 |

| tyu | 2020-03-16 10:57:12 |

| qaq | 2020-03-16 10:57:12 |

+------+---------------------+

4 rows in set (0.00 sec)

 

 

 

通过select查询时，使用计算列表达式作为查询的结果：

select 字段名 (计算符号+-*/等) 字段名 from 表名;

 

例如简单的计算：(查询结果会出现两数相乘，常用于利润等计算)

mysql> select name*dept_id from dept;

+--------------+

| name*dept_id |

+--------------+

|      0 |

|      0 |

|      0 |

|      0 |

+--------------+

4 rows in set, 4 warnings (0.00 sec)

 

 

通过函数计算年龄(year(uBirth)获取出生日期的年份)（year()获取括号中的年份）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image138.gif)

 

 

 

为查询结果中的列指定列标题：（为查询表中的字段取一个别名）

默认情况下，结果集显示的列标题就是查询列的名称，当希望查询结果中显示的列使用自己的列标题时，可能使用AS关键字更改结果集中的列标题。

select 字段名 as 别名,字段名2 as 别名2,字段名n as 别名n from 表名;

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image140.gif)

 

 

where条件在select中的使用：

select 字段,,, from 表名 where 条件;

条件可为数值运算符/

如此时查询dept_id大于111的所有记录：

mysql> select dept_id from dept where dept_id>111;

+---------+

| dept_id |

+---------+

|   233 |

|   666 |

+---------+

2 rows in set (0.00 sec)

 

 

 

sql中条件语句的与或非：

非：not()

其中，如果在条件中使用not()函数，且其中在括号中输入表达式。

则该表达式意为  非（....）

如where not(a>50)  意思为   a不大于50

 

或：or

eg. where a>50 or b<100;  意思为  a大于50或者b小于100

 

且：and

eg. where a>50 and b=100;  意思为  a大于50与b等于100

 

 

 

 

 

 

 

 

 

 

 

 2020/03/17 

 

范围运算符：（between....and）（在两者之间）

sql：WHERE 表达式 [NOT] BETWEEN 初始值 AND 终止值

用于查询数据的范围

*其中not between...and(意为不在这两者之间)

***（表达式大多为字段名，如字段名要求＝什么什么 称为表达式）

 

in运算符等价or运算：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image142.gif)

（最后取这些值中间的某一个）

（相比or简化语法：如 厦门or广东or北京   ...   in(厦门,广东,北京)  ）

 

 

精确查询：等号就是精确查询

模糊查询（又叫like查询）：

where 表达式 like '查询字词';

*在字词中使用通配符达到模糊查询的效果

***/通配符（所谓通配，可以理解为代替当前位置的字符）：

\1. 百分号% （通配任意个字符）     2. 下划线_（通配一个字符）

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image144.gif)

 

转义字符：

当查询的字符串中含通配符时，MySQL采用转义字符来实现，默认的转义字符为"\”

（与c语言转义字符类似，用于去除本来的意思，如 \% 此时该%只是普通的%，而不是通配符）

 

转义字符语句escape：

该语句escape '字符'放在like查询字词的后面，表示查询字词中的某个字符为转义字符

/如where name like '我的！是！_' escape '!';  此时查询字词中的感叹号均为转义字符

 

 

选择行：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image146.gif)

该表中模式都为通配符，仅适用于regexp运算符

 

 

is null或者is not null（sql中不能用=null进行判断，与c语言不相似）:

where 列名 is [not] null

即查询该不为空的记录

 

 

 

使用DISTINCT消除重复结果集：

selsct distinct 字段名 from 表名;

（distinct消除重复记录）（注：distinct要写在字段名前面）

 

 

 

 

 

 2020/03/23 

 

 

 

 

排序：

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image148.gif)

 

*ASC：升序排序

*DESC：降序排序

mysql> select * from dept;

+---------+------+----------+-------------+

| dept_id | name | location | description |

+---------+------+----------+-------------+

|   110 | nun | num   | NULL    |

|   111 | zvd | zxg   | NULL    |

|   233 | tyu | ghj   | NULL    |

|   666 | qaq | qwq   | qnq     |

+---------+------+----------+-------------+

4 rows in set (0.02 sec)

 

mysql> select dept_id name,location,description

  -> from dept

  -> order by dept_id desc;

+------+----------+-------------+

| name | location | description |

+------+----------+-------------+

| 666 | qwq   | qnq     |

| 233 | ghj   | NULL    |

| 111 | zxg   | NULL    |

| 110 | num   | NULL    |

+------+----------+-------------+

4 rows in set (0.00 sec)

 

 

LIMIT（限制结果集返回的行数）：

LIMIT [OFFSET,]记录数

与order by使用方法类似，意思为：只返回你想要的记录数量，如上述命令中记录数填3，则只会查询3条记录

*OFFSET指的是从第几条记录开始，如limit n,m；表示从第n+1条记录开始后的m条记录.

注意：编号是从0开始的,所以记录应该为n+1

 

 

 

 

数据分组统计：

 

在对表进行数据检索时，经常需要对结果进行统计计算。

●在SELECT语句中，使用聚合函数、GROUP BY子句能够实现对查询结果集进行分组和统计

●聚合函数：聚合函数能够实现将数据表在指定列上的值,或对一组记录指定的列值进行特定的运算，并返回单个数值。

常用的聚合函数如：sum（求和），max，min等

 

SELECT 聚合函数(字段名) FROM 表名;

 

 

统计记录个数的命令:

  SELECT COUNT(*) FROM 表名;

 

**/ count：统计记录的个数

 

分组显示我的记录：（GROUP BY子句）

GROUP BY [ALL]  列名1,列名2 [ ..n] [ WITH ROLLUP] [HAVING条件表达式]

一般来说，分组是配合聚合函数使用的

 

参数说明: ALL将显示所有组，是默认值。列名为为分组依据列。不能使用在SELECT列表中定义的列别名来指定组合列;使用WITHROLLUP关键字指定的结果集不仅包含由GROUPBY提供的行，同时还包含汇总行; HAVING用 来指定分组后的数据集进行过滤。

 

通过group by进行分组会让语句有不同的涵义。如：

select uCity,count(*) from user;    // 此时查询的是uCity城市 以及 count记录的个数

\~~~

select uCity,cont(*) from user group by uCity;  //按照uCity进行分组   此时该语句查询的是uCity城市的个数

 

 

 

GROUP BY和GROUP_ CONCAT函数一起使用：

GROUP_CONCAT([DISTINCT] 表达式[ORDER BY列名] [SEPARATOR分隔符])

 

参数说明: DISTINCT 可以排除重复值;如果希望对结果中的值进行排序，可以使用ORDER BY子句;

SEPARATOR是-一个字符串值，它被用于插入到结果值中，缺省分隔符为逗号(",")，也可以指定SEPARATOR ""完全地移除这个分隔符。

group_concat用处：实现某一分组中的 值用分隔符隔开

 

 

GROUP BY和WITH ROLLUP函数一起使用（对分组的结果做一个汇总）

GROUP BY 表名 WITH ROLLUP;

 

 

 

**对分组以后的结果再进行筛选：（对最终结果进行筛选）

GROUP BY和HAVING一起使用

●HAVING关键字和WHERE关键字都用于设置条件表达式对查询结果集进行筛选

●HAVING关键字后可以跟聚合函数，且只能跟GROUP BY一起使用

注意：聚合函数只能放在having中，不能放在where中

 

2020.6.17  // 并不是用了group by之后就不能再用where，而是group by分组后的值不能再用where去判断条件。

l 一般可以先where排除一些记录，再去group by分组，分组之后having再排除一些记录，最后得出查询结果

 

 

 

//注意：GROUP_ CONCAT，WITH_ROLLUP函数 必须联合GROUP BY一起使用

 

 

 

 

 

 

 

2020/03/24 

 

 

 

连接查询多表数据（多表查询）：

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image150.gif)

 

内连接：

 

内连接只输出满足条件的结果

 

（1）内连接的一种方法：

***/由SELECT语句的FROM子句中的JOIN关键字来实现

SELECT [ALL | DISTINCT] * |列名.[列.名....列名n]

FROM 表名1 [别名1] JOIN 表名2 [别名2]

[ON 表名1.关系列=表名2.关系列]

[ WHERE 条件表达式];

 

 

(2)内连接的第二种方法：

SELECT [ALL | DISTINCT] * |列名.[列.名....列名n]

FROM 表名1 [别名1],表名2 [别名2]

WHERE [表名1.关系列=表名2.关系列] AND 条件表达式;

 

//其中FROM中的别名不需要加as，直接空格后接别名即可

上述两种方法等效，如

//第一种方法

mysql> select tName,gdCode,gdName,gdPrice

  -> from goodstype join goods

  -> on goodstype.tID=goods.tID

  -> where tName='服饰';

+--------+--------+-----------+---------+

| tName | gdCode | gdName  | gdPrice |

+--------+--------+-----------+---------+

| 服饰  | 001  | 迷彩帽  |   63 |

| 服饰  | 005  | 运动鞋  |   400 |

| 服饰  | 008  | A字裙   |   128 |

+--------+--------+-----------+---------+

3 rows in set (0.01 sec)

 

//第二种方法

mysql> select tName,gdCode,gdName,gdPrice

  -> from goodstype,goods

  -> where goodstype.tID=goods.tId and tName='服饰';

+--------+--------+-----------+---------+

| tName | gdCode | gdName  | gdPrice |

+--------+--------+-----------+---------+

| 服饰  | 001  | 迷彩帽  |   63 |

| 服饰  | 005  | 运动鞋  |   400 |

| 服饰  | 008  | A字裙   |   128 |

+--------+--------+-----------+---------+

3 rows in set (0.00 sec)

 

 

当连接的表大于两张时：(方法二更易于理解)

//方法1

SELECT [ALL | DISTINCT] * |列名.[列.名....列名n]

FROM 表名1 [别名1] JOIN 表名2 [别名2] [ON 表名1.关系列=表名2.关系列]

JOIN 表名3 [别名3] [ON 表名3.关系列=表名2.关系列]

JOIN 表名4 [别名4] [ON 表名4.关系列=表名1|2|3.关系列]

...

JOIN 表名n [别名n] [ON 表名n.关系列=表名n|m.关系列]

[ WHERE 条件表达式];

 

//方法2

SELECT [ALL | DISTINCT] * |列名.[列.名....列名n]

FROM 表名1 [别名1],表名2 [别名2],,,表名n [别名n]

WHERE [表名1.关系列=表名2.关系列] AND...AND [表名1.关系列=表名n.关系列] AND 条件表达式;

 

注意：当内连接查询N张表中关联的同一字段（如表1字段uID，表2字段uID）时，需要明确表名你查询的字段是哪张表的（如此时是表一的uID还是表二的uID），其中用 . 隔开 如 表1名 .uID

 

 

自连接：（同一张表的）

 

 

 

 2020/03/30  

 

 

 

外连接：

外连接返回的结果集除了包括符合条件的记录外，还会返回FROM子句中至少一个表中的所有行，不满足条件的数据行将显示为空值，又分为左外连接、右外连接和完全连接。

 

●左外连接(LEFT JOIN) :结果集中除了包括满足连接条件的行外，还包括左表中不满足条件的记录行。当左表中不满足条件的记录与右表记录进行组合时，右表相应列值为NULL。

●右外连接(RIGHT JOIN) :结果集中除了包括满足连接条件的行外，还包括右表中不满足条件的记录行。当右表中不满足条件的记录与左表记录进行组合时，左表相应列值为NULL。.

select 字段1名,字段1名,,,,字段n名

from 表名1 [别名1] [LEFT]|[RIGHT] JOIN 表名2 [别名2]

ON 表名1 [别名1].字段名=表名2 [别名2].字段名;

 

*注意：外连接只适用与两张表，不适用自连接

 

 

 

联合查询(将n个select查询联合在一起查询):

SELECT 语句1 

UNION[ALL]

SELECT 语句2

[UNION [ALL]< SELECT语句3>]

[..n];

 

 

 

***重点内容

子查询（查询中的查询）：

 

子查询是一种SELECT语句的使用方法，它嵌套在SELECT、INSERT0、UPDATE、DELETE 语句或其他的子查询语句中，用来表示复杂的查询。子查询的执行过程为:首先执行子查询中的语句，并将返回的结果作为外层查询的过滤条件，然后再执行外部查询。在子查询中通常要使用比较运算符、IN、 ANY及EXISTS等关键字。

SELECT 字段名1,字段名2,,,,字段名n

FROM 表名

WHERE 字段名=(SELECT 语句1)

 

//连接查询与子查询的对比

mysql> select gdID,gdName,gdPrice,gdSaleQty

  -> from goods,goodstype

  -> where goods.tID=goodstype.tID and tName='服饰';

+------+-----------+---------+-----------+

| gdID | gdName  | gdPrice | gdSaleQty |

+------+-----------+---------+-----------+

|  1 | 迷彩帽  |   63 |    29 |

|  5 | 运动鞋  |   400 |    200 |

|  8 | A字裙   |   128 |    200 |

+------+-----------+---------+-----------+

3 rows in set (0.01 sec)

 

//自查询

mysql> select gdID,gdName,gdPrice,gdSaleQty

  -> from goods

  -> where tID=(select tID from goodstype where tName='服饰');

+------+-----------+---------+-----------+

| gdID | gdName  | gdPrice | gdSaleQty |

+------+-----------+---------+-----------+

|  1 | 迷彩帽  |    63 |    29 |

|  5 | 运动鞋  |   400 |    200 |

|  8 | A字裙   |   128 |    200 |

+------+-----------+---------+-----------+

3 rows in set (0.00 sec)

*/可以理解为子查询就是嵌套查询，其中最多只能嵌套32层查询。

/且执行顺序是从内到外的

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image152.gif)

 

 

使用ANY、SOME或ALL关键字的子查询：

WHERE表达式比较运算符{ANY|SOME |ALL}(子查询)

 

ANY:满足任何一个条件 //表达式 > any (1,2,3) any括号中只要有一个值符合就可以

SOME:和any一个意思

ALL:满足所有的条件  //表达式 > all(1,2,3)  all括号中的值都要复合才可以

 

//该1,2,3可以是子查询select中查询出来的不同的值

熟练使用any，all等关键字再配合使用子查询进行价格等字段的比对

 

 

 

子查询作为派生表：（了解）

由于SELECT命令查询的结果集是关系表，因此子查询的结果集亦可放置

在FROM子句后作为查询的数据源表，这种表称为派生表。在SELECT命

令中需要使用别名来引用派生表。

 

 

相关子查询：

相关子查询又称为重复子查询，子查询的执行依赖于外层查询，即子查询

依赖外层查询的某个属性来获取查询结果集。派生表或表达式的子查询只

执行一次，而相关子查询则要反复执行其执行过程如下:

(1) 子查询为外层查询的每一行记录执行一 -次, 外层查询将子查询引用的列传给子

查询中引用列进行比较。

(2)若子查询中有行与其匹配，外层查询则取出该行放入结果集。

(3)重复执行(1) - (2) ，直至所有外层查询的表的每一-行都处理完。

 

 

●使用EXISTS关键字的子查询

 使用EXISTS的子查询不需要返回任何实际数据，而仅返回一个逻辑值（要么为真要么为假）

WHERE [NOT] EXISTS(子查询)

（如果真，则输出子查询。否则不输出）

 

*/exists括号中的子查询查询的字段一般用*表示，因为查询确立字段没有意义，因为不需要确立的返回值

 

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image154.gif)

此时运行过程为：

 内查询检查外查询users表的第一条记录中users.uID=orders.uID？，如果等于，则返回true，则输出uName等数据

​         如果不等于，则返回false，则不输出uName等数据

当第一条记录检测完毕后，EXISTS将自动跳转到外查询users表中的下一条记录，直到外查询中的所有记录检查完毕为止

(可以理解为EXISTS自带了个for循环，将外查询中的所有记录都循环一遍)

 

 

索引类型：

（1）普通索引

-是MySQL中的基本索引类型，允许在定义索引的列中插入重复值和空值

（2）唯一索引

索引列的值必须唯一， 但允许有空值。如果是复合索引，则列值的组合必须唯一-。 主键索引是一种特殊的唯一索引，不允许有空值。

（3）复合索引(又称组合索引或多列索引)

复合索引是在数据表的多个字段组合.上创建的索引，只有在查询条件中使用了这些字段的左边字段时，索弓|才会被使用。

（4）全文索引（只支持myISAM）

全文索引是一种特殊类型的索引，它查找的是文本中的关键词，而不是直接比较索引中的值。

（5）空间索引（只支持myISAM）

空间索引是对空间数据类型的字段建立的索引

 

索引创建原则：

高效的索引有利于快速查找数据,而设计不合理的索引可能会对数据库和应用程序的性能造成障碍。因此，创建索引|时应尽量考虑符合以下原则，便于提升索弓|的使用效率。

(1) 不要建立过多的索引

(2)为用于搜索、排序或分组的列创建索引，而对于用作输出显示的列则不宜创建索引。

(3)使用唯一索引， 并考虑数据列的基数。数据列的基数是指它所容纳的所有非重复值的个数。

(4)使用短索引，应尽量选用长度较短的数据类型。

(5)选用字符串为索弓|时，应尽可能指定前缀长度。

 

索引方式：

索弓|是在存储弓|擎中实现的，每种存储弓|擎的索弓|都不一定完全相同，并且每种存储弓|擎也不一定支持所有索引类型。

依据MySQL中不同索引的存储类型

l BTREE

l HASH

MylSAM和InnoDB存储弓|擎只支持BTREE索引，BTREE索弓|也是MySQL中最常用的索引结构，

MEMORY/HEAP存储弓|擎可以支持HASH和BTREE索引。

 

在创建表中创建索引：

CREATE TABLE 表名(

  字段名1 数据类型1，

  ...

  字段名n 数据类型n，

  [UNIQUE | FULLTEXT |SPATIAL] INDEX 索引名(字段名[(索引长度)] [desc|asc])

);

 

在已有表上创建索引：

ALTER TABLE表名 ADD [UNIQUE | FULLTEXT |SPATIAL] [INDEX| KEY] 索引名(字段名[(长度)] [ASC|DESC]) );

索引的查看：

查看表结构：

SHOW CREATE TABLE 表名; 

查看索引：

SHOW INDEX FROM 表名;   SHOW KEYS FROM 表名;

 

 

视图：

视图的概念：

\1.   视图是一一个虚拟的表，是从数据库中一个或多个表中导出来的表，其内容由查询语句定义。

\2.   同真实的数据表一样，视图由行和列组成

\3.   数据库中只存放了视图的定义，而并没有存放视图中的数据，这些数据均存放在原来的数据表中

\4.   使用视图查询数据时，数据库系统会从原来的表中取出对应的数据

\5.   图中的数据 是依赖原始数据表中的数据， 一旦表中的数据发生改变， 显示在视图中的数据也会发生改变。

\6.   在定义了一一个视图之后，就可以把它当作表来引用。

\7.   视图永远依赖原数据表

 

修改视图：

ALTER [ALGORITHM={UNDEFINED | MERGE | TEMPTABLE}]

VIEW 视图名[(属性清单)]

AS SELECT语句

[WITH [CASCADED | LOCAL] CHECK OPTION]

或者：

CREATE OR REPLACE [ALGORITHM={UNDEFINED | MERGE | TEMPTABLE}]

VIEW 视图名[(属性清单)]

AS SELECT语句

[WITH [CASCADED | LOCAL] CHECK OPTION]

 

 

 

 

 

 

面向系统数据库编程：

 

SQL变量：

变量是指程序运行过程中会变化的量，MySQL支 持的变量类型有4种类型。

l 用户变量:这种变量用一个@字符作为前缀，在MySQL会话末端结束其定义。（定义或者赋值的时候都要以一个@开头）

l 系统变量和服务器变量:这种变量包含了MySQL服务器的状态或属性。它们以@@字符作为前导符(例如: @@binlog_ cache_ size) 。（全局变量，以两个@开头，sql的全局变量是系统自带的，是用户不能自己定义的）

l 结构化变量:这种变量是系统变量的一种特例。MySQL目前只在需要定义更多的MylSAM索弓|缓存区时才会用到这些变量。

l 局部变量:这种变量处于存储过程中，而且只是在存储过程中有效。它们没有特殊的前导标识，因此，给它们起的名字必须与数据表和数据列的名字有所区别。

 

（1）用户变量：

1.用户变量即用户定义的变量。用户变量可以被赋值，也可以在后面的其他语句中弓|用其值。

2.用户变量的名称由"@"字符作为前缀标识符。

3.用户变量到哪里都不能省略@，只要引用了变量，就得加@

4.用户变量使用SET命令和SELECT命令给其赋值

l SET命令使用的赋值操作符是“=” 或 ":="

l SELECT命令使用的赋值操作符只能是 ":=” 。

delimiter//       //delimiter：转换结束符。此时将结束符从分号改成双斜杠

SET @id = 3;  //用等号直接赋值

SELECT @name := '张三';  //可以通过select和冒号等号进行赋值

//

 

（2）局部变量：

l 局部变量一般用在SQL语句块(如存储过程的BEGIN和END)中

l 其作用域仅限于语句块，当语句块执行完毕后，局部变量就消失了

l 局部变量一般用DECLARE来声明，可以使用DEFAULT来设置默认值。

CREATE PROCEDURE proc_add(in a int,in b int)  //形参a,b

BEGIN  //BEGIN~END语句块，相当于c语言中的花括号

​    DECLARE c int DEFAULT 0;

​    SET c = a + b;  //SQL的每个赋值必须要在前面加SET标记

​    SELECT c AS 'Result';

END;    

 

用户变量和局部变量的区别：

![clipboard.png](mySQL&navicat%20for%20mysql.assets/clip_image156.gif)

会话变量：当前命令当中  全局变量

 

（3）系统变量：

l MySQL中的系统变量分为SESSION (会话)变量和GLOBAL (全局)变量。

l SESSION变量只对当前会话(当前连接)有效。 （可以使用SESSION代替@@）

l GLOBAL变量则对整个服务器全局有效。

l 都可以使用SET命令来修改其值。

l 当一个全局变量被改变时，新的值对所有新的连接有效，但对已经存在的连接无效。

l 而会话变量的改变只对当前连接有效，当一个新的连接出现时，会话变量的默认值起作用。

查看全局变量和会话变量：

全局：SHOW GLOBAL VARIABLES;

会话：SHOW SESSION VARIABLES;

 

l 常量，字符串变量，转义字符，运算符和表达式

 

 

SQL流程控制：

 

条件分支语句：

l IF  IFNULL  IF...ELSE  CASE

（1）三目运算符（  ? : ）

IF(条件表达式,结果1,结果2);  //条件表达式为true则返回结果1，反之返回结果2

例如：

select uName,if(uEmail is null,'nothing',uEmail) from users;

（2）IFNULL（双目运算符）

IFNULL(结果1,结果2); //若结果1的值不为空，则返回结果1，否则返回结果2

（3）IF...ELSE（只能在存储过程中使用）

IF 条件表达式1 THEN

<语句块1>;

[ELSE IF条件表达式2 THEN <语句块2>;]

...

[ELSE <语句块n>;]

END IF;

（4）CASE（c语言的switch）

CASE 表达式

WHEN 数值1 THEN 结果值1;

[WHEN 数值2 THEN 结果值2;]

...

[ELSE 结果值n+1;]

END CASE;       

 

循环语句：

 （1）LOOP（需要配合break等语句进行循环跳出）

[开始标签:]LOOP

  <语句块>

END LOOP [结束标签];  

（2）WHILE-DO（while）

[开始标签:] WHILE <条件表达式> DO

  <语句块>

END WHILE [结束标签];

（1_2 .5）LEAVE语句和ITERATE语句

LEAVE 标签名;  // 配合LOOP,WHILE使用的 类似c语言中break语句

ITERATE 标签名;  // 类似c语言中的continue语句

示例：(求1-100以内除了50的和)

set @count = 0;

set @sum = 0;

add_num:loop

  set @count = @count + 1;

  if @count = 100 then

​    LEAVE add_num;

  else if @count = 50 then

​    ITERATE add_num;

​    end if;

  end if;

  set @sum = @sum + @count;

end loop add_num;    

（3）REPEAT（do...while）

REPEAT

​    <语句块>

  UNTIL <条件语句>  //条件为真则跳出，注意：条件是为 真 （与c语言不一样）  

END REPEAT;  

 

 

 

 

 

 

存储过程：

存储过程的定义：

定义一段完成特定功能的SQL语句集，经编译后存储在数据库中，用户可以通过指定的存储过程名称并给出参数来执行它，这样的语句集称为存储过程。

语句格式：

CREATE PROCEDURE 函数名(in 形参1 数据类型,in 形参2 数据类型,...,in 形参n 数据类型)

BEGIN

<语句组>

END;

调用存储过程：

CALL 函数名(形参1,形参2,...,形参n);

存储过程的优点：

l 存储过程是在MySQL服务器中存储和执行的，可以减少客户端和服务器端的数据传输，可以利用服务器的计算能力，执行速度快。

l 存储过程执行一次后， 其执行规划就驻留在高速缓冲存储器中，在以后的操作中，只需从高速缓冲存储器中调用已编译好的二进制代码即可，提高了系统性能。

l 存储过程在被创建后，可以在程序中多次调用，而不必重新编写，避免开发人员重复的编写相同的SQL语句。数据库专业人员可以随时对存储过程进行修改，对应用程序源代码毫无影响。

l 存储过程可以用流控制语句编写，可以完成复杂的判断和较复杂的运算。

l 确保数据库的安全性和完整性。系统管理员通过对某一存储过程的权限进行限制，能够实现对相应的数据的访问权限的限制，避免非授权用户对数据的访问。

 

再谈存储过程：（输入输出参数的存储过程）

创建存储过程的基本语法如下

CREATE PROCEDURE sp_name ([proc_parameter])

[characteristic..] routine_ body

proc_ parameter: [IN | OUT | INOUT]param_ name type

sp_name:定义的存储过程的名称;

proc_parameter表示存储过程的参数列表;

characteristic表示指定存储过程的特性;其说明跟函数定义里该参数说明相同。

routine_ body参数是SQL代码的内容，可以用BEGIN--END来标识SQL代码的开始和

结束。

IN | OUT | INOUT:表示参数方向，其中IN表示输入参数; OUT表示输出参数; INOUT表示输入输出参数;

param_ name:表示存储过程的参数名称;type表示指定存储过程的参数类型，要该类

!四2竹品

出可以是MySQL的任意数据类型

执行过程：

CALL 函数名(形参1 | 变量1,形参2 | 变量2 ,..., 形参n | 变量n)

//形参对应创建过程中的in 变量对应创建过程中的out

删除存储过程：

DROP PROCEDURE 函数名;

修改存储过程：

//不建议修改，建议直接删除再创建一份

实例：

create procedure sp5(in name varchar(30),out id int)

begin

  select uID into id  // 将查询uID的结果into给id， 当返回多个字段时，可以select a,b,c into n,m,l

  from users

  where uName=name;

end;

 

call sp5('张三',@id);  //输出的参数以变量@id的形式返回了  

 

 

流程控制：

 

 

 

游标：

（1）游标的定义：（游标使用前必须要先定义）

DECLARE <游标名> CURSOR FOR sql_statement;

sql_statement则是用于定义i游标所要操作结果集的select语句

（2）打开游标：

OPEN <游标名>;

（3）推进游标：(也叫使用游标)（因为游标每次只能指向一条记录，所以需要循环推进指针的位置）

且MySQL中游标是仅向前的且只读的，也就是说，游标只能顺序地从前往后一条条读取结果集

FETCH <游标名> INTO var_name[,var_name];

其中，cursor_name表示游标的名称;var_name用于存储游标中的SELECT语句查询的结果信息。var_ name中变量必须事先定义，且变量的个数必须和游标返回字段的数量相同，否则游标提取数据失败。

（4）关闭游标：

CLOSE <游标名>;

 

**/游标实例：

create procedure p7(in id int) //begin程序写在存储过程之中，配合call p7(参数);调用

 

begin

 

declare ordersid int;  //定义变量ordersid

 

declare done int;   //定义变量done

 

declare cur_orders cursor for select oID from orders where uID = id;  //定义游标，且id为存储过程传入的参数

 

//***异常处理：（只有当数据被全部遍历完毕后才会调用异常处理）

declare CONTINUE HANDLER FOR SQLSTATE '02000' set done = 1;

//declare：声明句柄

//sqlstate '02000' ：遍历结果集之后，若表的数据全部读完 ; 02：错误条件类别，无数据；000：02错误类别的一个子类 

//set done = 1 ：则将done设置为1

//其中，sqlstate '02000' 可以用 not found 代替 ，意为没有记录

 

open cur_orders;  //打开游标

  repeat   //do...while循环 （循环才能推进游标）

​    fetch cur_orders into ordersid;  //提取oID的值赋值给ordersid变量

​    delete from orderdetail where oID = ordersid;  //将oID = ordersid的表中记录删除

  until done  // 条件为真时跳出循环，此时这里如果触发异常处理，则done = 1时条件为真

end repeat; //循环结束

  

close cur_orders; //关闭游标

 

  end;    

 

 

mysql函数：

内部函数和自定义函数：内部如数学函数，字符串函数等。

（1）定义函数

CREATE FUNCTION 函数名(参数名1 数据类型1,...,参数名n 数据类型n) RETURNS 返回类型

  BEGIN

  <代码块>

​    END

（2）查看函数的定义

SHOW FUNCTION STATUS [LIKE 'pattern'];

或者

SHOW CREATE FUNCTION 函数名;

 

函数与存储过程的区别：

最大区别：函数有return返回值，存储过程没有。

 

 

触发器：

\1.   触发器是一种特殊的存储过程

a)   可以用来对表实施复杂的完整性约束，保持数据的一致性。

\2.   当触发器所保护的数据发生改变时，触发器会自动被激活。

\3.   般激活触发器的事件包括INSERT、UPDATE和DELETE事件。

\4.   MySQL提供了两个逻辑表NEW和OLD。

a)   NEW和OLD的表结构与触发器所在数据表的结构完全一 致，当触发器的执行完成之后，这两个表也会被自动删除。

b)   OLD表用来存放更新前的记录。

​        i.     对于UPDATE语句，OLD表中存放的是更新前的记录(更新完后即被删除)

​       ii.     对于DELETE语句，该表中存放的是被删除的记录。

c)    NEW表用来存放更新后的记录。 

​        i.     对于INSERT语句，NEW表中存放的是要插入的记录;对于UPDATE语句，该表中存放的是要更新的记录。

（1）触发器的创建：

CREATE TRIGGER trigger_name trigger_time trigger_event ON tbl_name FOR EACH ROW trigger_stmt;

 

其中，trigger_name参数指要创建的触发器的名称;

trigger_time 参数是指触发器执行的时间，它可以是BEFORE或AFTER，以指明触发器是在激活它的语句之前或之后触发;

trigger_event参数是指激活触发程序的语句类型，包括INSERT、UPDATE和DELETE;

tbl_name参数是指触发事件操作的表的名称;

FOR EACH ROW参数表示任何一条记录上的操作满足触发事件都会触发该触发器; //行级触发器 mysql不支持语句级触发器

trigger_stmt参数指触发器被触发后执行的程序语句，以begin...end开始。

 

*触发器返回问题：(在MYSQL5之后出现的问题)

解释：当触发器成功触发后将执行begin语句块内容，此时该内容不允许直接通过select直接查询返回结果集。

CREATE TRIGGER trigger_name trigger_time trigger_event ON tbl_name FOR EACH ROW

begin

select count(old.id) from tbl_name;  // 试图直接显示该结果，将触发Not allowed to return a result set from a trigger错误

end;

解决方案：将返回的结果集赋值进系统变量中，通过该变量另外查询达到直接返回的效果

CREATE TRIGGER trigger_name trigger_time trigger_event ON tbl_name FOR EACH ROW

begin

  set @a;

select count(old.id) into a from tbl_name;  // 将select结果返回到系统变量a中

end;

select @a;  // 在触发器结束后查询该变量

 

 