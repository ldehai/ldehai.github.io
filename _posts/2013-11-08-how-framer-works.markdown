---
layout: post
comments: true
title:  "Framer简介"
date:   2013-11-08 10:41:38
categories: framer
---
Framer是一个开源项目，一个基于JavaScript的原型工具，专为设计师打造。现在的应用更注重交互设计，它可以让你效率更高。项目地址[http://www.framerjs.com](http://www.framerjs.com)，使用Framer包括4个步骤:

1. 用Photoshop设计原型图
2. 用Framer.app从Photoshop中导出设计图
3. 在app.js里写视图展示及交互代码，可以加各种特效
4. 用浏览器打开index.html文件就可以查看和调试了
<!--more-->

Framer项目的代码结构是这样的：

    app.js          //【重要】用来写视图和交互代码
    framer/         //Framerjs的框架，app.js里用到的功能都是它提供的
      framer.js     //framer核心文件
      framerps.js   //根据views.app.js生成frame对象
      views.app.js  //根据psd文件的结构导出的文件，用来记录图片属性和图层关系(用json格式表示)，app.js中要用到
    images/         //从photoshop导出的图片都在这里
    index.html      //预览时的首页

## Framer可以说是由三部分组成：

### 1. 图片导出工具

支持从photoshop和sketch导出图片。
这里从photoshop导出设计图。实际上是使用了Adobe ExtendScript脚本。Adobe ExtendScript脚本语言可以操纵photoshop执行一些自动化操作。
Mac下作者用Framer.app包装了脚本代码，windows下则直接提供了[脚本](http://www.framerjs.com/static/downloads/FramerPS.jsx.zip)

导出工具不仅导出了图片，还生成了一个上面目录结构的完整工程，用浏览器打开index.html就可以了。

在原型设计的过程中，可能会需要多次导出图片，而导出工具不会覆盖app.js文件，所以不用担心你写的代码会被覆盖。它只会在没有文件的情况下才自动生成。

### 2. JS类库

这里提供了View(视图),animation(动态特效),event(事件绑定)等类，可供使用者调用。详见最后一部分介绍，或直接至官网查看。

### 3. app.js

Framer默认生成app.js文件。在使用Framer的过程中，除了用Photoshop设计原型图之外，这里也是花时间最多的地方。前面准备的素材就看你怎么组织了。视图的创建、切换、动态特效、手势、事件绑定，都可以在这个文件里完成。</p>

## Framejs框架介绍

它包括View(视图),animation(动态特效),event(事件绑定)。下面的代码可以在线修改，马上就能看到效果。

### View:

#### 基本视图 [http://www.framerjs.com/editor/index.html?path=lessons/views.js](http://www.framerjs.com/editor/index.html?path=lessons/views.js)

#### 嵌套视图 [http://www.framerjs.com/editor/index.html?path=lessons/hierarchy.js](http://www.framerjs.com/editor/index.html?path=lessons/hierarchy.js)

#### 其他视图类型 [http://www.framerjs.com/editor/index.html?path=lessons/viewtypes.js](http://www.framerjs.com/editor/index.html?path=lessons/viewtypes.js)

### 特效：

[官方文档](http://www.framerjs.com/editor/index.html?path=lessons/animation.js)

### 支持的手势：[https://medium.com/building-potluck/2e405d50b600](https://medium.com/building-potluck/2e405d50b600)

### 事件绑定：

[http://www.framerjs.com/editor/index.html?path=lessons/events.js](http://www.framerjs.com/editor/index.html?path=lessons/events.js)

## 结语

Framer拉近了设计师与开发者的距离，从另一个层面来讲，我觉得设计师也在逐渐侵入开发者的领地，要抢饭碗的节奏。
Framer目前只侧重于前端展示，如果加上数据处理，那么一个完整的Html5的应用开发框架就成型了。
