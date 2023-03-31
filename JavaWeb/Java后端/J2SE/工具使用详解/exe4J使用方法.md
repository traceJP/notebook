**用途：用于将java中的Class文件打包成一个可执行exe文件，该exe文件可在无jdk环境PC下运行。**

 

（1）先将你要转化的文件打包成一个模块（注意：只能转化一整个模块，不能单独转化部分）

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image002.gif)

 

（2）项目结构设置（设置打包成jar文件前的文件结构）

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image004.gif)

 

2.1：点击加号添加jar文件结构

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image006.gif)

 

2.2：项目打包的存放路径一定是在src文件下的，否则将报错

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image008.gif)

 

 

（3）项目打包

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image010.gif)

 

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image012.gif)

 

3.1：通过以上两个步骤，将在Java的out文件中生成jar文件

  该jar文件可以通过cmd命令 java -jar 文件路径  启动

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image014.gif)

 

3.1.1：如图，成功启动说明打包成功

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image016.gif)

 

3.2：之后可在文件路径中将该jar文件移动到其他文件夹

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image018.gif)

 

 

 

 

（4）exe4J启动

 

1.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image020.gif)

 

2.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image022.gif)

 

3.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image024.gif)

 

4.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image026.gif)

4.1.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image028.gif)

 

5.

-Dfile.encoding=utf-8

 

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image030.gif)

5.1.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image032.gif)

5.2.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image034.gif)

 

6.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image036.gif)

6.1.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image038.gif)

6.2。

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image040.gif)

6.3.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image042.gif)

6.4.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image044.gif)

 

7.

![clipboard.png](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/clip_image046.gif)

 

8.

![image-20230322081412180](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/image-20230322081412180.png)

 

9-10.

![image-20230322081421416](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/image-20230322081421416.png)

 

11.

 ![image-20230322081427810](exe4J%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.assets/image-20230322081427810.png)
