---
title: linux中文本处理工具
date: 2018-05-22 16:08:45
tags:
---
linux中有一些非常好用的文本处理工具

|工具名           | 用途           |
| :------------- | :------------- |
| cat            | 把文本中所有内容显示到屏幕中（从首行到尾行）   |
| more           | 显示文本内容，只能向后翻页|
| less           |对文件或其它输出进行分页显示的工具     |
| Header         |查看前n行（不加n默认是10行）    |
| cut            | 文本切割 |
| tail           | 显示文件结尾内容      |
| sort           | 文本排序（只是影响文本显示排序，并不改变文本原来的排序）（按ascll表排序） |
| uniq           |略过相邻重复的行 |
| wc             | 显示文件数目      |
| grep           |文本过滤器     |
| sed            | 流编辑器 |
| awk            | 报告生成器，格式化以后，显示      |

本章重点介绍grep，sed和awk这三大文本处理利器，其他的工具会做说明但不做详细介绍
<!---more--->

在正式介绍这三大文本处理器之前，先简单介绍一下正则表达式
#### 正则表达

#### grep 文本过滤器
- grep 语法

```
grep [option] pattern file_name/input_string
option：
  -i 忽略匹配字符的大小写
  --color 将匹配的字符以高亮颜色标记出来
  -v 显示没有被匹配的行
  -o 只显示被模式匹配到的字符串
  -E 使用扩展正则表达式
  -A 当某一行被grep匹配到，它下一行也会显示  grep -A 2 ‘cord id’ /proc/cpuinfo
  -B  前面行也会显示
  -C  前后行也会显示
pattern:

```

#### sed 流编辑器
- sed 语法

```
sed [option] 'AdressCommond' file_name
option:
  -n 静默模式，不显示默认空间的行
  -i 直接修改原文件
  -e SCRIPF  -e SCRIPT 可以
Adress:
  1.StartLine,Endline  比如：1,100  $:最后一行
  2./RegExp/ 例如：/^root/
  3./pattern1/,/pattern2/  第一次被pattern1匹配的行开始，至第二次被pattern2匹配到的行.结束
  4.LineNumber指定的行
  5.StartLine，+N 从startLine开始，向后的N行
Commond:
d：删除符合条件的行；
   p：显示符合条件的行
   a\string ：在指定的行后面追加新行，内容为string
        \n 可以用于换行
   i\string 在指定的行前面
   r FILE 将指定的文件的内容添加至符合条件的行处
   w FILE 将地址指定范围内的行另存至指定的文件中
   s/pattern/string/  修饰符：查找并替换，默认只替换每行中第一次被模式匹配到的字符串
   加修饰符
      g: 全局替换
      i: 忽略字符大小写
```
#### awk 报告生成器，格式化以后，显示
- awk语法

```
awk [option] 'pattern{action}' file1,file2....
option:

pattern
  1、Regexp: 正则表达式，格式为/regular expression/
  2、expresssion： 表达式，其值非0或为非空字符时满足条件，如：$1 ~ /foo/ 或 $1 == "magedu"，用运算符~(匹配)和!~(不匹配)。
  3、Ranges： 指定的匹配范围，格式为pat1,pat2
  4、BEGIN/END：特殊模式，仅在awk命令执行前运行一次或结束前运行一次
  5、Empty(空模式)：匹配任意输入行；
action:
  1、Expressions:
  2、Control statements
  3、Compound statements
  4、Input statements
  5、Output statements
```
对于action部分，awk是支持各种语句和函数的。

