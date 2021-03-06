## 常用目录

```
/						根目录
/bin				二进制文件（可执行文件）
/sbin				root用户二进制文件
/boot				启动目录
/dev				device,设备目录，外部设备等
/etc				配置文件
/etc/rc.d		启动的配置文件和脚本
/home				~user，用户主目录
/lib				动态链接库文件
/tmp				公共临时文件
/mnt				临时挂载其他文件系统
/proc				虚拟目录，process，内存映射，可以获取系统信息
/var				variable，常态变动目录，存放一些如日志文件等经常读写的文件
/usr				用户目录
/lost+found	非正常关机时暂存文件
```

## shell运算符

算术预算符 和 逻辑运算符 与其他语言基本一致

### 关系运算符

| 运算符 | 作用                                |
| ------ | ----------------------------------- |
| -eq    | equal，判断相等                     |
| -ne    | no-equal，判断不相等                |
| -gt    | greater than，判断是否大于          |
| -lt    | less than，判断是否小于             |
| -ge    | greater and equal，判断是否大于等于 |
| -le    | less and equal，判断是否小于等于    |

### 布尔运算符

| 运算符 | 作用    |
| ------ | ------- |
| !      | 非      |
| -o     | 或，or  |
| -a     | 与，and |

### 字符串运算符

| 运算符 | 作用                                                  |
| ------ | ----------------------------------------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。               |
| !=     | 检测两个字符串是否相等，不相等返回 true。             |
| -z     | zero?检测字符串长度是否为0，为0返回 true。            |
| -n     | non-zero?检测字符串长度是否不为 0，不为 0 返回 true。 |
| $      | 检测字符串是否为空，不为空返回 true。                 |

### 文件测试运算符

用于检测文件属性

| 运算符  | 作用                                                         |
| ------- | ------------------------------------------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            |
| -r file | 检测文件是否可读，如果是，则返回 true。                      |
| -w file | 检测文件是否可写，如果是，则返回 true。                      |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          |

## 获取shell命令中的参数

```shell
$0	#脚步启动名（包含路径）
$n	#第n个参数
$#	#传递到脚本的参数个数
$*	#所有参数列表，以单字符串形式呈现
$@	#与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
$$	#脚本当前的进程号
$!	#后台运行的最后一个进程号
$-	#显示脚本使用的当前选项，与set命令功能相同
$?	#显示最后命令的退出状态，0表示无错误，其他任何值表明有错误。
```

## sed

sed是一种流编辑器。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），

接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。

接着处理下一行，这样不断重复，直到文件末尾。

文件内容并没有改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。

用法与vi类似。

### 命令

```
a\ 在当前行下面插入文本。
i\ 在当前行上面插入文本。
c\ 把选定的行改为新的文本。
d 删除，删除选择的行。
D 删除模板块的第一行。
s 替换指定字符
h 拷贝模板块的内容到内存中的缓冲区。
H 追加模板块的内容到内存中的缓冲区。
g 获得内存缓冲区的内容，并替代当前模板块中的文本。
G 获得内存缓冲区的内容，并追加到当前模板块文本的后面。
l 列表不能打印字符的清单。
n 读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。
N 追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码。
p 打印模板块的行。
P(大写) 打印模板块的第一行。
q 退出Sed。
b lable 分支到脚本中带有标记的地方，如果分支不存在则分支到脚本的末尾。
r file 从file中读行。
t label if分支，从最后一行开始，条件一旦满足或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。
T label 错误分支，从最后一行开始，一旦发生错误或者T，t命令，将导致分支到带有标号的命令处，或者到脚本的末尾。
w file 写并追加模板块到file末尾。  
W file 写并追加模板块的第一行到file末尾。  
! 表示后面的命令对所有没有被选定的行发生作用。  
= 打印当前行号码。  
# 把注释扩展到下一个换行符以前。  
```

### 替换标记

```
g 表示行内全面替换。  
p 表示打印行。  
w 表示把行写入一个文件。  
x 表示互换模板块中的文本和缓冲区中的文本。  
y 表示把一个字符翻译为另外的字符（但是不用于正则表达式）
\1 子串匹配标记
& 已匹配字符串标记
```

## 链接

### 硬链接

硬链接是通过索引节点进行的链接。

