## Google Python Style Guide
> http://google.github.io/styleguide/pyguide.html#381-docstrings

### 1 Background
This style guide is a list of dos and don’ts for Python programs.
> dos and don'ts: 注意事项，行为准则

### 2 Python Language Rules

#### 2.1 Lint
Run `pylint` over your code.

   - 2.1.1 定义（Definition）   

   `pylint`是在Python源代码中查找`bug`和样式问题的工具。它可以发现编译器在处理`C`和`C++`等非动态语言时通常会捕捉到的问题。由于Python的动态特性，一些警告可能是不正确的;然而，虚假的警告应该相当少。

   - 2.1.2 优点（Pros）   

   捕获容易被忽略的错误，如打字错误、在赋值前使用vars等。

   - 2.1.3 缺点（Cons）   

   pylint并不完美。为了利用它，我们有时需要:
      - 围绕它写(Write around it)
      - 抑制它的警告(Suppress its warnings)
      - 改进它(Improve it)

   - 2.1.4 决策（Decision）   
   
   Make sure you run `pylint` on your code.   

   如果警告是不恰当的，就把它们隐藏起来，这样就不会隐藏其他问题。**为了抑制警告，你可以设置一个行级注释**   
   ```py
   dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
   ```   
   pylint警告由符号名(空文档字符串)标识，以**g-开头的特定于google的警告**。

   如果从符号名称中不清楚抑制的原因，则添加解释。

   以这种方式抑制的好处是，我们可以很容易地寻找抑制并重新访问它们。

   你可以得到一个pylint警告列表:
   ```py
   pylint --list-msgs
   ```

   要获取关于特定消息的更多信息，请使用:
   ```py
   pylint --help-msg=C6409
   ```
   
   未使用的参数警告可以通过删除函数开头的变量来抑制。总是包括一个解释你为什么要删除它的评论。“未使用”就足够了。例如:
   ```py
   def viking_cafe_order(spam, beans, eggs=None):
       del beans, eggs  # Unused by vikings.
       return spam + spam + spam
   ```

   其他禁止此警告的常见形式包括使用' _ '作为未使用的参数的标识符、在参数名前面加上' unused_ '或将它们分配给' _ '。这些形式是允许的，但不再鼓励。前两个break调用程序按名称传递参数，而最后一个调用程序并不强制这些参数实际上是未使用的。

#### 2.2 Imports
只对包和模块使用`import`语句，而不是对单独的类或函数.

- 2.2.1 定义
从一个模块到另一个模块共享代码的可重用性机制。

- 2.2.2 Pros
名称空间管理约定很简单。各标识符的来源以一致的方式表示;`x.Obj`表示对象`Obj`在模块`x`中定义。

- 2.2.3 Cons
模块名仍然可能发生冲突。有些模块名称长得很不方便。

- 2.2.4 Decision
   - 使用`import x`导入包和模块。
   - 使用`from x import y`，其中`x`是包前缀，`y`是没有前缀的模块名。
   - 如果要导入两个名为`y`的模块，或者`y`是一个不太方便的长名称，则使用`from x import y as z`。
   - 只有当`z`是标准缩写时才使用`import y as z`(例如，`np`表示`numpy`)。

For example the module sound.effects.echo may be imported as follows:
```py
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```

不要在导入中使用相对名称。即使模块在同一个包中，也要使用完整的包名。这有助于防止无意中两次导入包。

#### 2.3 Packages
使用模块的完整路径名位置导入每个模块。

- 2.3.1 Pros
避免由于模块搜索路径不符合作者的期望而导致模块名称冲突或导入错误。更容易找到模块。

- 2.3.2 Cons
因为您必须复制包层次结构，所以很难部署代码。对于现代的部署机制来说，这并不是一个真正的问题。

- 2.3.3 Decision
所有新代码都应该通过完整的包名导入每个模块。

Imports should be as follows:

