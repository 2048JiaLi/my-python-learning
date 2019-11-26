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

___
> Nov. 26, 2019

### 注释
- 当代码修改时，始终优先更新注释

- 注释应该是完整的句子。如果注释是一个短语或句子，它的第一个单词的首字母应该大写，除非它是一个以小写字母开头的标识符（不更改标识符的情况下！）。

- 如果注释很短，末尾可以不加句号。注释块通常由一个或多个段落组成，这些段落由完整的句子组成，并且每个句子都应该以句号结尾。

- 在句尾的句号后边使用两个空格。

- 注释尽量使用英语。  

### 注释块

注释块通常适用于一些（或全部）紧跟其后的代码，并且那些代码应使用相同级别的缩进。注释块的每行以一个#和一个空格开始（除非注释里面的文本有缩进）。

注释块内的段落之间由仅包含#的行隔开。

### 行内注释

- 谨慎地使用行内注释。  

- 行内注释就是注释和代码在同一行，它与代码之间至少用两个空格隔开。并且它以#和一个空格开始。

- 如果行内注释指出的是显而易见，那么它就是不必要的。    
不要这样做：
```
x = x + 1                 # Increment x
```
但有时，这样是有用的：
```
x = x + 1                 # Compensate for border
```

### 文档字符串
[PEP 257](https://www.python.org/dev/peps/pep-0257/)

- 为所有公共模块，函数，类和方法书写文档字符串。

- 对非公开的方法书写文档字符串是没有必要的，但应该写注释描述这个方法是做什么的。
> 这些注释应该写在def行后面。

- PEP 257描述了好的文档字符串约定。最重要的是，多行文档字符串以一行`"""`结束。   
例如：
```
"""Return a foobang

Optional plotz says to frobnicate the bizbaz first.
"""
```
> 对于只有一行的文档字符串，"""同一行上。

## 命名规范

Python库的命名规范有点儿混乱，所以我们不会将他们变得完全一致——不过，这是目前推荐的命名标准。   

新模块和包（包括第三方框架）应该按这些标准书写，但对有不同的风格的已有库，保持内部一致性是首选

#### 根本原则
用户可见的API的公开部分的名称，应该遵循反映用法而不是实现的约定。

#### 命名风格
下面的命名风格是最常见的：

- b（单个小写字母）

- B（单个大写字母）

- 小写字符串

- 带下划线的小写字符串

- 大写字符串

- 带下划线的大写字符串

- 首字母大写的字符串（或CapWords，或驼峰命名法——因其字母看起来高低不平而得名[3]）。这有时也被称为StudlyCaps。
> 注意：当CapWords中使用缩写，大写所有的缩写字母。因此HTTPServerError优于HttpServerError。

- 混用大小写的字符串（与首字母大写字符串不同的是它以小写字母开头）

- 带下划线的首字母大写字符串（**令人厌恶的！**）

还有一种风格，使用简短独特的前缀组织相关的名字在一起。Python中很少这样用，提一下是为了文档的完整性。例如，os.stat()函数返回一个元祖，它的元素名字通常类似st_mode，st_size，st_mtime等等这样。（这样做是为了强调与POSIX系统调用结构体一致，这有助于程序员熟悉这些。）

X11库的所有的公开函数以X开头。Python中，这种风格通常认为是不必要的，因为属性名和函数名以对象名作前缀，而函数名以模块名作前缀。

另外，以下特殊形式，前导或后置下划线是公认的（一般可以与任何约定相结合）：

- 单前导下划线:弱“内部使用”标志。例如`from M import *`不会导入以下划线开头的对象。

- 单后置下划线：按惯例使用避免与Python关键字冲突，例如。
```
Tkinter.Toplevel(master, class_='ClassName')
```
- 双前导下划线：当命名类属性，调用时名称改编（类FooBar中，__boo变成 _FooBar__boo；见下文）。

- 前导和后置都是双下划线：存在于用户控制的命名空间的“神奇”的对象或属性。

> 例如：`__init__`，`__import__`或`__file__`。**不要创造这样的名字；仅像文档中那样使用他们。**

### 规定：命名约定

#### 避免采用的名字

不要使用字符‘l’（小写字母el），‘O’（大写字母oh）或‘I’（大写字母eye）作为单字符变量名。

> 在某些字体中，这些字符与数字1和0是没有区别的。当想使用‘l’时，用‘L’代替。

#### 包名和模块名
模块名应该短，所有的字母小写。可以在模块名中使用下划线来提高可读性。Python包名也应该短，所有的字母小写，不鼓励使用下划线。

当一个C或C++书写的扩展模块，伴随Python模块来提供了一个更高层次（例如更面向对象）的接口时，C/C++模块名有一个前导下划线（如_socket）.

#### 类名
- 类名通常使用首字母大写字符串的规则。

- 函数的命名规则  主要用来可调用的。

- 在接口被记录并且主要用作调用的情况下，用函数的命名规则来代替类名的命名规则。

- 注意内置名有一个单独的规则：大多数的内置名是一个单词（或两个单词一起），首字母大写字符串的规则仅用于异常名和内置常量。

#### 类型变量名称
类型变量名称应该首字母大写，并且尽量短，比如：T, AnyStr, Num。对于协变量和有协变行为的变量，建议添加后缀__co或者__contra。
```
from typing import TypeVar

VT_co = TypeVar('VT_co', covariant=True)
KT_contra = TypeVar('KT_contra', contravariant=True)
```

#### 异常名
因为异常应该是类，所以类的命名规则在这里也同样适用。然而，异常名（如果这个异常确实是一个错误）应该使用后缀“Error”。

#### 全局变量名
（希望这些变量是在一个模块内使用。）这些规则和那些有关函数的规则是相同的。

模块设计为通过from M import *来使用，应使用__all__机制防止导出全局变量，或使用加前缀的旧规则，为全局变量加下划线（可能你像表明这些全局变量是“非公开模块”）。

#### 函数名
函数名应该是小写字母，必要时单词用下划线分开以提高可读性。

混合大小写仅用于这种风格已经占主导地位的上下文（例如threading.py），以保持向后兼容性。

#### 函数和方法参数

- 使用self做实例化方法的第一个参数。

- 使用cls做类方法的第一个参数。

- 如果函数的参数名与保留关键字冲突，最好是为参数名添加一个后置下划线而不是使用缩写或拼写错误。

- 因此class_ 比clss好。（也许使用同义词来避免更好。）。
> 如 beautifulSoup

#### 方法名和实例变量

采用函数命名规则：小写字母，必要时单词用下划线分开以提高可读性。

仅为非公开的方法和实例变量使用一个前导下划线。

为了避免和子类命名冲突，使用两个前导下划线调用Python的名称改编规则。

Python用类名改编这些名字：如果类Foo有一个属性名为__a，通过Foo.__a不能访问。（可以通过调用Foo._Foo__a来访问。）通常，两个前导下划线仅用来避免与子类的属性名冲突。

#### 常量
常量通常定义于模块级别并且所有的字母都是大写，单词用下划线分开。例如MAX_OVERFLOW和TOTAL。

#### 继承的设计
确定类的方法和实例变量（统称为：“属性”）是否公开。如果有疑问，选择非公开；之后把其变成公开比把一个公开属性改成非公开要容易。

#### 程序设计建议

- 编写的代码应该不损害其他方式的Python实现（PyPy，Jython，IronPython，Cython，Psyco等等）。
> 例如，不要依赖CPython的高效实现字符串连接的语句形式 += b或a = a + b。这种优化即使在CPython里也是脆弱的（它只适用于某些类型），并且在不使用引用计数的实现中它完全不存在。在库的性能易受影响的部分，应使用''.join()形式。这将确保跨越不同实现的连接发生在线性时间。

- 与单值比如None比较使用is 或is not ，不要用等号操作符。

- 如果你真正的意思是if x is not None，谨防编写if x。
> 例如，当测试一个默认是None的变量或参数是否被置成其它的值时。这个其它值可能是在布尔上下文为假的类型（例如容器）。

- 使用is not 操作符而不是not...is。虽然这两个表达式的功能相同，前者更具有可读性并且更优。
```
Yes:
if foo is not None:

No:
if not foo is None:
```

- 当实现有丰富的比较的排序操作时，最好实现所有六个操作符（__eq__，__ne__，__lt__，__le__，__gt__，__ge__）而不是依靠其它代码只能进行一个特定的比较。

- 为了减少所涉及的工作量，functools.total_ordering()装饰器提供了一个工具来生成缺失的比较函数。

- PEP 207表明，Python假定自反性规则。因此，编译器可以交换y > x和x < y，y >= x和x <= y，也可以交换参数x == y和x != y。sort()和min()操作保证使用< 操作符并且max()功能使用> 操作符。不管怎样，最好实现所有六个操作，这样在其它上下文就不会产生混淆了。

- 使用def语句而不是使用赋值语句将lambda表达式绑定到标识符上。
```
Yes:
def f(x): return 2*x

No:
f = lambda x: 2*x
```

- Python 3中产生异常，使用raise ValueError('message')替换老的形式raise ValueError,'message'。

- 捕获异常时，尽可能提及特定的异常，而不是使用空的except：子句。
```
try:
    import platform_specific_module
except ImportError:
    platform_specific_module = None
```

- 空的except：子句将捕获SystemExit和KeyboardInterrupt异常，这使得很难用Control-C来中断程序，也会掩饰其它的问题。如果想捕获会导致程序错误的所有异常，使用except Exception:（空异常相当于except BaseException:）

- 对于所有的try/except子句，限制try子句到必须的绝对最少代码量。这避免掩盖错误。
```
Yes:
try:
    value = collection[key]
except KeyError:
    return key_not_found(key)
else:
    return handle_value(value)

No:
try:
    # Too broad!
    return handle_value(collection[key])
except KeyError:
    # Will also catch KeyError raised by handle_value()
    return key_not_found(key)
```

- 返回语句保持一致。函数中的所有返回语句都有返回值，或都没有返回值。如果任意一个返回语句有返回值，那么任意没有返回值的返回语句应该明确指出return None，并且明确返回语句应该放在函数结尾（如果可以）。

```
Yes:
def foo(x):
    if x >= 0:
        return math.sqrt(x)
    else:
        return None

def bar(x):
    if x < 0:
        return None
    return math.sqrt(x)

No:
def foo(x):
    if x >= 0:
        return math.sqrt(x)

def bar(x):
    if x < 0:
        return
    return math.sqrt(x)
```

- 使用字符串方法代替string模块。

- 使用“.startswith() ”和“.endswith()”代替字符串切片来检查前缀和后缀。
> startswith()和endswith()更清晰，并且减少错误率。例如：
```
Yes: if foo.startswith('bar'):
No:  if foo[:3] == 'bar':
```

- 对象类型的比较使用isinstance()代替直接比较类型。
```
Yes: if isinstance(obj, int):

No:  if type(obj) is type(1):
```

- 对于序列（字符串，列表，元组），利用空序列是false的事实。
```
Yes: if not seq:
     if seq:

No:  if len(seq):
     if not len(seq):
```

- 不要用==来将布尔值与True或False进行比较。
```
Yes:   if greeting:
No:    if greeting == True:
Worse: if greeting is True:
```

### 脚注

[5]悬挂缩进是段落中除第一行之外其它所有行都缩进的格式。在Python中，这个术语用来描述左括号括起来的语句，这行的最后非空白的字符，随后行缩进，直到右括号。

### 参考
[1]PEP 7，van Rossum写的C编码风格指南   
[2]Barry的GNU Mailman风格指南   
 http://barry.warsaw.us/software/STYLEGUIDE.txt   
[3]http://www.wikipedia.com/wiki/CamelCase   
[4]PEP 8现代化，2013年7月http://bugs.python.org/issue18472   
