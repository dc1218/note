# lua 摘录

## - 注释类型：  
--单行注释  
--[[  
多行  
注释  
]]  
## - 标识符：  
与c类似，最好不要使用下划线和大写字母，因为Lua保留字也是这样的

## - 关键字：

lua|关|键|字|集
:-:|:-:|:-:|:-:|:-:
and|break|do|else|elseif
end|false|for|function|if
in|local|nil|not|or
repeat|return|then|true|until
while| | | |

- 变量默认是全局变量，如果想删除一个变量，只需要将变量赋值为nil  

## - 数据类型  
lua有8个基本类型：  

基本类型|描述
-|-
nil|这个最简单，只有值nil属于该类，表示一个无效值（在条件表达式中相当于false）。
boolean|包含true和false
number|表示双精度类型的浮点数
string|字符串，由一对双引号或者单引号表示
function|由C或者lua表示的函数
userdata|表示任意存储在变量中的C数据结构
thread|表示执行的独立线路，用于执行协同程序
table|lua中的表其实是一个关联数组，数组的索引可以是数字、字符串或者表类型。在lua里table的创建是“构造表达式”来完成的，最简单的表达式{}用来创建一个表。

## - lua 循环
repeat ...until 不停重复循环直到条件为真

## - 函数
 lua的函数可以有多个返回值
```
local function retValue()
    return 2,1
end
```
可变参数 ...
lua函数接受可变参数，和c类似
```
function add(...)
    local sum = 0
    for i,v in ipairs{...} do
        sum  = sum + v
    end
    return sum
end
print(add(1,2,3))
```

select("#",...) -- 获取可变参数的数量

## - lua运算符

加|减|乘|除|模|幂|负
-|-|-|-|-|-|-
+|-|*|/|%|^|-

- 关系运算符

等于 | 不等于 | 大于 | 小于 | 大于等于 | 小于等于
:-:|:-:|:-:|:-:|:-:|:-:
==|~=|>|<|>=|<=

-逻辑运算符

逻辑与|逻辑或|逻辑非
-|-|-
and|or|not

-其他运算服

连接两个字符串|一元运算符，返回字符串或表的长度
:-:|:-:
..|#

## - 运算符优先级

> 由高到低:  

^  
not -(unary)  
\*   /  
\+   -  
..
< > <= >= ~= ==  
and  
or  

## - lua字符串

可以有三种方式表示：  
1. 单引号间的一串字符
2. 双引号间的一串字符
3. [[和]]间的一串字符
```
name1 = 'hello'
name2 = "hello"
name3 = [[hello]]
```

转义字符|意义|ACSCLLII码值（10进制）
:-|:-|:-
\a|响铃（BEL）|007
\b|退格（BS），将当前位置移动到前一列|008
\f|换页（FF），将当前位置移动到下页开头|012
\n|换行（LF），将当前位置移动到下一行开头|010
\r|回车（CR），将当前位置移动到本行开头|013
\t|水平制表（HT）（跳到下一个TAB位置）|009
\v|垂直制表（VT）|011
\\\\|代表一个反斜杠字符“\”|092
\\'|表示一个单引号（撇号）字符|039
\\"|表示一个双引号字符|034
\0|空字符（NULL）|000
\ddd|1到3位八进制数所代表的任意字符|三位八进制
\xhh|1到2位十六进制所代表的任意字符|二位十六进制

## - 字符串操作

