# 文件查找

### 1、which查找执行档

which指令是根据『PATH』这个环境变量所规范的路径，去搜寻『执行档』的档名

![](/assets/which usage.png)

![](/assets/which 例子.png)

### 2、whereis查找特定目录的文件

whereis只会查找特定目录中的文件，特定目录可以用whereis -l查看。

![](/assets/whereis usage.png)

![](/assets/whereis用例.png)

### 3、locate查找数据库中的文件

locate从已建立的数据库 /var/lib/mlocate/文件中查找文件，但数据库并不是实时更新，若有新文件刚刚被创建，因此需要_sudo updatedb_更新数据库。

![](/assets/locate usage.png)

### 4、find查找硬盘文件

find usage：find \[PATH\] \[option\] \[action\]，下面为option详情。

##### （1）、与时间有关的选项

![](/assets/与时间相关的选项.png)

![](/assets/find相关的时间参数意义.png)

_find /var -mtime -4_

_find /var -mtime +4_

_find /var -mtime 4_

_find /etc -newer /etc/passwd_

