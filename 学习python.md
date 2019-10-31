## 标识符

- 第一个字符必须是字母表中字母或下划线 **_** 。
- 标识符的其他的部分由字母、数字和下划线组成。
- 标识符对大小写敏感。

在 Python 3 中，非 ASCII 标识符也是允许的了。

```python
import keyword
print(keyword.kwlist)
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

> 注释

Python中单行注释以 **#** 开头，实例如下：

print ("Hello, Python!") # 第二个注释

多行注释可以用多个 **#** 号，还有 **'''** 和 **"""**：

```python
#!/usr/bin/python3
 
# 第一个注释
# 第二个注释
 
'''
第三注释
第四注释
'''
 
"""
第五注释
第六注释
"""
print ("Hello, Python!")

```

> 行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号{}

```python
if True:
  print ("True")
else:
  print ("False")
```

以下代码最后一行语句缩进数的空格数不一致，会导致运行错误：

```python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误
```

以上程序由于缩进不一致，执行后会出现类似以下错误：

```
 File "test.py", line 6
    print ("False")    # 缩进不一致，会导致运行错误
                                      ^
IndentationError: unindent does not match any outer indentation level
```

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠(\)来实现多行语句，例如：

```python
total = "item_one" + \
        "item_two" + \
        "item_three"
```

在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)，例如：

```python
total2 = ['item_one', 'item_two', 'item_three',
          'item_four', 'item_five']
print(total2)
```

> 数字（Number）类型

python中数字有四种类型：整数，布尔型，浮点数和复数

- int

- bool           True
- float           1.23
- complex     1+2j

> 字符串（String）

- python中单引号和双引号使用完全相同。
- 使用三引号('''或""")可以指定一个多行字符串。
- 反斜杠可以用来转义，使用 r 可以让反斜杠不发生转义。。 如 r"this is a line with \n" 则\n会显示，并不是换行。
- 按字面意义级联字符串，如"this " "is " "string"会被自动转换为this is string。
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。
- 字符串可以用 + 运算符连接在一起，用 * 运算符重复。
- Python中的字符串不能改变。
- 字符串的截取的语法格式如下：**变量[头下标:尾下标:步长]**

```python
str='Runoob'
 
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符 Runoo
```

>  空行

函数之间或类的方法之间用空行分隔，表示一段新的代码的开始。类和函数入口之间也用一行空行分隔，以突出函数入口的开始。

空行与代码缩进不同，空行并不是Python语法的一部分。书写时不插入空行，Python解释器运行也不会出错。但是空行的作用在于分隔两段不同功能或含义的代码，便于日后代码的维护或重构。

**记住：**空行也是程序代码的一部分。

> 等待用户输入

```python
input("\n\n按下enter键后退出")
#\n\n 在结果输出之前会输出两个新的空行。一旦用户按下enter键时,程序将退出
```

>  同一行显示多条语句

```
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

> 多个语句构成代码组

缩进相同的一组语句构成一个代码块，我们称之代码组。

像if、while、def和class这样的复合语句，首行以关键字开始，以冒号( : )结束，该行之后的一行或多行代码构成代码组。

我们将首行及后面的代码组称为一个子句(clause)。

```
if expression : 
   suite
elif expression : 
   suite 
else : 
   suite
```

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end=""**：

> Print 输出

print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end=""**：

> import与from ... import

在python用import或者from ... import 来导入相应的模块.

将整个模块导入，格式为：**import  somemodule**

从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

将某个模块中的全部函数导入，格式为： **from somemodule import \***

>  导入 sys 模块

```python
import sys 
print('================Python import mode=========================='); 
print ('命令行参数为:') 
for i in sys.argv:     
  print (i) 
print ('\n python 路径为',sys.path)
```

导入sys模块的argv，path成员

```python
from sys import argv,path #导入特定的成员
print('================python from import===================================')
print('path:',path)
# 因为已经导入path成员，所以此处引用时不需要加sys.path
```

> Python3 基本数据类型

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

等号（=）用来给变量赋值。

counter = 100          # 整型变量
miles   = 1000.0       # 浮点型变量
name    = "runoob"     # 字符串
print (counter)
print (miles)
print (name)

>  多个变量赋值

Python允许你同时为多个变量赋值。例如：

```
a = b = c = 1
```

以上实例，创建一个整型对象，值为 1，从后向前赋值，三个变量被赋予相同的数值。

您也可以为多个对象指定多个变量。例如：

```
a, b, c = 1, 2, "runoob"
```

