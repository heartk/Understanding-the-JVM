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

