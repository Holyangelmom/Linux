# XFS 文件系统备份 xfsdump

### 1.简介

其实 xfsdump 的功能颇强！他除了可以进行文件系统的完整备份 \(full backup\) 之外，还可以进行累积备份 \(Incremental backup\) 喔！ 用一张简图来说明累积备份 。

![](/assets/xfsdump增量备份.png)

如上图所示，上方的『实时文件系统』是一直随着时间而变化的数据，例如在 /home 里面的文件数据会一直变化一样。 而底下的方块则是 xfsdump 备份起来的数据，第一次备份一定是完整备份，完整备份在 xfsdump 当中被定义为 level 0 喔！等到第二次备份时，/home 文件系统内的数据已经与level 0 不一样了，而 level 1 仅只是比较目前的文件系统与 level 0 之间的差异后，备份有变化过的文件而已。至于 level 2 则是与 level 1 进行比较啦！这样了解呼？至于各个 level 的纪录文件则放置于 /var/lib/xfsdump/inventory 中。

另外，使用 xfsdump 时，请注意底下的限制喔

* xfsdump 只能备份 XFS 文件系统啊！也就是说不能备份目录！
* xfsdump 不支援没有挂载的文件系统备份！所以只能备份已挂载的！
* xfsdump 必须使用 root 的权限才能操作 \(涉及文件系统的关系\)
* xfsdump 备份下来的数据 \(文件或储存媒体\) 只能让 xfsrestore 解析
* xfsdump 是透过文件系统的 UUID 来分辨各个备份档的，因此不能备份两个具有相同 UUID 的文件系统喔！

### 2.xfsdump简单使用

### ![](/assets/xfsdump usage.png)3.用 xfsdump 备份完整的文件系统

（1） 先确定 /boot 是独立的文件系统喔！

![](/assets/查看/boot是否是独立的文件系统.png)

（2）将完整备份的文件名记录成为 /srv/boot.dump 

_xfsdump -l 0 -L boot\_all -M boot\_all -f /srv/boot.dump /boot_

![](/assets/xfsump执行备份.png)

这样很简单的就建立起来 /srv/boot.dump 文件，该文件将整个 /boot/ 文件系统都备份下来了！ 并且将备份的相关信息 \(文件系统/时间/session ID 等等\) 写入 /var/lib/xfsdump/inventory 中，准备让下次备份时可以作为一个参考依据。 

### 4.用 xfsdump 进行累积备份 \(Incremental backups\)

（1）你一定得要进行过完整备份后 \(-l 0\) 才能够继续有其他累积备份 \(-l 1~9\) 的能耐！所以先查看之前有无备份：_xfsdump -I_

![](/assets/xfsdump查看有无Level0备份.png)

（2）制作差异化文件

_dd if=/dev/zero of=/boot/test.img bs=1M count=10_

（3）执行增量备份

_xfsdump -l 1 -L boot\_2 -M boot\_2 -f /srv/boot.dump1 /boot_

（4）查看文件大小

boot.dump1的大小确实是刚刚新增的10M

![](/assets/查看xfsdump level2备份.png)