序号|方法&用途
:-|:-
1|string.upper(argument)<br>字符串全转换成大写
2|string.lower(argment)<br>字符串全部转换成小写
3|string.gsub(mainString,findString,replace String,num)<br>在字符串中替换，mainString为要替换的字符串，fingString为被替换的字符串，replaceString为要替换的字符串，num为替换次数（长度）
4|string.find (s, pattern [, init [, plain]])<br>函数在字符串s里查找第一个和参数pattern匹配的子串，如果找到了一个匹配的子串，就会返回这个子串的起始索引和结束索引，否则就会返回nil。另外，参数init作为一个数字，指定了搜索的起始位置，这个数字默认为1可以一个负数，表示从后往前数的字符个数。参数plain作为第四个可选参数默认为flase，传入参数true表示关闭模式匹配，所以函数只做简单的查找子串的操作，如果子串pattern没有字符为空字符串""将会被认为是魔法字符。 如果模式匹配子串被找到了，一个成功被找到的子串将会作为第三个返回值，放在两个索引返回值的后边而返回<br>``` string,find("Hello lua user","lua",1)```<br>7 9
5|string.reverse(arg)<br>字符串反转
6|string，format(...)<br>返回一个类似printf的字符串<br>string.format("age : %d",12)      --"age : 12"
7|string.char(arg)和string.byte(arg[,int])<br>char将整型数字转成字符并连接，byte转换字符为整数（可以指定某个字符，默认第一个字符）
8|string.len(arg)<br>计算字符串长度
9|string.rep(string,n)<br>返回字符串string的n个拷贝<br>string.rep("ab",2) --abab
10|..<br>拼接两个字符串
11|string.gamth(str,pattern)<br>返回一个迭代器，每次调用这个函数，返回一个在字符串str找到的下一个pattern描述的子串，如果阐参数pattern描述的字符串没有找到，迭代器返回nil<br>for word in string.gmatch("Hello Lua user", "%a+") do print(word) end <br>Hello<br>Lua<br>user<br>%a表示单个字母，%a+也就是匹配多个，可以理解成一个单词。
12|string.match(str,pattern,init)<br>string.match()只寻找源字符串str中的第一个配对，参数init可选，指定搜寻过程的起点，默认为1.<br>在成功匹配时，函数将返回配对表达式中的所有捕获结果，如果没有设置捕获标记，则返回整个配对字符串，当没有成功配对时，返回nil<br>print(string.match("I have 2 questions for you.", "%d+ %a+")) --2 questions<br>print(string.format("%d, %q", string.match("I have 2 questions for you.", "(%d+) (%a+)"))) -- 2 "questions"


格式字符串
格式|描述
-|-
%c | 接受一个数字, 并将其转化为ASCII码表中对应的字符
%d, %i | 接受一个数字并将其转化为有符号的整数格式
%o | 接受一个数字并将其转化为八进制数格式
%u | 接受一个数字并将其转化为无符号整数格式
%x | 接受一个数字并将其转化为十六进制数格式, 使用小写字母
%X | 接受一个数字并将其转化为十六进制数格式, 使用大写字母
%e | 接受一个数字并将其转化为科学记数法格式, 使用小写字母e
%E | 接受一个数字并将其转化为科学记数法格式, 使用大写字母E
%f | 接受一个数字并将其转化为浮点数格式
%g(%G) | 接受一个数字并将其转化为%e(%E, 对应%G)及%f中较短的一种格式
%q | 接受一个字符串并将其转化为可安全被Lua编译器读入的格式
%s | 接受一个字符串并按照给定的参数格式化该字符串

为进一步细化格式，可以在%后面添加参数：  
1.符号：一个+号表示其后的数字转义符将让证书显示正号，默认情况下只有负数显示符号 <br>print(string.format("%+d", 17.0) )             -- 输出+17  
2.占位符：一个0,表示在后面指定了字符宽度时占位用，不填时默认占位符时空<br>-- 日期格式化<br>
date = 2; month = 1; year = 2014<br>
print(string.format("日期格式化 %02d/%0d/%05d", date, month, year))<br>--日期格式化 02/1/02014
3.对齐标识：在指定了字符宽度时，默认为右对齐，增加-号可以改为左对齐
4.宽度数值
5.小数位数/字串裁切：在宽度数值后增加的小时部分n，若后接（浮点数转义符，如%6.3f）则设定改浮点数的小数只保留n位,若后接s（字符串转义符，如%5.3s）则设定改字符传只显示前n位<br>print(string.format("%6.3f", 13))              -- 输出13.000<br>print(string.format("%5.3s", "monkey"))        -- 输出  mon

## - 匹配模式  
lua中的匹配模式直接用常规的字符串来描述，它3用于匹配模式函数string.find string.gmatch string.gsub string.match,你可以下任何模式中使用字符类。  
字符类指可以匹配一个特定字符集合内任何字符的模式项。比如，字符类%d匹配任意数字，所以可以使用模式串%d%d/%d%d/%d%d%d%d搜索dd/mm/yyyy格式的日期:<br>s = "Deadline is 30/05/1999, firm"<br>date = "%d%d/%d%d/%d%d%d%d"<br>print(string.sub(s, string.find(s, date)))    --> 30/05/1999

