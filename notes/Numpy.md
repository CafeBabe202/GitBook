# NumPy

> NumPy(Numerical Python) 是 Python 语言的一个扩展程序库，支持大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。
>
> NumPy 的前身 Numeric 最早是由 Jim Hugunin 与其它协作者共同开发，2005 年，Travis Oliphant 在 Numeric 中结合了另一个同性质的程序库 Numarray 的特色，并加入了其它扩展而开发了 NumPy。NumPy 为开放源代码并且由许多协作者共同维护开发。
>
> NumPy 是一个运行速度非常快的数学库，主要用于数组计算，包含：
>
> - 一个强大的N维数组对象 ndarray
> - 广播功能函数
> - 整合 C/C++/Fortran 代码的工具
> - 线性代数、傅里叶变换、随机数生成等功能

## NumPy 数据类型

| 名称       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| bool_      | 布尔型数据类型（True 或者 False）                            |
| int_       | 默认的整数类型（类似于 C 语言中的 long，int32 或 int64）     |
| intc       | 与 C 的 int 类型一样，一般是 int32 或 int 64                 |
| intp       | 用于索引的整数类型（类似于 C 的 ssize_t，一般情况下仍然是 int32 或 int64） |
| int8       | 字节（-128 to 127）                                          |
| int16      | 整数（-32768 to 32767）                                      |
| int32      | 整数（-2147483648 to 2147483647）                            |
| int64      | 整数（-9223372036854775808 to 9223372036854775807）          |
| uint8      | 无符号整数（0 to 255）                                       |
| uint16     | 无符号整数（0 to 65535）                                     |
| uint32     | 无符号整数（0 to 4294967295）                                |
| uint64     | 无符号整数（0 to 18446744073709551615）                      |
| float_     | float64 类型的简写                                           |
| float16    | 半精度浮点数，包括：1 个符号位，5 个指数位，10 个尾数位      |
| float32    | 单精度浮点数，包括：1 个符号位，8 个指数位，23 个尾数位      |
| float64    | 双精度浮点数，包括：1 个符号位，11 个指数位，52 个尾数位     |
| complex_   | complex128 类型的简写，即 128 位复数                         |
| complex64  | 复数，表示双 32 位浮点数（实数部分和虚数部分）               |
| complex128 | 复数，表示双 64 位浮点数（实数部分和虚数部分）               |

### 数据类型对象

数据类型对象（numpy.dtype 类的实例）用来描述与数组对应的内存区域是如何使用，它描述了数据的以下几个方面：：

- 数据的类型（整数，浮点数或者 Python 对象）
- 数据的大小（例如， 整数使用多少个字节存储）
- 数据的字节顺序（小端法或大端法）
- 在结构化类型的情况下，字段的名称、每个字段的数据类型和每个字段所取的内存块的部分
- 如果数据类型是子数组，那么它的形状和数据类型是什么。

字节顺序是通过对数据类型预先设定 **<** 或 **>** 来决定的。 **<** 意味着小端法(最小值存储在最小的地址，即低位组放在最前面)。**>** 意味着大端法(最重要的字节存储在最小的地址，即高位组放在最前面)。

dtype 对象是使用以下语法构造的：

```python
# object - 要转换为的数据类型对象
# align - 如果为 true，填充字段使其类似 C 的结构体。
# copy - 复制 dtype 对象 ，如果为 false，则是对内置数据类型对象的引用
numpy.dtype(object, align, copy)
```

### 自定义 dtype

```python
# 结构化数据
dt = np.dtype([('age', np.int8)])
print(dt)

# 将数据类型应用于 ndarray 对象
dt = np.dtype([('age', np.int8)])
a = np.array([(10,), (20,), (30,)], dtype=dt)

# 输出各个元素的各个“属性”
dt = np.dtype([('age', np.int8), ('a', np.int8)])
a = np.array([(1, 2), (3, 4), (5, 6)], dtype=dt)
print(a)
print(a['age'])
print(a['a'])
```

## 数组属性

NumPy 数组的维数称为秩（rank），秩就是轴的数量，即数组的维度，一维数组的秩为 1，二维数组的秩为 2，以此类推。

在 NumPy中，每一个线性的数组称为是一个轴（axis），也就是维度（dimensions）。比如说，二维数组相当于是两个一维数组，其中第一个一维数组中每个元素又是一个一维数组。所以一维数组就是 NumPy 中的轴（axis），第一个轴相当于是底层数组，第二个轴是底层数组里的数组。而轴的数量——秩，就是数组的维数。

很多时候可以声明 axis。axis=0，表示沿着第 0 轴进行操作，即对每一列进行操作；axis=1，表示沿着第1轴进行操作，即对每一行进行操作。

NumPy 的数组中比较重要 ndarray 对象属性有：

| 属性             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| ndarray.ndim     | 秩，即轴的数量或维度的数量                                   |
| ndarray.shape    | 数组的维度，对于矩阵，n 行 m 列                              |
| ndarray.size     | 数组元素的总个数，相当于 .shape 中 n*m 的值                  |
| ndarray.dtype    | ndarray 对象的元素类型                                       |
| ndarray.itemsize | ndarray 对象中每个元素的大小，以字节为单位                   |
| ndarray.flags    | ndarray 对象的内存信息                                       |
| ndarray.real     | ndarray元素的实部                                            |
| ndarray.imag     | ndarray 元素的虚部                                           |
| ndarray.data     | 包含实际数组元素的缓冲区，由于一般通过数组的索引获取元素，所以通常不需要使用这个属性。 |

