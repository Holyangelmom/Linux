### 1、SUID

例如 /usr/bin/passwd文件的权限状态为：『-rwsr-xr-x』，此时就被称为 Set UID，简称为 SUID 的特殊权限。基本上 SUID 有这样的限制与功能：

* SUID 权限仅对二进制程序\(binary program\)有效；
* 执行者对于该程序需要具有 x 的可执行权限；
* 本权限仅在执行该程序的过程中有效 \(run-time\)；
* 执行者将具有该程序拥有者 \(owner\) 的权限