```
一、print
print的使用格式：
    print item1, item2, ...
要点：
1、各项目之间使用逗号隔开，而输出时则以空白字符分隔；
2、输出的item可以为字符串或数值、当前记录的字段(如$1)、变量或awk的表达式；数值会先转换为字符串，而后再输出；
3、print命令后面的item可以省略，此时其功能相当于print $0, 因此，如果想输出空白行，则需要使用print ""；
例子：
# awk 'BEGIN { print "line one\nline two\nline three" }'
awk -F: '{ print $1, $3 }' /etc/passwd
```
```
二、awk变量
2.1 awk内置变量之记录变量：
FS: field separator，读取文件本时，所使用字段分隔符；
RS: Record separator，输入文本信息所使用的换行符；
OFS: Output Filed Separator:
ORS：Output Row Separator：
awk -F:
OFS="#"
FS=":"
2.2 awk内置变量之数据变量：
NR: The number of input records，awk命令所处理的记录数；如果有多个文件，这个数目会把处理的多个文件中行统一计数；
NF：Number of Field，当前记录的field个数；
FNR: 与NR不同的是，FNR用于记录正处理的行是当前这一文件中被总共处理的行数；
ARGV: 数组，保存命令行本身这个字符串，如awk '{print $0}' a.txt b.txt这个命令中，ARGV[0]保存awk，ARGV[1]保存a.txt；
ARGC: awk命令的参数的个数；
FILENAME: awk命令所处理的文件的名称；
ENVIRON：当前shell环境变量及其值的关联数组；
如：awk 'BEGIN{print ENVIRON["PATH"]}'
2.3 用户自定义变量
gawk允许用户自定义自己的变量以便在程序代码中使用，变量名命名规则与大多数编程语言相同，只能使用字母、数字和下划线，且不能以数字开头。gawk变量名称区分字符大小写。
2.3.1 在脚本中赋值变量
在gawk中给变量赋值使用赋值语句进行，例如：
awk 'BEGIN{var="variable testing";print var}'
2.3.2 在命令行中使用赋值变量
gawk命令也可以在“脚本”外为变量赋值，并在脚本中进行引用。例如，上述的例子还可以改写为：
awk -v var="variable testing" 'BEGIN{print var}'
```

```
三、printf
printf命令的使用格式：
printf format, item1, item2, ...
要点：
1、其与print命令的最大不同是，printf需要指定format；
2、format用于指定后面的每个item的输出格式；
3、printf语句不会自动打印换行符；\n
format格式的指示符都以%开头，后跟一个字符；如下：
%c: 显示字符的ASCII码；
%d, %i：十进制整数；
%e, %E：科学计数法显示数值；
%f: 显示浮点数；
%g, %G: 以科学计数法的格式或浮点数的格式显示数值；
%s: 显示字符串；
%u: 无符号整数；
%%: 显示%自身；
修饰符：
N: 显示宽度；
-: 左对齐；
+：显示数值符号；
例子：
# awk -F: '{printf "%-15s %i\n",$1,$3}' /etc/passwd
```

```
四、输出重定向
print items > output-file
print items >> output-file
print items | command
特殊文件描述符：
/dev/stdin：标准输入
/dev/sdtout: 标准输出
/dev/stderr: 错误输出
/dev/fd/N: 某特定文件描述符，如/dev/stdin就相当于/dev/fd/0；
例子：
# awk -F: '{printf "%-15s %i\n",$1,$3 > "/dev/stderr" }' /etc/passwd
```

```
五、awk的操作符：
5.1 算术操作符：
-x: 负值
+x: 转换为数值；
x^y:
x**y: 次方
x*y: 乘法
x/y：除法
x+y:
x-y:
x%y:
5.2 字符串操作符：
只有一个，而且不用写出来，用于实现字符串连接；
5.3 赋值操作符：
=
+=
-=
*=
/=
%=
^=
**=
++
--
需要注意的是，如果某模式为=号，此时使用/=/可能会有语法错误，应以/[=]/替代；
5.4 布尔值
awk中，任何非0值或非空字符串都为真，反之就为假；
5.5 比较操作符：
x < y    True if x is less than y.
x <= y    True if x is less than or equal to y.
x > y    True if x is greater than y.
x >= y    True if x is greater than or equal to y.
x == y    True if x is equal to y.
x != y    True if x is not equal to y.
x ~ y    True if the string x matches the regexp denoted by y.
x !~ y    True if the string x does not match the regexp denoted by y.
subscript in array      True if the array array has an element with the subscript subscript.
5.6 表达式间的逻辑关系符：
&&
||
5.7 条件表达式：
selector?if-true-exp:if-false-exp
if selector; then
  if-true-exp
else
  if-false-exp
fi
a=3
b=4
a>b?a is max:b ia max
5.8 函数调用：
function_name (para1,para2)
```