硬链接只能在同一文件系统中的文件之间进行链接，不能对目录进行创建。

如果删除硬链接对应的源文件，则硬链接文件仍然存在，而且保存了原有的内容，这样可以起到防止因为误操作而错误删除文件的作用。

创建

```shell
link oldfile newfile 
ln oldfile newfile
```

### 软链接

软链接又叫符号链接，文件用户数据块中存放的内容是另一文件的路径名的指向。

软链接就是一个普通文件，只是数据块内容有点特殊。软链接可对文件或目录创建。

软链接主要应用于以下两个方面：一是方便管理，例如可以把一个复杂路径下的文件链接到一个简单路径下方便用户访问；

另一方面就是解决文件系统磁盘空间不足的情况。

例如某个文件文件系统空间已经用完了，但是现在必须在该文件系统下创建一个新的目录并存储大量的文件，

那么可以把另一个剩余空间较多的文件系统中的目录链接到该文件系统中，这样就可以很好的解决空间不足问题。

删除软链接并不影响被指向的文件，但若被指向的原文件被删除，则相关软连接就变成了死链接。

创建

```shell
ln -s old.file soft.link
ln -s old.dir soft.link.dir
```



## 文件权限

### 用户id的分类

| 用户ID类别                      | 含义                         |
| ------------------------------- | ---------------------------- |
| 实际用户ID 实际组ID             | 实际的用户，即执行操作的用户 |
| 有效用户ID 有效组ID 附加组ID    | 用于文件存取许可权检查       |
| 保存设置-用户-ID 保存设置-组-ID | 由exec函数保存               |

### setid和setgid

setid

让执行该命令的用户以该命令拥有者的权限去执行

setgid

让执行该命令的用户组以该命令拥有者所在组的权限去执行

在标志为中以s为标志。，出现在x的位置上，例如```-rwsr-xr-x ```

通过`chmod u+s` 和 `chmod g+s` 进行设置。

### 粘滞位

sticky 位的工作方式：它对文件没有影响，但当它在目录上使用时，所述目录中的所有文件只能由其所有者删除或移动。

一个典型的例子是 `/tmp` 目录，通常系统中的所有用户都对这个目录有写权限。设置 sticky 位使用户不能删除其他用户的文件。

sticky 位在可执行位上用 `t` 来标识。同样，小写的 `t` 表示可执行权限 `x`也被设置了，否则你会看到一个大写字母 `T`。

通过`chmod o+t` 进行设置。



## 启动流程

### Linux的启动流程

1. BIOS(basic I/O system)上电自检(POST, power on self test)，成功后产生一个INT 13H中断；
2. 从硬盘0柱面 0磁道 第一扇区读512字节的MBR主引导记录；
3. 运行引导程序Grub2( GRand Unified BootLoader, Verison 2)，并根据其配置加载kernel镜像后初始化；
4. 根据/etc/inittab中系统初始化配置执行/etc/rc.sysinit脚本；
5. 根据第3步读到的runlevel值启动对应服务；
6. 运行/etc/rc.local;
7. 生成终端待用户登录。

### runlevel

- 0 停机，关机
- 1 单用户，无网络连接，不运行守护进程，不允许非超级用户登录
- 2 多用户，无网络连接，不运行守护进程
- 3 多用户，正常启动系统
- 4 用户自定义
- 5 多用户，带图形界面
- 6 重启



## crontab

cron是任务的意思，tab 表示table。crontab 可以理解为，任务时间表。 crontab 命令是用来让计算机替我们执行周期性任务

守护进程是crond

命令格式

```
* * * * *  执行的任务
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * command to be executed
```

符号含义

| 符号 | 含义                   | 举例                                                         |
| ---- | ---------------------- | ------------------------------------------------------------ |
| *    | 表示任意时间的意思。   | 比如 “* * * * * 任务” 就代表每分钟执行一次                   |
| ,    | 代表不连续的时间。     | 比如 “ 0 1,3 * * * 任务” 就代表每天的1点整，3点整分别都执行一次 |
| -    | 代表连续的时间范围。   | 比如 “0 2 * * 1-3 任务” 就代表每周一到周三的凌晨2点0分执行任务 |
| */n  | 代表每隔多久执行一次。 | 比如 “*/30 * * * * 任务” 就代表每三十分钟执行一次任务        |

