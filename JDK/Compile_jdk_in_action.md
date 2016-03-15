1.6 实战：自己编译 JDK

想要一探 JDK 内部的实现机制，最便捷的路径之一就是自己编译一套 JDK, 通过阅读和跟踪 调试 JDK 源码去了解 Java 技术体系的原理，虽然门槛会高一点，但肯定会比阅读，各种书籍、文章更加贴近本质。另外，JDK中的很多底层方法都是本地化的（Native）的，需要跟踪这些方法的运作活对 JDK 进行Hack的时候，都需要自己编译一套 JDK.

我选择了 OpenJDK 进行这次编译实战

1.6.1 获取 JDK 源码
从Source Bundle Releases页面 https://jdk7.java.net/source.html
http://download.java.net/openjdk/jdk7/
这是OpenJDK / jdk7 / jdk7 log： http://hg.openjdk.java.net/jdk7/jdk7
这是openJdk1.8的 http://download.java.net/openjdk/jdk8/
取得打好包的源码，到本地直接解压即可


这时候恰好碰到一个好帖：系统介绍 jdk 和 openjdk 区别的好帖；
http://stackoverflow.com/questions/11547458/what-is-the-difference-between-jvm-jdk-jre-openjdk

Linux 下编译jdk的好帖：
http://rednaxelafx.iteye.com/blog/875957
Windows 下编译jdk的好帖：
http://www.iteye.com/topic/1097344/

下载完之后进行解压：

1.6.2 构建编译环境：
准备编译环境的第一步是去安装一个CYGWIN （音标； [’sigwin]）。这是一个在Windows平台下模拟Linux运行环境的软件，提供了一系列的Linux命令支持。需要CYGWIN的原因是在编译中要使用GNU Make来执行Makefile文件（C/C++程序员肯定很熟悉，如果只使用Java，那把这个东西当做C++版本的ANT看待就可以了）。安装CYGWIN时不能直接默认安装，因为表1-2中所示的工具都不会进行默认安装，但又是编译过程中需要的，因此要在图1-6的安装界面中进行手工选择。

![](../images/1/1.6.2—1.jpeg)


CYGWIN安装时的定制包选择界面如图1-6所示： 


建立编译环境的第二步是安装编译器。JDK中最核心的代码（Java虚拟机及JDK中Native方法的实现等）是使用C++语言及少量的C语言编写的，官方文档中说他们的内部开发环境是在Microsoft Visual Studio C++ 2003（VS2003）中进行编译，同时也在Microsoft Visual Studio C++ 2010（VS2010）中测试过，所以最好只选择这两个编译器之一进行编译。如果选择VS2010，那么在编译器之中已经包含了Windows SDK v 7.0a，否则可能还要自己去下载这个SDK，并且更新PlatformSDK目录。由于笔者没有购买Visual Studio 2010的IDE，所以仅仅下载了VS2010 Express中提取出来的C++编译器，这部分是免费的，但单独安装好编译器比较麻烦。建议读者选择使用整套Visual Studio C++ 2010或Visual Studio C++ 2010 Express版进行编译。 
　　需要特别注意的一点：CYGWIN和VS2010安装之后都会在操作系统的PATH环境变量中写入自己的bin目录路径，必须检查并保证VS2010的bin目录一定要在CYGWIN的bin目录之前，因为这两个软件的bin目录之中各自都有个连接器“link.exe”，但是只有VS2010中的连接器可以完成OpenJDK的编译。 
　　准备JDK编译环境的第三步就是下载一个已经编译好了的JDK。这听起来也许有点滑稽——要用鸡蛋孵小鸡还真得必须先养一只母鸡呀？但仔细想想其实这个步骤很合理：因为JDK包含的各个部分（Hotspot、JDK API、JAXWS、JAXP……）有的是使用C++编写的，而更多的代码则是使用Java自身实现的，因此编译这些Java代码需要用到一个可用的JDK，官方称这个JDK为“Bootstrap JDK”。而编译OpenJDK 7的话，Bootstrap JDK必须使用JDK6 Update 14或之后的版本，笔者选用的是JDK6 Update 21。 
　　最后一个步骤是下载一个Apache ANT，JDK中Java代码部分都是使用ANT脚本进行编译的，ANT版本要求在1.6.5以上，这部分是Java的基础知识，对本书的读者来说应该没有难度，笔者就不再详述。 
　　
下载：microsoft visual studio 2010
　　 https://www.microsoft.com/en-us/Search/result.aspx?q=visual%20studio%202010%20c%2B%2B
　　 
visual studio C++ 的指导教程:
　　 https://msdn.microsoft.com/zh-cn/ms235630(v=vs.100)