```
六 awk的模式：
awk 'program' input-file1 input-file2 ...
其中的program为:
pattern { action }
pattern { action }
...
6.1 常见的模式类型：
1、Regexp: 正则表达式，格式为/regular expression/
2、expresssion： 表达式，其值非0或为非空字符时满足条件，如：$1 ~ /foo/ 或 $1 == "magedu"，用运算符~(匹配)和!~(不匹配)。
3、Ranges： 指定的匹配范围，格式为pat1,pat2
4、BEGIN/END：特殊模式，仅在awk命令执行前运行一次或结束前运行一次
5、Empty(空模式)：匹配任意输入行；
7.2 常见的Action
1、Expressions:
2、Control statements
3、Compound statements
4、Input statements
5、Output statements
/正则表达式/：使用通配符的扩展集。
关系表达式：可以用下面运算符表中的关系运算符进行操作，可以是字符串或数字的比较，如$2>%1选择第二个字段比第一个字段长的行。
模式匹配表达式：
模式，模式：指定一个行的范围。该语法不能包括BEGIN和END模式。
BEGIN：让用户指定在第一条输入记录被处理之前所发生的动作，通常可在这里设置全局变量。
END：让用户在最后一条输入记录被读取之后发生的动作。
```
```
七 控制语句：
7.1 if-else
语法：if (condition) {then-body} else {[ else-body ]}
例子：
awk -F: '{if ($1=="root") print $1, "Admin"; else print $1, "Common User"}' /etc/passwd
awk -F: '{if ($1=="root") printf "%-15s: %s\n", $1,"Admin"; else printf "%-15s: %s\n", $1, "Common User"}' /etc/passwd
awk -F: -v sum=0 '{if ($3>=500) sum++}END{print sum}' /etc/passwd
7.2 while
语法： while (condition){statement1; statment2; ...}
awk -F: '{i=1;while (i<=3) {print $i;i++}}' /etc/passwd
awk -F: '{i=1;while (i<=NF) { if (length($i)>=4) {print $i}; i++ }}' /etc/passwd
7.3 do-while
语法： do {statement1, statement2, ...} while (condition)
awk -F: '{i=1;do {print $i;i++}while(i<=3)}' /etc/passwd
7.4 for
语法： for ( variable assignment; condition; iteration process) { statement1, statement2, ...}
awk -F: '{for(i=1;i<=3;i++) print $i}' /etc/passwd
awk -F: '{for(i=1;i<=NF;i++) { if (length($i)>=4) {print $i}}}' /etc/passwd
for循环还可以用来遍历数组元素：
语法： for (i in array) {statement1, statement2, ...}
awk -F: '$NF!~/^$/{BASH[$NF]++}END{for(A in BASH){printf "%15s:%i\n",A,BASH[A]}}' /etc/passwd
7.5 case
语法：switch (expression) { case VALUE or /REGEXP/: statement1, statement2,... default: statement1, ...}
7.6 break 和 continue
常用于循环或case语句中
7.7 next
提前结束对本行文本的处理，并接着处理下一行；例如，下面的命令将显示其ID号为奇数的用户：
# awk -F: '{if($3%2==0) next;print $1,$3}' /etc/passwd
```
```
八 awk中使用数组
8.1 数组
array[index-expression]
index-expression可以使用任意字符串；需要注意的是，如果某数据组元素事先不存在，那么在引用其时，awk会自动创建此元素并初始化为空串；因此，要判断某数据组中是否存在某元素，需要使用index in array的方式。
要遍历数组中的每一个元素，需要使用如下的特殊结构：
for (var in array) { statement1, ... }
其中，var用于引用数组下标，而不是元素值；
例子：
netstat -ant | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
每出现一被/^tcp/模式匹配到的行，数组S[$NF]就加1，NF为当前匹配到的行的最后一个字段，此处用其值做为数组S的元素索引；
awk '{counts[$1]++}; END {for(url in counts) print counts[url], url}' /var/log/httpd/access_log
用法与上一个例子相同，用于统计某日志文件中IP地的访问量
8.2 删除数组变量
从关系数组中删除数组索引需要使用delete命令。使用格式为：
delete  array[index]
```
```
九、awk的内置函数
split(string, array [, fieldsep [, seps ] ])
功能：将string表示的字符串以fieldsep为分隔符进行分隔，并将分隔后的结果保存至array为名的数组中；数组下标为从0开始的序列；
netstat -ant | awk '/:80\>/{split($5,clients,":");IP[clients[1]]++}END{for(i in IP){print IP[i],i}}' | sort -rn | head -50
length([string])
功能：返回string字符串中字符的个数；
substr(string, start [, length])
功能：取string字符串中的子串，从start开始，取length个；start从1开始计数；
system(command)
功能：执行系统command并将结果返回至awk命令
```
