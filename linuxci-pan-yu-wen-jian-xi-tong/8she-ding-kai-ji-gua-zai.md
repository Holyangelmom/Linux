# 设定开机挂载

1、前言

手动处理 mount 不是很人性化，我们总是需要让系统『自动』在开机时进行挂载的！也就是说刚刚手动挂载的分区若不写入/etc/fstab，下次开机时将不会自动挂载。当然系统挂载也有一些限制：

* 根目录 / 是必须挂载的﹐而且一定要先于其它 mount point 被挂载进来。
* 其它 mount point 必须为已建立的目录﹐可任意指定﹐但一定要遵守必须的系统目录架构原则 \(FHS\)
* 所有 mount point 在同一时间之内﹐只能挂载一次。
* 所有 partition 在同一时间之内﹐只能挂载一次。
* 如若进行卸除﹐您必须先将工作目录移到 mount point\(及其子目录\) 之外

2、开机挂载 /etc/fstab 及 /etc/mtab

先查阅/etc/fstab，每列代表信息是这样：\[装置/UUID 等\] \[挂载点\] \[文件系统\] \[文件系统参数\] \[dump\] \[fsck\]

第一栏：磁盘装置文件名/UUID/LABEL name

*  文件系统或磁盘的装置文件名，如 /dev/vda2 等
*  文件系统的 UUID 名称，如 UUID=xxx
*  文件系统的 LABEL 名称，例如 LABEL=xxx

第二栏：挂载点 \(mount point\):

*  一定是目录

第三栏：磁盘分区槽的文件系统

*  包括 xfs, ext4, vfat, reiserfs, nfs 等等

第四栏：文件系统参数

*  同时具有 rw, suid, dev, exec, auto, nouser, async 等参数。 基本上，预设情况使用defaults 设定即可！其他参数可查阅资料。

第五栏：能否被 dump 备份指令作用

*  dump 是一个用来做为备份的指令，不过现在有太多的备份方案了，所以这个项目可以不要理会啦！直接输入 0 就好了！

第六栏：是否以 fsck 检验扇区：

*  早期开机的流程中，会有一段时间去检验本机的文件系统，看看文件系统是否完整 \(clean\)。 不过这个方式使用的主要是透过 fsck 去做的，我们现在用的 xfs 文件系统就没有办法适用，因为 xfs会自己进行检验，不需要额外进行这个动作！所以直接填 0 就好了

![](/assets/查阅/etc/fstab.png)若在 /etc/fstab 输入的数据错误，导致无法顺利开机成功。所以先测试一下是否写入成功。下面将/dev/sda4、/dev/sda5自动挂载到系统：

查看这两装置是否挂载

![](/assets/查看挂载.png)

若挂载先卸载

_sudo umount /dev/sda4_

_sudo umount /dev/sda5_

先使用blkid查看UUID再将他们写入/etc/fstab，**文件系统这一列别写错了。**

![](/assets/写入/etc/fstab.png)最后用 mount命令挂载 fstab 中的所有文件系统：mount -a，若写入数据有误系统会提示。改正后即可reboot。over！装置可以使用啦！



