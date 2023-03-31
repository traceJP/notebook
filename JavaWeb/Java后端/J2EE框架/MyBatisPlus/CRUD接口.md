# 一、BaseMappe接口

- 以下T泛型都指代User-POJO类，并且对应数据库user表
- insert、delete、update返回值都为记录影响的条数（int）。

## 1、insert

（1）int insert(T entity)

```java
// 插入一条记录
User user = new User();
user.setXXX(YYY);  // INSERT INTO user(xxx) VALUES(YYY)
...
mapper.insert(user);  // 插入一条记录

// 插入记录后会自动将自增id映射到POJO中
user.getId();  // 获取当前记录的自增id
```


## 2、delete

（1）int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);

- 根据 entity 条件，删除记录


（2）int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```java
// 删除（根据ID 批量删除）
List<Integer> list = new ArrayList<>();
list.add(..);  // WHERE id in(list)
...
mapper.deleteBatchIds(list);  // 删除记录：条件为list集合中所有id值
```
（3）int deleteById(Serializable id);
```java
// 根据 ID 删除
// WHERE id = 1
mapper.deleteById(1);  // 删除id为1的记录
```
（4）int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```java
// 根据 columnMap 条件，删除记录
Map<String, Object> map = new HashMap<>();

// String为表字段，Object为字段对应的值
map.put("email", "123@123.com");  // WHERE email = "123@123.com"
map.put(...);
...
userMapper.deleteByMap(map);  // 删除记录，条件为map中封装的条件
```


## 3、update

（1）int update(@Param(Constants.ENTITY) T entity, @Param(Constants.WRAPPER) Wrapper<T> updateWrapper);

- 根据 whereEntity 条件，更新记录


（2）int updateById(@Param(Constants.ENTITY) T entity);
```java
// 根据 ID 修改
User user = new User();
user.setId(3);  // WHERE id = 3
user.setXXX(YYY);  // SET xxx = YYY
...
mapper.updateById(user);
```


## 4、select

（1）T selectById(Serializable id);
```java
// 根据 ID 查询
User user = mapper.selectById(1);  // SELECT * FROM user WHERE id=1;
```
（2）T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 entity 条件，查询一条记录

（3）List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

```java
// 查询（根据ID 批量查询）
List<Integer> list = new ArrayList<>();
list.add(1);  // WHERE id in(list)
list.add(3);
...
List<User> res = userMapper.selectBatchIds(list);
```
（4）List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

- 根据 entity 条件，查询全部记录


（5）List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```java
// 查询（根据 columnMap 条件）
Map<String, Object> map = new HashMap<>();
map.put("age", 21);  // WHERE age = 21
...
List<User> res = userMapper.selectByMap(map);
```
（6）List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

- 根据 Wrapper 条件，查询全部记录


（7）List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

- 根据 Wrapper 条件，查询全部记录。注意： 只返回第一个字段的值


（8）分页：
```java
// 根据 entity 条件，查询全部记录（并翻页）
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询全部记录（并翻页）
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询总记录数
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```


# 二、IService接口

- 其IService接口本质上是封装了BaseMapper接口，并再BaseMapper接口上做一个增强操作，并提供对数据库记录批处理的方法。具体方法可以参考官方文档。

- IService接口是介于MVC三层中，mapper层和service层的中心。

- 使用该接口需要：

    - 用mapper层接口继承BaseMapper接口。

    - 用service层接口继承IService接口。

    - 用service实现类继承ServiceImpl接口，并实现service层接口。


1、mapper接口：
```java
public interface UserMapper extends BaseMapper<User> {
}
```
2、service接口：
```java
public interface UserService extends IService<User> {
}
```
3、service实现类：
```java
public class UserServiceImpl extends ServiceImpl<UserMapper, User>
	implements UserService {
    
}
```