lua支持的所有字符类
单个字符（除^$()%.[]*+-?外）：与改字符自身配对
.|与任意字符配对
-|-
%a|与任意字母配对
%c|与认可控制符配对（如\n）
%d|与任何数字配对
%l|与任何小写字母配对
%u|与任何大写字母配对
%w|与任何字母/数字配对
%p|与任何标点配对
%s|与任何空白字符配对
%z|与任何代表0的字符配对
%x|与任何十六进制数配对
%x|此处的X时非字母非数字字符。与字符X配对，主要用来处理表达式中有功能的字符（^$()%.[]*+-?）的配对问题，例如%%与%配对
[数个字符类]|与任何[]中包含的字符类配对，例如[%w_]与认可字母/数字，或 _（下划符号）配对
[^数个字符类]|与认可不包含在[]中的字符类配对，例如[^%s]与任何非空字符配对
> 当上述的字符类用大写书写时，表示与非此字符类的任何字符配对，例如%S表示与任何非空白字符配对，%A表示与非字母配对


- '%'用作特殊的转义字符，'%.'匹配点，'%%'匹配%，转义符%不仅可以用来转义特殊字符，还可以用于所有非字母的字符。  
模式条目可以是：  
单个字符类匹配改类别中任意单个字符  
单个字符类跟一个'*'，将匹配零或多个该类的字符，这个条目总是匹配尽可能长的串  
单个字符类根一个‘+’，将匹配一或更多个该类的字符，这个条目总是匹配尽可能长的串  
单个字符类跟一个‘-’，将匹配零或更多个该类字符，和‘*’不同，这个条目匹配尽可能短的串  
单个字符类跟一个‘？’，将匹配零或一个该类的字符，只要有可能，他会匹配一个  
%n，这里的n可以从1到9 这个条目匹配一个等于n号捕获物的子串  
%bxy，这里的x和y是两个明确的字符，这个条目匹配以x开始y结束，且其中x和y保持平衡的字符串，意思是如果从左到右读这个字符串，对每次读到一个x就+1读到一个y就-1，最终结束处的那个y是第一个计数到0的y。例如：条目%b() 可以匹配到括号平衡的表达式  
%f[set]，指边境模式，这个条目会匹配到一个位于set内某个字符串之前的一个空串，且这个空船的前一个字符不属于set，集合set的含义如前所述，匹配出的哪个空串之开始和结束点的计算就堪称该处有个字符‘\0’一样

模式：一个模式条目的序列，在模式最前面加上符号'^'将锚定从字符串开始处做匹配，在模式最后面加上符号‘$’将使匹配过程锚定到字符串的结尾，如果他们出现在其他位置均只表示自身不再有特殊含义

捕获：模式可以在内部用小括号括起一个子模式，这些子模式被称为捕获物，当成功匹配时，由捕获物匹配到的字符串中的子串被保护起来，用于未来的用途，捕获物以他们左括号的次序来编号，例如：对于模式"(a*(.)%w(%s*))",字符串中匹配到"(a*(.)%w(%s*))"的部分保存在第一个捕获物中，因此编号时1，由"."匹配到的字符是2号捕获物，匹配到"%s*"的那部分是3号。

作为一个特列，空的捕获（）将捕获到当前字符串的位置（是一个数字）例如，如果将模式"()aa()"作用到字符串"flaaap"上，将产生两个捕获物：3 和 5


## - 数组

lua的数组大小不是固定的 
lua的索引是1为始，但也可以指定0开始而且还能以负数为数组索引
```
array = {}
for i =-2,2 do
    array[i] = i;
end
for i =-2,2 do
    print(array[i])
end
```
输出：  
-2<br>-1<br>0<br>1<br>2

## - 迭代器
是一种对象，它能够用来遍历标准模板容器的部分或全部元素，每个迭代器对象代表容器中的确定地址。在lua中迭代器是一种支持指针类型的结构，它可以遍历结集合的每一个元素。  

1.泛型for迭代器
泛型for在自己内部保存迭代函数，实际上它保存三个值：迭代函数、状态常量、控制变量
。它提供了集合的key/value 语法格式：
```
for k,v in pairs(t) do 
    print(k,v)
end

-- 实例
array = {"hello","world"}
for k,v in ipairs(array)
do
    print(k,v)
end    
```

实例使用了lua默认提供的迭代函数ipairs。
在lua中常常使用函数来描述迭代器，每次调用该函数就返回集合的下一个元素，lua的迭代器包含以下两种类型：  
无状态的迭代器  
多状态的迭代器  

1.无状态迭代器  
无状态迭代器是指不保留任何状态的迭代器，因此在循环中我们可以利用无状态迭代器避免创建闭包花费的额外代价，每次迭代，迭代函数都是用两个变量（状态常量和控制变量）的值作为参数被调用，一个无状态的迭代器只利用这两个值可以获取下一个元素，这种无状态迭代器的典型列子是ipairs 它遍历数组的每一个元素

2.多状态迭代器
很多情况下，迭代器需要保存多个状态信息而不是肩带的状态常量和控制变量，最简单的方法是使用闭包，还有一种方法就是将所有的状态信息封装到table内，将table作为迭代器的状态常量，因此这种情况可以将所有的信息放在table内，所以迭代函数通常不需要第二个参数，

## - lua 表

表的构建  
--初始化表  
mytable = {}  
--指定值  
mytable[1] = "lua"  
mytable["key"] = "value"
--移除引用  
mytable = nil

table的操作

序号|方法&用途
-|-
1|table.coucat(table[,sep[,start[,end]]])<br>concat是concatenate(连锁，连接)的缩写。table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素，元素间可以用分隔符（sep）隔开
2|table.insert(table[,pos[,value]])<br>在table的数组部分指定位置（pos）插入值为value的一个元素，pos参数可选，默认为数组部分末尾
3|table.maxn(table)<br>指定table中所有正数key值中最大的key值，如果不存在key值为正数的元素，则返回0（lua5.2该方法已不存在了）
4|table.remove(table[,pos])<br>返回table数组部分位于pos位置的元素，其后的元素会被前移，pos参数可选，默认是table的长度，即从最后一个元素删起
5|table.sort(table[,comp])<br>升序排序

## - 模块和包

模块类似于一个封装库。lua的模块是有变量、函数等已知元素组成的table。因此创建一个模块很简单，就是创建一个table，然后把需要导出的常量、函数放入其中最后返回这个table就行
```
--文件名为module.lua  
--定义一个module的模块  
module={}  
--定义一个常量  
module.constant = "这是一个常量"  
--定义个函数  
function module.func1()
    io.write("这是一个共有函数\n")
end

local function func2()
    print("这是一个私有函数")
end

function module.func3()
    func2()
end

return moudle
```
由此可见，模块的结构就是一个table的结构，因此可以想操作调用table里的元素那样操作调用模块里的常量或函数。上面的func2声明为程序快的局部变量，即表示一个私有函数，因此是不能从外部访问模块里面的私有函数，必须通过模块里的共有函数来调用  

## - require 函数

lua提供了一个名为require的函数用来加载模块  
require "模块名" 或  require ("模块名")  
require 后会返回一个模块常量或函数组成的table并且还会定义一个包含该table的全局变量  
```
--test_module.lua
--module模块使用
require "module"

print(module.constant)
module.func3()

```
## - lua元表（metatable）

类似于c++的继承，元表相当于table的父类。
例如：使用元表我们可以计算两个table相加，首先检查两者之一是否有元表，然后检查是否有叫—__add的字段，若找到则调用对应的值，__add等对应的值（往往是一个函数或table）就是元方法

处理元表的两个重要函数：  
  setmetatable(table,metatable)<br>对指定table设置metatable元表，如果metatable中存在__metatable键值，setmetatable会失败。  
  getmetatable(table)<br>返回对象的元表
  
  mytable = setmetatable({},{})  
  getmetatable(mytable)  
  
  > __index 元方法  
  这是metatable最常用的键。  
  当你通过键来访问table的时候，如果这个键没有值，那么lua就会找该table的metatable（假定有metatable）中的__index键，如果__index包含一个表，lua会在表中查找相应的键  
  lua查找一个表元素时的规则，有如下几个步骤：  
  1.在表中查找，找到返回元素，没有找到继续2。
  2.判断该表是否有元表（根据__index），如果__index方法为nil那么返回nil，如果__index是一个表那么再从1开始执行，如果是一个函数，那么直接返回函数的返回值。
  
  > __newindex元方法  
  __newindex元方法用来对表的更新,__index则用来对表的访问  
  当给你表的一个缺少的索引赋值，解释器就会查找__newindex元方法，如果存在则会调用这个函数而不进行赋值操作
  
