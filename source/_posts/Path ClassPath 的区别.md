title: Path ClassPath 的区别
tags:
  - Classpath
  - Path
id: 206
categories:
  - Java
date: 2015-05-15 12:24:25
---

ClassNotFoundException(所需要的支持类库放错了地方，并没有放在类路径CLASSPATH里面)

NoClassDefFoundError（运行函数没有放在path中）.

**1.path的作用**

**  **  path是系统用来指定可执行文件的完整路径，即使不在path中设置JDK的路径也可执行JAVA文件，但必须把完整的路径写出来，如C:\Program Files\Java\jdk1.6.0_10\bin\javac TheClass.java。path是用来搜索所执行的可执行文件路径的，如果执行的可执行文件不在当前目录下，那就会依次搜索path中设置的路径；而java的各种操作命令是在其安装路径中的bin目录下，所以在path中设置了JDK的安装目录后就不用再把java文件的完整路径写出来了，它会自动去path中设置的路径中去找；

**2.classpath的作用**

**    **classpath是指定你在程序中所使用的类（.class）文件所在的位置，就如在引入一个类时：import javax.swing.JTable这句话是告诉编译器要引入javax.swing这个包下的JTable类，而classpath就是告诉编译器该到哪里去找到这个类（前提是你在classpath中设置了这个类的路径）；如果你想要编译在当前目录下找，就加上“.”,如：.;C:\Program Files\Java\jdk\,这样编译器就会到当前目录和C:\Program Files\Java\jdk\去找javax.swing.JTable这个类；还提下：大多数人都是用Eclipse写程序，不设classpath也没关系，因为Eclipse有相关的配置；

path是os用
classpath java用
path里面不光有Java的bin，还可以包含许多其他的，tc啊，masm阿，只要在path中设了这些环境的路径，你在dos下的任何路径上都可以调用这些路径下的命令。
classpath是java专用的查找类的路径

系统变量是环境变量的一种，环境变量一种仅本用户适用，另一种即系统变量整个系统的用户都适用,两者都可以在使用应用程序时提供快捷.一般在编辑java文件或者C#文件时需要修改,设计到多个文件夹之间的切换时也可以根据自己的需要设置.
简单的说就是，如果设置系统变量和用户变量，都叫做设置环境变量，设置系统变量时，该系统的所有帐号的用户都可以使用，但是设置用户变量时，其他的帐号登陆时就不一定可以使用。

下面以java环境变量为例设置方法:
1、如果是Win95/98，在\autoexec.bat的最后面添加如下3行语句：
JAVA_HOME=c:\j2sdk1.4.1
PATH=%JAVA_HOME%\bin;%PATH%
CLASSPATH=.;%JAVA_HOME%\lib
看好了CLASSPATH中第一个"."，这个代表当前目录，很多人HelloWorld没有运行起来大多是这个原因。

2、如果是Win2000或者XP，使用鼠标右击"我的电脑"-&gt;属性-&gt;高级-&gt;环境变量
系统变量-&gt;新建-&gt;变量名：JAVA_HOME 变量值：c:\j2sdk1.4.1
系统变量-&gt;新建-&gt;变量名：CLASSPATH 变量值：.;%JAVA_HOME%\lib
系统变量-&gt;编辑-&gt;变量名：Path 在变量值的最前面加上：%JAVA_HOME%\bin;
CLASSPATH前面的那个"."和上面的意义是一样的。

3、如果是Linux用户
在你的环境中，通常我加在.bashrc文件中，你可以加在你的Profile文件中。
/usr/local/jdk 为你安装jdk的目录。
export JAVA_HOME=/usr/local/jdk
export CLASSPATH=.:$JAVA_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin