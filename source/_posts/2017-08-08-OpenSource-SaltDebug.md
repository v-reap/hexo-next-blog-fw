---
title: "saltstack  debug的技巧"
date: 2017-08-08 08:00:00
updated: 2017-08-07 13:59:19
tags: ["Saltstack"]
---
运行 Salt 一旦 shim 发现 Python 解释器，并且一旦 salt_state.tgz tar 包部署完毕（如果有必要的话），最终就可以执行Salt 命令。和传统的 Salt 运行方式不一样，它不需要以守护进程（daemon）方式运行，而是以本地模式（local mode）方式执行 salt-call 命令，就像它是一个 Minion 一样。输出结果会通过 Master 端的 Salt SSH 客户端收集、解析并展示给用户。我们可以以 trace日志级别来查看运行信息。
  
```shell
[root@localhost salt]# salt 's*' test.ping --log-level trace
[DEBUG] Reading configuration from /etc/salt/master
[DEBUG] Missing configuration file: /root/.saltrc
[TRACE] None of the required configuration sections, 'logstash_udp_handler'
and 'logstash_zmq_handler', were found the in the configuration. Not loading
the Logstash logging handlers module.
[DEBUG] Configuration file path: /etc/salt/master
[WARNING ] Insecure logging configuration detected! Sensitive data may be
logged.
[DEBUG] Reading configuration from /etc/salt/master
[DEBUG] Missing configuration file: /root/.saltrc
[DEBUG] MasterEvent PUB socket URI:
ipc:///var/run/salt/master/master_event_pub.ipc
[DEBUG] MasterEvent PULL socket URI:
ipc:///var/run/salt/master/master_event_pull.ipc
[DEBUG] Sending event - data = {'_stamp': '2017-08-07T05:54:21.903494'}
[TRACE] get_event() received = {'tag': 'salt/event/new_client', 'data':
{'_stamp': '2017-08-07T05:54:21.903494'}}
[TRACE] func get_cli_event_returns()
[DEBUG] LazyLoaded mysql.get_load
[DEBUG] LazyLoaded config.option
[INFO ] Using default for mysql ssl_key
[INFO ] Using default for mysql ssl_cert
[INFO ] Using default for mysql ssl_ca
[DEBUG] get_iter_returns for jid 20170806225421938741 sent to set(['salt-
master']) will timeout at 22:54:27.014845
[TRACE] get_event() received = {'tag': 'salt/job/20170806225421938741/new',
'data': {'tgt_type': 'glob', 'jid': '20170806225421938741', 'tgt': 's*',
'_stamp': '2017-08-07T05:54:21.938959', 'user': 'root', 'arg': [], 'fun':
'test.ping', 'minions': ['salt-master']}}
[TRACE] get_event() received = {'tag': 'salt/job/20170806225421938741/ret
/salt-master', 'data': {'fun_args': [], 'jid': '20170806225421938741',
'return': True, 'retcode': 0, 'success': True, 'cmd': '_return', '_stamp':
'2017-08-07T05:54:22.062106', 'fun': 'test.ping', 'id': 'salt-master'}}
[DEBUG] jid 20170806225421938741 return from salt-master
[DEBUG] LazyLoaded nested.output
[TRACE] data = {'salt-master': True}
salt-master:
 True
[DEBUG] jid 20170806225421938741 found all minions set(['salt-master'])
[root@localhost salt]#
  
```