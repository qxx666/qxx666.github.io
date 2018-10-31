## python 数据类型
1、number类型
2、序列类型
3、字符串类型
4、元组类型
5、集合类型
6、字典类型
<!---more--->
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
|+|相加
|-| 减
|*|乘
|/|整除
|%|取余

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
|	expandtabs(tabsize=8)|把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8 。
|	splitlines([keepends])|按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。
|	startswith(str, beg=0,end=len(string))|检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查|
|translate(table, deletechars="")|根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 deletechars 参数中


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

###### 辅助格式化符号
|辅助格式化符号|解释|
|----|-----|
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
可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典

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

|操作符|解释|
|----|---|
|in|判断字符是否在集合中|
|not in|判断字符是否不在集合中|
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
