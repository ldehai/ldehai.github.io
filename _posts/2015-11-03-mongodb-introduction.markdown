---
layout: post
title:  "mongodb简介"
date:   2015-11-03 15:38:00
categories:
---
###简介
mongodb是久负盛名的非关系数据库。它以文档为存储单位，与严格的关系数据库形成泾渭分明的区别。

###db->collection->document

mongodb的存储结构是数状结构的，可以建多个数据库，数据库底下建多个collection,collection下
是最终的document,存放实际的数据。这个结构类似于硬盘上文件的组织方式。有分区、目录、文件。

如果非要跟关系数据库做类比，那么这里的collection就相当于表，document相当于表中的一行数据。
最大的区别在于，同一个collection下的文档不需要有一样的数据字段，是灵活的。

###安装

理解一件新东西的最好方法自然是动手试试，现在就来安装吧。
mongodb安装方法参照官网.我用的阿里云服务器，使用的是centos6.5，64位系统。参照以下地址完成安装。 [https://docs.mongodb.org/manual/tutorial/install-mongodb-enterprise-on-red-hat/](https://docs.mongodb.org/manual/tutorial/install-mongodb-enterprise-on-red-hat/)

###mongod与mongo

跟其他数据库系统类似，[mongodb](https://docs.mongodb.org/manual/)也有自己的服务进程mongod,centos下以服务的形式开启的方式为：

    sudo service mongod start

<!--more-->
这样终端窗口关了之后，程序还能继续运行。当然，你也可以在命令行下输入mongod启动，只是窗口关了程序也停了。
mongodb的交互程序是mongo（在mongod启动后，新开一个命令窗口输入mongo),输入的命令发送给mongod进程，然后返回处理结果。

###use database_name

mongodb没有新增数据库的命令，可以使用use "数据库名" 新建数据库，但新建的数据库实际上没有写入
存储，只有当在下面新增collection时才会实际创建。
例如：

    use aventlabs
    db.createCollection('member')
    show dbs

###插入记录

使用show dbs命令可以查看当前已经建好的数据库。（注意命令中的大小写）
先来插入一个记录看看：

    db.member.insert({
       "name" : "andy",
       "email" : "ldehai@gmail.com",
    })

在mongo命令行下输入以上命令，回车，就成功插入了一条记录，反馈如下：

    WriteResult({ "nInserted" : 1 })

###查询member中的记录：

    db.memer.find()

查询结果：

    { "_id" : ObjectId("5638735b01e6622e13ac34fb"), "name" : "andy", "email" : "ldehai@gmail.com" }

###条件查询,只查name为andy的一条记录：

    db.member.findone({'name':'andy'})

###删除记录

    db.member.remove({'name':'andy'})

进一步的操作方法，参照官网教程[使用mongo命令行工具操作mongodb](https://docs.mongodb.org/getting-started/shell/client/)

使用python的同学，可以看[使用pymongo操作mongodb](https://api.mongodb.org/python/3.1/tutorial.html)

使用tornado的同学，可以看[tornado+pymongo+mongodb](https://blog.openshift.com/day-25-tornado-combining-tornado-mongodb-and-angularjs-to-build-an-app/)

motor是跟pymongo类似的一个mongodb驱动库，不过它支持异步操作，有兴趣的同学可以看这里[Motar and Pymongo](https://motor.readthedocs.org/en/latest/differences.html)

关于更多mongodb的详细特性，请参见[官方文档](https://docs.mongodb.org/manual/)。
