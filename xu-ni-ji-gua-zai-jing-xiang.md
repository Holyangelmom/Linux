# 虚拟机挂载安装时所用的ISO镜像

### 1、在虚拟机设置中挂载iso镜像

若不设置，则直接挂在会报错：mount: no medium found on /dev/sr0

### ![](/assets/虚拟机设置挂载镜像.png)2、执行挂载，**前提是需要虚拟机中已安装VMwareTools**

_sudo mount /dev/sr0 /mnt/cdrom_

/mnt/cdrom是自己手动建的目录

