# 动态SQL基本概念

## 1、基本概念

- 动态 SQL 是 MyBatis 的强大特性之一。如果你使用过 JDBC 或其它类似的框架，你应该能理解根据不同条件拼接 SQL 语句有多痛苦，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。利用动态 SQL，可以彻底摆脱这种痛苦。
- 如JDBC中String sql = "select * from user"; String sql2="where id=1";
- 要将sql和sql2拼接起来必须要注意其中要有一个空格等等。
- 与JSP技术中的JSTL标签类似。
- 在MyBatis中只有四大动态标签：
    - if
    - choose (when, otherwise)
    - trim (where, set)
    - foreach

## 2、OGNL表达式

- MyBatis中的进行判断的强大功能是基于OGNL表达式进行执行的。在MyBaits动态标签的test属性中都是使用OGNL表达式进行判断的。
- OGNL概念：OGNL ( Object Graph Navigation Language )对象图导航语旨，这是一种强大的表达式语，通过它可以非常方便的来操作对象属性。类似于我们的EL表达式等。
- OGNL官方文档：http://commons.apache.org/proper/commons-ognl/language-guide.html
- 简单的元素判断规则：
    - 访问对象属性：person.name
    - 调用方法：person.getName()
    - 调用静态属性/方法：@java.lang.Math@PI / java.util.UUID@randomUUID()
    - 调用构造方法：new com.atguigu.bean.Person('admin').name
    - 运算符：+,-,*,/,%
    - 逻辑运算符：in,not in,>,>=,<,<=,==,!=

### （1）bing标签

- bind 元素允许你在 OGNL 表达式以外创建一个变量，并将其绑定到当前的上下文。
```xml
<select id="selectBlogsLike" resultType="Blog">
  <!-- name：唯一标识；value：OGNL表达式，可以采用''进行拼接 -->
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}  <!-- 直接通过#{}即可引用bing标签 -->
</select>
```


### （2）IF

- 使用if标签最常见的地方就是根据条件包含sql中的WHERE子句的一部分：

```xml
<select id="findActiveBlogWithTitleLike" resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = 'ACTIVE'
  <!-- test：判断表达式（OGNL表达式） -->
  <if test="title != null">
     AND title like #{title}
  </if>
</select>

// 预编译分析：
  // 1 传入了title字段时：
  select * from BLOG where state='ACTIVE' and title like ?;
  // 2 没传入title字段时：
  select * from BLOG where state='ACTIVE';

// test中OGNL表达式探究：
  // 表达式的书写通常需要包含查找掩码或通配符字符
  // 如：author != null and author.name != null使用and代替&&符号
```


### （3）trim (where, set)

- where标签与if标签：

- IF案例的漏洞：如果将第一个state=‘ACTIVE’也设置为``<if>``动态条件：


```xml
<select id="findActiveBlogLike" resultType="Blog">
 SELECT * FROM BLOG
 WHERE
 <if test="state != null">
  state = #{state}
 </if>
 <if test="title != null">
  AND title like #{title}
 </if>
 <if test="author != null and author.name != null">
  AND author_name like #{author.name}
 </if>
</select>
```
- 如果没有匹配的条件会怎么样？最终这条 SQL 会变成这样：

```mysql
SELECT * FROM BLOG
WHERE
```
- 如果匹配的只是第二个条件又会怎样？这条 SQL 会是这样：

```mysql
SELECT * FROM BLOG
WHERE
AND title like ‘someTitle’
```

- 
    解决IF漏洞的方案：

    - 使用where标签：where 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头（注意：只会去掉开头的）为 “AND” 或 “OR”，where 元素也会将它们去除。

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG
  <where>  <!-- 在这些if标签的外面套一层where标签 -->
     <if test="state != null">
       state = #{state}
     </if>
     <if test="title != null">
       AND title like #{title}
     </if>
     <if test="author != null and author.name != null">
       AND author_name like #{author.name}
     </if>
  </where>
</select>
```


2、使用trim标签自定义where标签的功能：

- 在使用where标签时，他只能将开头的ANDOR删除，假设此时AND写在语句的后面，那么where标签将不起作用。

```xml
<trim
  <!-- 以下操作是指：在trim标签内所有的字符串拼接完成后再进行的添加删除 -->
  prefix="WHERE"  <!-- 前缀添加 -->
  prefixOverrides="AND|OR"  <!-- 前缀删除 -->
  suffix=""  <!-- 后缀添加 -->
  suffixOverrides=""  <!-- 后缀删除 -->
  \>

  <if test="state != null">
     state = #{state}
  </if>
  <if test="title != null">
     AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
     AND author_name like #{author.name}
  </if>
</trim>
```


3、使用set标签进行数据的动态更新：

- 在update进行数据更新过程中，可以使用set-if标签对传入的数据进行判断。如以下例子，set 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）。

```xml
<update id="updateAuthorIfNecessary">
  update Author
     <set>
       <if test="username != null">username=#{username},</if>
       <if test="password != null">password=#{password},</if>
       <if test="email != null">email=#{email},</if>
       <if test="bio != null">bio=#{bio}</if>
     </set>
  where id=#{id}
</update>
```
- 也可也使用trim标签定义与set标签一样的功能：

```xml
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```


### （4）choose (when, otherwise)

- Java中带了break的switch-case-default标签效果类似。

    - switch -> choose

    - case -> when

    - default -> otherwise

- 注意：Java中switch是根据值进行查询，而MyBaits中choose指的是从上至下的检查when标签中test属性的表达式。如果符合就选择这条标签中的内容，然后执行break跳出choose标签。

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
     <choose>
       <when test="title != null">
         AND title like #{title}
       </when>
       <when test="author != null and author.name != null">
         AND author_name like #{author.name}
       </when>
       <otherwise>
         AND featured = 1
       </otherwise>
     </choose>
</select>
```
### （5）foreach

- 动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。

- 通过此标签与其他标签结合可以实现批量插入，批量修改等操作。

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST
  WHERE ID in
     <foreach 
       collection="list"  <!-- 指定要遍历的集合 -->
       item="item"  <!-- 当前遍历出的元素赋值给指定的变量 -->
       index="index"  <!-- 开始的索引变量：list是索引，map是key -->
       separator=","  <!-- 每个元素之间的分隔符 -->
       open="("  <!-- 遍历出的所有结果添加一个开始的字符 -->
       close=")"  <!-- 遍历出的所有结果添加一个结束的字符 -->
     \>
       \#{item}
     </foreach>
</select>

// 执行过程分析:假设list集合中有元素1，2，3，index=0
  1 拼接开始字符 (
  2 首先遍历出第一个元素 1 
  3 添加separator分隔符 ,
  4 遍历出第二个元素 2
  5 添加separator分隔符 ,
  6 遍历出第三个元素 3
  7 拼接结束字符 )
  结果：(1,2,3)
```