## - 为表添加操作符

模式|描述
:-:|:-:
__add|对应的算术运算符"+"
__sub|对应的算术运算符"-"
__mul|对应的算术运算符"*"
__div|对应的算术运算符"/"
__mod|对应的算术运算符"%"
__unm|对应的算术运算符"-"
__concat|对应的算术运算符".."
__eq|对应的算术运算符"=="
__lt|对应的算术运算符"<"
__le|对应的算术运算符"<="


## - __call元方法  

它在lua调用一个值时调用

- __tostring元方法  

用于修改表的输出行为

- lua协同程序（coroutine）  
 协同程序，与线程类似。它拥有独立的堆栈，对立的指针，同时又与其他协同程序共享全局变量和其他大部分东西。  
协同是非常强大的功能，但用起来也很复杂。  
线程和协同程序的区别：  
线程和协同程序的主要区别在于，一个具有多线程的程序可以同时运行几个线程，而协同程序却需要彼此协作的运行。再任意指定的时刻只能有一个协同程序再运行，并且这个正在运行的协同程序只有在明确的被要求挂起的时候才会被挂起。协同程序有点类似同步的多线程，在等待同一个线程锁的几个线程有点类似协同

基本语法  
方法| 描述
-|-
coroutine.create()|创建coroutine 返回coroutine。参数是一个函数，当和resume配合使用的时候就唤醒函数调用
coroutine.resume()|重启 coroutine 和 create配合使用
coroutine.yield()|挂起coroutine 将coroutine设置为挂起状态，这个和resume配置使用能有很多有用的效果
coroutine.status()|查看coroutine的状态<br>注：coroutine的状态有三种：dead、suspended、running
coroutine.wrap()|创建coroutine 返回一个函数，一旦调用这个函数，就进入coroutine，和create功能重复
coroutine.running()|返回正在运行的coroutine，一个coroutine就是一个线程，当使用running的时候，就是返回一个corouting的线程号

## - lua 文件I/O  
lua I/O库用于读取和处理文件，分为简单模式（和C一样）、完全模式。  
简单模式（simple model）：拥有一个当前输入文件和一个当前输出文件，并且提供针对这些文件的相关操作  
完全模式（complete model）：使用外部文件的句柄来实现，他是一种面向对象的形式，将所有的文件操作定义为文件句柄的方法。  
简单模式在做一些简单的文件操作时较为合适，但是在进行一些高级的文件操作的时候，简单模式就显得力不从心了，例如同时读取多个文件的操作。使用完全模式则较为合适。  
**简单模式**：  
打开文件的操作:  
file = io.open(filename[,mode])

mode值有：  
模式 | 描述
-|-
r|以只读的方式打开文件，文件必须已经存在
w|以只能写的方式打开文件，若文件存在会覆盖原有内容，不存在则会创建
a|以追加的方式打开文件，若文件存在新写入的数据会追加到文件末尾（EOF符保留），若文件不存在则创建文件。
r+|以可读可写的方式打开文件，文件必须存在
w+|以可读可写的方式打开文件，文件存在则覆盖原有内容，不存在则会创建
a+|与a类似，但是此类可读可写
b|以二进制模式
+|表示文件可读可写

io.read() 可以有参数:   
例如：  
file.read("*n"):读取一个数字并返回它  
file.read("*a"):从当前位置读取整个文件  
file.read("*l"):默认值。读取下一行，在文件尾（EOD）处则返回nil  
file.read(number)：返回一个指定字符个数的字符串，或在EOF时返回nil

其他io的方法有：  
io.tmpfile()：返回一个临时文件句柄，该文件以更新的模式到开，程序结束时自动删除  
io.type(file)：检测obj是否时一个可用的文件句柄  
io.flush():向文件写入缓冲的所有数据  
io.lines(optional file name)：返回一个迭代函数，每次调用将获得文件中的一行内容，当到文件末尾的时候，将返回nil但不关闭文件

