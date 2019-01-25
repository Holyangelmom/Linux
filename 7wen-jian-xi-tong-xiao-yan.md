### 简单问题

### 1、CentOS7 yum安装软件提示:another app is currently holding the yum lock;waiting for it to exit

![](/assets/packagekit占用yum.png)

yum在锁定状态中,可以通过强制关掉yum进程：

_rm -f /var/run/yum.pid_

### 2、xxx is not in the sudoers file.This incident will be reported.

1. 切换到root用户下
2.  添加sudo文件的写权限：\\_chmod u+w /etc/sudoers
3. 编辑sudoers文件：vi /etc/sudoers，找到这行 root ALL=\\(ALL\\) ALL,在他下面添加xxx ALL=\\(ALL\\) ALL \\(这里的xxx是你的用户名\\)
4. 撤销sudoers文件写权限：chmod u-w /etc/sudoers

### 3、centos7增加启动终端快捷键

点击系统工具-》设置-》设备-》键盘，点击增加，输入快捷项名称和命令：

![](/assets/终端启动快捷键.png)