```python
# 生成一个维度
a = np.arange(24)
print(a.ndim)  # 获得维度的秩
b = a.reshape(4, 6)  # 重新调整维度
print(b)

# 获得每个元素的大小
print(a.itemsize)
# C_CONTIGUOUS (C)	数据是在一个单一的C风格的连续段中
# F_CONTIGUOUS (F)	数据是在一个单一的Fortran风格的连续段中
# OWNDATA (O)	数组拥有它所使用的内存或从另一个对象中借用它
# WRITEABLE (W)	数据区域可以被写入，将该值设置为 False，则数据为只读
# ALIGNED (A)	数据和所有元素都适当地对齐到硬件上
# UPDATEIFCOPY (U)	这个数组是其它数组的一个副本，当这个数组被释放时，原数组的内容将被更新
```

## 创建数组

### 一个维度

```python
a = np.array([1, 2, 3])
```

### 多个维度

```python
a = np.array([[1, 2], [3, 4]])
```

###  最小维度

```python
# ndim：设置最小维度
a = np.array([1, 2, 3, 4, 5], ndmin=2)
```

### 设置类型

```python
# dtype 前边已经介绍
a = np.array([1, 2, 3], dtype=complex)

# int8, int16, int32, int64 四种数据类型可以使用字符串 'i1', 'i2','i4','i8' 代替
dt = np.dtype('i2')
print(dt)

# 字节顺序标注，小端法 大端法
dt = np.dtype('<i8')
print(dt)
```

### 未初始化

```python
a = np.empty((2, 2), dtype=np.int32, order='c')
```

### 0、1 填充

```python
# ones() 与之相似
a = np.zeros((5, 5), dtype=[('x', np.int32), ('y', np.int32)])
```

### asarray()

```python
# numpy.asarray(a, dtype = None, order = None)
# 任意形式的输入参数，可以是，列表, 列表的元组, 元组, 元组的元组, 元组的列表，多维数组
a = [1, 2, 3]
b = [(1, 2, 3), (4, 5, 6)]
c = (1, 2, 3)
```

### 从流

```python
s = b"zhanghao"  # 声明是一个bytes
print(np.frombuffer(s, dtype='S1'))
```

### 从迭代器

```python
a = np.fromiter(iter(range(6)))
```

### 从给定范围

```python
# numpy.arange(start, stop, step, dtype)
print(np.arange(5))  # 默认步长是 1
print(np.arange(0, 10, 2, dtype=np.int32))
```

### 数列数组

```python
# 创建一个一维数组，数组是一个等差数列构成的
# np.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
# start	序列的起始值
# stop	序列的终止值，如果 endpoint 为 true，该值包含于数列中
# num	要生成的等步长的样本数量，默认为 50，也就是要生成多少个数据
# endpoint	该值为 true 时，数列中包含 stop 值，反之不包含，默认是 True
# retstep	如果为 True 时，生成的数组中会显示间距，反之不显示。
# dtype	ndarray 的数据类型
print(np.linspace(0, 10, 7, True, True, np.float32))

# 创建一个于等比数列
# start	序列的起始值为：base ** start
# stop	序列的终止值为：base ** stop。如果 endpoint 为 true，该值包含于数列中
# num	要生成的等步长的样本数量，默认为 50
# endpoint	该值为 true 时，数列中中包含 stop 值，反之不包含，默认是 True
# base	对数 log 的底数
# dtype	ndarray 的数据类型
print(np.logspace(1, 10, 10, True, 2, np.int16))
```

## 切片

```python
a = np.arange(10)
s = slice(2, 9, 3)  # slice(start, stop, step)
print(a[s])
print(a[4])
print(a[2:5])  # [from : to]
print(a[2:9:3])  # [start : stop ： step]
print(a[6:])  # [from : len]
print(a[:3])  # [0 : to]
print(a[5::2])  # [from : len : step]
print(a[:5:2])  # [0 : to : step]
print()
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(a[..., 1])  # 某一列
print(a[1, ...])  # 某一行
print(a[0:2, 1:3])  # 行(from : to) , 列(from : to)
```

### 高级切片(欠)

## 广播机制

```python
a = np.array([1, 2, 3])
b = np.array([3, 4, 5])

# a.shape == b.shape 对应位进行操作
print(a+b)
print(a*b)

# a.shape != b.shape 将自动出发广播机制，规则：
# 让所有输入数组都向其中形状最长的数组看齐，形状中不足的部分都通过在前面加 1 补齐
# 输出数组的形状是输入数组形状的各个维度上的最大值
# 如果输入数组的某个维度和输出数组的对应维度的长度相同或者其长度为 1 时，这个数组能够用来计算，否则出错
# 当输入数组的某个维度的长度为 1 时，沿着此维度运算时都用此维度上的第一组值

# 简单理解：对两个数组，分别比较他们的每一个维度（若其中一个数组没有当前维度则忽略），满足：
# 数组拥有相同形状
# 当前维度的值相等
# 当前维度的值有一个是 1
# 若条件不满足，抛出 "ValueError: frames are not aligned" 异常。
a = np.array([[1, 2, 3], [4, 5, 3]])
b = np.array([[1, 2, 3], [4, 5, 3]])
print(a+b)
print(a*b)
```

## 迭代

```python
# 迭代
a = np.arange(6).reshape(2, 3)
print(a)
for x in np.nditer(a, order='F'):  # order：C是行优先；F是列优先
    print(x, end=",")
print()

# 修改
a = np.arange(9).reshape(3, 3)
for x in np.nditer(a, op_flags=['readwrite']):
    x[...] = 2 * x
print(a)
```







