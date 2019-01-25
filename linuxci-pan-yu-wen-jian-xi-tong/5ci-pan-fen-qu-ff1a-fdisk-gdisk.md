# 磁盘分区及格式化：fdisk，gdisk，mkfs

### 目录

1. 前言
2. gdisk，mkfs
3. fdisk，mkfs

### 1、前言

对磁盘进行分区，可使用fdisk或gdisk，但是要注意的是：**MBR 分区表请使用 fdisk 分区， GPT 分区表请使用 gdisk 分区！要想使用一个磁盘要经理三个主要步骤：分区、格式化、挂载。**

### 2、gdisk，mkfs

##### （1）、查看分区表，确定使用gdisk还是fdisk

_sudo parted -l_

**因为使用gpt进行分区，所以不会有MBR的分区限制！**

![](/assets/查看分区表.png)

##### （2）、查看硬盘sda是否有剩余的容量

_sudo gdisk /dev/sda_

发现剩余容量14G

![](/assets/查看剩余容量.png)

![](/assets/查看剩余容量2.png)

##### （3）、新增分区

增加以下三个分区：

* 1GB 的 xfs 文件系统 \(Linux\)
* 1GB 的 vfat 文件系统 \(Windows\)
* 0.5GB 的 swap \(Linux swap\)\(这个分区等一下会被删除喔！\)

![](/assets/新增分区1.png)

![](/assets/新增分区2.png)

![](/assets/新增分区3.png)

查看新增分区

![](/assets/查看新增分区.png)

保存

![](/assets/写入分区.png)

##### （4）、查看新增分区是否生效

因为核心还没有更新，因此没有新增的分区。两种方法处理：一是重启，二是使用partprobe指令更新核心。下面使用partprobe更新。

![](/assets/查看新增分区是否生效.png)

更新核心：_sudo partprobe -s_

![](/assets/打印分区号.png)

查看更新是否生效：_cat /proc/partitions_

![](/assets/查看更新是否生效.png)

![](/assets/查看更新是否生效2.png)

##### （5）、尝试删除第6分区

![](/assets/删除分区.png)

##### （6）、文件系统格式化

分区成功后需要文件系统格式化，可使用mkfs，也可输入mkfs后按下tab键查看其它命令。

格式化/dev/sda4：_sudo mkfs.xfs /dev/sda4_

![](/assets/格式化sda4.png)

![](/assets/格式化sda4-2.png)

格式化/dev/sda5：_mkfs -t vfat /dev/sda5_

![](/assets/格式化sda5.png)

![](/assets/格式化sda5-2.png)

使用dumpe2fs查看文件系统：_sudo dumpe2fs -h /dev/sda4_

可以看到以下错误提示，是因为dumpe2fs仅可以查看ext2/ext3/ext4文件系统，不支持xfs。

![](/assets/dumpe2fs错误提示.png)

再使用xfs\_growfs查看文件系统详情_：sudo xfs\_growfs /dev/sda4_

可以看到以下错误提示，是因为刚刚格式化的sda4并没有挂载到Linux目录中。

![](/assets/xfs_growfs错误提示.png)

##### （7）、文件系统挂载

见《Linux磁盘与文件系统》第六篇《6.文件系统挂载与卸载》

### 3、fdisk，mkfs

（预留）

