---
layout: post
comments: true
title:  "用Base64嵌入图片"
date:   2013-11-12 10:41:38
categories: blog
---
<blockquote>一天冷似一天，阴冷的空气无声无息的悬在城市的上空，纷飞的落叶也在述说着岁月更替的故事。</blockquote>

在使用wxPython写程序时，如果用到了图标，而打包的时候又想嵌到程序里，就要使用base64对图片进行编码，然后把生成的字符串直接写到程序里就好了,废话不多说，上代码: <!-- more --></p>
<!--more-->
{% highlight python %}
# -*- coding:utf-8 -*-
#导入需要的库
import wx
from wx.lib.embeddedimage import PyEmbeddedImage #用于嵌入经过base64编码的图片

#用经过Base64处理的字符串构造图片，用图片生成的base64字符串替换下面括号里的内容即可
imgBase64 = PyEmbeddedImage("iVBORw0KGgoAAAANSUhEUgAAWCAYAAAAinad/.......+KElFTkSuQmCC")

#生成空位图
self.ESImage = imgBase64.GetImage()#.Scale(22,24)

#把上一步生成的图片加到控件上去
self.imageCtrl =    wx.StaticBitmap(self.RightPanel,wx.ID_ANY,wx.BitmapFromImage(self.ESImage))
</code></pre>
{% endhighlight %}

那么怎么把一个图片用Base64编码呢，请看:

{% highlight python %}
# -*- coding:utf-8 -*-
import base64

file = open('x1.png', 'rb')
pic = file.read()
b64 = base64.b64encode(pic)
print b64
{% endhighlight %}

在iOS中，如果我们发Html格式邮件的时候需要带上一个本地图片文件，应该怎么办呢，用附件不合适，这时候也要用Base64处理，注意html代码中base64的标记，
这样浏览器才能正确的解码。

{% highlight objectivec %}
MFMailComposeViewController *mailViewController = [[MFMailComposeViewController alloc] init];

//构造发送的邮件内容，注意base64的标记
NSString *strMessage = @"img src='data:image/png;base64,%@'<a href=https://itunes.apple.com/us/app/shoebox-find-your-shoes-quickly/id640885172?ls=1&mt=8>View in App Store</a>";

//把图片用base64编码，加到邮件的内容里
UIImage *appicon = [UIImage imageNamed:@"Icon.png"];
NSData *imageData = UIImagePNGRepresentation(appicon);
strMessage = [NSString stringWithFormat:strMessage,[imageData base64EncodedString]];

[mailViewController setMessageBody:strMessage isHTML:YES];
{% endhighlight %}
