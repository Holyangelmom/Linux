# Linux的Ext2文件系统

### 目录

1. Ext2文件系统
2. data block
3. inode table
4. superblock

### 1、Ext2文件系统

文件系统一开始就将 inode 与 block 规划好了，除非重新格式化\(或者利用resize2fs 等指令变更文件系统大小\)，否则 inode 与 block 固定后就不再变动。整个来说，Ext2 格式化后有点像底下这样：

![](/assets/Ext2文件系统示意图.png)

### 2、data block

![](/assets/block大小产生的限制.png)

### ![](/assets/block基本限制.png)3、inode table

基本上，inode 记录的文件数据至少有底下这些：

* 该文件的存取模式\(read/write/excute\)；
* 该文件的拥有者与群组\(owner/group\)；
* 该文件的容量；
* 该文件建立或状态改变的时间\(ctime\)； 最近一次的读取时间\(atime\)；
* 最近修改的时间\(mtime\)；
* 定义文件特性的旗标\(flag\)，如 SetUID...；
* 该文件真正内容的指向 \(pointer\)

inode 的数量与大小也是在格式化时就已经固定了，除此之外 inode 还有些以下特色：

* 每个 inode 大小均固定为 128 bytes \(新的 ext4 与 xfs 可设定到 256 bytes\)；
* 每个文件都仅会占用一个 inode 而已；
* 承上，因此文件系统能够建立的文件数量与 inode 的数量有关；
* 系统读取文件时需要先找到 inode，并分析 inode 所记录的权限与用户是否符合，若符合才能够开始实际
* 读取 block 的内容。

我们约略来分析一下 EXT2 的 inode / block 与文件大小的关系好了。inode 要记录的数据非常多，但偏偏又只有 128bytes 而已， 而 inode 记录一个 block 号码要花掉 4byte ，假设我一个文件有400MB 且每个 block 为 4K 时， 那么至少也要十万笔 block 号码的记录呢！inode 哪有这么多可记录的信息？为此我们的系统很聪明的将 inode 记录 block 号码的区域定义为 12 个直接，一个间接,一个双间接与一个三间接记录区。这是啥？我们将 inode 的结构画一下好了。

![](/assets/inode示意图.png)

### 4、superblock

superblock记录的信息主要有：

* block 与 inode 的总量；
* 未使用与已使用的 inode / block 数量；
* block 与 inode 的大小 \(block 为 1, 2, 4K，inode 为 128bytes 或 256bytes\)；
* filesystem 的挂载时间、最近一次写入数据的时间、最近一次检验磁盘 \(fsck\) 的时间等文件系统的相关信息；
* 一个 valid bit 数值，若此文件系统已被挂载，则 valid bit 为 0 ，若未被挂载，则 valid bit 为 1 。



