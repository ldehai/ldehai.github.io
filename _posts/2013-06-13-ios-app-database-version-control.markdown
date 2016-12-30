---
layout: post
title:  "iOS APP Database Version Control"
date:   2013-06-13 10:41:38
categories: iOS
---
APP里一直使用SQLite数据库保存数据，不过有个问题觉得挺麻烦，当我要加新功能，而原来的表结构已经不能满足要求时，需要修改表，或者新增表。那么老版本升级到新版本时就需要升级数据库，之前我都是判断新表有没有存在，不存在则创建。这样很容易搞乱掉，如果涉及到多个表，判断起来逻辑不清晰，把自己都搞晕了。</p>

在经历过几次折腾后，我发现数据库也需要用版本来控制起来，就像代码有版本一样。这样我只需要判断当前的数据库版本是多少，就知道要不要升级数据库了。假设之前版本是1.0，现在版本是1.1,代码很简单：
<!--more-->
{% highlight objectivec %}
NSUserDefaults *pref = [NSUserDefaults standardUserDefaults];
NSString* ver = [pref stringForKey:@"dbver"];
if ([ver isEqualToString:@"1.0"])
{
    NSString *dbPath  = [NSHomeDirectory() stringByAppendingPathComponent:@"Documents/database.db"];

    EGODatabase* database = [EGODatabase databaseWithPath:dbPath];

    NSString *strSql = @"create table newtable(id integer primary key asc, main text);";
    [database executeQuery:strSql];

    [database close];

    NSUserDefaults *pref = [NSUserDefaults standardUserDefaults];
    [pref setValue:@"1.1" forKey:@"dbver"];
}
{% endhighlight %}

当然自己还是要记录好每次升级的变动情况，备查。
