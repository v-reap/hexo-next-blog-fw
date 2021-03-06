---
title: "difflib文本内容对比 - Python自动化运维"
date: 2017-06-20 08:00:00
updated: 2017-06-19 16:49:18
tags: ["Python"]
---
1. 安装Pip & Python 2.7 (or 3.6),并配置环境变量(默认安装会自动添加)
2. 设置Notepad++作为IDE开发工具:运行中输入命令cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT
3. 安装第三方库: pip install difflib

Examples:
 
```python
 # -- coding: utf-8 --

 #!/usr/bin/python

 import difflib

 

 text1 = """text1:

 This module provides classes and functions for comparing sequences.

 including HTML and context and unified diffs.

 difflib document v7.4

 add string

 """

 

 text1_lines = text1.splitlines()

    

 text2 = """text2:

 This module provides classes and functions for Comparing sequences.

 including HTML and context and unified diffs.

 difflib document v7.5"""



 text2_lines = text2.splitlines()



 d = difflib.Differ()

ff = d.compare(text1_lines, text2_lines)

n'.join(list(diff))

比较2个文件的不同之处):



 # -- coding: utf-8 --

/usr/bin/python

 import difflib

 import sys

 

 try:

        textfile1=sys.argv[1]

        textfile2=sys.argv[2]

    except Exception,e:

        print "Error:"+str(e)

        print "Usage: simple3.py filename1 filename2"

        sys.exit()

    

    def readfile(filename):

        try:

            fileHandle = open (filename, 'rb' ) 

            text=fileHandle.read().splitlines()

            fileHandle.close()

            return text

        except IOError as error:

           print('Read file Error:'+str(error))

           sys.exit()

    

    if textfile1=="" or textfile2=="":

        print "Usage: simple3.py filename1 filename2"

        sys.exit()

    

    

    text1_lines = readfile(textfile1) 

    text2_lines = readfile(textfile2) 

    

    d = difflib.HtmlDiff()

    print d.make_file(text1_lines, text2_lines)

效果图:

![blob.png](/uploads/ueditor/php/upload/image/20170619/1497861412.png)

