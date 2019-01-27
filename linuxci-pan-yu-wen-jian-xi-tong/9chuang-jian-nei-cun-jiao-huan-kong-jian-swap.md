# 创建内存交换空间swap

1、前言

以前的年代因为内存不足，因此那个可以暂时将内存的程序拿到硬盘中暂放的内存置换空间 \\(swap\\)就显的非常的重要！ 否则，如果突然间某支程序用掉你大部分的内存，那你的系统恐怕有损毁的情况发生喔！所以，早期在安装 Linux 之前，大家常常会告诉你： 安装时一定需要的两个 partition ，一个是根目录，另外一个就是 swap\\(内存置换空间\\)。

一般来说，如果硬件的配备资源足够的话，那么 swap 应该不会被我们的系统所使用到， swap 会被利用到的时刻通常就是物理内存不足的情况了。我们知道 CPU 所读取的数据都来自于内存， 那当内存不足的时候，为了让后续的程序可以顺利的运作，因此在内存中暂不使用的程序与数据就会被挪到 swap 中了。 此时内存就会空出来给需要执行的程序加载。由于swap 是用磁盘来暂时放置内存中的信息，所以用到 swap 时，你的主机磁盘灯就会开始闪个不停啊！

虽然目前\\(2015\\)主机的内存都很大，至少都有 4GB 以上啰！因此在个人使用上，你不要设定 swap 在你的 Linux 应该也没有什么太大的问题。不过服务器可就不这么想了～由于你不会知道何时会有大量来自网络的要求，因此最好还是能够预留一些 swap 来缓冲一下系统的内存用量！ 至少达到『备而不用』的地步啊！

透过本章上面谈到的方法，你可以使用如下的方式来建立你的 swap 啰！

* 设定一个 swap partition
* 建立一个虚拟内存的文件

2、使用实体分区槽建置 swap

建立 swap 分区槽的方式也是非常的简单的！透过底下几个步骤就搞定啰：

1. 分区：先使用 gdisk 在你的磁盘中分区出一个分区槽给系统作为 swap 。由于 Linux 的 gdisk 预设会将分区槽的 ID 设定为 Linux 的文件系统，所以你可能还得要设定一下 system ID 就是了。
2. 格式化：利用建立 swap 格式的『mkswap 装置文件名』就能够格式化该分区槽成为 swap 格式啰
3. 使用：最后将该 swap 装置启动，方法为：『swapon 装置文件名』。
4. 观察：最终透过 free 与 swapon -s 这个指令来观察一下内存的用量吧！

下面试验一下：

（1）、建立分区

查看分区表，确定使用gdisk还是fdisk

_sudo parted -l_

![](/assets/查看分区表.png)

使用gdisk查看整颗硬盘sda是否有剩余的容量

_sudo gdisk /dev/sda_

从打印信息发现剩余容量12G

![](/assets/sda剩余容量.png)

新增分区

![](/assets/新增swap分区.png)

更新核心

![](/assets/更新核心.png)

（2）、格式化swap分区

格式化：mkswap /dev/sda6

![](/assets/格式化swap分区.png)

查看格式化是否成功：sudo blkid \|grep sda6

![](/assets/查看swap是否格式化成功.png)查看swap空间是否增加：free -h，发现还是1G。

![](/assets/查看swap是否增加.png)

（3）、启动swap装置

swapon /dev/sda6

![](/assets/启动swap装置.png)

再次查看swap空间，已经增加了1G

![](/assets/再次查看swap空间.png)

（4）、写入开机挂载文件/etc/fstab

先卸载刚挂载的swap：sudo swapoff /dev/off

![](/assets/卸载swap.png)

写入/etc/fstab，第二列挂载点则写swap，因为swap没有挂载点。不过我试验的第一次竟然报错，提示找不到UUID=XXX，第二次却成功。（脑瓜疼）

![](/assets/swap写入/etc/fstab.png)最后测试写入文件是否正确：sudo swapon -a

![](/assets/测试swap是否正确写入文件.png)

3、使用文件建置 swap

