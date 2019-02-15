# 其他常见的压缩与备份工具：dd，cpio

### 1.dd

dd指令通常用以指定大小的块拷贝一个文件，并在拷贝的同时进行指定的转换。但dd 指令最大的功效，鸟哥认为，应该是在于『备份』啊！ 因为 dd 可以读取磁盘装置的内容\(几乎是直接读取扇区"sector"\)，然后将整个装置备份成一个文件呢！

**（1）dd usage**

![](/assets/dd usage.png)**（2）备份文件**

_dd if=/etc/passwd of=/tmp/passwd.back_

**（3）备份光驱至映像档**

_dd if=/dev/sr0 of=/tmp/system.iso_

**（4）备份映像档至USB**

假设USB 是 /dev/sda

_dd if=/tmp/system.iso of=/dev/sda_

**（5）备份整个文件系统至映像档**

_dd if=/dev/vda2 of=/tmp/vda2.img_

**（6）备份磁盘A至磁盘B**

_dd if=/dev/sda of=/dev/sdb_

当然/dev/sdb 不需要分区与格式化， 因为该指令可以将 /dev/sda 内的所有资料，包括 MBR 与partition table 也复制到 /dev/sdb ！ 不过进行该操作时，恐怕得要理解一下每种文件系统的挂载要求！比如XFS文件系统，清除 文件系统内的 log 之后， 重新给予一个跟原本不一样的 UUID 后，才能够顺利挂载！同时，为了让系统继续利用后续没有用到的磁盘空间，那个 xfs\_growfs 就得要理解一下。

### 2.cpio

cpio 可以备份任何东西，包括装置设备文件。不过 cpio 有个大问题， 那就是 cpio 不会主动的去找文件来备份！cpio 得要配合类似 find 等可以找到文件名的指令来告知 cpio 该被备份的数据在哪里啊！ 虽然cpio 好像不怎么好用呦！但是，他可是备份的时候的一项利器呢！因为他可以备份任何的文件，包括 /dev 底下的任何装置文件！

（1）cpio usage

![](/assets/cpio usage.png)（2）备份boot（必要时切换至root）

我们先进入/，再进行备份。因为cpio不会理会你给的是绝对路径还是相对路径的文件名，所以如果你加上绝对路径的 / 开头,那么未来解开的时候，它就一定会覆盖掉原本的 /boot 耶！那就太危险了！这个我们在 tar 也稍微讲过那个 -P 的选项！！理解吧！

_cd /_

_find boot \| cpio -oBvc &gt; /tmp/boot.cpio_

解压：

_cd /root_

_cpio -iducv &lt; /tmp/boot.cpio_

（3）备份全部文件到磁带机（必要时切换至root）

find / \| cpio -oBcv &gt; /dev/st0

解压：

cd /

cpio -idcv &lt; /dev/st0

