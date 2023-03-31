# 一、Java与sql类型转换：

| Java类型           | SQL类型                  |
| ------------------ | ------------------------ |
| boolean            | BIT                      |
| byte               | TINYINT                  |
| short              | SMALLINT                 |
| int                | INTEGER                  |
| long               | BIGINT                   |
| String             | CHAR/VARCHAR/LONGVARCHAR |
| byte array         | BINARY/VAR BINARY        |
| java.sql.Date      | DATE                     |
| java.sql.Time      | TIME                     |
| java.sql.Timestamp | TIMESTAMP                |

# 二、函数拓展：

- DATEDIFF()函数：
    - 解释：用于计算两个时间的天数差值

- DATEDIFF(startdate,enddate)
    - startdate：开始日期
    - enddate ：结束日期
- now()：获取当前的系统日期+时间
- MOD()函数：
    - MOD(x,y)   返回x对y的取余余数，等同 x%y ，但比x%y效率高

# SQL语句补充：

## （1）查看数据库所有的存储过程：
```mysql
select * from mysql.proc where db='数据库名';
```

## （2）利用cmd导入sql文件：

1.通过use选择你想导入到哪个数据库

2.通过source命令进行导入
```mysql
source命令使用方法： source [sql文件位置];
```

## （3）CHECK约束（检查约束）：

- 解释：在表中使用，可以规定字段记录的取值范围。

- 注意：在Mysql中只能写在所有字段创建结束的后面，而SQLServerOracle可以直接写在字段后面


建表时使用:
```mysql
create table 表名(

  id int (10),

  CHECK(id in(1,2,3))  // 表中字段id的取值只能为1，2，3中的一个

);
```
- 建表后修改使用:

```mysql
alter table 表名 ADD CONSTRAINT 检查约束名 CHECK(<约束条件>);
```
- 删除约束:

```mysql
alter table 表名 DROP CHECK 检查约束名;  
```
- 如何使用CHECK约束让一个字段的取值只能为数字：

```mysql
n check (id like '[0-9][0-9][0-9][0-9][0-9][0-9]')  // id字段类型为char(6)
```

## （4）CUBE关键字（ROLLUP关键字类似）

- 解释：主要作用是自动对group by子句中列出的字段进行分组汇总运算。


- 分组分类汇总：

```mysql
cube(a,b)  #  统计分组分类包含：(a,b)、(a)、(b)、()

cube(a,b,c) # 统计分组分类包含：(a,b,c)、(a,b)、(a,c)、(b,c)、(a)、(b)、(c)、()

group by 字段1,字段2...字段n with cube;

# 在mysql5.6.17版本中,只定义了cube,但是不支持cube操作
```
## （4）UNION、UNION ALL关键字

- 解释：UNION将多条查询语句查询出来的结果合并在一起显示，并去除重复的记录。UNION ALL不会去除重复记录

```mysql
<查询语句1>
UNION | UNION ALL
<查询语句2>
UNION | UNION ALL
...
<查询语句n>
# 每个查询语句中可能会出现查询的字段不同，有可能查询语句1要查询两个字段，而查询语句2要查询三个字段，当两个查询结果组合在一起时，会出现字段对齐不了的现象
# 解决方案:以查询最长的字段为基准，将查询短的字段中补上null，以达到字段对齐的效果
```

## 知识体系补充：

（1）mysql中的 不等于 符号为 <>      //与c语言中的 != 不一致

（2）视图的消解：

- 视图：视图所有的数据都是虚无的，都是来源于查询的原表。当视图的数据被增删改之后，查询的原表也会变化。

- 消解：当增删改时，最终是在原表上进行操作，即过程为现在原表上增删改，再将定义视图的查询语句运行一遍，然后视图再被更新，以完成视图的增删改。


（3）like模糊查询在存储过程中的用法：

- 解释：当传入的参数需要用于lke中的查询条件时

- 用法：

```mysql
condition like concat(a,'%');  # a为传入的参数

# 注意：一定要使用concat()函数使like模糊出来的条件只有一个
```

 

 