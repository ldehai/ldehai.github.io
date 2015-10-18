---
layout: post
title:  "File Already Exists but Should Not Pyconfig"
date:   2013-11-02 10:41:38
categories: jekyll update
---
接[上一篇](blog/2013/09/22/pyinstaller-matplotlib/)，python项目用到了scipy计算模块，之后用pyinstaller打包后，每次运行就会报警告file already exists but should not:&hellip;&hellip;./pyconfig.h。虽然不影响运行，还是觉得有必要解决这个问题，在搜索到stackoverflow[这个帖子](http://stackoverflow.com/questions/19055089/pyinstaller-onefile-warning-pyconfig-h-when-importing-scipy-or-scipy-signal)后，问题解决。
<!--more-->
问题的原因是pyinstaller打包时pyconfig.h多打了一次，所以会报已经存在了。这个解决方案就是把多余的pyconfig.h去掉。

下面开始详细说明如何修改：

上一篇讲到使用如下命令打包exe：

    c:/python27/python.exe c:/pyinstaller/pyinstaller.py --noconfirm --noconsole --onefile --icon=Icon.ico myapp.py

请仔细观察myapp.py所在目录，会自动生成myapp.spec文件，注意是每次都会重新生成，内容如下：

{% highlight python %}
a = Analysis(['myapp.py'],
         pathex=['c:\\src'],
         hiddenimports=[],
         hookspath=None,
         runtime_hooks=None)
pyz = PYZ(a.pure)
exe = EXE(pyz,
      a.scripts,
      a.binaries,
      a.zipfiles,
      a.datas,
      name='myapp.exe',
      debug=False,
      strip=None,
      upx=True,
      console=False , icon='Icon.ico')
{% endhighlight %}

大概解释下文件的内容：

第一行a = Anaylysis(&hellip;)是用来分析程序用到的库，下面就是打包成exe的一些参数。所以我们在第一行代码之后加代码把多余的pyconfig.h去掉，新的spec文件如下：

{% highlight python %}
a = Analysis(['myapp.py'],
         pathex=['c:\\src'],
         hiddenimports=[],
         hookspath=None,
         runtime_hooks=None)
#add_begin
for d in a.datas:
    if 'pyconfig' in d[0]:
        a.datas.remove(d)
        break
#add_end
pyz = PYZ(a.pure)
exe = EXE(pyz,
      a.scripts,
      a.binaries,
      a.zipfiles,
      a.datas,
      name='myapp.exe',
      debug=False,
      strip=None,
      upx=True,
      console=False , icon='Icon.ico')
{% endhighlight %}

为了使用修改后的spec文件来打包，需要使用新的命令：

    c:/python27/python.exe c:/pyinstaller/pyinstaller.py myapp.spec

试试，warning没了吧
