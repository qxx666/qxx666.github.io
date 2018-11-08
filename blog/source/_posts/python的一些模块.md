---
title: python的一些模块
date: 2018-11-08 16:00:29
tags:
---
## python的标准模块
```
1、os
2、sys
3、glob模块提供了一个函数用于从目录通配符搜索中生成文件列表
4、re
5、math
6、urllib
7、datetime
8、zlib
9、doctest
10、unittest
11、time
12、timeit
```
<!--more-->
## 一、os模块
os 模块提供了非常丰富的方法用来处理文件和目录。

常用的方法如下表所示：

1、	
os.access(path, mode)
检验权限模式

2、	
os.chdir(path)
改变当前工作目录

3、	
os.chflags(path, flags)
设置路径的标记为数字标记。

4、	
os.chmod(path, mode)
更改权限

5、	
os.chown(path, uid, gid)
更改文件所有者

6、	
os.chroot(path)
改变当前进程的根目录

7、	
os.close(fd)
关闭文件描述符 fd

8、	
os.closerange(fd_low, fd_high)
关闭所有文件描述符，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略

9、	
os.dup(fd)
复制文件描述符 fd

10、	
os.dup2(fd, fd2)
将一个文件描述符 fd 复制到另一个 fd2

11、	
os.fchdir(fd)
通过文件描述符改变当前工作目录

12、	
os.fchmod(fd, mode)
改变一个文件的访问权限，该文件由参数fd指定，参数mode是Unix下的文件访问权限。

13、	
os.fchown(fd, uid, gid)
修改一个文件的所有权，这个函数修改一个文件的用户ID和用户组ID，该文件由文件描述符fd指定。

14、	
os.fdatasync(fd)
强制将文件写入磁盘，该文件由文件描述符fd指定，但是不强制更新文件的状态信息

15、
os.fdopen(fd[, mode[, bufsize]])
通过文件描述符 fd 创建一个文件对象，并返回这个文件对象

16、	
os.fpathconf(fd, name)
返回一个打开的文件的系统配置信息。name为检索的系统配置的值，它也许是一个定义系统值的字符串，这些名字在很多标准中指定（POSIX.1, Unix 95, Unix 98, 和其它）。

17、	
os.fstat(fd)
返回文件描述符fd的状态，像stat()。

18、	
os.fstatvfs(fd)
返回包含文件描述符fd的文件的文件系统的信息，像 statvfs()

19、	
os.fsync(fd)
强制将文件描述符为fd的文件写入硬盘。

20、	
os.ftruncate(fd, length)
裁剪文件描述符fd对应的文件, 所以它最大不能超过文件大小。

21、	
os.getcwd()
返回当前工作目录

22、	
os.getcwdu()
返回一个当前工作目录的Unicode对象

23、	
os.isatty(fd)
如果文件描述符fd是打开的，同时与tty(-like)设备相连，则返回true, 否则False。

24、	
os.lchflags(path, flags)
设置路径的标记为数字标记，类似 chflags()，但是没有软链接

25、	
os.lchmod(path, mode)
修改连接文件权限

26、	
os.lchown(path, uid, gid)
更改文件所有者，类似 chown，但是不追踪链接。

27、	
os.link(src, dst)
创建硬链接，名为参数 dst，指向参数 src

28、	
os.listdir(path)
返回path指定的文件夹包含的文件或文件夹的名字的列表。

29、	
os.lseek(fd, pos, how)
设置文件描述符 fd当前位置为pos, how方式修改: SEEK_SET 或者 0 设置从文件开始的计算的pos; SEEK_CUR或者 1 则从当前位置计算; os.SEEK_END或者2则从文件尾部开始. 在unix，Windows中有效

30、	
os.lstat(path)
像stat(),但是没有软链接

31、	
os.major(device)
从原始的设备号中提取设备major号码 (使用stat中的st_dev或者st_rdev field)。

32、	
os.makedev(major, minor)
以major和minor设备号组成一个原始设备号

33、	
os.makedirs(path[, mode])
递归文件夹创建函数。像mkdir(), 但创建的所有intermediate-level文件夹需要包含子文件夹。

34、	
os.minor(device)
从原始的设备号中提取设备minor号码 (使用stat中的st_dev或者st_rdev field )。

35、	
os.mkdir(path[, mode])
以数字mode的mode创建一个名为path的文件夹.默认的 mode 是 0777 (八进制)。

36、	
os.mkfifo(path[, mode])
创建命名管道，mode 为数字，默认为 0666 (八进制)

37、	
os.mknod(filename[, mode=0600, device])
创建一个名为filename文件系统节点（文件，设备特别文件或者命名pipe）。

38、	
os.open(file, flags[, mode])
打开一个文件，并且设置需要的打开选项，mode参数是可选的

39、	
os.openpty()
打开一个新的伪终端对。返回 pty 和 tty的文件描述符。

40、	
os.pathconf(path, name)
返回相关文件的系统配置信息。

41、	
os.pipe()
创建一个管道. 返回一对文件描述符(r, w) 分别为读和写

42、	
os.popen(command[, mode[, bufsize]])
从一个 command 打开一个管道

43、	
os.read(fd, n)
从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。

44、	
os.readlink(path)
返回软链接所指向的文件

45、	
os.remove(path)
删除路径为path的文件。如果path 是一个文件夹，将抛出OSError; 查看下面的rmdir()删除一个 directory。

46、	
os.removedirs(path)
递归删除目录。

47、	
os.rename(src, dst)
重命名文件或目录，从 src 到 dst

48、	
os.renames(old, new)
递归地对目录进行更名，也可以对文件进行更名。

