
## python 数据类型
1、number类型
2、序列类型
3、字符串类型
4、元组类型
5、集合类型
6、字典类型
<!---more--->

---
title: 浅谈python
date: 2018-08-05 14:48:30
tags:
---
1、python的数据类型

2、python的一些标准模块

3、python的异常处理

4、python的一些说明
## python 数据类型
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3
#### 一、number类型
###### 类型
1、整型（integer）
2、长整型（long integer）
3、布尔类型（booleam）
4、双精度浮点型（double-precision floating）
5、复数（complex number）
###### 操作符
| 操作符    | 说明                |
|:-------------|:---------------|
<<<<<<< HEAD
|+|相加
|-| 减
|*|乘
|/|整除
|%|取余
=======
|+|
|-|
|*|
|/|
|%|
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3

###### 内置函数
1、通用函数（既适合数值类型也适合其他类型）


| 函数    | 说明                |
|:-------------|:-----------------------        |
|cmp(A,B)|比较二者大小，前者大返回1，前者小返回-1，相等返回0|
|str(A)|将参数转化为字符|
|type(A)|判断参数的类型|
|bool(A)|将参数转换为布尔类型|
|int(A)|将参数转换为整数类型，以十进制表示|
|long(A)|将参数转换为长整型，以十进制表示|
|float(A)|将参数转换为浮点类型|
|complex(A)|将参数转换为复数类型|

2、数值类型的特定函数

| 函数    | 说明                |
|:-------------|:-----------------------        |
|abs(A)|取绝对值|
|coerce(A,B)|将A和B转换成一个类型，并生成一个元组|
|divmod(A,B)|除模操作，生成一个元祖，形式为(A/B,A%B)|
|pow(A,B)|幂操作，结果为A的B次方|
|round(A)|返回参数的四舍五入结果|
|hex(A)|将A转换为十六进制的字符串|
|oct(A)|将A转换为八进制的字符串|
|bin(A)|将A转换为二进制的字符串|
|chr(A)|将A转换成ASCII字符，要求0<<A<<255|
|ord(A)|chr(A)的反函数|

#### 二、序列类型
###### 操作符
| 操作符    | 说明                |
|:-------------|:----------------|
|$|

###### 内置函数

| 函数    | 说明                |
|:-------------|:-----------------------        |
|enumerate(A)|对序列A生成一个可枚举的对象，对象中的每个元素是一个二元组，远足内容为（index，item）即（索引号，序列元素）|
|len(A)|求A序列的长度|
|list(A)|转换为List类型|
|max(a,b,c.....)|返回所有参数中最大的元素|
|min(A)|A是一个序列，返回A中最大的元素|
|min(a,b,c,d,....)|返回所有参数中最小的元素|
|reversed(A)|生成A的反序列,使用list(reversed(A))打印|
|sorted(A,func=Noe,key=Noe,reverse=False)|对A排序，排序规则按照参数fun、key、reverse指定的规则进行|
|sum(A,init=0)|对A中的元素求和|
|tuple(A)|转换为tuple类型|

####  三、字符串类型
###### 字符串格式化
###### 内置函数
| 函数    | 说明                |
|:-------------|:-----------------------        |
|capitalize()|将字符串中的第一个字符大写|
|center(width)|返回一个长度为width的字符串，并使原字符串的内容居中|
|count(str,beg=0,end=len(string))|返回str在string里面出现的次数，可以用开始索引(beg)和结束索引（end）指定搜索的范围|
|decode(encoding='UTF-8',errors='strict')|以encoding指定的编码格式解码|
|encode(encoding='UTF-8',errors='strict')|以encoding指定的编码格式编码|
|endswith=(obj,beg=0,end=olen(string))|检查字符串是否以obj结束，如果是，则返回True，否则返回Flase，可以用开始索引(beg)和结束索引（end）指定搜索的范围|
|expandtabs(tabsize=8)|把字符串string中的Tab符号转换为空格，默认的空格数是tabsize|
|find(str,beg=0,end=len(string))|检测str是否包含在string中；可以用开始索引（beg）和结束索引（end）指定搜索的范围。找到返回索引值，找不到则返回-1|
|index(str,beg=0,end=len(string))|如果str在string中，则返回str的索引，如果str不在string中，则报一个异常。|
|isalnum()|如果返现有一个字符并且所有字符都是字母或数字，则返回True，否则返回False|
|isalpha()|如果发现有一个字符并且所有字符都是字母，则返回True，否则返回False|
|isdecimal()|如果可解释为十进制数字，则返回True，否则返回False|
|isdigit()|如果可解释为数字，则返回True，否则返回Flase|
|islower()|如果字符串中的字符都是小写，，则返回True，否则返回Flase|
|isnumeric()|如果只包含数字字符，则返回True，否则返回Flase|
|isspace()|如果字符串是空格，则返回True，否则返回Flase|
|istitle()|如果字符串是标题化的，则返回True，否则返回Flase|
|isupper()|如果字符串中的字符都是大写的，则返回True，否则返回Flase|
|ljust(width)|返回一个原字符串左对齐，并使用空格填充至长度width的新字符串|
|rjust(width)|返回一个原字符串右对齐，并使用空格填充至长度width的新字符串|
|lstrip()|截掉string左边的空格|
|rstrip()|截掉字符串末尾边的空格|
|strip()|在字符串上执行rstrip()和lsrip()|
|lower()|转换所有大写字符为小写|
|upper()|转换所有小写字符为大写|
|replace(str1,str2,num=count(str1))|把string中str1替换成str2，num指定替换的最大次数|
|rfind(str,beg=0,end=len(string))|类似于find(),但是从右边开始查找|
|rindex(str,beg=0,end=len(string))|类似于index(),但是从右边开始查找|
<<<<<<< HEAD
|	expandtabs(tabsize=8)|把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 。
|	splitlines([keepends])|按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。
|	startswith(str, beg=0,end=len(string))|检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查|
|translate(table, deletechars="")|根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 deletechars 参数中
=======
|expandtabs()|
|splitlines()|
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3


