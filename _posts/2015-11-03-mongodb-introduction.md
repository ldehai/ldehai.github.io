---
layout: post
title:  "mongodb简介"
date:   2015-11-03 15:38:00
categories:
---
####安装
mongodb安装方法参照官网.我用的阿里云服务器，使用的是centos6.5，64位系统。参照以下地址完成安装。 [https://docs.mongodb.org/manual/tutorial/install-mongodb-enterprise-on-red-hat/](https://docs.mongodb.org/manual/tutorial/install-mongodb-enterprise-on-red-hat/)

####mongod与mongo
跟其他数据库系统类似，[mongodb](https://docs.mongodb.org/manual/)也有自己的服务进程mongod,centos下以服务的形式开启的方式为：

    sudo service mongod start

这样终端窗口关了之后，程序还能继续运行。当然，你也可以在命令行下输入mongod启动，只是窗口关了程序也停了。
mongodb的交互程序是mongo（在mongod启动后，新开一个命令窗口输入mongo),输入的命令发送给mongod进程，然后返回处理结果。

#### use database_name
mongodb没有新增数据库的命令，可以使用use "数据库名" 新建数据库，但新建的数据库实际上没有写入
存储，只有当在下面新增collection时才会实际创建。
例如：

   use aventlabs
   db.createCollection('member')
   show dbs
使用show dbs命令可以查看当前已经建好的数据库。

####db->collection->document
mongodb的存储结构是数状结构的，可以建多个数据库，数据库底下建多个collection,collection下
是最终的document,存放实际的数据。这个结构类似于硬盘上文件的组织方式。有分区、目录、文件。

关于mongodb的详细特性，请参见[官方文档](https://docs.mongodb.org/manual/)。
