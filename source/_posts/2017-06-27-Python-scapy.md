---
title: "scapy交互式数据包处理 - Python自动化运维"
date: 2017-06-27 08:00:00
updated: 2017-06-22 09:23:32
tags: ["Python"]
---
scapy是python写的一个功能强大的交互式数据包处理程序，可用来发送、嗅探、解析和伪造网络数据包，常常被用到网络攻击和测试中。它可以代替hping,arpspoof.ARP SK,arping,p0f,甚至是部分nmap,Tcpdump和tshark。

1. 安装Pip & Python 2.7 (or 3.6),并配置环境变量(默认安装会自动添加)
2. 设置Notepad++作为IDE开发工具:运行中输入命令cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT
3. 安装第三方库: pip install scapy

Examples:
 
```python
 # -- coding: utf-8 --

 import os,sys,time,subprocess

 import warnings,logging

 warnings.filterwarnings("ignore", category=DeprecationWarning)

 logging.getLogger("scapy.runtime").setLevel(logging.ERROR)

om scapy.all import traceroute

mains = raw_input('Please input one or more IP/domain: ')

 target =  domains.split(' ')

    dport = [80]

    if len(target) >= 1 and target[0]!='':

        res,unans = traceroute(target,dport=dport,retry=-2)

        res.graph(target="> test.svg")

        time.sleep(1)

        subprocess.Popen("/usr/bin/convert test.svg test.png", shell=True)

    else:

        print "IP/domain number of errors,exit"

  

  

  

