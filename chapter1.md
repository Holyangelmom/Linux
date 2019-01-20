# 常用命令

## 目录

1. 查看内核信息

2. 查看版本信息

3. 查看发行版信息

4. 查看CPU信息

5. 改变属性与权限

6. 设置root密码

### 1、查看内核信息

_uname －a_

![](/assets/查看内核信息.png)

### 2、查看版本信息

_cat /proc/version_

![](/assets/查看版本信息.png)

### 3、查看发行版信息

_cat /etc/issue_

![](/assets/查看发行版信息.png)

### 4、查看CPU信息

_cat /proc/cpuinfo_![](/assets/查看CPU信息.png)

### 5、改变属性与权限

chgrp \[-R\] 组名 dirname/filename

chown \[-R\] 账户名 dirname/filename

chmod \[-R\] 权限操作 dirname/filename，其中权限操作有以下两种

##### **（1）数字类型改变文件权限**

chmod \[-R\] xyz dirname/filename xyz代表文档拥有者、群组、其他人的读写执行的权限数字总和，读写执行的权限分别为4/2/1。

![](/assets/数字类型权限.png)

##### （2）符号类型改变文件权限

### ![](/assets/符号类型权限.png)![](/assets/符号类型权限使用一.png)![](/assets/符号类型权限使用二.png)6、设置root密码

_su passwd_

![](/assets/设置root密码.png)

