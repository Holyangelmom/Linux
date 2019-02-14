# XFS 文件系统还原 xfsrestore

### 1.xfsrestore usage

### ![](/assets/xfsrestore usage.png)2.还原备份

（1）简单复原 level 0 的文件系统

执行指令：

_xfsrestore -f /srv/boot.dump -L boot\_all /boot_

直接复原的结果就是『同名的文件会被覆盖，其他系统内新的文件会被保留』喔！

**可以选择性还原部分文件**：_xfsrestore -f /srv/boot.dump -L boot\_all -s grub2 /tmp/boot2_

（2）还原增量备份文件

其实复原累积备份与复原单一文件系统相似耶！如果备份数据是由 level 0 -&gt; level 1 -&gt; level 2... 去进行的， 当然复原就得要相同的流程来复原！因此当我们复原了 level 0 之后，接下来当然就要复原level 1 到系统内啊！ 执行指令：

_xfsrestore -f /srv/boot.dump1 /boot_

