## 一、前提概述

（1）java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。

（2）输入输出：其本质上是一个相对的概念，一定要考虑到当前的站位（一般指内存）所对应的输入和输出.

- 如图第一个：对于程序来说，文件是输入进来；而对于文件来说，程序是输出出去。


![clipboard.png](IO%E6%B5%81%E8%AF%A6%E8%A7%A3.assets/clip_image002.gif)

（3）流的分类：

- 按操作数据单位不同分为：字节流(8 bit)， 字符流(16 bit)

- 按数据流的流向不同分为：输入流，输出流

- 按流的角色的不同分为：节点流，处理流


| （抽象基类） | 字节流       | 字符流 |
| ------------ | ------------ | ------ |
| 输入流       | InputStream  | Reader |
| 输出流       | OutputStream | Writer |

- Java的IO流涉及40多个类，实际上非常规则，都是从4个抽象基类进行派生的。


（3.1）IO流体系：

![clipboard.png](IO%E6%B5%81%E8%AF%A6%E8%A7%A3.assets/clip_image004.gif)

- 需要根据继承的基类所命名的后缀判断流的昵称。如BufferedOuputStream是个字节输出流


（4）基本的操作思想：（IO操作基本围绕4点进行）

- File类的实例化

- 流的实例化

- 读入或输入的操作

- 资源的关闭


（5）其他注意：

- 一般对于文本文件（.txt .java .c .cpp等），使用字符流处理；对于非文本文件（.jpg.mp3.doc.ppt等），使用字节流处理。

- 在使用流的时候一定要记住处理异常，一般使用try块处理，以正常关闭流


## 二、字符流：

（1）基本的文件字符输入流读取：（FileReader）

- 基本读取示例：

```java
// 1.实例化File类的对象，指明要操作的文件
File test = new File("test文件.txt");

// 2.提供具体的流
FileReader fr = new FileReader(test);

// 3.数据读入：read()：返回读入的一个字符（int型），如果到达文件末尾，返回-1
int data = fr.read();

while(data != -1) {
  System.out.print((char) data);
  data = fr.read();
}

// 关闭流：一般关闭操作放在异常处理finally里中进行，以保证能够正常关闭
fr.close();
```
- 改进读取示例：（上一个示例每次仅读入一个字符，效率慢）

```java
File test = new File("test文件.txt");
FileReader fr = new FileReader(test);

// 将读入的字符放置到数组cbuf中，即每次读入5个字符。再一并输出这个数组。
char[] cbuf = new char[5];

// read(<数组>)：读入数据直到填满数组，返回每次填充数组的字符个数。如果到达文件末尾，返回-1
int len;

while((len = fr.read(cbuf)) != -1) {
  for(int i = 0;i <len;i++) {
    System.out.print(cbuf[i]);
  }
}

fr.close();
```
（2）基本的文件字符写入输入流：（FileWriter）

- 基本写入示例：


```java
// 在进行输出操作时，对应的File的文件可以不存在且不会异常，若不存在，则会自动新建一个文件并写入数据
File file = new File("test文件.txt");

// FileWriter(File file, boolean append); 为false（默认）时，每次运行会对源文件进行覆盖，反之则会继续在文件后写入
FileWriter fw = new FileWriter(file);

// 写出操作
fw.write("<字符串>");

fw.close();
```
## 三、字节流：

- 理解：字节流和字符流使用过程基本一致。字节流只是一种不同的传输方式而已，在读取和写入过程中，字符流是以字符为单位进行读取写入的，而字节流是以二进制的形式进行读取写入的，在一些非文本文件中，是没有字符的，他们都以二进制进行保存，所以进行这些文件的读取写入时候，需要使用字节流。
- 具体读写API详细参考InputStream、OutputStream类JDK文档

## 四、缓冲流：

- 解释：缓冲流是包装处理流的一种，用于对基本流（字节、字符流）的包装，使得流传输的效率更加高效。

- 提高读写速度的原因：内部提供了一个缓冲区

- 示例：复制一个文本文件：

```java
// 1.造文件
  File test = new File("test.txt");
  File test1 = new File("test1.txt");

// 2.造流：此时相当于缓冲流包装了字符流

  // 2.1 造字符流
  FileReader Out = new FileReader(test);
  FileWriter Put = new FileWriter(test1);

  // 2.2 造缓冲流
  BufferedReader OutBuffer = new BufferedReader(Out);
  BufferedWriter PutBuffer = new BufferedWriter(Put);

// 3.输入输出操作
  int isEnd;
  while((isEnd = OutBuffer.read()) != -1) {
    PutBuffer.write((char)isEnd);
  }

// 4.关闭流：要求先关闭外层的流，再关闭内层的流
  OutBuffer.close();
  PutBuffer.close();

  // 4.1 关闭外层的流后内层的流也会自动关闭，所以内层流关闭代码可以省略
  Out.close();
  Put.close();
```

- 形象的包装代码改进：

```java
/* 层层递进的包装关系：文件流->字符流->缓冲流 */
BufferedReader OutBuffer = new BufferedReader(
    new FileReader(
        new File("test.txt")
    )
);

BufferedWriter PutBuffer = new BufferedWriter(
    new FileWriter(
        new File("test1.txt")
    )
);
```

## 五、转换流：

- 解释：转换流提供了再字节流和字符流之间的转换

- 两个转换流的java-api：

    - InputStreamReader：字节输入流->字符输入流

    - OutputStreamWriter：字符输出流->字节输出流

