---
title: "Centos 日志分析"
date: 2017-03-28 08:00:00
updated: 2017-04-01 08:59:50
tags: ["Linux"]
---
# 日志系统主要分为三类

## 系统接入日志:
多个程序会记录该日志记录到/var/log/wtmp和/var/run/utmp文件中telnet、ssh等程序会更新wtmp和utmp文件系统管理员可以根据该日志跟踪到谁在何时登录到系统。

## 进程统计日志:
linux内核记录该日志当一个进程终止时进程统计文件pacct或acct中会进行记录。进程统计日志可以供系统管理员分析系统使用者对系统进行的配置以及对文件进行的操作。

## 错误日志:
Syslog日志系统已经被许多设备兼容Linux的syslog可以记录系统事件主要由syslogd程序执行Linux系统下各种进程、用户程序和内核都可以通过Syslog文件记录重要信息错误日志记录在/var/log/messages中。有许多Linux/Unix程序创建日志。像HTTP和FTP这样提供网络服务的服务器也保持详细的日志。
![blob.png](/uploads/ueditor/php/upload/image/20170327/1490579873.png)

## 用于入侵检测日志: 
> 需要检查的系统日志文件主要包括一般信息日志、网络连接日志、文件传输日志以及用户登录日志等。
  
## 可疑的日志记录:
  1. 非正常时间凌晨的用户登录;
  2. 关键日志记录损坏尤其是记录用户登录登出信息的wtmp文件;
  3. 非正常IP的用户登录;
  4. 用户登录失败甚至一再尝试登录并失败的日志记录;
  5. 非正常的超级用户权限切换su指令;
  6. 非正常的控制进程启动或重启记录。
  
## 检查步骤:
  1. users命令在一行打印出当前登录的用户信息每个用户名代表对应一个登录会话。结合who命令以及进程统计日志可以分析黑客伪造这两个命令的情况
  2. 发现当前系统存在哪些不法用户: who /var/log/wtmp 或 last [root] lastlog
  3. ac命令根据系统的wtmp文件中的登入登出信息生成用户连接时间单位小时的报告如下所示
  4. 用netstat –an看一下发现比较奇怪的端口
  5. 来到/var/log目录下看一下secure日志
  6. 再来定位一下图一中异常的screen程序用命令 Locate screen来定位输出如下图所示看来有很多文件要查
  7. 发现一个uid0的bone用户uid0不是管理员用户吗我们到/etc/passwd中发现果真是存在一个root用户bone
  8. 侵者已经取得了系统的最高权限。将其删除
  
通过调查分析和日志取证找出异常程序和入侵帐户。从我们后面的分析日志中分析得出系统被入侵的原因是因为存在一个存在密码为passwd的弱口令帐号入侵者通过探测到这个弱口令帐户远程登陆到系统并且利用一些方法提高了权限最后得到了管理员root权限。
  
其实在服务器部署完后安全工作我们也要做的非常到位，避免留过多的隐患给有坏心眼的人。有一遍安全部署方面的帖子，链接是：<http://ks0101.blog.51cto.com/8639108/1365721>  有兴趣可以看看，会起到很大作用的。
  
## 附录: 日 志 文 件 说 明
/var/log/message 系统启动后的信息和错误日志，是Red Hat Linux中最常用的日志之一
/var/log/secure 与安全相关的日志信息
/var/log/maillog 与邮件相关的日志信息
/var/log/cron 与定时任务相关的日志信息
/var/log/spooler 与UUCP和news设备相关的日志信息
/var/log/boot.log 守护进程启动和停止相关的日志消息
  
```shell
系统：
# uname -a# 查看内核/操作系统/CPU信息
# cat /etc/issue
# cat /etc/redhat-release # 查看操作系统版本
# cat /proc/cpuinfo  # 查看CPU信息
# hostname# 查看计算机名
# lspci -tv# 列出所有PCI设备
# lsusb -tv# 列出所有USB设备
# lsmod # 列出加载的内核模块
# env # 查看环境变量
资源：
# free -m# 查看内存使用量和交换区使用量
# df -h # 查看各分区使用情况
# du -sh <目录名>  # 查看指定目录的大小
# grep MemTotal /proc/meminfo # 查看内存总量
# grep MemFree /proc/meminfo # 查看空闲内存量
# uptime# 查看系统运行时间、用户数、负载
# cat /proc/loadavg  # 查看系统负载
磁盘和分区：
# mount | column -t  # 查看挂接的分区状态
# fdisk -l# 查看所有分区
# swapon -s# 查看所有交换分区
# hdparm -i /dev/hda  # 查看磁盘参数(仅适用于IDE设备)
# dmesg | grep IDE  # 查看启动时IDE设备检测状况
网络：
# ifconfig# 查看所有网络接口的属性
# iptables -L# 查看防火墙设置
# route -n# 查看路由表
# netstat -lntp# 查看所有监听端口
# netstat -antp# 查看所有已经建立的连接
# netstat -s# 查看网络统计信息
进程：
# ps -ef# 查看所有进程
# top # 实时显示进程状态（另一篇文章里面有详细的介绍）
用户：
# w # 查看活动用户
# id <用户名># 查看指定用户信息
# last # 查看用户登录日志
# cut -d: -f1 /etc/passwd # 查看系统所有用户
# cut -d: -f1 /etc/group # 查看系统所有组
# crontab -l# 查看当前用户的计划任务
服务：
# chkconfig –list  # 列出所有系统服务
# chkconfig –list | grep on # 列出所有启动的系统服务
程序：
# rpm -qa# 查看所有安装的软件包
```
