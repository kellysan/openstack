##openstack glance操作
```shell
glance image-create --name "New_Base_CentOS6" --file 2b1a6db5-5035-4932-ac33-c1cebb95fb2f --disk-format qcow2 --container-format bare --is-public true --progress
```
>命令解读
image create
--name NAME 镜像名称
--file 源快照ID或源文件目录
--disk-format 镜像格式
--container-format bare 图片的容器格式
--is-public [treu][false] 是否共享镜像
--progress 上传进度条
