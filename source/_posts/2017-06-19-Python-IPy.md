---
title: "IPy - Python自动化运维"
date: 2017-06-19 17:00:00
updated: 2017-06-23 09:53:51
tags: ["Python"]
---
1. 安装Pip & Python 2.7 (or 3.6),并配置环境变量(默认安装会自动添加)
2. 设置Notepad++作为IDE开发工具:运行中输入命令cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT
3. 安装第三方库: pip install IPy

Examples:
 
```python
Examples:

 
 
 # -- coding: utf-8 --

 #!/usr/bin/env, python pip install ipy

 

 from IPy import IP

 

 ip_s = raw_input('Please input an IP or net-range: ')

 ips = IP(ip_s)

 

 

 if len(ips) > 1:

  print('net: %s' % ips.net())

  print('netmask: %s' % ips.netmask())

  % ips.broadcast())

  print('reverse address: %s' % ips.reverseNames()[0])

  print('subnet: %s' % len(ips))

 else:

  print('reverse address: %s' % ips.reverseNames()[0])

 

 print('hexadecimal: %s' % ips.strHex())

 print('binary ip: %s' % ips.strBin())

 print('iptype: %s' % ips.iptype())

相关教程PDF:

![](http://10.1.134.114:55555/laravel-u-editor/dialogs/attachment/fileTypeImag
es/icon_pdf.gif)[Python自动化运维：技术与最佳实践-
signed.pdf](/uploads/ueditor/php/upload/file/20170623/1498182828.pdf)

  

