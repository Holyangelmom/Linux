# 磁盘分区及格式化：fdisk，gdisk，mkfs

### 1、前言

对磁盘进行分区，可使用fdisk或gdisk，但是要注意的是：**MBR 分区表请使用 fdisk 分区， GPT 分区表请使用 gdisk 分区！**

### 2、gdisk，mkfs

（1）、查看分区表，确定使用gdisk还是fdisk

sudo parted -l

![](/assets/查看分区表.png)

（2）、查看硬盘sda是否有剩余的容量

sudo gdisk /dev/sda

发现剩余容量14G

![](/assets/查看剩余容量.png)

![](/assets/查看剩余容量2.png)

（3）、新增分区

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

（4）、查看新增分区是否生效

因为核心还没有更新，因此没有新增的分区。两种方法处理：一是重启，二是使用partprobe指令更新核心。下面使用partprobe更新。

![](/assets/查看新增分区是否生效.png)

更新核心：_sudo partprobe -s_

![](/assets/打印分区号.png)

查看更新是否生效：cat /proc/partitions

![](/assets/查看更新是否生效.png)

![](/assets/查看更新是否生效2.png)

（5）、分区后需要进行文件系统格式化



### 3、fdisk

（预留）

