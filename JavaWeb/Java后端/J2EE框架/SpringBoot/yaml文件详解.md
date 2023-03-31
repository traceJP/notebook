## 一、配置文件

- 在SpringBoot中使用一个全局配置文件修改自动配置中的默认参数，其中名称固定为application
    - application.properties
    - application.yml
    - application.yaml（与yml一样）
- 其中yml是YAML（YAML Ain't Markup Language）语言的文件，以数据为中心，比Json、xml等更适合做配置文件。其语法类似Python语言。
    - YMAL：A Markup Language（是一个标记语言）
    - YMAL：Isn't Markup Language（不是一个标记语言）
- yml和xml对比：

- Yml

```yml
server:
 port: 8081
```
- Xml

```xml
<server>
  <port>8081</port>
</server>  
```
## 二、基本语法

- 使用Key<冒号><空格>Value 来表示一对键值对：

    - 注意中间的空格不能省略

- 注释：

    - 使用#行注释

- 以空格缩进控制对象的层级关系：（标准空两格）

```yml
server:
 port: 8081
 path: /hello  # 同一层级
```
- 属性和值大小写敏感

- 值的数据类型：

    - 其中字符串默认不用加上单引号或双引号：其引号有特殊的意义
- 单引号：不会转义特殊字符（"test\ntest" -> test\ntest）
- 双引号：会转义特殊字符（"test\ntest" -> test换行test）
```yml
person:
  userName: zhangsan  # String
  boos: true  # boolean
  birth: 2020/1/1  # Date
  age: 18  # int 
  salarys: 123.123  # float
 
  interests: [篮球, 足球]  # 数组(set-list)写法1
  interests:  # 数组(set-list)写法2
    - 篮球  # -后的空格不能省略
    - 足球

  score:  # map写法1
    english: 80  # map中键值对
    math: 90
    
  score: {english:80, math:90}  #map写法2：其大括号中的冒号无需加空格
```





