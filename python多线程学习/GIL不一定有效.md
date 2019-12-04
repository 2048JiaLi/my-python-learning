## python由于GIL的存在，多线程并不一定有效

python 的多线程 `threading` 有时候并不是特别理想. 最主要的原因是就是, Python 的设计上, 有一个必要的环节, 就是 `Global Interpreter Lock (GIL)`. 这个东西让 Python 还是一次性只能处理一个东西.

[GIL介绍](https://github.com/2048JiaLi/PY3_privacy/blob/master/Python%E9%9D%A2%E8%AF%95/%E9%A2%98%E7%9B%AE%E4%B8%8E%E7%AD%94%E6%A1%88/python%E4%B8%AD%E7%9A%84GIL.md)

> PY3_privacy/Python面试/题目与答案/python中的GIL.md