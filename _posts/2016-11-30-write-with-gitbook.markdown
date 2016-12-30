---
layout: post
title:  "使用Gitbook搭建写作环境"
date:   2016-11-30 08:21:00
tags: gitbook
---
### gitbook简介

Gitbook.com是一个写作和出版的平台，他们使用的系统是开源的。支持Markdown和AsciiDoc格式，最后输出的是静态网页。还可以通过[calibre](https://calibre-ebook.com/download_osx)把书[转成](http://toolchain.gitbook.com/ebook.html)pdf、epub、mobi等常用格式，方便在各种设备上阅读。
![]({{ site.url }}/assets/gitbooksample.png)

### 要达成的目标
由于Gitbook和Markdown的开放性，可以在本地搭建写作环境，对我来说很有吸引力。我使用如下的组合来搭建我的写作环境：gitbook+atom+github+calibre
<!--more-->

[Gitbook](https://github.com/GitbookIO/gitbook)：用来创建书，遵循它的文件组织形式，可以生成带目录的一本完整的书。

[Calibre](https://calibre-ebook.com)：用它把写好的书转换成其他格式，方便在不同设备上阅读，包括kindle。

[Atom](https://atom.io/)：Github出的编辑器，可以用来写markdown格式的文件。当然你可以用自己喜欢的任何编辑器。

[Github](https://github.com)：把书稿放到Github，方便版本管理，多人协作。如果你开通了私有库，存放自己不想公开的书也很方便。

### Gitbook安装
参照[Gitbook官方安装说明](https://github.com/GitbookIO/gitbook/blob/master/docs/setup.md)。
以macOS为例，安装步骤如下：

    $ npm install gitbook-cli -g

npm是nodejs的包管理器，gitbook使用的nodejs，所以如果你还没有安装npm，请移步到[nodejs安装](https://nodejs.org/en/download/)。这里会安装nodejs，npm是包含在里面一起安装的。

### 新建一本书
![]({{ site.url }}/assets/gitbook-cli.png)

新建一本书的目录，在ternimal里面进入这个目录，执行以下命令初始化一本书：

    $gitbook init

### 「可选」放到github
首先登录到github，新建repo，然后把本地的代码push到github就可以了。放到github上还有一个好处是，github是支持编辑markdown文件的。只要有网络，你随时可以登上去写。当然，这一步并不是必须的。只保存在本地也是没问题的。

    echo "# mybook" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin "你的repo路径"
    git push -u origin master

接下来你可以选择自己喜欢的编辑器去写文章了，写完想看最终的排版效果，或者转成其他格式的电子书，就接着往下看。

### 在本地预览书
gitbook会启动一个本地的web服务器[http://localhost:4000](http://localhost:4000), 在浏览器里访问这个地址就可以查看了。

    $ gitbook serve

### 把md文件编译成静态页面，注意查看当前目录底下生成的文件

    $gitbook build

### 生成其他格式文件
gitbook生成其他格式的书是借助calibre的转换功能实现的(ebook-convert)，安装完calibre后，修改系统$PATH定义，把转换程序的路径加到系统PATH里，要不然会找不到转换程序。打开Terminal（终端），按照下面的操作步骤修改PATH。

    vi ~/.bash_profile

把下面这句附加到.bash_profile文件里，然后保存退出。

    export PATH=$PATH:/Applications/calibre.app/Contents/MacOS/

使用source命令使文件立即生效

    source ~/.bash_profile
    echo $PATH

#### 生成pdf文件

    $ gitbook pdf ./ mybook.pdf

#### 生成epub文件

    $ gitbook epub ./ mybook.epub

#### 生成mobi文件，支持kindle

    $ gitbook mobi ./ mybook.mobi
