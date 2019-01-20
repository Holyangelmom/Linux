# 文件特殊权限： SUID, SGID, SBIT

### 目录

1. SUID

2. SGID

3. SBIT

4. SUID/SGID/SBIT 权限设定

### 1、SUID

当 s 标志出现在文件owner的 x 项目时，例如 /usr/bin/passwd文件的权限状态为：『-rwsr-xr-x』，此时就被称为 Set UID，简称为 SUID 的特殊权限。基本上 SUID**仅仅对文件** 有这样的限制与功能：

* SUID 权限仅对二进制程序\(binary program\)有效；
* 执行者对于该程序需要具有 x 的可执行权限；
* 本权限仅在执行该程序的过程中有效 \(run-time\)；
* 执行者将具有该程序拥有者 \(owner\) 的权限

![](/assets/SUID程序执行过程.png)

另外，**SUID 仅可用在 binary program 上， 不能够用在 shell script 上面！**这是因为 shell script 只是将很多的 binary 执行档叫进来执行而已！所以 SUID 的权限部分，还是得要看 shell script 呼叫进来的程序的设定， 而不是 shell script 本身。当然，SUID 对于目录也是无效的～这点要特别留意。

### 2、SGID

当 s 标志出现在文件group的 x 项目时，例如/usr/bin/locate文件的权限状态为：『-rwx--s--x』，此时就被称为 Set GID，简称为 SGID 的特殊权限。

与 SUID 不同的是，SGID 可以针对文件或目录来设定！

对于文件，SGID有如下功能：

* SGID 对二进制程序有用；
* 程序执行者对于该程序来说，需具备 x 的权限；
* 执行者在执行的过程中将会获得该程序群组的支持！

对于目录，SGID有如下功能：

* 用户若对于此目录具有 r 与 x 的权限时，该用户能够进入此目录；
* 用户在此目录下的有效群组\(effective group\)将会变成该目录的群组；
* 用途：若用户在此目录下具有 w 的权限\(可以新建文件\)，则使用者所建立的新文件，该新文件的群组与此目录的群组相同

针对文件举个例子：

使用/usr/bin/locate指令查找某个文件时，locate会从/var/lib/mlocate/mlocate.db文件中查找文件的位置，但locate指令和mlocate.db的权限如下：![](/assets/SGID例子.png)

与 SUID 非常的类似，若我使用 dmtsai 这个账号去执行 locate 时，那 dmtsai 将会取得 slocate 群  
组的支持， 因此就能够去读取 mlocate.db 啦！非常有趣吧！

### 3、SBIT

Sticky Bit, 简称SBIT， 目前只针对目录有效，对于文件已经没有效果了。SBIT 对于目录的作用是：

* 当用户对于此目录具有 w, x 权限，亦即具有写入的权限时；
* 当用户在该目录下建立文件或目录时，仅有自己与 root 才有权力删除该文件

换句话说：当甲这个用户于 A 目录是具有群组或其他人的身份，并且拥有该目录 w 的权限， 这表示『甲用户对该目录内任何人建立的目录或文件均可进行 "删除/更名/搬移" 等动作。』 不过，如果将 A 目录加上了 SBIT 的权限项目时，则甲只能够针对自己建立的文件或目录进行删除/更名/移动等动作，而无法删除他人的文件。

举个例子，/tmp的权限是『drwxrwxrwt』，在这样的权限内容下，任何人都可以在 /tmp内新增、修改文件，但仅有该文件/目录建立者与 root 能够删除自己的目录或文件。这个特性也是挺重要的啊！做个简单测试：

1. 以 root 登入系统，并且进入 /tmp 当中；
2. touch test，并且更改 test 权限成为 777 ；
3. 以一般使用者登入，并进入 /tmp；
4. 尝试删除 test 这个文件！
5. 会发现报错

![](/assets/SBIT测试.png)

### 4、SUID/SGID/SBIT 权限设定

SUID/SGID/SBIT的权限分别为：

* **4为 SUID**
* **2为 SGID**
* **1为 SBIT**

uasage：

![](/assets/特殊权限设定.png)

最后一个操作执行后特殊权限都是大写，是因为user, group 以及others 都没有 x 这个可执行的标志\( 因为 666 嘛 \)，所以，这个 S, T 代表的就是『空的』啦！除了用数字更改权限，还可以用符号法修改。

