- java.io.File类:文件和文件目录路径的抽象表示形式，与平台无关

- File能新建、删除、重命名文件和目录，但File不能访问文件内容本身。如果需要访问文件内容本身，则需要使用输入/输出流。

- 想要在Java程序中表示-一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象， 可能没有一个真实存在的文件或目录。

- File对象可以作为参数传递给流的构造器


（1）File构造函数：

```java
File file1 = new File("D:");  // 代表文件D盘的file对象

File file2 = new File("D:", "testfile\\test.txt");  // 代表文件D:\testfile\test.txt的file对象

File file3 = new File(file1, "testfile\\test.txt");  // 代表文件D:\testfile\test.txt的file对象
```
- 关于路径的分隔符：

    - 路径中的每级目录之间用一个路径分隔符隔开。

    - 路径分隔符和系统有关：windows 和DOS系统默认使用“\”来表示，UNIX和URL使用“/” 来表示。

    - Java程序支持跨平台运行，因此路径分隔符要慎用。

    - 为了解决这个隐患，File类提供 了一个常量：public static final String separator。根据操作系统，动态的提供分隔符。

```java
File file = new File("D:\\test.txt");  // 为保证斜杠不被当成转义字符，需要使用双斜杠

File file = new File("D:" + File.separator + "test.txt");
```


（2）常用方法：（增删改查）

- File类的获取功能：

```java
public String getAbsolutePath()  // 获取绝对路径

public String getPath()  // 获取路径

public String getName()  // 获取名称

public String getParent()  // 获取上层文件目录路径。若无，返回null

public long length()  // 获取文件长度(即:字节数)。不能获取目录的长度。

public long lastModified()  // 获取最后一次的修改时间，毫秒值

public String[] list()  // 获取指定目录下的所有文件或者文件目录的名称数组

public File[] listFiles()  // 获取指定目录下的所有文件或者文件目录的File数组
```
- File类的重命名功能：

```java
public boolean renameTo(File dest)  // 把文件重命名为指定的文件路径
```
- File类的判断功能：

```java
public boolean isDirectory()  // 判断是否是文件目录

public boolean isFile()  // 判断是否是文件

public boolean exists()  // 判断是否存在

public boolean canRead()  // 判断是否可读

public boolean canWrite()  // 判断是否可写，

public boolean isHidden()  // 判断是否隐藏
```
- File类的创建功能：

```java
/* 如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目路径下。 */

public boolean createNewFile()   // 创建文件。若文件存在，则不创建，返回false

public boolean mkdir()  // 创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的，上层目录不存在，也不创建。

public boolean mkdirs()  // 创建文件目录。如果上层文件目录不存在，一并创建
```

- File类的删除功能：


```java
public boolean delete():删除文件或者文件夹

/* Java中的删除不走回收站。要删除一个文件目录，请注意该文件目录内不能包含文件或者文件目录 */
```





