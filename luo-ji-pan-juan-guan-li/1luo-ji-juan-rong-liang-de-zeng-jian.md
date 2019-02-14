# 逻辑卷容量的增减

### 1.LVM扩容（分配LVM剩余空间）

**（1）查看LVM是否有剩余容量**

_vgdisplay_

![](/assets/查看LVM剩余容量.png)

**（2）查看待扩容的逻辑卷路径**

_lvdisplay_

![](/assets/查看逻辑卷名称.png)

**（3）扩容**

_lvextend -L +3G /dev/centos00/root_

![](/assets/执行lvm扩容.png)**（4）同步到文件系统**

使用resize2fs同步到文件系统：_resize2fs /dev/centos00/root_，报错。因为resize2fs只适用于ext2/ext3/ext4。应使用xfs\_growfs同步。

![](/assets/xfs系统使用resize2fs同步lvm.png)

2.LVM扩容（分配磁盘空间）

