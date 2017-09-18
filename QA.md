##Openstack 问题集合
---
**1、虚拟机断电后恢复不能启动**
>1.此openstack是通过fuel安装，处理问题时请注意安装方式
>2.宿主机版本：CentOS 6.5
>3.不能通过virsh命令启动


**查看虚拟机状态**
```
nova list --all-tenants
```
**重置虚拟机**
```
nova reset-state 85fb23a5-35fd-40cd-9b4a-1ac74d035ab7
```
**停止虚拟机**
```
nova stop 85fb23a5-35fd-40cd-9b4a-1ac74d035ab7
```
**启动虚拟机**
```
nova start 85fb23a5-35fd-40cd-9b4a-1ac74d035ab7
```