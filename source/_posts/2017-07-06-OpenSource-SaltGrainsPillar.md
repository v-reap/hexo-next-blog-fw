---
title: "saltstack grains 与 pillar比较"
date: 2017-07-06 10:00:00
updated: 2017-07-06 11:37:07
tags: ["读书","Python","运维","Saltstack"]
---
saltstack中有pillar和grains的概念，获得pillar数据和grains数据的命令差不多。获取pillar数据的命令：
salt '*' pillar.data nginx  
  
获得grains数据的命令：
salt '*' grains.item nginx  
  
尽管看上去差不多，但是两者还是有本质区别的，
pillar数据是存储在master端的，而在客户端有缓存，通常pillar数据是一些配置信息，pillar数据可以通过salt '*'
saltutil.refresh_pillar来刷新pillar数据。
  
而grains数据通常是minion端的一些特性数据，比如操作系统类型，磁盘，内存信息等，他是存储在minion端的，缓存在master端的。
  
名称存储位置数据类型数据采集更新方式应用
Grains
Minion端
静态数据
Minion启动时采集，也可以使用saltutil.sync_grains进行刷新。
存在Minion基本数据。比如用于匹配Minion，自身数据可以用来做资产管理等。
Pillar
Master端
动态数据
在Master端定义，指定给对应的Minion。可以使用saltutil.refresh_pillar刷新。
存储Master指定的数据，只有指定的Minion可以看到。用于敏感数据保存。
  
