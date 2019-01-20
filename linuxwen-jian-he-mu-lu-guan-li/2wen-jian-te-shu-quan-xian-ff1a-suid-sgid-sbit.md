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

与 SUID 非常的类似，若我使用 dmtsai 这个账号去执行 locate 时，那 dmtsai 将会取得 slocate 群组的支持， 因此就能够去读取 mlocate.db 啦！非常有趣吧！





