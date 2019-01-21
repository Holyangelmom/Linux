# 文件查找

### 1、which/type查找执行档

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

_find /var -mtime -4_

_find /var -mtime +4_

_find /var -mtime 4_

_find /etc -newer /etc/passwd_

##### （2）、与使用者或组别相关的选项

![](/assets/与使用者或组名相关的选项.png)

_find /home -user dmtsai_

_find / -nouser_

##### （3）、与文件名及权限相关的选项

![](/assets/与文件名及权限相关的选项.png)

_find /home -name test_

_find /home -name "\*passwd\*"_

_find / -size +100M_

_find /var -type s_

_find /usr/bin /usr/sbin -perm /6000_

_find /etc -size +50k -a -size -60k -exec ls -l {} \;   -a为and的意思_

_find /etc -size +50k -a ! -user root -exec ls -ld {} \;    ! 为非的意思_

_find /etc -size +1500k -o -size 0                             -o为或的意思_

_find /usr/bin /usr/sbin -perm /7000 -exec ls -l {} \;_

find 的特殊功能就是能够进行额外的动作\(action\)。我们将最后一个例子以图解来说明如下：

![](/assets/find额外的动作.png)

* {}代表的是『由 find 找到的内容』，如上图所示，find 的结果会被放置到 {} 位置中；
* -exec 一直到 \; 是关键词，代表 find 额外动作的开始 \(-exec\) 到结束 \(\;\) ，在这中间的就是 find 指令内的额外动作。 在本例中就是『 ls -l {} 』啰！
* 『 ; 』在 bash 环境下是有特殊意义的，因此利用反斜杠来跳脱。



