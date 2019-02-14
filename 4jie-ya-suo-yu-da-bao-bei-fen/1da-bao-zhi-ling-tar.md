# 打包指令tar

### 目录

1. tar指令详情
2. 仅解压缩单个文件
3. 仅备份比某个时刻还要新的文件
4. 特殊应用：利用管线命令与数据流
5. 备份系统及还原
6. 解压后的SELinux问题

### 1.tar指令详情

![](/assets/tar usage.png)

### 2.仅解压缩单个文件

先找到我们要的档名，假设解开 shadow 文件好了

_tar -jtv -f /root/etc.tar.bz2 \| grep 'shadow'_

再进行解压

_tar -jxv -f /root/etc.tar.bz2 etc/shadow_

### 3.仅备份比某个时刻还要新的文件

先由 find 找出比 /etc/passwd 还要新的文件

_find /etc -newer /etc/passwd_

使用 tar 来进行打包吧！日期为上面看到的 2015/06/17

_tar -jcv -f /root/etc.newer.then.passwd.tar.bz2 \  
--newer-mtime="2015/06/17" /etc/\*_

### 4.特殊应用：利用管线命令与数据流

将 /etc 整个目录一边打包一边在 /tmp 解开（你可以将 - 想成是在内存中的一个装置\(缓冲区\)）

_tar -cvf - /etc \| tar -xvf -_

### 5.备份系统及还原

（1）建立备份目录并设置权限（也可以备份到网盘或者U盘）

_mkdir /backups  
chmod 700 /backups  
ll -d /backups_

（2）有些目录是无用的，例如“/proc”、“/lost+ found”（Ubuntu特有文件夹）、“/sys”。当然，“backup.gz”这个档案文件本身必须排除在外，否则你可能会得到一些超出常理的结果。如果不把“/mnt”排 除在外，那么挂载在“/mnt”上的其它分区也会被备份。另外需要确认一下“/media”上没有挂载任何东西\(例如光盘、移动硬盘\)，如果有挂载东西， 必须把“/media”也排除在外。下边执行备份：

_tar -cvpz -f /backups/backup.tgz --exclude=/proc \_

_--exclude=/lost+found --exclude=/backup.tgz \_

_ --exclude=/mnt --exclude=/sys \_

_/_

（3）还原

_tar -xvpz -f backup.tgz -C /_

（4）别忘了重新创建那些在备份时被排除在外的目录

_mkdir proc_

_mkdir lost+found_

_mkdir mnt_

_mkdir sys_

### 6.解压后的SELinux问题

如果因为某些缘故，所以你的系统必须要以备份的数据来回填到原本的系统中，那么得要特别注意复原后的系统的 SELinux 问题！它可能会让你的系统无法存取某些配置文件内容，导致影响到系统的正常使用权。

**鸟哥：**

这两天 \(2015/07\) 接到一个网友的 email，他说他使用鸟哥介绍的方法透过 tar 去备份了 /etc 的数据，然后尝试在另一部系统上面复原回来。 复原倒是没问题，但是复原完毕之后，无论如何就是无法正常的登入系统！明明使用单人维护模式去操作系统时，看起来一切正常～但就是无法顺利登入。其实这个问题倒是很常见！大部分原因就是因为 /etc/shadow 这个密码文件的 SELinux 类型在还原时被更改了！导致系统的登入程序无法顺利的存取它， 才造成无法登入的窘境。

那如何处理呢？简单的处理方式有这几个：

* 透过各种可行的救援方式登入系统，然后修改 /etc/selinux/config 文件，将 SELinux 改成 permissive 模式，重新启动后系统就正常了；
* 在第一次复原系统后，不要立即重新启动！先使用 restorecon -Rv /etc 自动修复一下 SELinux 的类型即可。
* 透过各种可行的方式登入系统，建立 /.autorelabel 文件，重新启动后系统会自动修复 SELinux 的类型，并且又会再次重新启动，之后就正常了！

鸟哥个人是比较偏好第 2 个方法，不过如果忘记了该步骤就重新启动呢？那鸟哥比较偏向使用第 3个方案来处理，这样就能够解决复原后的 SELinux 问题啰！ 至于更详细的 SELinux ，我们得要讲完程序 \(process\) 之后，你才会有比较清楚的认知，因此还请慢慢学习，到第 16 章你就知道问题点了！ ^\_^

