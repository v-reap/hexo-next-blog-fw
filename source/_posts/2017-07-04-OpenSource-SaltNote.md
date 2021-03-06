---
title: "slatstack注意点及解决方案"
date: 2017-07-04 13:00:00
updated: 2017-07-05 11:18:38
tags: ["Python","运维"]
---
研究slatstack时踩过的坑，注意点及解决方案
运行问题
  
1、直接物理性移除minion或者更换minion原先连接的master，会导致先前的master始终无法ping通minion
  
[root@localhost salt]# salt '*' test.ping
minion_centos7:
 True
win_minion:
 True
10.1.240.193:
 Minion did not return. [No response]
解决办法：在master上查看已接受的key
  
[root@localhost salt]# salt-key -L
Accepted Keys:
10.1.240.193
minion_centos7
win_minion
Denied Keys:
Unaccepted Keys:
Rejected Keys:
把删除的那台minion的key删除就好了
  
[root@localhost salt]# salt-key -d 10.1.240.193
其他汇总
  
1、minion安装的版本不同可能会导致master的部分请求解析失败，master同理
  
解决办法：查看minion的版本，升级或降级，务必统一版本
  
2、遇到一次同步后的grains为空白的情况，grains空白后导致整个minion连接异常，无论执行任何正常命令，报错都是minion无响应，与minion不在线的提示相同
  
不知什么原因，目前怀疑是与salt的modules命名相同或者是grains中有异常代码导致。前者还好处理，后者的话可能要加安全校验机制
  
解决办法：cd到/var/cache/salt/minion/extmods/grains/路径，把与modules同名的文件删除即可，不行的话清空整个目录内的文件
  
windows系统的解决办法差不多，找到安装目录下的grains存放位置，把异常的文件删除即可，比如我传来的grains是存放在C:\salt\var\cache\salt\minion\extmods\grains
  
3、saltstack模块内部出现错误时，salt内部抛出且不会回显，但是调用该模块时会出现问题，所以若模块调用正常，但写成api时结果不正常，有可能是模块的问题
  
4、minion上的防火墙服务firewalld开启可能会导致Minion did not return. [No response]
  
如果需要传参的话，最好不要用debug模式，因为-l debug这些字符可能传参处理时会引发异常从而导致运行失败
  
5、由于salt返回的一律是字典型，所以将输出写入文件时一定要转化为字符串，用str处理就好
  
6、可用**kwargs来接收不限数量的参数，但外部参数必须用kwargs['__pub_arg'][0]来获得，而且一旦不能转化为字典型就会报错
  
7、传送大文件时，有时返回成功，但是传送文件的目录里，文件名后面有个.part证明传输异常中断了
  
8、salt-master的ip有时会变，若配置文件没相应变更ip，重启salt时会提示[WARNING ] Unable to bind socket,
error: [Errno 99] Cannot assign requested
  
9、写grains时，grains的模块名字命名不可以与grains.item 的命名相同，否则只会输出none
  
