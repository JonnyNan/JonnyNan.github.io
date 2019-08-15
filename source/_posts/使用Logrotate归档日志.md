---
title: 使用Logrotate归档日志
date: 2019-08-15 16:59:43
tags: [日志,运维]
---

# 关于日志切割
日志文件包含了关于系统中发生的事件的有用信息，在排障过程中或者系统性能分析时经常被用到。对于忙碌的服务器，日志文件大小会增长极快，服务器会很快消耗磁盘空间，这成了个问题。除此之外，处理一个单个的庞大日志文件也常常是件十分棘手的事。
　　logrotate是个十分有用的工具，它可以自动对日志进行截断（或轮循）、压缩以及删除旧的日志文件。例如，你可以设置logrotate，让/var/log/foo日志文件每30天轮循，并删除超过6个月的日志。配置完后，logrotate的运作完全自动化，不必进行任何进一步的人为干预。

# 安装

以 centos 为例：

> $ yum -y install logrotate

安装后应该存在三个文件夹:

```
/etc/cron.daily/logrotate
/etc/logrotate.conf # 主配置文件
/etc/logrotate.d # 配置目录
```

logrotate的配置文件是/etc/logrotate.conf，通常不需要对它进行修改。日志文件的轮循设置在独立的配置文件中，它（们）放在/etc/logrotate.d/目录下。

# 实践
假设我们要分割的是tomcat的日志catalina.out

## 创建tomcat的配置文件

> vim /etc/logrotate.d/tomcat

内容：

```
/application/tomcat/logs/catalina.out {
daily
copytruncate
rotate 30
compress
notifempty
dateext
missingok
}
```
注意上面内容中大括号前面内容为 待处理日志文件的绝对路径。
大括号里面的都是Logrotate工具的参数


关于每个参数的作用我们可以查看全局文件cat /etc/logrotate.conf 我们可以把配置文件写在/etc/logrotate.conf里面或者放在/etc/logrotate.d下面

## 参数解释

```
daily 表示每天整理一次
rotate 20 表示保留20天的备份文件
dateext 文件后缀是日期格式,也就是切割后文件是:xxx.log-20171205.gz
copytruncate 用于还在打开中的日志文件，把当前日志备份并截断
compress 通过gzip压缩转储以后的日志（gzip -d xxx.gz解压）
missingok 如果日志不存在则忽略该警告信息
notifempty 如果是空文件的话，不转储
#size 5M #当catalina.out大于5M就进行切割，可用可不用！
```

## 不常用的参数

```
weekly 指定转储周期为每周
monthly 指定转储周期为每月
nocompress 不需要压缩时，用这个参数
nocopytruncate 备份日志文件但是不截断
create mode owner group 转储文件，使用指定的文件模式创建新的日志文件
nocreate 不建立新的日志文件
delaycompress 和 compress 一起使用时，转储的日志文件到下一次转储时才压缩
nodelaycompress 覆盖 delaycompress 选项，转储同时压缩
errors address 转储时的错误信息发送到指定的Email 地址
ifempty 即使是空文件也转储，这个是 logrotate 的缺省选项。
mail address 把转储的日志文件发送到指定的E-mail 地址
nomail 转储时不发送日志文件
olddir directory 转储后的日志文件放入指定的目录，必须和当前日志文件在同一个文件系统
noolddir 转储后的日志文件和当前日志文件放在同一个目录
prerotate/endscript 在转储以前需要执行的命令可以放入这个对，这两个关键字必须单独成行
postrotate/endscript 在转储以后需要执行的命令可以放入这个对，这两个关键字必须单独成行
```
温馨提示：配置文件里一定要配置rotate 文件数目这个参数。如果不配置默认是0个，也就是只允许存在一份日志，刚切分出来的日志会马上被删除

# 测试


1. 调试 （d = debug）参数为配置文件，不指定则执行全局配置文件

> logrotate -d /etc/logrotate.d/tomcat.conf

2. 强制执行（-f = force），可以配合-v(-v =verbose）使用，注意调试信息默认携带-v；

> logrotate -v -f /etc/logrotate.d/tomcat.conf

配合crontab定时器即可实现每天备份当天日志并压缩归档了。
