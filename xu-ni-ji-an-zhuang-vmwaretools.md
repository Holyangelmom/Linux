虚拟机安装VMwareTools

1、启动虚拟机

2、点击VM工具栏中“虚拟机”-&gt;"安装VMware Tools"。

3、若虚拟机是minimal版本，则需安装几个依赖

yum install -y perl

yum install -y net-tools

yum install -y gcc

yum install -y kernel-headers