- 所有字符串类型的内置函数

```
string.capitalize  string.format      string.isupper     string.rindex      string.strip
string.center      string.index       string.join        string.rjust       string.swapcase
string.count       string.isalnum     string.ljust       string.rpartition  string.title
string.decode      string.isalpha     string.lower       string.rsplit      string.translate
string.encode      string.isdigit     string.lstrip      string.rstrip      string.upper
string.endswith    string.islower     string.partition   string.split       string.zfill
string.expandtabs  string.isspace     string.replace     string.splitlines  
string.find        string.istitle     string.rfind       string.startswith  

```


###### 格式化符号表
|格式化符号|解释|
|----|----|
|%c|转换单个字符|
|%r|转为用repr()函数表达的字符串|
<<<<<<< HEAD
|%s|格式化字符串|
|%d or %i|格式化整数
|%o|格式化无符号八进制
|%x|格式化无符号十六进制数
|%X|格式化无符号十六进制数（大写）
|%e|用科学技术法格式化浮点数
|%E|作用同%e，用科学计数法格式化浮点数
|%f or %F|格式化浮点数字，可指定小数点后的精度
|%g|%f和%e的简写
|%G|%f和%E的简写
|%%|字符“%”
=======
|%s|转为用str()函数表达的字符串|
|%d or %i|
|%o|
|%x|
|%X|
|%e|
|%E|
|%f or %F|
|%g|
|%G|
|%%|
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3

###### 辅助格式化符号
|辅助格式化符号|解释|
|----|-----|
<<<<<<< HEAD
|*|定义宽度或者小数点精度
|-|	用做左对齐
|+|在正数前面显示加号( + )
|< spr >|	在正数前面显示空格
|#|在八进制数前面显示零('0')，在十六进制前面显示'0x'或者'0X'(取决于用的是'x'还是'X')
|0|显示的数字前面填充'0'而不是默认的空格
|m.n|m 是显示的最小总宽度,n 是小数点后的位数(如果可用的话)
|(var)	|映射变量(字典参数)
|%	|'%%'输出一个单一的'%'
###### 转义字符解释
|转义字符|解释|
|-----|----|
|\a|响铃
|\b|退格（backspace）
|\f|换页
|\n|换行
|\r|回车
|\t|横向制表符
|\v|纵向制表符
|\\|反斜杠符号
|\’|单引号
|\"|双引号
|\?|问号
|\000|空
=======
|*|
|-|
|+|
|< spr >|
|#|
|0|
|m.n|
###### 转义字符解释
|转义字符|解释|
|-----|----|
|\a|
|\b|
|\f|
|\n|
|\r|
|\t|
|\v|
|\\|
|\’|
|\"|
|\?|
|\0|
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3
#### 四、Tuple类型

元组类型是一个特殊的序列类型，用圆括号表示，一旦定义就不可修改，操作比列表类型快，如果有一个常量集只用来读，则可以元组类型定义

#### 五、列表类型
列表用[]定义，可以增删改查
###### 内置函数
l
```
list=['ad','asd','ew']
list.append()   list.extend()   list.insert()   list.remove()   list.sort()     
list.count()    list.index()    list.pop()      list.reverse()  

```

#### 六、集合类型（set 类型）
set类型即集合，用以表示相互之间无序的一组对象，集合在算术上的运算包括并集，交集，补集等，python中的集合分为两种类型：普通集合和不可变集合。普通集合在初始化后支持并集，交集，补集等运算，而不可变集合初始化后就不能变化了
###### 定义
python中用set和frozenset关键字定义普通集合和不可变集合，初始化集合内容的方法是向其传入序列类型簇的变量
<<<<<<< HEAD
可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典
=======
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3

