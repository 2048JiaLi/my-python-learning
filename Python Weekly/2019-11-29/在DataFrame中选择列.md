## Tips for Selecting Columns in a DataFrame
> https://pbpython.com/selecting-columns.html

### Introduction

本文讨论使用`iloc`处理具有大量列的数据集的几个技巧和快捷方式，加速分析并避免在代码中键入大量的列名。
> `iloc`函数与`loc`函数都来自于Pandas。
>
> `loc`函数：通过行索引 "Index" 中的具体值来取行数据（如取"Index"为"A"的行）
>
> `iloc`函数：通过行号来取行数据（如取第二行的数据）
> 
> [loc、iloc常见的五种用法](https://blog.csdn.net/w_weiying/article/details/81411257)

### Why Do We Care About Selecting Columns?

在许多标准数据科学示例中，列的数量相对较少。例如，Titanic有8个，Iris有4个，Boston Housing有14个。Real-life data sets很混乱，常常包含许多额外的(可能不必要的)列。

可能因为以下原因，而需要选择部分列：
- 将数据过滤为只包含相关列有助于缩小内存占用并加快数据处理速度。

- 限制列的数量可以减少在头脑中保留数据模型的脑力开销。

- 在研究新数据集时，可能需要将任务分解成可管理的块。

- 在某些情况下，您可能需要遍历列并执行计算或清理，以获得需要用于进一步分析的格式的数据。

- 您的数据可能只包含不需要的额外或重复信息。

不管什么原因，您可能并不总是需要这些技术。但是，当您这样做时，下面列出的技巧可以减少您在数据列上处理的时间。

> 另外，如果你喜欢这种类型的内容，我鼓励你去看看[Kevin Markham’s pandas tricks](https://www.dataschool.io/python-pandas-tips-and-tricks/)，它是下面一些技巧的灵感来源。

### The Data
为了举例说明，我将使用来自中央公园松鼠普查的一个数据集。
> 3023行，31列。

读数据
```
import pandas as pd
import numpy as np

df = pd.read_csv(
    'https://data.cityofnewyork.us/api/views/vfnx-vebw/rows.csv?accessType=DOWNLOAD&bom=true&format=true'
)
```

有时候，根据索引来记住每个列名及其所在的位置是很困难的。下面是一个简单的列表理解，用于构建包含所有列及其索引的引用列表。
```
col_mapping = [f"{c[0]}:{c[1]}" for c in enumerate(df.columns)]
```
结果
```
['0:X',
'1:Y',
'2:Unique Squirrel ID',
'3:Hectare',
'4:Shift',
'5:Date',
 ...
'33:Borough Boundaries',
'34:City Council Districts',
'35:Police Precincts']
```

在某些情况下，如果你想重命名一堆列，你可以使用字典来创建数据的字典视图:
```
col_mapping_dict = {c[0]:c[1] for c in enumerate(df.columns)}
```
结果
```
{0: 'X',
1: 'Y',
2: 'Unique Squirrel ID',
3: 'Hectare',
4: 'Shift',
5: 'Date',
...
33: 'Borough Boundaries',
34: 'City Council Districts',
35: 'Police Precincts'}
```
> 在分析过程中，定义这些变量可能很有用。您可以在分析过程中再次检查变量名，而不是重复查看原始文件。

另一个常见任务是**重命名一组跨文件命名不一致的列**。可以使用字典来轻松地重命名所有的列，使用类似`df.rename(columns=col_mapping)`的方法输入所有的列名可能是一个容易出错的任务。

一个简单的技巧是复制excel中的所有列，并使用`pd.read_clipboard()`来构建一个小的`DataFrame`，并将这些列转换成一个字典。如果需要，也可以手动输入新名称。

下面是一个快速示例
```
df_cols = pd.read_clipboard()
col_mapping = {c[1]:'' for c in enumerate(df_cols.columns)}
```

它创建了一个相对容易填充新名称的字典:
```
{'X': '',
'Y': '',
'Unique': '',
'Squirrel': '',
'ID': '',
'Hectare': '',
'Shift': '',
...
'Police': '',
'Precincts': ''}
```


### Using iloc
panda的`iloc`是用于基于整数位置的索引。
新用户可能会有点困惑，因为`iloc`和`loc`可以使用一个布尔数组，这将导致更强大的索引。由于这两个函数都可以使用一个布尔数组作为输入，所以有时这些函数会产生相同的输出。然而，本文只关注`iloc`列的选择。

这里有一个简单的图形来说明`iloc`的主要用法

![](https://pbpython.com/images/iloc.png)

例如，如果您想查看所有行的数据的Unique Squirrel ID列:
```
df.iloc[:, 2]
```

```
0       37F-PM-1014-03
1       37E-PM-1006-03
2        2E-AM-1010-03
3        5D-PM-1018-05
4       39B-AM-1018-01
             ...
3018    30B-AM-1007-04
3019    19A-PM-1013-05
3020    22D-PM-1012-07
3021    29B-PM-1010-02
3022     5E-PM-1012-01
Name: Unique Squirrel ID, Length: 3023, dtype: object
```

如果想查看X和Y的位置以及ID，你可以传入一个整数列表`[0,1,2]`:
```
df.iloc[:, [0,1,2]]

# 或者
df.iloc[:, 0:3]
```
```
X	Y	Unique Squirrel ID
0	-73.956134	40.794082	37F-PM-1014-03
1	-73.957044	40.794851	37E-PM-1006-03
2	-73.976831	40.766718	2E-AM-1010-03
3	-73.975725	40.769703	5D-PM-1018-05
4	-73.959313	40.797533	39B-AM-1018-01
...	...	...	...

3019	-73.970402	40.782560	19A-PM-1013-05
3020	-73.966587	40.783678	22D-PM-1012-07
3021	-73.963994	40.789915	29B-PM-1010-02
3022	-73.975479	40.769640	5E-PM-1012-01
3023 rows × 3 columns
```

`r_`会 Translate slice objects to concatenation along the first axis

下面是一个稍微复杂一点的例子，展示了如何在单个列表项和切片范围的组合上工作:
```
np.r_[0:3,15:19,24,25]

>>> array([ 0,  1,  2, 15, 16, 17, 18, 24, 25])
```

与`iloc`结合
```
df.iloc[:, np.r_[0:3,15:19,24,25]]
```
```
X	Y	Unique Squirrel ID	Running	Chasing	Climbing	Eating	Tail flags	Tail twitches
0	-73.956134	40.794082	37F-PM-1014-03	False	False	False	False	False	False
1	-73.957044	40.794851	37E-PM-1006-03	True	False	False	False	False	False
2	-73.976831	40.766718	2E-AM-1010-03	False	False	True	False	False	False
3	-73.975725	40.769703	5D-PM-1018-05	False	False	True	False	False	False
...	...	...	...	...	...	...	...	...	...
3020	-73.966587	40.783678	22D-PM-1012-07	False	False	False	True	False	False
3021	-73.963994	40.789915	29B-PM-1010-02	False	False	False	True	False	False
3022	-73.975479	40.769640	5E-PM-1012-01	False	False	False	True	False	False
3023 rows × 9 columns
```

在使用`read_csv`时也可以应用该技巧：
```
df_2 = pd.read_csv(
    'https://data.cityofnewyork.us/api/views/vfnx-vebw/rows.csv?accessType=DOWNLOAD&bom=true&format=true',
    usecols=np.r_[1,2,5:8,15:30],
)
```

关于`np.r_`的最后一点意见是有一个可选的步骤参数。在这个例子中，我们可以指定这个列表将增加2:
```
np.r_[2:10:2]

>>> array([2, 4, 6, 8])
```

### iloc and boolean arrays

筛选列的最强大的方法之一是向`iloc`传递一个布尔数组来选择列的子集。这听起来有点复杂，但是通过几个例子应该可以理解。

最重要的概念是，我们不是手工生成布尔数组，而是使用另一个panda函数的输出来生成数组并将其提供给`iloc`。

在本例中，我们可以像使用panda数据的任何其他列一样对列索引使用`str`访问器。这将生成`iloc`所需的布尔数组。

如果我们想看看哪些列包含单词“run”:
```
run_cols = df.columns.str.contains('run', case=False)
print(run_cols)
```

```
array([False, False, False, False, False, False, False, False, False,
    False, False, False, False, False, False,  True, False, False,
    False, False, False, False, False, False, False, False, False,
    False,  True, False, False, False, False, False, False, False])
```
然后`df.iloc[:, run_cols].head()`   

```
Running	Runs from
0	False	False
1	True	True
2	False	False
3	False	True
4	False	False
```

在实际操作中，很多人会使用`lambda`:
```
df.iloc[:, lambda df:df.columns.str.contains('run', case=False)]
```

使用`str`函数的好处是，可以对潜在的过滤器选项进行完善。例如，如果我们希望名称中包含“district”、“district”或“boundaries”的所有列:
```
df.iloc[:, lambda df: df.columns.str.contains('district|precinct|boundaries',
                                              case=False)].head()
```

```
	Community Districts	Borough Boundaries	City Council Districts	Police Precincts
0	19	4	19	13
1	19	4	19	13
2	19	4	19	13
3	19	4	19	13
4	19	4	19	13
```

### Caveats

在处理列的数字索引时需要记住的一点是，您需要了解数据的来源。如果您希望您的ID列总是位于特定的位置，并且它更改了数据中的顺序，那么在后续的数据处理中可能会遇到问题。这种情况下，您的领域知识和专业知识将发挥作用，以确保解决方案对于给定的情况足够健壮。

### Summary

我的大多数数据分析都涉及到过滤和选择行级别的数据。然而，有时以列方式处理数据是有帮助的。对于快速有效地处理具有许多列数据的数据集，Pandas `iloc`是一个有用的工具。