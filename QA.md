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
**2、删除无用节点数据库信息**
>1、虚拟机移除某个节点后，数据库内容不会自动清除，需要手工删除
>2、使用fuel检查会出现错误
>3、涉及到的表 nova.compute_nodes,nova.services

**compute_node节点表**
查询表中节点信息
```sql
select * from nova.compute_nodes where hypervisor_hostname = 'node-11.domain.tld'\G
*************************** 1. row ***************************
          created_at: 2017-09-26 11:53:56
          updated_at: 2017-09-27 07:45:30
          deleted_at: NULL
                  id: 4
          service_id: 9
               vcpus: 32
           memory_mb: 128948
            local_gb: 11119
          vcpus_used: 0
      memory_mb_used: 512
       local_gb_used: 0
     hypervisor_type: QEMU
  hypervisor_version: 2000000
            cpu_info: {"vendor": "Intel", "model": "Haswell", "arch": "x86_64", "features": ["ssse3", "smap", "avx", "fxsr", "clflush", "sep", "rtm", "vme", "dtes64", "invpcid", "msr", "sse", "xsave", "pge", "vmx", "erms", "xtpr", "cmov", "hle", "smep", "pcid", "est", "pat", "monitor", "smx", "tsc", "lm", "abm", "adx", "3dnowprefetch", "nx", "syscall", "tm", "sse4.1", "pae", "sse4.2", "pclmuldq", "acpi", "fma", "tsc-deadline", "popcnt", "mmx", "osxsave", "cx8", "mce", "mtrr", "rdtscp", "ht", "dca", "lahf_lm", "rdseed", "pdcm", "mca", "pdpe1gb", "apic", "fsgsbase", "f16c", "pse", "ds", "pni", "tm2", "avx2", "aes", "sse2", "ss", "bmi1", "bmi2", "pbe", "de", "fpu", "cx16", "pse36", "ds_cpl", "movbe", "rdrand", "x2apic"], "topology": {"cores": 8, "threads": 2, "sockets": 1}}
disk_available_least: 11113
         free_ram_mb: 128436
        free_disk_gb: 11119
    current_workload: 0
         running_vms: 0
 hypervisor_hostname: node-11.domain.tld
             deleted: 0
             host_ip: 10.20.0.5
 supported_instances: [["i686", "qemu", "hvm"], ["i686", "kvm", "hvm"], ["x86_64", "qemu", "hvm"], ["x86_64", "kvm", "hvm"]]
           pci_stats: []
             metrics: []
     extra_resources: NULL
               stats: {}
       numa_topology: {"cells": [{"mem": {"total": 65442, "used": 0}, "cpu_usage": 0, "cpus": "0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30", "id": 0}, {"mem": {"total": 65536, "used": 0}, "cpu_usage": 0, "cpus": "1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31", "id": 1}]}
1 row in set (0.00 sec)
```
删除表中节点数据
```sql
delete from nova.compute_nodes where hypervisor_hostname = 'node-11.domain.tld';
```
** nova.services服务表**
查询表中数据
```sql
select * from nova.service where host = 'node-11.domain.tld'\G
*************************** 1. row ***************************
     created_at: 2017-09-26 11:53:55
     updated_at: 2017-09-27 08:57:53
     deleted_at: NULL
             id: 9
           host: node-11.domain.tld
         binary: nova-compute
          topic: compute
   report_count: 7545
       disabled: 0
        deleted: 0
disabled_reason: NULL
1 row in set (0.00 sec)
```
删除服务数据
```sql
delete from nova.services where host = 'node-11.domain.tld'
```