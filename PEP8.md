## 代码布局
### 缩进
每级缩进使用4个空格

Yes :
```
# 与分界符对齐
foo = long_function_name(var_one, var_two,
                         var_three, var_four)


# Add 4 spaces (an extra level of indentation) to distinguish arguments from the rest.
# 增加4个空格（或更多）以区别于其他
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
# 悬挂缩进应增加一个级别
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

if语句条件块足够长时需要编写多行。可以接受的选项包括，但不仅限于：
```
# No extra indentation.
# 没有额外缩进
if (this_is_one_thing and
    that_is_another_thing):
    do_something()

# Add a comment, which will provide some distinction in editors
# supporting syntax highlighting.
# 添加一行注释 ，这将为编辑器支持语法高良提供区分
if (this_is_one_thing and
    that_is_another_thing):
    # Since both conditions are true, we can frobnicate.
    do_something()

# Add some extra indentation on the conditional continuation line.
# 在条件接续行，增加额外的缩进
if (this_is_one_thing
        and that_is_another_thing):
    do_something()
```

多行结构中的结束花括号/中括号/圆括号是最后一行的第一个非空白字符，如：
```
my_list = [
    1, 2, 3,
    4, 5, 6,
    ]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
    )
```
或者是最后一行的第一个字符，如：
```
my_list = [
    1, 2, 3,
    4, 5, 6,
]
result = some_function_that_takes_arguments(
    'a', 'b', 'c',
    'd', 'e', 'f',
)
```

### 制表符还是空格？
空格是缩进方法的首选。

### 行的最大长度
Python标准库采取保守做法，要求行限制到79个字符（文档字符串/注释到72个字符）。

下垂的长块结构限制为更少的文本（文档字符串或注释），行的长度应该限制在72个字符。

折叠长行的首选方法是在小括号，中括号，大括号中使用Python隐式换行。长行可以在表达式外面使用小括号来变成多行。连续行使用反斜杠更好。

反斜杠有时可能仍然是合适的。例如，长的多行的with语句不能用隐式续行，可以用反斜杠：
```
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

### 换行应该在二元操作符的前面还是后面？

Yes : 
```
# Yes: easy to match operators with operands
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)
          - ira_deduction
          - student_loan_interest)
```

### 空行
顶级函数和类的定义之间有两行空行。

类内部的函数定义之间有一行空行。

额外的空行用来（谨慎地）分离相关的功能组。相关的行（例如：一组虚拟实现）之间不使用空行。


### 源文件编码

Python核心发布中的代码应该始终使用UTF-8（或Python2中用ASCII）。
文件使用ASCII（Python2中）或UTF-8（Python3中）不应有编码声明。

在标准库中，非默认编码仅用于测试目的或注释或文档字符串需要提及包含非ASCII字符的作者名；否则，使用\x，\u，\U，或\N是字符串中包含非ASCII数据的首先方式。

Python3.0及以上版本，为标准库（参见PEP 3131）规定以下策略：Python标准库中的所有标识符必须使用ASCII标识符，在可行的地方使用英文单词（在很多例子中，使用非英文的缩写和专业术语）。另外，字符串和注释必须用ASCII。仅有的例外是（a）测试非ASCII的特点，（b）测试作者名。不是基于拉丁字母表的作者名必须提供一个他们名字的拉丁字母表的音译。

### 导入
导入通常是单独一行，例如：

```
Yes: import os
     import sys
     from subprocess import Popen, PIPE


No:  import sys, os
```

导入常常位于文件顶部，在模块注释和字符串文档之后，在模块的全局变量和常量之前。

导入应该按照以下顺序分组：
1. 标准库导入
2. 相关的第三方导入
3. 特定的本地应用/库导入
在每个导入组之间放一行空行。

把任何相关__all__规范放在导入之后。

推荐绝对导入，因为它们更易读，并且如果导入系统配置的不正确（例如当包中的一个目录结束于sys.path）它们有更好的表现（至少给出更好的错误信息）：
```
import mypkg.sibling
from mypkg import sibling
from mypkg.sibling import example
```

从一个包含类的模块中导入类时，通常下面这样是好的写法：
```
from myclass import MyClass
from foo.bar.yourclass import YourClass

# 如果这种写法导致本地名字冲突
import myclass
import foo.bar.yourclass
# 使用“myclass.MyClass”和“foo.bar.yourclass.YourClass”来访问。
```

避免使用通配符导入（from <模块名> import *）

### 模块级别的内置属性
模块级别的内置属性（名字有前后双下划线的），例如__all__, __author__, __version__，应该放置在模块的文档字符串后，任意import语句之前，from __future__导入除外。
> Python强制要求from __future__导入必须在任何代码之前，只能在模块级文档字符串之后。

```
"""This is the example module.

This module does stuff.
"""

from __future__ import barry_as_FLUFL

__all__ = ['a', 'b', 'c']
__version__ = '0.1'
__author__ = 'Cardinal Biggles'

import os
import sys
```