以上实例，两个整型对象 1 和 2 的分配给变量 a 和 b，字符串对象 "runoob" 分配给变量 c。

> 标准数据类型

Python3 中有六个标准的数据类型：

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）。

```python
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```

此外还可以用 isinstance 来判断：

```python
>>>a = 111
>>> isinstance(a, int)
True
>>>
```

> String(字符串)

Python中的字符串用单引号 **'** 或双引号 **"** 括起来，同时使用反斜杠 **\** 转义特殊字符。

字符串的截取的语法格式如下：

```
变量[头下标:尾下标]
```

![img](https://www.runoob.com/wp-content/uploads/2013/11/o99aU.png)



> List （列表）

List （列表）是python中使用最频繁的数据类型

列表可以完成大多数集合类的数据结构实现。列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）。

列表是写在方括号 **[]** 之间、用逗号分隔开的元素列表。

和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。

列表截取的语法格式如下：

```
变量[头下标:尾下标]
```

索引值以 0 为开始值，-1 为从末尾的开始位置。

![img](https://www.runoob.com/wp-content/uploads/2013/11/list_slicing1.png)

```python
list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob']
print (list)            # 输出完整列表
print (list[0])         # 输出列表第一个元素
print (list[1:3])       # 从第二个开始输出到第三个元素
print (list[2:])        # 输出从第三个元素开始的所有元素
print (tinylist * 2)    # 输出两次列表
print (list + tinylist) # 连接列表
```

- 1、List写在方括号之间，元素用逗号隔开。
- 2、和字符串一样，list可以被索引和切片。
- 3、List可以使用+操作符进行拼接。
- 4、List中的元素是可以改变的。

> Set（集合）

集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。

基本功能是进行成员关系测试和删除重复元素。

可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}

{'Mary', 'Rose', 'Tom', 'Jack', 'Jim'}

> 注意：集合是无序的

print(a - b)     # a 和 b 的差集

print(a | b)     # a 和 b 的并集

print(a & b)     # a 和 b 的交集

print(a ^ b)     # a 和 b 中不同时存在的元素

> dictionnary

字典（dictionary）是Python中另一个非常有用的内置数据类型。

列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。

字典是一种映射类型，字典用 **{ }** 标识，它是一个无序的 **键(key) : 值(value)** 的集合。

键(key)必须使用不可变类型。

在同一个字典中，键(key)必须是唯一的。

```python
dict = {}
dict['one'] = "1 - 菜鸟教程"
dict[2]     = "2 - 菜鸟工具"
 
tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}
print (dict['one'])       # 输出键为 'one' 的值
print (dict[2])           # 输出键为 2 的值
print (tinydict)          # 输出完整的字典
print (tinydict.keys())   # 输出所有键
print (tinydict.values()) # 输出所有值
```

输出结果

```
1 - 菜鸟教程
2 - 菜鸟工具
{'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}
dict_keys(['name', 'code', 'site'])
dict_values(['runoob', 1, 'www.runoob.com'])
```

> range()函数

如果你需要遍历数字序列，可以使用内置range()函数。它会生成数列，例如:

```python
>>>for i in range(5):
...     print(i)
...
0
1
2
3
4
```

你也可以使用range指定区间的值：

```python
>>>for i in range(5,9) :
    print(i)
 
    
5
6
7
8
>>>
```

也可以使range以指定数字开始并指定不同的增量(甚至可以是负数，有时这也叫做'步长'):

```python
>>>for i in range(0, 10, 3) :
    print(i)
 
    
0
3
6
9
>>>
```

```python
>>>for i in range(-10, -100, -30) :
    print(i)
 
    
-10
-40
-70
>>>
```

>  pass 语句

Python pass是空语句，是为了保持程序结构的完整性。

pass 不做任何事情，一般用做占位语句，如下实例

```python
while True:
  ... pass #等待键盘中断 ctrl+c
```

```python
class MyEmptyClass:
  ... pass
```



#####python函数

- 函数代码块以def关键词开头，后接函数标识符名称和()

- 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
- 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明
- 函数内容以冒号起始，并且缩进
- **return [表达式]** 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

```python
def functionname(params):
  "函数_文档字符串"
  function_suite
  return [expression]

```



#####Python 模块(Module)

Python 模块(Module)，是一个 Python 文件，以 .py 结尾，包含了 Python 对象定义和Python语句。

模块让你能够有逻辑地组织你的python代码段。

把相关的代码分配到一个模块里能让你的代码更好用，更易懂。

模块能定义函数，类和变量，模块里也能包含可执行的代码。

比如要引用模块 math，就可以在文件最开始的地方用 **import math** 来引入。在调用 math 模块中的函数时，必须这样引用：

```
模块名.函数名
```

当解释器遇到import语句，如果模块在当前的搜索路径就会被导入。

```python
support.py 模块：
def print_func( par ):
   print "Hello : ", par
   return
```

搜索路径是一个解释器会先进行搜索的所有目录的列表。如想要导入模块 support.py，需要把命令放在脚本的顶端：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 导入模块
import support
 
# 现在可以调用模块里包含的函数了
support.print_func("Runoob")
```

from...import

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中。语法如下：

```python
from modname import name1[, name2[, ... nameN]]
```

例如，要导入模块fib的fibonacci函数

```python
from fib import fibonacci
```

这个声明不会把整个 fib 模块导入到当前的命名空间中，它只会将 fib 里的 fibonacci 单个引入到执行这个声明的模块的全局符号表。

from ...import*语句

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：

```python
from modname import *
```

这提供了一个简单的方法来导入一个模块中的所有项目。然而这种声明不该被过多地使用。

例如我们想一次性引入 math 模块中所有的东西，语句如下：

```python
from math import *
```

当你导入一个模块，python解析器对应模块位置的搜索顺序是

- 当前目录
- 如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。

- 如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/。

模块搜索路径存储在 system 模块的 sys.path 变量中。变量里包含当前目录，PYTHONPATH和由安装过程决定的默认目录。

PYTHONPATH变量

作为环境变量，PYTHONPATH 由装在一个列表里的许多目录组成。PYTHONPATH 的语法和 shell 变量 PATH 的一样。



#####python文件I/O

######打印到屏幕

最简单的输出方法是print语句，可以给它传递0或多个用逗号隔开的表达式。此函数把你传递的表达式转换成一个字符串表达式，并将结果写到标准输出如下

```python
# -*- coding: UTF-8 -*-
print "python is good"
```

###### 读取键盘输入

python提供了两个内置函数从标准输入读入一行文本，默认的标准输入是键盘。

- raw_input
- input

raw_input([prompt]) 函数从标准输入读取一个行，并返回一个字符串（去掉结尾的换行符）：

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*- 
 
str = raw_input("请输入：")
print "你输入的内容是: ", str
```

**input([prompt])** 函数和 **raw_input([prompt])** 函数基本类似，但是 input 可以接收一个Python表达式作为输入，并将运算结果返回。

遍历数字序列，可使用内置函数range()函数，它会生成数列。

###### pickle模块

该模块实现了基本的数据序列和反序列化

通过pickle模块的序列化操作我们能够将程序中运行的对象信息保存到文件中去，永久存储

通过pickle模块的反序列化操作，我们能够从文件中创建上一次程序保存的对象



##### python之正则表达式

```js

^ 匹配字符串的开头
$ 匹配字符串的末尾
. 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符
[...]用来表示一组字符，单独列出：[amk]匹配'a','m'或'k'
[^...]不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。
re* 匹配0个或多个的表达式
re+ 匹配1个或多个的表达式
re? 匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式
re{n}精确匹配n个前面表达式，例如,o{2}不能匹配 "Bob" 中的 "o"，但是能匹配 "food" 中的两个 o。
re{n,}匹配 n 个前面表达式。例如， o{2,} 不能匹配"Bob"中的"o"，但能匹配 "foooood"中的所有 o。"o{1,}" 等价于 "o+"。"o{0,}" 则等价于 "o*"。
re{n,m}匹配n到m次由前面的正则表达式定义的片段，贪婪方式
a|b 匹配a或b
(re) 对正则表达式分组并记住匹配的文本
(?imx)正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。
(?-imx)正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。
(?:re)类似 (...), 但是不表示一个组
(?imx: re)	在括号中使用i, m, 或 x 可选标志
(?-imx: re)	在括号中不使用i, m, 或 x 可选标志
(?#...)	注释.
 (?=re)	前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。
(?!re)	前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功
(?>re)匹配的独立模式，省去回朔
\w 匹配字母数字及下划线
\W 匹配非字母数字及下划线
\s 匹配任意空白字符，等价于 [\t\n\r\f].
\S 匹配任意非空字符
\d 匹配任意数字 <=> [0-9]
\D 匹配任意非数字
\A 匹配字符串开始
\Z 匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。
\z 匹配字符串结束
\G 匹配最后匹配完成的位置。
\b	匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。
\B	匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。
\1...\9	匹配第n个分组的内容。

```

