**完全模式**：  
通常我们需要在同一时间处理多个文件时需要使用file:function_name来代替io.function_name  
read的参数与简单模式一致  
其他方法：  
file:seek(optional whence,optional offset):<br>设置和获取当前文件位置，成功则返回最终的文件位置（按字节），失败则返回nil加错误信息。  
参数whence可以是:  
 "set" 从文件头开始  
 "cur" 从当前位置开始【默认】
 "end" 从文件末尾开始
 "offset" 默认为0
不带参数的file:seek()则返回当前位置。  
file:seek("set") 则定位到文件开头

file:flush() 向文件写入缓冲区中的所有数据  
io.lines(optional file name)打开指定的文件filename为读模式并返回一个迭代函数，每次调用将得到文件中的一行内容，当到文件末尾时，将返回nil并自动关闭文件  
若不带参数时io.lines()<=>io.input():lines();读取默认输入设备的内容，但结束时不关闭文件

## - lua 错误处理
  

assert(type(a) == "number", "a 不是一个数字")  
error(message[,level])  
功能：终止正在执行的函数，并返回messgge的内容作为错误信息（error函数永远都不会返回）通常情况下，error会附加一些错误位置的信息到message头部  
level参数指示错误的位置  
level=1[默认] 为调用error位置（文件+行号）
level=2 指出哪个调用error的函数的函数
level=0 不添加错误位置信息

**pcall和xpcall、debug**

lua中处理错误，可以使用函数pcall（protected call）来包装需要执行的代码  
pcall接收一个函数和要传递给后者的参数，并执行，执行结果：有错误、无错误；返回true或false或errorinfo  
语法格式：  
```
if pcall(function_name,...)then  
--no error  
else  
--some error  
end  

print(pcall(function(i) print(i) end, 33))
pcall(function(i) print(666) error('error..') end)

```
pcall以一种保护模式来调用第一个参数，因此pcall可以捕获函数执行中的任何错误。  
通常在发生错误时，希望能得到更多是调式信息，而不只是发生错误的位置。但pcall返回时，他已经销毁了调用栈的部分内容。lua提供了xpcall函数，xpcall接收第二个参数（一个错误处理函数），当错误发生时，lua会在调用栈展开（unwind）前调用错误处理函数，于是就可以在这个函数中使用debug库来获取关于错误的额外信息了  
debug库提供了两个通用的错误处理函数：  
debug.debug 提供一个lua提示符，让用户检查错误的原因  
debug.traceback 根据调用栈来构建一个扩展的错误信息  

## - lua 调式

序号|方法&描述
-|-
1|debug()<br>进入一个用户交互模式，运行用户输入的每个字符，使用简单的命令以及其他调试设置，永和可以检阅全局变量和局部变量，改变变量的值，计算一些表达式等。输入一行仅包含cont的字符串将结束这个函数。
2|getfenv(object)<br>返回对象的环境变量
3|gethook(optional thread)<br>返回三个表示线程钩子设置的值，当前钩子函数，当前钩子掩码，当前钩子计数
4|getinfo([thread,]f[,what])<br>返回关于一个函数信息的表，你可以直接提供该函数，也可以用一个数字f表示该函数，数字f表示运行在指定线程的调用栈对应层次上的函数：0层表示当前函数（getinfo自身）1层表示调用getinfo的函数（除非时尾调用，这种情况不计入栈）等等，如果f是一个比较活跃函数数量还大的数字，getinfo返回nil
5|getlocal([thread,]f,local)<br>此函数返回在栈的f层处函数的索引为local的局部变量的名字和值，这个函数不仅用于访问显示定义的局部变量，也包括形参、临时变量等。  
6|getmetatable(value)<br>把给定索引指向的值的元表压入堆栈，如果索引无效或这个值没有有元表函数将返回0并且不会像栈上压任何东西
7|getregistry()<br>表示注册表表，这是一个预定义出来的表，可以用来保存任何C代码想保存的lua值
8|getupvalue(f,up)<br>此函数返回函数f的第up个上值的名字和值，如果该函数没有哪个上值，返回nil。以（ 打头的变量名表示没有名字的变量
9|sethook([thread,]hook,mask[,count])<br>将一个函数作为钩子函数设入，字符串mask以及数字count决定了钩子将在任何时候调用，掩码是有下列字符组成的字符串，每个字符串有其含义：<br>‘c’ 每当lua调用一个函数时，调用钩子<br>‘r’ 每当lua从一个函数内返回时，调用钩子 <br>‘l’ 每当lua进入新的一行时，调用钩子
10|setlocal([thread,]level,local,value)<br>这个函数将value赋值给栈上第level层的第local个局部变量，如果没有那个变量，函数返回nil。如果level越界抛出一个错误。
11|setmetatable(value,table)<br>将value的元表设置为table（可以时nil），返回value
12|setupvalue(f,up,value)<br>这个函数将value设为函数f的第up个上值，如果函数没有那个上值，返回nil否则返回该上值的名字。
13|tracback([thread,][message[,level]])<br>如果message有且不是字符串或nil函数不做任何处理直接返回message否则它返回调用栈的栈回溯信息，字符串可选项message被添加在栈回溯信息的开头，数字可选level指明从栈的哪一层开始回溯（默认为1，即调用traceback那里）

