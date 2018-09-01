---
title: linux的一些杂记
date: 2018-09-01 15:17:40
tags:
---
## shell特殊变量
bash变量类型：
	1. 环境变量
	2. 本地变量(局部变量)
	3. 位置变量
	4. 特殊变量
###### 环境变量

```
export VARNAME=VALUE
VARNAME=VALUE
export VARNAME  “导出”
```

###### 本地变量：

```
set VARNAME=VALUE: 作用域为整个bash进程；
```

###### 局部变量

```
local VARNAME=VALUE：作用域为当前代码段；
```
###### 位置变量

```
$1, $2, ...
shift 轮替
```

###### 特殊变量
```
$：表示当前shell的进程ID即PID
$0:当前脚本的文件名
$n：传递给脚本或函数，n是一个数字，表示第几个参数
$#：传递给脚本或函数的参数个数
$*：传递给脚本或函数的所有参数
$@：传递给脚本或函数的所有参数，被引号包含时与$*不同
$?:上一个命令的退出状态或函数的返回值
$$：当前shell进程ID，对于shell脚本就是这些脚本所在的进程ID

```

###### 撤消变量：

```
unset VARNAME
```
###### 查看当shell中变量：

```
set
```

###### 查看当前shell中的环境变量：

```
printenv
env
export
```


## bash中如何实现条件判断

条件测试类型：
1、整数测试
2、字符测试
3、文件测试

条件测试的表达式：
[ expression ]
[[ expression ]]
test expression


###### 整数比较

```
    -eq: 测试两个整数是否相等；比如 $A -eq $B
    -ne: 测试两个整数是否不等；不等，为真；相等，为假；
    -gt: 测试一个数是否大于另一个数；大于，为真；否则，为假；
    -lt: 测试一个数是否小于另一个数；小于，为真；否则，为假；
    -ge: 大于或等于
    -le：小于或等于
```
例如：

```
INT1=63
INT2=77
[ $INT1 -eq $INI2 ]
[[ $INT1 -eq $INT2 ]]
test $INT1 -eq $INT2
```

###### 文件测试

```
-e FILE：测试文件是否存在
-f FILE: 测试文件是否为普通文件
-d FILE: 测试指定路径是否为目录
-r FILE: 测试当前用户对指定文件是否有读取权限；
-w FILE: 测试
-x FILE: 测试
```
例如：

```
[ -e /etc/inittab ]
[ -x /etc/rc.d/rc.sysinit ]   
```

###### 字符测试

```
==：测试是否相等，相等为真，不等为假
!=: 测试是否不等，不等为真，等为假
>
<
-n string: 测试指定字符串是否为空，空则真，不空则假
-z string: 测试指定字符串是否不空，不空为真，空则为假
```

###### 组合测试条件

```
  -a ：与关系
  -o ：或关系
   ！：非关系
例如
if [ $# -gt 1 -a $# -le 3 ] 测试大于1小于3的数
等价于 if [ $# -gt 1 ]&&if[ $# -le 3]  
```

###### shell中如何进行算术运算：

```
A=3
B=6
1、let 算术运算表达式
    let C=$A+$B
2、$[算术运算表达式]
    C=$[$A+$B]
3、$((算术运算表达式))
    C=$(($A+$B))
4、expr 算术运算表达式，表达式中各操作数及运算符之间要有空格，而且要使用命令引用
    C=`expr $A + $B`
```
###### 定义脚本退出状态码
exit: 退出脚本
当在脚本运行到某个命令满足退出条件时，使用exit n退出当前脚本。
```
exit #   //如果脚本没有明确定义退出状态码，那么，最后执行的一条命令的退出码即为脚本的退出状态码
exit ：退出码不变，即为最后一个命令的退出码
0表示成功（Zero - Success）

非0表示失败（Non-Zero  - Failure）

2表示用法不当（Incorrect Usage）

127表示命令没有找到（Command Not Found）

126表示不是可执行的（Not an executable）

>=128 信号产生
```

###### 测试脚本是否有语法错误：

```
bash -n 测试脚本是否有语法错误
```

## 特殊权限SUID等详解

```
passwd:s

SUID: 运行某程序时，相应进程的属主是程序文件自身的属主，而不是启动者；
    chmod u+s FILE  给文件加s
    chmod u-s FILE  给文件减s
        如果FILE本身原来就有执行权限，则SUID显示为s；否则显示S；
SGID: 运行某程序时，相应进程的属组是程序文件自身的属组，而不是启动者所属的基本组；
    chmod g+s FILE
    chmod g-s FILE
        develop team, hadoop, hbase, hive 开发团队下三个用户
        /tmp/project/  
            develop
Sticky: 在一个公共目录，每个都可以创建文件，删除自己的文件，但不能删除别人的文件；
    chmod o+t DIR
    chmod o-t DIR

SUID:SGID:Sticky
000-111

chmod 5755 /backup/test
umask 0022
```
