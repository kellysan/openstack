##nova操作命令
[TOC]
>执行命令之前先 source 环境变量
>nova命令操作都在控制节点

==实例表名称：nova==
==instances 实例表==
==volumes 磁盘表==
==service nova服务列表==

** 1、查看计算节点情况**
```shell
nova-manage service list
```
==笑脸表示节点正常，XXX表示节点有问题==
** 2、disable 失效节点**
```shell
nova service-disable Host Binary
```
** 3、查看计算节点资源使用情况**
```shell
nova-manage service describe_resource Host
```
** 4、查看instance状态** **
```shell
nova show UUID
```
** 5、查看instance启动情况**
```shell
nova list
```

##nova虚拟机“冷迁移”
** 1、关闭虚拟机**
```shell
nova stop UUID
nova list #查看下状态
```
** 2、通过scp或者rsync 同步instance到目标节点**
```shell
rsync -avzP /var/lib/nova/instances/UUID root@172.16.170.111:/var/lib/nova/instances/
```
** 3、修改虚拟机节点信息**
==查看instance属于哪个节点==
```shell
nova show UUID |grep 'OS-EXT-SRV-ATTR:host'
```
**修改数据库数据(host和node)**
==旧节点：node-7.domain.tld 新节点：node-10.domain.tld==
==修改完成后需要再次查看是否修改正确==
```sql
use nova
select host,node from nova.instances where uuid = 'UUID'
update nova.instances set host = 'node-7.domain.tld',node = 'node-10.domain.tld' where uuid = 'UUID'
```
** 4、目标节点重启nova-computer服务**
```shell
/etc/init.d/openstack-nova-compute restart
```
** 5、启动instance**
== 控制节点操作==
```shell
nova start UUId
nova list #查看状态
```