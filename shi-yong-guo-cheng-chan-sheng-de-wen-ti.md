# 简单问题

### 1、CentOS7 yum安装软件提示:another app is currently holding the yum lock;waiting for it to exit

![](/assets/packagekit占用yum.png)

yum在锁定状态中,可以通过强制关掉yum进程：

rm -f /var/run/yum.pid

### 2、xxx is not in the sudoers file.This incident will be reported.

1. 切换到root用户下
2. 添加sudo文件的写权限：_chmod u+w /etc/sudoers
3. 编辑sudoers文件
4.  撤销sudoers文件写权限：_chmod u-w /etc/sudoers