```
In [24]: list=['feaf']
In [25]: sample=set(list)
In [26]: sample
Out[26]: {'feaf'}
In [27]: print(sample)
set(['feaf'])
```
###### 内置函数
```
set.add                          set.intersection                 set.remove
set.clear                        set.intersection_update          set.symmetric_difference
set.copy                         set.isdisjoint                   set.symmetric_difference_update
set.difference                   set.issubset                     set.union
set.difference_update            set.issuperset                   set.update
set.discard                      set.pop                          
```
###### 集合操作符
<<<<<<< HEAD

|操作符|解释|
|----|---|
|in|判断字符是否在集合中|
|not in|判断字符是否不在集合中|
=======
|操作符|解释|
|----|---|
|in|
|not in|
|!=|
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3
|<|
|<=|
|>|
| \>=|
|&|
|\||
|-|
|^|
|\|=|
|&=|
|_=|
|^=|

#### 七、字典类型
字典类型即字典，代表一个键值存储库，工作方式想映射表，是python中唯一表示映射关系的类，所以其有自己都有的定义和操作方式

###### 定义

```
dict={'name':'bob','age':25,'sex':'man'}

```

###### 内置函数

```
dict.clear       dict.get         dict.iteritems   dict.keys        dict.setdefault  dict.viewitems
dict.copy        dict.has_key     dict.iterkeys    dict.pop         dict.update      dict.viewkeys
dict.fromkeys    dict.items       dict.itervalues  dict.popitem     dict.values      dict.viewvalues

```
<<<<<<< HEAD
=======

## python的标准模块
```buildoutcfg
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
#### os模块
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

#### re模块
正则表达式是一个特殊的字符序列，它能帮助你方便的检查一个字符串是否与某种模式匹配。

Python 自1.5版本起增加了re 模块，它提供 Perl 风格的正则表达式模式。

re 模块使 Python 语言拥有全部的正则表达式功能。

compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换。

re 模块也提供了与这些方法功能完全一致的函数，这些函数使用一个模式字符串做为它们的第一个参数。
###### re中的函数

```
In [8]: re.
re.DEBUG        re.L            re.S            re.U            re.compile      re.findall      re.search       re.sub          
re.DOTALL       re.LOCALE       re.Scanner      re.UNICODE      re.copy_reg     re.finditer     re.split        re.subn         
re.I            re.M            re.T            re.VERBOSE      re.error        re.match        re.sre_compile  re.sys          
re.IGNORECASE   re.MULTILINE    re.TEMPLATE     re.X            re.escape       re.purge        re.sre_parse    re.template     
```
###### 重点介绍几个函数
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
###### 正则表达式对象
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
## python异常处理 ##
python异常处理使用try，catche，else和finally关键字来尝试可能未成功的操作，处理失败及正常情况以及在事后清理资源
```
try：
	block_try
except Exception 1：  、、第一种except形式
	block_when_exceptin_1_happen:
except (Exception_1 Exception_2 Exception_3): //第二种except形式
	block_when_exception_1_or_2_or_3_happen
except Exception_5 variance:
	block_when_exception_5_happen
except (Exception_6 Exception_7),variance:
	block_when_exception_6_or_7_happen
except :
	block_for_all_other_exceptions
else:
	block_for_no_exception
finally:
	block_anyway
```
##### 自定义异常
自定义异常的边长方法是建立一个继承子系统异常的子类，并且需要引发该异常时用raise语句跑出异常


## python的一些说明

#### 构造函数
构造函数是一种特殊的类成员方法，主要用在创建对象时初始化对象
python中的构造函数用__init__命名
#### 析构函数
析构函数是构造函数的反函数，在销毁对象时调用他们，往往用来做清理善后，
例如：数据库连接对象可以在析构函数中释放对数据库实例资源的占用
python中为类定义析构函数的方法是在类中定义一个名为__del__的没有返回值和参数的函数
python中显示销毁对象的方法，使用del关键

#### 静态函数和类函数
python中支持两种基于类名访问成员函数：静态函数和类函数
不同点：类函数有一个隐形参数cls可以用来获取类信息
静态函数使用装饰器@staicmethod定义
类函数使用装饰器@classmethod定义

#### 生成器与yield

#### 闭包
#### 装饰器
装饰器是将其他函数增强
1、装饰器本身是一个函数，目的用于装饰其他函数
2、增强被装饰函数的功能
3、装饰器一般接受一个函数对象作为参数以对其进行功能的增强
4、装饰器用@deco定义装饰
>>>>>>> 47d34524dc4c2a4271fee313eac74249199d59b3
