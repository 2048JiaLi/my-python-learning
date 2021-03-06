## 什么样的函数是一个好函数？

- [命名合理](#一、命名)

- [单一功能](#二、单一功能)

- [包括文档字符串](#三、文档字符串（Docstrings）)

- 返回一个值

- 不超过50行

- 是幂等函数或纯函数

### 一、命名
> 计算机科学中只有两件事很让人头疼：缓存失效和命名。

下面是一个反面案例：
```py
def get_knn(from_df):
```

>> 这个函数命名的第一个问题是它使用了缩写。

对于那些并不出名的缩略词来说，使用完整的英语单词会更好。

缩写通常是特定领域的。在上面的代码中，KNN指的是“K-Nearest Neighbors”，df指的是“DataFrame”，这是一个数据结构。如果另一个不熟悉这些首字母缩写的程序员正在阅读代码，几乎很难看懂。

关于这个函数的名字还有另外两个小瑕疵：

>> “get”这个词是无关紧要的。对于大多数命名比较好的函数来说，很明显有一些东西会从函数中返回，它的名字将反映这一点。

>> from_df也不是必要的。如果没有明确的参数名称，函数的文档字符串或类型注释会描述参数的类型。

那么我们如何重命名这个函数呢？很简单：
```py
def k_nearest_neighbors(dataframe):
```

即使是外行，这个函数要计算的内容也很清楚，参数的名称(dataframe)也清楚地表明了参数类型。

### 二、单一功能
单一功能原则不仅适用于类和模块，也同样适用于函数。

一个函数应该只有一个功能。也就是说，它应该只做一件事。

一个重要的原因是，如果每个函数只做一件事，只有这件事发生了变化，才需要改变这个函数。此外，如果这个函数的单个功能不再需要了，直接把它删了就行了。

例如，下面这个函数，可以做不止一件“事情”:
```py
def calculate_and print_stats(list_of_numbers):
    sum = sum(list_of_numbers) 
    mean = statistics.mean(list_of_numbers) 
    median = statistics.median(list_of_numbers) 
    mode = statistics.mode(list_of_numbers) 
    print('-----------------Stats-----------------') 
    print('SUM: {}'.format(sum) print('MEAN: {}'.format(mean)
    print('MEDIAN: {}'.format(median) 
    print('MODE: {}'.format(mode)
```

这个函数做了两件事：一是计算一组关于数字列表的统计数据，二是将它们打印。

如果需要计算新的或不同的统计数据，或者需要改变输出的格式，就需要对这个函数进行调整。

所以，这个函数最好写成两个独立的函数：一个用来执行并返回计算结果，另一个用来获取这些结果并打印出来。

这种处理方式，不仅能让测试函数更容易，并且还允许这两个部分有了迁移性，如果合适的话，还可能一起应用到不同的模块中。

在编程中，你会发现好多函数都可以做很多很多事情。同样，为了可读性和可测试性，这些函数应该被分解成更小的函数，每个函数只有一个功能。

### 三、文档字符串（Docstrings）
PEP - 257是关于文档字符串介绍的。

其中的关键内容是：

- 每个函数都需要有一个文档字符串

- 使用适当的语法和标点符号；用完整的句子写

- 首先对函数的作用进行一句话的总结

- 使用说明性语言而不是描述性语言

在编写函数时，要养成写文档字符串的习惯，并在编写函数代码之前尝试写一下。

如果你不能写一个清晰的文档字符串来描述函数做什么，就说明你需要再考虑考虑为什么要写这个函数了。

### 四、返回值
函数可以被认为是一些独立的程序。它们以参数的形式接受一些输入，并返回一些结果。

参数有没有都可以，但从Python内部的角度来看，返回值是必须要有的。你不可能创建一个没有返回值的函数。如果函数没有返回值，Python会“强制”返回None。你可以测试一下这段代码：
```py
>>> def add(a, b):
...   print(a + b)
...
>>> b = add(1, 2)
3
>>> b
>>> b is None
True
```

你会发现 b 的返回值实际上是 None。即使你写的函数没有返回语句，它仍然会返回一些东西。而且，每个函数都应该返回一个有用的值，测试起来也会更方便。毕竟，你写的代码应该能够被测试。试想一下，测试上面的add函数会有多艰难。

### 五、函数长度
函数的长度直接影响可读性，从而影响可维护性。所以要保持你的函数简短。50行是一个随意的数字，在我看来是合理的。你编写的大多数函数应该要短一些。

如果一个函数遵循单一功能原则，它很可能是相当短的。如果它是纯函数或是幂等的(下面讨论) ，它也可能是短的。

那么，如果函数太长，应该怎么做？重构。这会改变程序的结构而不改变其行为。

从一个长函数中提取几行代码，并把它们变成自己的函数。这是缩短长函数的最快、也是最常见的方式。

加上你给所有这些新函数取了合适的名称，因此生成的代码读起来也会更容易。

### 六、幂等和函数纯度
不管被调用了多少次，幂等函数总是在给定相同参数集的情况下返回相同的值。

结果不依赖于非局部变量、参数的可变性或来自任何I / O流的数据。下面的这个add_three(number)函数是幂等函数
```py
def add_three(number):
    """Return *number* + 3."""
    return number + 3
```

不管一个人调用`add_three(7)`多少次，答案总是10。以下是一个非幂等函数：
```py

def add_three():
    """Return 3 + the number entered by the user."""
    number = int(input('Enter a number: '))
    return number + 3
```
这个函数的返回值取决于I / O，即用户输入的数字。对add_three()的每次调用都会返回不同的值。

幂等性的一个现实中例子是在电梯前点击“向上”按钮。第一次按时，电梯会被“通知”你要上去。因为按按钮是幂等的，所以反复按它都没有什么影响。结果是一样的。

**为什么幂等很重要？**
幂等函数很容易测试，因为在使用相同的参数时，它们总是返回相同的结果。

测试仅仅是检查通过不同调用返回值的预期值。更重要的是，这些测试很快，这是单元测试中一个重要且经常被忽视的问题。

而在处理幂等函数时，重构是轻而易举的事情。无论如何在函数之外更改代码，使用相同的参数调用它的结果总是一样的。

**什么是纯函数？**
在函数编程中，如果一个函数既幂等又没有可观察到的副作用，它就被认为是**纯函数**。函数外部的任何东西都不会影响这个值。