Yes:
```py
# Reference absl.flags in code with the complete name (verbose).
import absl.flags
from doctor.who import jodie

FLAGS = absl.flags.FLAGS
```

```py
# Reference flags in code with just the module name (common).
from absl import flags
from doctor.who import jodie

FLAGS = flags.FLAGS
```

No: (assume this file lives in doctor/who/ where jodie.py also exists)
```py
# Unclear what module the author wanted and what will be imported.  The actual
# import behavior depends on external factors controlling sys.path.
# Which possible jodie module did the author intend to import?
import jodie
```
不应该假定主要二进制文件所在的目录在`sys.path`中。尽管在某些环境中会发生这种情况。

#### 2.1 Exceptions
允许有异常，但必须谨慎使用。

- 2.4.1 Definition
异常是一种打破代码块的正常控制流来处理错误或其他异常情况的方法。

- 2.4.2 Pros
正常操作代码的控制流不会被错误处理代码打乱。它还允许控制流在特定条件发生时跳过多个帧，例如，在一个步骤中从N个嵌套函数返回，而不必携带错误代码。

- 2.4.3 Cons
可能会导致控制流混乱。在调用库时很容易忽略错误情况。

- 2.4.4 Decision
Exceptions must follow certain conditions:
   - Raise exceptions like this: `raise MyError('Error message')` or `raise MyError()`. Do not use the two-argument form (`raise MyError`, `'Error message'`).

   - 在需要的时候使用内置的异常类。例如，引发一个ValueError来指出一个编程错误，比如违反了先决条件(例如，如果传递给您一个负数，但需要一个正数)。不要使用断言语句来验证公共API的参数值。assert用于确保内部正确性，而不是强制正确使用，也不是指示发生了意外事件。如果后一种情况需要异常，则使用raise语句。例如:
```py
Yes:
  def connect_to_next_port(self, minimum):
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.

    Returns:
      The new minimum port.

    Raises:
      ConnectionError: If no available port is found.
    """
    if minimum < 1024:
      # Note that this raising of ValueError is not mentioned in the doc
      # string's "Raises:" section because it is not appropriate to
      # guarantee this specific behavioral reaction to API misuse.
      raise ValueError('Minimum port must be at least 1024, not %d.' % (minimum,))
    port = self._find_next_open_port(minimum)
    if not port:
      raise ConnectionError('Could not connect to service on %d or higher.' % (minimum,))
    assert port >= minimum, 'Unexpected port %d when minimum was %d.' % (port, minimum)
    return port
```
```py
No:
  def connect_to_next_port(self, minimum):
    """Connects to the next available port.

    Args:
      minimum: A port value greater or equal to 1024.

    Returns:
      The new minimum port.
    """
    assert minimum >= 1024, 'Minimum port must be at least 1024.'
    port = self._find_next_open_port(minimum)
    assert port is not None
    return port
```

   - 库或包可以定义它们自己的异常。这样做时，它们必须继承现有的异常类。异常名应该以`Error`结尾，should notr introduce stutter(`foo.FooError`)。

   - Never use catch-all except: statements, or catch Exception or StandardError, unless you are
      - 重新引发异常
      - 在程序中创建一个隔离点，不传播异常，而是记录并抑制异常，例如通过保护最外层的块来防止线程崩溃。

      Python在这方面非常宽容，除了:它会捕获所有的异常，包括拼写错误的名称、sys.exit()调用、Ctrl+C中断、单元测试失败和其他各种您不想捕获的异常。

      - 最小化`try/except`块中的代码量。`try`的主体越大，异常由您不希望引发异常的代码行引发的可能性就越大。在这些情况下，`try/except`块隐藏了一个真正的错误。

      - 无论在`try`块中是否引发异常，都可以使用`finally`子句执行代码。这通常对清理很有用，例如关闭文件。

      - 捕获异常时，使用as而不是comma。例如:
      ```py
      try:
          raise Error()
      except Error as error:
          pass
      ```