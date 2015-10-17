---
layout: post
title:  "Python程序打包成EXE--使用pyinstaller打包matplotlib、scipy、wxpython"
date:   2013-09-22 10:41:38
categories:
---
用python帮朋友写了个科学计算的程序，用到了matplotlib和scipy，以及wxpython。代码写完，发现不能直接拿给朋友用，windows系统默认是没有运行环境的，得装python,matplotlib,scipy以及其他的运行库。这对于程序小白来说实在强人所难。好在有pyinstaller，可以用它打包成一个exe文件，所有的库都在一起了。妈妈再也不用担心没有环境跑程序了。

到pyinstaller[官网](http://www.pyinstaller.org/">http://www.pyinstaller.org/)下载安装包。
详细安装说明见[官方文档](http://pythonhosted.org/PyInstaller/#installing-in-windows)。

这里简要说明一下步骤：
直接下载压缩包，现在的版本是2.1.解压后打开命令行，执行如下命令：

{% highlight python %}
    c:\python setup.py install
{% endhighlight %}

可以用如下命令检查是不是已经装好了

{% highlight python %}
    c:\pyintaller --version
{% endhighlight %}

ok，pyinstaller 装好了，开始打包

{% highlight python %}
    c:\c:/python27/python.exe c:/pyinstaller/pyinstaller.py --noconfirm --noconsole --onefile --icon=Icon.ico myapp.py
{% endhighlight %}

解释一下几个参数：

noconfirm:在打包过程中不需要人工确认。

noconsole:打包后的程序运行时没有命令行窗口(在测试打包的过程中，可以把这个参数去掉，这样运行的时候可以从命令行查看出错信息，所有错误排除后再加上)。

onefile:顾名思义就是打包成一个文件，如果不加就是一个exe加一个文件夹，文件夹里存放需要的各种库文件。

icon:指定打包后的exe文件的图标（运行时窗口的图标是在程序里设定的）

由于pyinstaller2.1正式版还不支持scipy库，直接运行会报找不到scipy module。而developer版本已经支持了，所以去下载开发版重新安装。pyinstaller的代码在[github上(https://github.com/pyinstaller/pyinstaller),选择develop分支下载。
pyinstaller对第三方库的支持情况见这里：[SupportedPackages](http://www.pyinstaller.org/wiki/SupportedPackages)。

安装步骤同上，不再赘述。安装完运行打包脚本会发现还有错误

{% highlight python %}
    ImportError: No module named scipy.sparse.csgraph._validation
{% endhighlight %}

这是因为pyinstaller没能自动把_validation库加进来，我们就在代码里手动加上吧。

{% highlight python %}
    from scipy.sparse.csgraph import _validation
{% endhighlight %}

再次运行，应该没问题了。
