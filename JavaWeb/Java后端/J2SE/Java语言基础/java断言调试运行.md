  断言：

**若未打开开关则会出现：**

- **断言语句在编译运行过程中被跳过**


**断言语句书写前需要打开运行断言的开关：**

运行时要屏蔽断言，可以用如下方法：
```shell
java –disableassertions | java –da 类名
```
运行时要允许断言，可以用如下方法：
```shell
java –enableassertions | java –ea 类名
```


**在idea编译器中需要：**

Run -> Edit Configurations -> VM options 中添加 -ea 即可

 

![clipboard.png](java%E6%96%AD%E8%A8%80%E8%B0%83%E8%AF%95%E8%BF%90%E8%A1%8C.assets/clip_image002.gif)

 

![clipboard.png](java%E6%96%AD%E8%A8%80%E8%B0%83%E8%AF%95%E8%BF%90%E8%A1%8C.assets/clip_image004.gif)

 

 