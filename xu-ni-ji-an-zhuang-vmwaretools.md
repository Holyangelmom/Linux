虚拟机安装VMwareTools

1、启动虚拟机

2、若虚拟机是minimal版本，则需安装几个依赖

_yum install -y perl_

_yum install -y net-tools_

_yum install -y gcc_

_yum install -y kernel-headers_

3、点击VM工具栏中“虚拟机”-&gt;"安装VMware Tools"，这时会自动挂载安装镜像，若没有自动挂载，则手动挂载

_mount /dec/cdrom /media_

若挂载时报错：mount: block device /dev/sr0 is write-protected,mounting read-only，说明此时/dev/sr0（虚拟硬盘）为写保护。

解决：_mount -o remount,rw /dev/sr0 /media_

4、拷贝至其他目录并解压

_cp VMwareTools-\*\*\*\*\*.tar.gz /home/panaidan_

_tar -xvf VMwareTools-\*\*\*\*\*.tar.gz_

5、进入解压后目录执行，并一路回车即可

_sudo  ./vmware-install.pl_

6、安装完毕则手动重启虚拟机

7、在虚拟机菜单栏上选择“查看”-&gt;“立即适应客户机”

