title: Sublime Text 2 插件安装
tags:
  - Sublime Text2 Plugins install
id: 88
categories:
  - Tools
date: 2015-04-27 03:43:17
---

[Sublime Text 2安装插件有两种方法：](http://www.cnblogs.com/dolphin0520/archive/2013/04/29/3046237.html)

1）离线安装，先下载好安装包，解压之后放到Packages文件夹下，重启Sublime即可。

2）在线安装，在线安装之前，需要安装”Packages Control“这个包管理插件，安装方法是：

选择”查看“—&gt;”显示控制台“，然后在下面弹出的框中输入：
<div class="cnblogs_code">
<pre>import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())</pre>
</div>
然后回车确认，安装完毕之后重启sublime，如果发现在Perferences中看到package control这一项，则安装成功。

然后就可以通过”Ctrl+Shift+P“打开命令面板，输入”install“命令，就可以看到安装包列表了。

下面推荐几款必备的常用插件：

1.Tag插件

Tag插件可以为web开发者提供html和css标签，很方便快捷，对于web前端设计者非常实用。

![](http://images.cnitblog.com/blog/288799/201304/29231537-2ddaba665be244be9661107d7f43b260.jpg)

2.Prefixr插件

为css3提供一些前缀，比如

![](http://images.cnitblog.com/blog/288799/201304/29231738-65012f61dfa04f9f87bf839f61ebb254.jpg)

3.Terminal插件

Terminal插件可以允许在Sublime Text2中打开cmd命令窗口，很实用的一个插件，安装好该插件好，打开cmd命令窗口的快捷键是

Ctrl+Shift+T。

4.SublimeTmpl插件

这个插件允许用户定义文件的模板，比如在写一个html文件时，老是重复文件头的一些引入信息很繁琐，可以定义一个模板直接生成必须的信息，具体的SublimeTmpl插件用法请自行百度。

5.SideBarEnhancements插件

一个增强侧边栏文件夹浏览功能的插件，比较不错。

6.DocBlockr插件

用来生成注释块的插件，安装好之后直接输入"/*"，然后再按回车键，即可生成代码注释块。

7.SublimeCodeIntel插件

智能提示插件，这个插件的智能提示功能非常强大，可以自定义提示的内容库，我的Python智能提示设置（配置文件路径为packages\SublimeCodeIntel-master\.codeintel\config）为：
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码">![复制代码](http://common.cnblogs.com/images/copycode.gif)</a></span></div>
<pre>{
    "Python": {
        "python":'D:/Program Files/Python26/python.exe',
        "pythonExtraPaths": ['D:/Program Files/Python26','D:/Program Files/Python26/DLLs','D:/Program Files/Python26/Lib','D:/Program Files/Python26/Lib/plat-win','D:/Program Files/Python26/Lib/lib-tk','D:/Program Files/Python26/Lib/site-packages']
    }
}</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码">![复制代码](http://common.cnblogs.com/images/copycode.gif)</a></span></div>
</div>
其中“pythonExtraPaths”就是需要智能提示所需要用到的内容库。

8.AndyPython插件

一款针对Python语言的智能提示插件，其需要提示的关键字和函数可以在Packages\AndyPython\PythonCompletions.py中设置。

9.AndyJS2插件

一款针对Javsscript和jquery智能提示的插件。

10.jquery插件

jquery提示库。

11.Ctags插件

该插件可以实现快速定位到函数定义的地方。

12.为了避免打开含中文字符的文件出现乱码，需要先安装GBK Encoding Support这个插件，再安装ConvertToUTF8插件即可。

如果有朋友觉得没有注册有时候会有弹窗比较讨厌，这里介绍一种破解办法：

用一种十六进制编辑器（我这里用的UltraEdit）打开sublime text 2安装目录下的文件sublime_text.exe，在此之前最好备份一下，如果没有破解成功可以恢复，然后定位到000CBB70这一行，找到8A C3，将其修改为B0 01，然后保存即可，这是破解注册成功的界面：

![](http://images.cnitblog.com/blog/288799/201305/05150245-21015e86b67246278d8c6622626f5a52.jpg)