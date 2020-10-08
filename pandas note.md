#                    pandas

## 一、数据类型

### 1. series

[`Series`](https://pandas.pydata.org/docs/reference/api/pandas.Series.html#pandas.Series) is a one-dimensional labeled array capable of holding any data type (integers, strings, floating point numbers, Python objects, etc.)

#### data类型为ndarry的创建

```
In [1]: import numpy as np
In [2]: import pandas as pd

s = pd.Series(data, index=index) 

```

data可以是各种python object: list, tuple, dict, set，及复合结构。

```python
s = pd.Series(np.random.randn(5), index=['a', 'b', 'c', 'd', 'e'])
```

The passed **index** is a list of axis labels.If `data` is an ndarray, **index** must be the same length as **data**.

```
s2=pd.Series([(1, 2, 3), (4, 5, 6)], index=['x', 'y'])
```

如果没传入index,会默认使用从0开始的序号：

```
s1=pd.Series((1, 3, 5, 7, 9))
```

#### data类型为dict的创建

When the data is a dict, and an index is not passed, the `Series` index will be ordered by the dict’s insertion order, if you’re using Python version >= 3.6 and Pandas version >= 0.23.

If you’re using Python < 3.6 or Pandas < 0.23, and an index is not passed, the `Series` index will be the lexically ordered list of dict keys.

```
d={'b':2.1, 'a':1.0, 'c':3.3}
pd.Series(d)
```

#### data类型为数值的创建

If `data` is a scalar value, an index must be provided. The value will be repeated to match the length of **index**.

```
s3=pd.Series(5., index=['h', 'i', 'j'])
```

#### 可以对series进行切片等操作

```
s[0]
s[:3]

s[s > s.median()]       # 选取大于中位数的所有对象

s[[4, 3, 1]]            # 切取地址为4，3，1的对象，和label无关

np.exp(s)
```

#### 类型转换

1. 转array: Series.array

```
s.array
```

2. 转ndarray: [`Series.to_numpy()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.to_numpy.html#pandas.Series.to_numpy)

```
s.to_numpy()
```

#### 对series内部对象的操作类似字典

1. 查询

```
s['a']
```

2. 修改

```
s['a']=3.5
```

3. 判断对象是否包含, 返回布尔值

```
'f' in s
```

4. get方法

```
s['f']                 # label不存在抛出异常
s.get('f', np.nan)         # 查找key,找不到返回第二个参数
```

#### Vectorized operations 向量化运算

```
s + s
s*2
```

如果相加的两个series长度不一样，一方缺省的label结果为nan。注意是找相同的label计算，不是相同位置的对象之间计算。

#### name attribute

```
s.name='series0'
s5=pd.Series(np.random.randn(3), name='series5')
```

```
s.rename('different')
```

### 2. dataframe

#### 可接受的数据类型

- Dict of 1D ndarrays, lists, dicts, or Series
- 2-D numpy.ndarray
- [Structured or record](https://numpy.org/doc/stable/user/basics.rec.html) ndarray
- A `Series`
- Another `DataFrame`

When the data is a dict, and `columns` is not specified, the `DataFrame` columns will be ordered by the dict’s insertion order, if you are using Python version >= 3.6 and Pandas >= 0.23.

If you are using Python < 3.6 or Pandas < 0.23, and `columns` is not specified, the `DataFrame` columns will be the lexically ordered list of dict keys.

```
d = {'one': pd.Series([1., 2., 3.], index=['a', 'b', 'c']),
     'two': pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])}
df = pd.DataFrame(d)
   one  two
a  1.0  1.0
b  2.0  2.0
c  3.0  3.0
d  NaN  4.0

dic={'col1': [1, 2, 3, 4, 5], 'col2': [6, 7, 8, 9, 10]}
df2 = pd.DataFrame(dic)
   col1  col2
0     1     6
1     2     7
2     3     8
3     4     9
4     5    10

```

```
pd.DataFrame(d, index=['d', 'b', 'a'], columns=['two', 'three'])
   two three
d  4.0   NaN
b  2.0   NaN
a  1.0   NaN
```

The row and column labels can be accessed respectively by accessing the **index** and **columns** attributes:

```
df.index
df.columns
```

The ndarrays must all be the same length. If an index is passed, it must clearly also be the same length as the arrays. If no index is passed, the result will be `range(n)`, where `n` is the array length.

```
data2 = [{'a': 1, 'b': 2}, {'a': 5, 'b': 10, 'c': 20}]
pd.DataFrame(data2)
```