- 注意：这两个流属于字符流


![clipboard.png](IO%E6%B5%81%E8%AF%A6%E8%A7%A3.assets/clip_image006.gif)

## 六、其他流：

- 标准输入流

    - 即System.in

- 标准输出流

    - 即System.out

- 打印流

    - 通过设置打印的位置即可指定标准流的流向位置

- 数据流

    - 用于读取或写出基本数据类型的变量或字符串，对象流的低级版：它无法对对象进行操作


## 七、对象流：

（1）解释：ObjectInputStream和OjbectOutputSteam用于存储和读取基本数据类型数据或对象的处理流。它的强大之处就是可以把Java中的对象写入到数据源中，也能把对象从数据源中还原回来。

- 序列化：用ObjectOutputStream类保存基本类型数据或对象的机制

- 反序列化：用ObjectInputStream类读取基本类型数据或对象的机制

- ObjectOutputStream和IObjectInputStream不能序列化static和transient修饰的成员变量


（1.1）对象的序列化：

- 对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。当其它程序获取了这种二进制流，就可以恢复成原来的Java对象

- 序列化的好处在于可将任何实现了Serializable接口的对象转化为字节数据，使其在保存和传输时可被还原

- 序列化是RMI ( Remote Method Invoke-远程方法调用)过程的参数和返回值都必须实现的机制，而RMI是JavaEE的基础。因此序列化机制是JavaEE平台的基础

- 如果需要让某个对象支持序列化机制，则必须让对象所属的类及其属性是可序列化的，为了让某个类是可序列化的，必须要当前类提供一个全局常量serialVersionUID（序列号值，可以随便写），而且该类必须实现如下两个接口之一 。否则，会抛出NotSerializableException异常

    - Serializable

    - Externalizable


（2）基本操作：

- 序列化过程：

```java
// 1.造输出流
ObjectOutputStream oos = new ObjectOutputStream(
   new FileOutputStream("test.dat")  // 文件类型为.dat
);

// 2.写出对象
oos.writeObject(new String("HelloWorld!!!"));

// 3.进行刷新操作(即时写入文件，可选操作)
oos.flush();

// 4.关闭流
oos.close();
```
- 反序列化过程：

```java
// 1.造读取流
ObjectInputStream ois = new ObjectInputStream(
  new FileInputStream("tset.dat")
);

// 2.读取对象
Object obj = ois.readObject();

// 3.对对象进行操作
String str = (String) obj;

// 4.关闭流
ois.close();
```


（3）自定义类的序列化过程：
```java
class Test implements Serializable {
  // 提供序列号值
  private static final long serialversionUID = 15345631567L;

  // 定义变量和方法等
  // <代码块>：基本数据类型是可序列化的，若有其他的数据类型，则该数据类型也必须调整至可序列化
}  
```
（3.1）serialversionUID解析：

- 凡是实现Serializable接口的类都有一个表示序列化版本标识符的静态常量：
```java
private static final long serialVersionUID;
```
- serialVersionUID用来表明类的不同版本间的兼容性。简言之，其目的是以序列化对象进行版本控制，有关各版本反序列化时是否兼容。

- 如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自动生成的。若类的实例变量做了修改，serialVersionUID 可能发生变化。故建议，显式声明。

- 简单来说，Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。在进行反序列化时，JVM 会把传来的字节流中的serialVersionUID.与木地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)


## 八、随机存储文件流：

（1）随机存储文件流解释

- RandomAccessFile类声明在java.io包下，但直接继承于java.lang.Object类。并且它实现了Datalnput、DataOutput这两个接口（IO体系中的特殊流），也就意味着这个类既可以读也可以写。

- RandomAccessFile类支持“随机访问”的方式，程序可以直接跳到文件的任意地方来读、写文件（一个流既可以作为输入流，又可以作为输出流）

    - 支持只访问文件的部分内容

    - 可以向已存在的文件后追加内容

- RandomAccessFile对象包含一个记录指针，用以标示当前读写处的位置。

- RandomAccessFile类对象可以自由移动记录指针:

    - long getFilePointer()：获取文件记录指针的当前位置

    - void seek(long pos)：将文件记录指针定位到pos位置


（2）API-构造方法：
```java
public RandomAccessFile(File file, String mode)
public RandomAccessFile(String name, String mode)
```
- 创建RandomAccessFile类实例需要指定一个mode参数，该参数指定RandomAccessFile的访问模式:

    -   r:以只读方式打开


    -   rw:打开以便读取和写入


    -   rwd:打开以便读取和写入;同步文件内容的更新;


    -   rws:打开以便读取和写入;同步文件内容和元数据的更新;


- 如果模式为只读r。则不会创建文件，而是会去读取一个已经存在的文件，如果读取的文件不存在则会出现异常。

- 如果模式为rw读写。如果文件不存在则会去创建文件，如果存在则不会创建。在写入过程中，字符是对原有的文件进行部分覆盖（默认从头覆盖）。


## 九、更多的IO流：

- JAVA-NIO：Java NIO (New IO，Non-Blocking IO)是从Java 1.4版本开始引入的一套新的IO API，可以替代标准的Java 1O API。NIO与原来的I0有同样的作用和目的，但是使用的方式完全不同，NIO支持面向缓冲区的(IO是面向流的)、基于通道的I0操作。NIO将以更加高效的方式进行文件的读写操作。


- Path

- Paths

- Files


 

 

 