调试类型:  
命令行：RemDebug、clidebugger、ctrace、xdbLua、LuaInterface - Debugger、Rldb、ModDebug  
图形界面：SciTE、Decoda、ZeroBrane Studio、akdebugger、luaedit  

## - lua 垃圾回收  
lua提供了以下函数collectgarbage([opt[,arg]])用来控制自动内存管理：  
collectgarbage("collect"):做一次完整的垃圾收集循环，通过opt它提供了一组不同的功能。  
collectgarbage("count"):以k字节数为单位返回lua使用的总内存数，这个值有小数部分，所以只需要乘上1024就能得到lua使用的准确字节数（除非溢出）  
collectgarbage("restart"):重启垃圾收集器的自动运行  
collectgarbage("setpause"):将arg设为收集器的间歇率，返回前一个间歇率  
collectgarbage("setstepmul"):返回步进倍率的前一个值  
collectgarbage("step"):单步运行垃圾收集器，步长”大小“由arg控制，传入0时，收集器步进（不可分割）一步，传入非0值，收集器收集相当于lua分配这些多（k字节）内存的工作，如果收集器结束一个循环将返回true  
collectgarbage("stop"):停止垃圾收集器的运行，在调用前重启，收集器只会显示的调用运行  

## - lua 面向对象  
```
-- Meta class
Shape = {area = 0}
-- 基础类方法 new
function Shape:new (o,side)
  o = o or {}
  setmetatable(o, self)
  self.__index = self
  side = side or 0
  self.area = side*side;
  return o
end
-- 基础类方法 printArea
function Shape:printArea ()
  print("面积为 ",self.area)
end
-- 创建对象
myshape = Shape:new(nil,10)
myshape:printArea()
-------------------------------------------------------------------
Square = Shape:new()
-- 派生类方法 new
function Square:new (o,side)
  o = o or Shape:new(o,side)
  setmetatable(o, self)
  self.__index = self
  return o
end

-- 派生类方法 printArea
function Square:printArea ()
  print("正方形面积为 ",self.area)
end

-- 创建对象
mysquare = Square:new(nil,10)
mysquare:printArea()
-------------------------------------------------------------------

Rectangle = Shape:new()
-- 派生类方法 new
function Rectangle:new (o,length,breadth)
  o = o or Shape:new(o)
  setmetatable(o, self)
  self.__index = self
  self.area = length * breadth
  return o
end

-- 派生类方法 printArea
function Rectangle:printArea ()
  print("矩形面积为 ",self.area)
end

-- 创建对象
myrectangle = Rectangle:new(nil,10,20)
myrectangle:printArea()
-----------------------------------------------------------------

```

## - lua&C 编译

- 编译配置： cc main.c \`pkg-config --cflags lua5.3\` \`pkg-config --libs lua5.3\`

## - lua堆栈图
- 在Lua中，Lua堆栈就是一个struct。  
`堆栈索引的方式可以是正数也可以是负数，区别是：正数索引1永远表示栈底，负数索引-1永远表示栈顶`

正||负
-|-|-
4|栈 顶|-1
3| ... |-2
2| ... |-3
1|栈 底|-4

## - lua文献
[lua手册](https://www.runoob.com/manual/lua53doc/contents.html#contents)  
[扶我起来，我还能写](https://www.runoob.com/lua/lua-data-types.html)
