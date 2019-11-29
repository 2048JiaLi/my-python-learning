## Type Hints for Busy Python Programmers
> https://inventwithpython.com/blog/2019/11/24/type-hints-for-busy-python-programmers/

```python
from typing import List, Tuple, Dict, Set, Frozenset
```

以问答的形式，介绍了`typing`（标准库）与`mypy`（第三方库）写类型提示的用法

例如

```python
class Cat:
    def __init__(self, name: str, color: str, weightkg: float) -> None:
        self.name = name
        self.color = color
        self.weightkg = weightkg

def getZophieClone() -> Cat:
    return Cat('Zophie', 'gray', 4.9)
```

> 这里是自定义了一个类`Cat`