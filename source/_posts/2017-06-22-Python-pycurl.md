---
title: "PycURL模拟用户访问服务器 - Python自动化运维"
date: 2017-06-22 08:00:00
updated: 2017-06-19 17:20:16
tags: ["Python"]
---
Libcurl 是一个支持FTP，FTPS，HTTP，HTTPS，GOPHER，TELNET，DICT，FILE 和
LDAP的客户端URL传输库。libcurl也支持HTTPS认证，HTTP、POST、HTTP
PUT、FTP上传，代理，Cookies，基本身份验证，FTP文件断点继传，HTTP代理通道等等。
1. 安装Pip & Python 2.7 (or 3.6),并配置环境变量(默认安装会自动添加)
2. 设置Notepad++作为IDE开发工具:运行中输入命令cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT
3. 安装第三方库: pip install pycurl
Examples:
 
```python
 # -- coding: utf-8 --
 import os,sys
 import time
 import sys
 import pycurl
 
 URL="http://www.xxx.com"
 #URL="http://www.baidu.com"
 #URL="http://www.google.com.hk"
= pycurl.Curl()
setopt(pycurl.URL, URL)
                 
 #连接超时时间,5秒
 c.setopt(pycurl.CONNECTTIMEOUT, 5)
 
 #下载超时时间,5秒
 c.setopt(pycurl.TIMEOUT, 5)
 c.setopt(pycurl.FORBID_REUSE, 1)
 c.setopt(pycurl.MAXREDIRS, 1)
    c.setopt(pycurl.NOPROGRESS, 1)
    c.setopt(pycurl.DNS_CACHE_TIMEOUT,30)
    indexfile = open(os.path.dirname(os.path.realpath(__file__))+"/content.txt", "wb")
    c.setopt(pycurl.WRITEHEADER, indexfile)
    c.setopt(pycurl.WRITEDATA, indexfile)
    try:
        c.perform()
    except Exception,e:
        print "connecion error:"+str(e)
        indexfile.close()
        c.close()
        sys.exit()
    
    NAMELOOKUP_TIME =  c.getinfo(c.NAMELOOKUP_TIME)
    CONNECT_TIME =  c.getinfo(c.CONNECT_TIME)
    PRETRANSFER_TIME =   c.getinfo(c.PRETRANSFER_TIME)
    STARTTRANSFER_TIME = c.getinfo(c.STARTTRANSFER_TIME)
    TOTAL_TIME = c.getinfo(c.TOTAL_TIME)
    HTTP_CODE =  c.getinfo(c.HTTP_CODE)
    SIZE_DOWNLOAD =  c.getinfo(c.SIZE_DOWNLOAD)
    HEADER_SIZE = c.getinfo(c.HEADER_SIZE)
    SPEED_DOWNLOAD=c.getinfo(c.SPEED_DOWNLOAD)
    
    print "HTTP Code: %s" %(HTTP_CODE)
    print "DNS time: %.2f ms"%(NAMELOOKUP_TIME*1000)
    print "start connecion time: %.2f ms" %(CONNECT_TIME*1000)
    print "perpare tran time: %.2f ms" %(PRETRANSFER_TIME*1000)
    print "tran start time: %.2f ms" %(STARTTRANSFER_TIME*1000)
    print "tran total time: %.2f ms" %(TOTAL_TIME*1000)
    
    print "download data size: %d bytes/s" %(SIZE_DOWNLOAD)
    print "HTTP head size: %d byte" %(HEADER_SIZE)
    print "avg download speed: %d bytes/s" %(SPEED_DOWNLOAD)
    
    indexfile.close()
    c.close()
![blob.png](/uploads/ueditor/php/upload/image/20170619/1497862725.png)