49、	
os.rmdir(path)
删除path指定的空目录，如果目录非空，则抛出一个OSError异常。

50、
os.stat(path)
获取path指定的路径的信息，功能等同于C API中的stat()系统调用。

51、	
os.stat_float_times([newvalue])
决定stat_result是否以float对象显示时间戳

52、	
os.statvfs(path)
获取指定路径的文件系统统计信息

53、	
os.symlink(src, dst)
创建一个软链接

54、	
os.tcgetpgrp(fd)
返回与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组

55、	
os.tcsetpgrp(fd, pg)
设置与终端fd（一个由os.open()返回的打开的文件描述符）关联的进程组为pg。

56、	
os.tempnam([dir[, prefix]])
返回唯一的路径名用于创建临时文件。

57、	
os.tmpfile()
返回一个打开的模式为(w+b)的文件对象 .这文件对象没有文件夹入口，没有文件描述符，将会自动删除。

58、	
os.tmpnam()
为创建一个临时文件返回一个唯一的路径

59、	
os.ttyname(fd)
返回一个字符串，它表示与文件描述符fd 关联的终端设备。如果fd 没有与终端设备关联，则引发一个异常。

60、
os.unlink(path)
删除文件路径

61、
os.utime(path, times)
返回指定的path文件的访问和修改的时间。

62、	
os.walk(top[, topdown=True[, onerror=None[, followlinks=False]]])
输出在文件夹中的文件名通过在树中游走，向上或者向下。

63、	
os.write(fd, str)
写入字符串到文件描述符 fd中. 返回实际写入的字符串长度

## 二、re模块
正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。

re 模块使 Python 语言拥有全部的正则表达式功能。

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。
#### re中的函数

```
In [8]: re.
re.DEBUG        re.L            re.S            re.U            re.compile      re.findall      re.search       re.sub          
re.DOTALL       re.LOCALE       re.Scanner      re.UNICODE      re.copy_reg     re.finditer     re.split        re.subn         
re.I            re.M            re.T            re.VERBOSE      re.error        re.match        re.sre_compile  re.sys          
re.IGNORECASE   re.MULTILINE    re.TEMPLATE     re.X            re.escape       re.purge        re.sre_parse    re.template     
```
#### 重点介绍几个函数
1、re.match函数

re.match 尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

```
re.match(pattern, string, flags=0)
```
函数参数说明：

参数	描述

pattern	匹配的正则表达式

string	要匹配的字符串。

flags	标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。

参见：正则表达式修饰符 - 可选标志

2、re.search方法

re.search 扫描整个字符串并返回第一个成功的匹配。

```
re.search(pattern, string, flags=0)
```
re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

3、re.sub方法

re.sub用于替换字符串中的匹配项。

```
re.sub(pattern, repl, string, count=0, flags=0)
```
参数：

pattern : 正则中的模式字符串。

repl : 替换的字符串，也可为一个函数。

string : 要被查找替换的原始字符串。

count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。

4、re.compile 函数

compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。

```
re.compile(pattern[, flags])
```
参数：

pattern : 一个字符串形式的正则表达式

flags : 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：

```
re.I 忽略大小写
re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
re.M 多行模式
re.S 即为 . 并且包括换行符在内的任意字符（. 不包括换行符）
re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
re.X 为了增加可读性，忽略空格和 # 后面的注释
```
5、re.findall方法

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
注意： match 和 search 是匹配一次 findall 匹配所有。

```
re.findall(pattern，string, flags)
```

参数	
pattern	匹配的正则表达式

string	要匹配的字符串。

flags	标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志

或者：

```
pattern = re.compile() 
pattern.findall(string, pos, endpos)
```
参数:
string : 待匹配的字符串。

pos : 可选参数，指定字符串的起始位置，默认为 0。

endpos : 可选参数，指定字符串的结束位置，默认为字符串的长度。

6、re.finditer

和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回。

```
re.finditer(pattern, string, flags=0)
```
参数：
pattern	匹配的正则表达式

string	要匹配的字符串。

flags	标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志

7、re.split方法

split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：

```
re.split(pattern, string[, maxsplit=0, flags=0])
```

参数	描述

pattern	匹配的正则表达式

string	要匹配的字符串。

maxsplit	分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数。

flags	标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：正则表达式修饰符 - 可选标志
#### 正则表达式对象
1、re.RegexObject

re.compile() 返回 RegexObject 对象。

2、re.MatchObject

group() 返回被 RE 匹配的字符串。

```
start() 返回匹配开始的位置
end() 返回匹配结束的位置
span() 返回一个元组包含匹配 (开始,结束) 的位置
```

实例：

```
>>>import re
>>> pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)   # re.I 表示忽略大小写
>>> m = pattern.match('Hello World Wide Web')
>>> print m                               # 匹配成功，返回一个 Match 对象
<_sre.SRE_Match object at 0x10bea83e8>
>>> m.group(0)                            # 返回匹配成功的整个子串
'Hello World'
>>> m.span(0)                             # 返回匹配成功的整个子串的索引
(0, 11)
>>> m.group(1)                            # 返回第一个分组匹配成功的子串
'Hello'
>>> m.span(1)                             # 返回第一个分组匹配成功的子串的索引
(0, 5)
>>> m.group(2)                            # 返回第二个分组匹配成功的子串
'World'
>>> m.span(2)                             # 返回第二个分组匹配成功的子串
(6, 11)
>>> m.groups()                            # 等价于 (m.group(1), m.group(2), ...)
('Hello', 'World')
>>> m.group(3)                            # 不存在第三个分组
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: no such group
```
## 三、sys模块