### 字符串引号
Python中，单引号字符串和双引号字符串是一样的。本PEP不建议如此。建议选择一条规则并坚持下去。当一个字符串包含单引号字符或双引号字符时，使用另一种字符串引号来避免字符串中使用反斜杠。这提高可读性。

三引号字符串，与PEP 257 文档字符串规范一致总是使用双引号字符。

### 表达式和语句中的空格
以下情况避免使用多余的空格：
- 紧挨着小括号，中括号或大括号。
```
Yes: spam(ham[1], {eggs: 2})
No:  spam( ham[ 1 ], { eggs: 2 } )
```
- 紧挨在逗号，分号或冒号前：
```
Yes: if x == 4: print x, y; x, y = y, x
No:  if x == 4 : print x , y ; x , y = y , x
```
- 在逗号和紧接的圆括号之间。
```
Yes: foo = (0,)
No:  bar = (0, )
```
- 切片
在切片中冒号像一个二元操作符，冒号两侧的有相等数量空格

Yes 
```
ham[1:9], ham[1:9:3], ham[:9:3], ham[1::3], ham[1:9:]
ham[lower:upper], ham[lower:upper:], ham[lower::step]
ham[lower+offset : upper+offset]
ham[: upper_fn(x) : step_fn(x)], ham[:: step_fn(x)]
ham[lower + offset : upper + offset]
```

No
```
ham[lower + offset:upper + offset]
ham[1: 9], ham[1 :9], ham[1:9 :3]
ham[lower : : upper]
ham[ : upper]
```

- 紧挨着索引或切片开始的左括号之前：
```
Yes: dct['key'] = lst[index]
No:  dct ['key'] = lst [index]
```

- 为了与另外的赋值（或其它）操作符对齐，不止一个空格。
Yes :
```
x = 1
y = 2
long_variable = 3
```

No :
```
x             = 1
y             = 2
long_variable = 3
```

- 其他
始终避免行尾空白。因为它们通常不可见，容易导致困惑：如果\后面跟了一个空格，它就不是一个有效的续行符了。
> 很多编辑器不保存行尾空白，CPython项目中也设置了commit前检查以拒绝行尾空白的存在。

始终在这些二元操作符的两边放置一个空格：赋值（= ），增强赋值（+= ，-= 等），比较（== ， < ， > ， != ， <> ， <= ， >= ，in ， not in ，is ，is not ），布尔（and ，or ，not ）。

如果使用了不同优先级的操作符，在低优先级操作符周围增加空格（一个或多个）。不要使用多于一个空格，二元运算符两侧空格数量相等。
Yes :
```
i = i + 1
submitted += 1
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

No :
```
i=i+1
submitted +=1
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)
```

当=符号用于指示关键字参数或默认参数值时，它周围不要使用空格。
Yes :
```
def complex(real, imag=0.0):
    return magic(r=real, i=imag)
```

No :
```
def complex(real, imag = 0.0):
    return magic(r = real, i = imag)
```

带注解的函数使用正常的冒号规则，并且在->两侧增加一个空格：
Yes :
```
def munge(input: AnyStr): ...
def munge() -> PosInt: ...
```

No :
```
def munge(input:AnyStr): ...
def munge()->PosInt: ...
```

如果参数既有注释又有默认值，在等号两边增加一个空格（仅在既有注释又有默认值时才加这个空格）。
Yes :
```
def munge(sep: AnyStr = None): ...
def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
```

No :
```
def munge(input: AnyStr=None): ...
def munge(input: AnyStr, limit = 1000): ...
```

不鼓励使用复合语句（同一行有多条语句）。
Yes:
```
if foo == 'blah':
    do_blah_thing()
do_one()
do_two()
do_three()
```

Rather not:
```
if foo == 'blah': do_blah_thing()
do_one(); do_two(); do_three()
```

尽管有时if/for/while的同一行跟一小段代码，在一个多条子句的语句中不要如此。避免折叠长行！
Rather not:
```
if foo == 'blah': do_blah_thing()
for x in lst: total += x
while t < 10: t = delay()
```

Definitely not:
```
if foo == 'blah': do_blah_thing()
else: do_non_blah_thing()

try: something()
finally: cleanup()

do_one(); do_two(); do_three(long, argument,
                             list, like, this)

if foo == 'blah': one(); two(); three()
```

### 什么时候使用尾部逗号？
尾部逗号通常都是可选的，除了一些强制的场景，比如元组在只有一个元素的时候需要一个尾部逗号。
Yes:
```
FILES = ('setup.cfg',)
```

OK, but confusing:
```
FILES = 'setup.cfg',
```

当列表元素、参数、导入项未来可能不断增加时，留一个尾部逗号是一个很好的选择。通常的用法是（比如列表）每个元素独占一行，然后尾部都有逗号，在最后一个元素的下一行写闭标签。如果你的数据结构都是写在同一行的，就没有必要保留尾部逗号了。

Yes:
```
FILES = [
    'setup.cfg',
    'tox.ini',
    ]
initialize(FILES,
           error=True,
           )
```

No:
```
FILES = ['setup.cfg', 'tox.ini',]
initialize(FILES, error=True,)
```