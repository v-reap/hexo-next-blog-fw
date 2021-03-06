---
title: "fabric实现远程操作和部署 - Python自动化运维"
date: 2017-07-03 08:00:00
updated: 2017-06-23 10:17:55
tags: ["Python"]
---
fabric模块是在paramiko基础上又做了一层封装，操作起来更方便。主要用于多台主机批量执行任务。Fabric用于简化使用SSH的应用程序部署或系统管
理任务。它提供的操作包括：执行本地或远程shell命令，上传/下载文件，以及其他辅助功能，如提示用户输入、中止执行等。与前几个包的比较:

paramiko：方便嵌套系统平台中，擅长远程执行命令，文件传输。

fabric：方便与shell脚本结合，擅长批量部署，任务管理。

pexpect：擅长自动交互，比如ssh、ftp、telnet。

1. 安装Pip & Python 2.7 (or 3.6),并配置环境变量(默认安装会自动添加)
2. 设置Notepad++作为IDE开发工具:运行中输入命令cmd /k python "$(FULL_CURRENT_PATH)" & ECHO. & PAUSE & EXIT
3. 安装第三方库: pip install fabric

Examples:
 
```python 
 #!/usr/bin/env python

 from fabric.api import *

 from fabric.context_managers import *

 from fabric.contrib.console import confirm

 

v.user='root'

 env.gateway='192.168.1.23'

 env.hosts=['192.168.1.21','192.168.1.22']

 env.passwords = {

     'root@192.168.1.21:22': 'SKJh935yft#',

  'root@192.168.1.22:22': 'SKJh935yft#',

  'root@192.168.1.23:22': 'KJSD9325hgs'

 }

 

h="/home/install/lnmp0.9.tar.gz"

/tmp/install"

 

ask

    def put_task():

        run("mkdir -p /tmp/install")

        with settings(warn_only=True):

            result = put(lpackpath, rpackpath)

        if result.failed and not confirm("put file failed, Continue[Y/N]?"):

            abort("Aborting file put task!")

    

    @task

    def run_task():

        with cd("/tmp/install"):

            run("tar -zxvf lnmp0.9.tar.gz")

            with cd("lnmp0.9/"):

                run("./centos.sh")

    

    @task

    def go():

        put_task()

        run_task()

  

  

