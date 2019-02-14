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

_tar -jcv -f /root/etc.newer.then.passwd.tar.bz2 \_

_--newer-mtime="2015/06/17" /etc/\*_

### 4.特殊应用：利用管线命令与数据流

将 /etc 整个目录一边打包一边在 /tmp 解开（你可以将 - 想成是在内存中的一个装置\(缓冲区\)）

_tar -cvf - /etc \| tar -xvf -_



