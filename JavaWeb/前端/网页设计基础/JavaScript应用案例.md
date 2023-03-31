具体JavaScript-API文档可以使用w3cschool网站进行查阅


JavaScript数组：（Array对象）

定义：数组在JavaScript中是以对象的形式存在的

定义JavaScript数组：
```javascript
数组对象实例名 = new Array()

var arr1 = new Array()  // 定义一个长度不定的数组

var arr2 = new Array(8)  // 定义一个长度为8的数组
```
（1）Array属性：
```javascript
length  // 与java一样，返回数组的长度
```
（2）Array方法：
```javascript
join()  // 返回数组中所有元素连接而成的字符串

reverse()  // 将数组元素逆转排列

sort()  // 对数组元素进行排序
```
（3）JavaScript与一些静态语言的数组对比：

- JavaScript中的数组允许一个数组中有不同的数据类型
```javascript
var arr = new Array(3)

arr[0] = 20  // int

arr[1] = false  // Boolean

arr[2] = "Hello"   // String
```
- JavaScript中数组的长度可以动态变化：

/*即使创建数组时定义了数组长度，但通过引用数组也可以使数组长度变长*/
```javascript
var arr = new Array(8)

arr[19] = 10  // 给arr数组第20个元素赋int值10，此时数组长度变为20
```
（4）JavaScript多维数组：（当数组元素是Array对象的实例时，就可以得到多维数组）
```javascript
var arr = new Array(8)

for(i=0; i<8; i++) arr[i] = new Array(5)  // 数组arr的每个元素都实例化一个数组，得到arr[8][5]二维数组
```

 

 