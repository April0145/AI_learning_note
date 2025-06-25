# 《利用python进行数据分析》



## 第二章：Python语言基础、Ipython及Jupyter notebook



### 2.2 Ipython基础



#### 2.21运行IPython命令行

1，在终端输入IPython运行

2，当你在Ipython中仅输入一个变量名，它会只返回

一个表示该对象的字符串

3，与常见的print打印语句不同，ipython中大多数python对象

被格式化为更可读，更美观的形式

4，jupyter 内核使用ipython系统进行内部活动



#### 2.22运行jupyter noteb

1，点击新建按钮选择"python3"即可新建笔记本

2，在Flie菜单下有"save and checkpoint"选项，会自动生成

一个后缀名为.ipynb的文件



#### 2,23ipython的其他功能

1，ipython支持Tap补全功能

2，对象内省

（1），在一个变量名后使用问号（？）可以显示一些关于该对象的

概要信息

（2），对象为一个函数或实例方法且文档字符串已经写好

​		使用？来显示文档字符串

​		使用？？可以显示函数的源代码

（3），？和*结合在一起，会显示所有匹配通配符表达式的命名

```python
import numpy as np
np.*load*?
"""
[out3]:
np._loader_
np.load
np.loads
"""
```

3,%run命令

可以在ipython会话中使用%run命令运行任意的python程序文件

```python
%run ipython_script_test.py
```

4,执行剪切板中的程序-使用魔术函数

（1），%paste会获得剪切板中的所有文本，并在

命令行中作为一个代码块去执行

（2），%cpaste与之类似，但它会给出一个特别的提示符

让你粘贴代码

5，ipython中的特殊命令被称为魔术命令，前缀符为%，可使用？查询

（详见书中p.33）



### 2.3Python语言基础（主要为补充）

#### 2.31语言语义

1，函数调用

向函数括号里传递0或多个参数，通常会把返回值赋值给

一个变量

2，赋值

在Python中进行赋值时，两个引用指向同一对象

```python
a = [1, 2, 3]
b = a
a.append(4)
b
'''
out[4]:
[1, 2, 3, 4]
'''
```

3，类型

（1），使用type（）可以查看对象类型

```python
a = 5
typr(a)
out:int
```

（2），isinstance函数接受一个包含类型的元组，你可以

检查对象的类型是否是元组中存在的类型之一

```python
a = 5
isinstance(a,(int, float))
out:True
```

4，可变对象和不可变对象

可变对象：列表，字典，Numpy数组和大多数用户自定义的类都是可变的

可变对象中包含的对象和值是可以被修改的

不可变对象：字符串，元组



#### 2.32标量类型

1，Python的标准库中拥有一个小的内建库类型集合，

本书称之为标量

*None：Python的“null”值（无效值）*

*str：字符串类型，包含Unicode（utf-8编码）字符串*

*bytes：原生Ascll字节（或者unicode编码字节）*

*float：双精度64位浮点数值（请注意没有独立的double类型）*

*bool：True或False*

*int：任意精度无符号整数*



2，字符串

（1），字符串是Unicode字符的序列，因此可以被看作是除了

列表和元组外的另一种序列

```
s = 'python'
list(s)
out:['p', 'y', 't', 'h', 'o', 'n']
s[:3]
out:['pyt']
```

(2)，反斜杠\是一种转义字符

如果有一个不含特殊符号但含有大量反斜杠的字符串时，

可在字符串前面加一个前缀符号r，表明这些字符是原生

字符（r是raw的缩写，表示原生的）

```
s = r'this\has\no\special\characters'
s
```



4，日期和时间

Python内建的datetime模块，提供了datetime，date，time类型

```
from datetime import datetime,date,time
dt = datetime(2011, 10, 29, 20, 30, 21)
dt.day
dt.minute
```

（1），分别使用date和time方法获取实例的date和time对象

```
dt.date()
dt.time()
```

（2），格式化为字符串

datetime对象变字符串：strftime（）

字符串变datetime对象：strptime（）

```
dt.strftime('%m/%d/%Y %H:%M')

datetime.strptime('20091031', '%Y%m%d')
```

（3），替换datetime时间序列中的值

```
dt.replace(minute=0, second=0)
```

注：不可变类型，产生新的类型

（4），时间差值

两个不同的datetime对象会产生一个datetime.timedelta类型的

对象

```
dt2 = datetime(2011, 11, 15, 22, 30)
delta = dt2 - dt
delta
type(delta)
```

将timedelta加到一个datetime上产生一个新对象

```
dt
dt + delta
```



## 第三章：内建数据结构、函数和文件



### 3.1数据结构和序列



#### 3.11元组

tuple（）函数将任意序列或迭代器转换为元组

元组的元素可以通过中括号[]获取

```
tup = tuple('string')
tup
tuo[0]
```



#### 3.12列表

1，与元组类似，两个列表可以使用+号连接

```
['4', 'foo', None] + [7, 8, (2, 3)]
```

2，可以使用extend方法向已经定义的列表添加多个元素

```
x = [4, None, 'foo']
x.extend([7, 8, (2, 3)])
x
```

3，切片

需要对列表或元组进行翻转时，一种很聪明的做法就是向步长传值-1

```
seq = [7, 2, 3, 7, 5, 6, 0, 1]
seq[::-1]
```



#### 3.13内建序列函数

1，None

（1），None是Python的null值，如果一个函数没有显示地返回值，

则它会隐式地返回None

（2），None还可以作为一个常用的函数参数默认值

```
def add(a, b, c=None):
	result = a + b
	if c is not None:
		result = result*c
	return result
```

（3），None不仅是一个关键字，它还是NoneType类型的唯一实物

```
type(None)
```

2，zip（）函数

（1），zip函数将列表和元组或其它序列的元素配对，新建一个元组构成的列表

```
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
list(zipped)
```

（2），zip可以处理任意长度的序列，它生成列表长度由最短的序列决定

```
seq3 = [False, True]
list(zip(seq1,(seq2, seq3)))
```

（3），zip的常用场景为同时遍历多个序列，有时候会和enumerate（）同时使用

```
for i, (a,b) in enumerate(zip(seq1, seq2)):
	print(f'{i}:{a},{b}')
```

3，reversed函数可以将实例化的元素倒序排列

```
list(reversed(range(3)))
```



#### 3.14字典

使用update方法将两个字典合并，传给update的数据若含有相同的键，

则它的值将会被覆盖

```
dl = {'a': 'somevalue', 'b': [1,2,3,4]}
dl.update({'b': 'foo', 'c': 12})
dl
```





#### 3.16列表、集合和字典推导式

1，列表推导式的基本形式为：

```
[expr for val in collection if condtion]
```

等价于

```
result = []
for val in collection:
	if condition:
		result.append(expr)
```

例子：

```
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
[x.upper() for x in strings if len(x) > 2]
```



2，字典推导式：(expr:表达式)

```
dict_comp = {key-expr : value-expr for value in collecton if conditon}
```

例子：

```
loc_mapping = {val : index for index, val in enumerate(strings)}
loc_mapping
```



3，集合推导式：

```
set_comp = {expr for value in collection if conditon}
```



### 3.2函数

函数是Python中最重要、最基础的代码组织和代码复用方式，如果需要

多次重复相同或类似的代码，就非常值得写一个可复用的函数



#### 3.21命名空间，作用域和本地函数

函数有两种连接变量的方式：全局、本地

在函数内部，任意变量都默认分配到本地命名空间，函数执行结束后，

本地命名空间就会被销毁

在函数内部可使用global关键字声明为全局变量，eg：global a



#### 3.23函数是对象

1，补充：re模块

 在Python中，re 是一个内置模块，用于支持正则表达式（Regular Expression）操作。正则表达式是一种强大的模式匹配工具，用于在字符串中进行搜索、替换和处理。
要使用 re 模块，需要首先导入它：

`import re`

一旦导入了 re 模块，就可以使用其中提供的函数和方法来操作正则表达式。
以下是 re 模块中常用的一些函数和方法：

**re.match(pattern, string): 从字符串的开头开始尝试匹配正则表达式 pattern，如果匹配成功，则返回一个匹配对象；如果匹配不成功，则返回 None。**
**re.search(pattern, string): 在字符串中搜索匹配正则表达式 pattern 的第一个位置，如果匹配成功，则返回一个匹配对象；如果匹配不成功，则返回 None。**
**re.sub(pattern, repl, string): 使用正则表达式 pattern 在字符串中搜索匹配项，并将其替换为 repl 字符串。**
**re.split(pattern, string): 根据正则表达式 pattern 将字符串分割为列表。**



2, `title()` 是一个内置的字符串方法,用于将字符串中每个单词的首字母大写,其余字母小写



2，数据清洗

使数据整齐，用于分析

```
import re
states = ['  alabama', 'Georgia!', 'Georgia', 'FlorTaf', 'afla   aflj##',
          'afaf  ?']

def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip()
        value = re.sub('[!#?]', '', value)
        value = value.title()
        result.append(value)
    return result
a = clean_strings(states)
a
```



#### 3.24匿名（lambda）函数

通过单个语句生成函数的方式，其结果是返回值，匿名函数用lambda定义

```
# 计算两个数的和
sum_func = lambda x, y: x + y
print(sum_func(3, 4))  # 输出 7

# 返回一个数的平方
square = lambda x: x**2
print(square(5))  # 输出 25

# 对列表进行排序
names = ["Alice", "Bob", "Charlie", "David"]
sorted_names = sorted(names, key=lambda x: len(x))
print(sorted_names)  # 输出 ['Bob', 'Alice', 'David', 'Charlie']

```



## 第四章；Numpy基础：数组与向量化计算



### 4.1numpy ndarray： 多维数组对象

ndarray是Python的一个大型数据容器

例子：

```
import numpy as np

# 生成随机数组
data = np.random.randn(2, 3)
data
data * 10
data + data
```

1，一个ndarray是一个通用的多维同类数据容器，它包含的

每一个元素均为相同类型

2，数组的shape属性：表征数组每一维度的数量

数组的dtype属性：描述数组的数据类型

数组的ndim属性：返回数组的维度

```
data.shape
data.dtype
data.ndim
```



#### 4.11 生成ndarray

1，array函数接收任意的序列型对象，生成一个新的包含传递数据的numpy数组

```
data1 = [6, 7.5, 8, 0, 1]
arr1 = np.array(data1)
arr1
```

嵌套序列，例如同等长度的列表，将会自动转换成多维数组

```
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2 = np.array(data2)
arr2
```

2. 其他创建新数组的函数：

   zeros：给定长度和形状后，一次性创造全0数组

   ones：一次性创造全1数组

   empty：创造一个没有初始化数值的数组

   注：想要创建高维数组，则需要为shape传递一个元组

   ```
   np.zeros(10)
   np.ones((3, 6))
   np.empty((2, 3, 2))
   ```

   3，arange是Python内建函数range的数组版

   ```
   np.arange(15)
   ```

   

#### 4.12 ndarray的数据类型

1,数据类型，即dytpe

数据的dtype通常都是按照一个方式命名：

类型名：比如float和int

后面接上表明每个元素位数的数字

```
arr1 = np.array([1, 2, 3], dtype=np.float64)
arr2 = np.array([1, 2, 3], dtype=np.int32)
arr1.dtype
arr2.dtype
```

2,astype方法显式的转换数组的数据类型

```
arr = np.array([1, 2, 3, 4, 5])
arr.dtype

float_arr = arr.astype(np.float64)
float_arr.dtype

```

如果把浮点数转化成整数，则小数点后的部分将被抹除

```
arr = np.array([3.7, -1.2, -2.6, 0.5, 12.9])
arr
arr.astype(np.int32)
```

如果有一个数组，里面的元素都是表达数字含义的字符串，也可以通过astype

将字符串转化为数字

如果转型失败会抛出一个ValueError

使用astype时总是生成一个新数组，即使你传入的dtype与之前一样



#### 4.13Numpy数组算术

数组之所以厉害是因为它允许你进行批量操作而无需任何for循环——称之为**向量化**

```
arr = np.array([[1, 2, 3], [4, 5, 6]])
arr
arr * arr
arr - arr
```

带有标量计算的算术操作， 会把计算机参数传递给数组的每一个元素

```
1 / arr
arr ** 0.5
```

同尺寸数组之间的比较，会产生一个布尔值数组

```
arr2 = np.array([[0, 4, 1], [7, 2, 12]])
arr2
arr2 > arr
```

不同尺寸数组间的操作，将会用到广播特性



#### 4.14基础索引和切片

1，对于一维数组

```
arr = np.arange(10)
arr
arr[5]
arr[5:8]
arr[5:8] = 12
```

`arr[5:8] = 12`: 这行代码将数组`arr`中索引从5到7的元素全部赋值为`12`，即输出是`array([ 0, 1, 2, 3, 4, 12, 12, 12, 8, 9])`。

区别于Python的内建列表，数组的切片是原数组的视图，这意味着数据并不是被复制了，任何对于数组的修改

都会返映到原数据上

```
arr_slice = arr[5:8]
arr_slice
```

```
arr_slice[1] = 12345
arr
```

不写切片值的[ : ]将会引用数组的所有值

```
arr_slice[:] = 64
arr
```

2，对于高维数组

在一个二维数组中，每个索引值对应的元素不再是一个值，而是一个一维数组

```
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
arr2d[2]
```

单个元素可以通过递归的方式获得，也可以通过传递一个索引的逗号分隔列表去选择单个元素，以下两种方式效果一样

```
arr2d[0][2]
arr2d[0, 2]
```

在多维数组中，你可以省略后续索引值，返回的对象将是降低一个维度的数组

```
arr3d = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
arr3d
# arr3d[0]是一个2*3的数组
arr3d[0]
```

标量和数组都可以传递给arr3d[0]

```
old_values = arr3d[0].copy()
arr3d[0] = 42
arr3d
arr3d[0] = old_values
arr3d
```

类似的，arr3d[1, 0]返回的是一个一维数组

```
arr3d[1, 0]
```



##### 4.14.1数组的切片索引

1，一维数组，与Python的语法类似

```
arr
arr[1:6]
```

2，二维数组，先沿着轴0（行）进行切片，在沿着轴1（列）进行切片

```
arr2d
arr2d[:2]
arr2d[:2, 1:]
```

将索引和切片混合，就可以得到低纬度的切片

```
#选择第二行但是只选择前两列
arr2d[1, :2]
#选择第三列，但是只选择前两行
arr2d[:2, 2]
```

单独一个冒号表示整个轴上的数组

```
arr2d[:, :1]
```

3，当然对切片表达式赋值时，整个切片都会重新赋值

```
arr2d[:2, 1:] = 0
arr2d
```



#### 4.15布尔索引

1，使用numpy.random中的randn函数来生成一些随机正太分布数据

```
names = np.array(['Bob', 'Will', 'Joe', 'Bob', 'Will', 'Joe', 'Joe'])
data = np.random.randn(7, 4)
names
data
```

数组的比较操作（比如==）也是可以向量化的

```
names = 'Bob'
```

在索引数组时可以传入布尔值数组,布尔值数组的长度

必须和数组轴索引长度一致

```
data[names == 'Bob']
```

例子：选择了 names == ‘Bob’ 的行，并索引了各个列

```
data[names == 'Bob', 2:]
```

2，取反

使用！=或在条件表达式前使用~对条件取反

```
names != 'Bob'
data[~(names == 'Bob')]
```

3，选择三个名字中的两个来组合多个布尔值条件时，需要使用布尔运算符，如&（and）和 |（or）

```
mask = (names == 'Bob') | (names == 'Will')
mask
data[mask]
```

注：布尔值索引选择数据时，总是生成数据的拷贝

4，基于常识来设置布尔值数组的值也是可行的，

```
#将data中所有的负值设置为0
data[data < 0] = 0
data
```

利用一维布尔值数组对每一行或每一列设置数值

```
data[names != 'Joe'] =  7
data
```



#### 4.16神奇索引

为了选出一个符合特定顺序的子集，你可以简单的通过传递一个包含

指明所需顺序的列表或数组来完成

```
arr = np.empty((8, 4)) #这行代码创建了一个形状为(8, 4)的空数组arr，其中有8行4列。
for i in range(8):
	arr[i] = i
arr
arr[[4, 3, 0, 6]]
```

如果使用负的索引，将从尾部进行选择

```
arr[[-3, -5, -7]]
```

当传递多个索引数组时，会根据每个索引元组对应的元素选出一个一维数组

```
#创建了一个包含0到31的一维数组，并且使用reshape函数将其转换为一个形状为(8, 4)的二维数组arr。
arr = np.arange(32).reshape((8, 4))
arr
arr[[1, 5, 7, 2], [0, 3, 1, 2]]
```

在上述例子中，元素（1， 0），（5，3），（7, 1），（2， 2）被选中

注：神奇索引与切片不同，它总是将数据复制到一个新的数组中



#### 4.17数组转置和换轴

转置是一种特殊的数据重组形式，可以返回底层数据的视图而不需要复制任何内容

1， T属性

```
arr = np.arange(15).reshape((3, 5))
arr
arr.T
```

2,对于高纬度的数组，transpose方法可以接收包含轴编号的元组，用于**重新排列**数组中的轴

```
arr = np.arange(16).reshape(2, 2, 4)
arr
arr.transpose((1, 0, 2))
#这行代码使用transpose函数，根据提供的参数(1, 0, 2)对数组arr的轴进行重新排列。在这个参数中，(1, 0, 2)表示对原数组的第1个轴和第2个轴进行交换
```

3，swapaxes方法

`swapaxes`方法用于交换数组的两个轴。它接受两个轴编号作为参数，并返回一个新的数组，其中指定的两个轴被**交换**。例如:

```python
import numpy as np
arr = np.array([[[1, 2, 3], [4, 5, 6]]])
swapped_arr = arr.swapaxes(0, 2)
```

在这个例子中，`arr`是一个三维数组，其形状为(1, 2, 3)。`arr.swapaxes(0, 2)`将会生成一个`swapped_arr`数组，其形状为(3, 2, 1)。



### 4.2通用函数：快速的逐元素数组函数

通用函数也称为ufunc，是一种在ndarray数据中进行逐元素操作的函数。

1，一元通用函数

（1）abs，fabs：逐元素地计算整数，浮点数，或复数的绝对值

（2）sqrt：计算每个元素的平方根（与arr ** 0.5相等）

（3）square：计算每个元素的平方（与arr ** 2相等）

（4）exp：计算每个元素的自然指数值（e）x

（5）log，log10，log2，log1p：分别对应自然对数（e为底），对数10为底，对数2为底，log（1+x）

（6）sign：计算每个元素的符号值：1（正数），0（0），-1（负数）

（7）ceil：计算每个元素的最高整数值（即大于等于给定数值的最小整数）

```
np.ceil(4.3)
# 5
np.floor(4.3)
# 4
```

（8）floor：计算每个元素的最小整数值（即小于等于给定元素的最大整数）

（9）rint：将元素保留到整数位，并保持dtype

（10）modf：分别将数组的小数部分和整数部分按数组形式返回

（11）isnan：返回数组中的元素是否是一个NaN（不是一个数值），形式为布尔值数组

（12）isfinite，isinf：分别返回数组中的元素是否有限（非inf，非NaN），是否无限的形式

为布尔值数组

（13）cos，tan，sin,  sinh,  cosh,  tanh：常规的双曲三角函数

（14）arccos，arccosh，arcsin，arcsinh，arctan，arctanh：反三角函数

2，二元通用函数

（1）add：将数组的对应元素相加

（2）subtract：逐元素地从第一个数组中减去第二个数组

```
import numpy as np

# 示例 1: 标量与标量之间的减法
result = np.subtract(10, 3)
print(result)  # 输出: 7

# 示例 2: 数组与标量之间的减法
array = np.array([1, 2, 3, 4])
result = np.subtract(array, 1)
print(result)  # 输出: [0 1 2 3]

# 示例 3: 数组与数组之间的逐元素减法
array1 = np.array([10, 20, 30])
array2 = np.array([1, 2, 3])
result = np.subtract(array1, array2)
print(result)  # 输出: [ 9 18 27]
```

（3）multiply：函数用于逐元素地对两个数组进行乘法操作

（4）divide，floor_divide：除或整除（放弃余数）

（5）power：将第二个数组的元素作为第一个数组对应元素的幂次方

（6）maximum，fmax：逐元素地计算两个数组中的最大值，fmax忽略NaN

（7）minimum，fmin：逐个元素计算两个数组中的最小值，fmin忽略NaN

（8）mod：按元素的求模运算（即求除法的余数）

（9）copysign：将数组1的元素绝对值与数组2中元素的符号位合并成一个新的数组

（10）greater，greater_equal, less, less_equal, equal, not_equal：进行逐个元素的比较，

返回布尔值数组（与数学中的操作符>, >=, <, <=, ==, !=效果一致）

（11）logical_and, logical_or, logical_xor：进行逐个元素的逻辑操作（与逻辑操作符&，|，^效果一致）

例子：

```
arr = np.arange(10)
arr
np.sqrt(arr)
np.exp(arr)
```

```
x = np.random.randn(8)
y = np.random.randn(8)
x
y
np.maximum(x, y)
np.add(x, y)
```

```
arr = np.random.randn(7) * 5
arr
remainder, whole_part = np.modf(arr)
remainder
whole_part
```

```
arr = np.array([1.2, 2.7, 3.3, 4.5, 5.9])
new_arr = np.ceil(arr)
newarr
new_arr2 = np.floor(arr)
new_arr2
```

```
arr1 = np.array([1.2, -2.7, 3.3, -4.5, 5.9])
arr2 = np.array([1, -1, 1, -1, 1])
new_arr = np.copysign(arr1, arr2)
new_arr
```



### 4.3使用数组进行面向数组编程

利用数组表达式来代替显式循环的方法，称为向量化



#### 4.31将条件逻辑作为数组操作

numpy.where函数是三元表达式x if condition else y的向量化版本

```
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])
result = np.where(cond, xarr, yarr)  #if c x else y
result
```

np.where的第二个参数和第三个参数不需要是数组，它们可以是标量

```
arr = np.random.randn(4, 4)
arr
arr > 0
np.where(arr > 0, 2, -2)
```



#### 4.32数学和统计方法

1，基础数组统计方法

sum:沿着轴向计算所有元素的累和，0长度的数组，累和为0

mean：数学平均，0长度的数组平均值为NaN

std，var：标准差和方差，可以选择自由度调整（默认分母是N）

min， max：最小值和最大值

argmin，argmax：最小值和最大值的位置

cumsum：从0开始元素累积和

cumprod：从1开始元素累积积

```
arr = np.random.randn(5, 4)
arr
arr.mean()
arr.sum()
np.mean(arr)
```

像mean，sum等函数可以接收一个可选参数axis，这个参数可以用于计算给定轴上的统计值，形成一个下降一个维度的数组

（0：列轴， 1：行轴）

```
#计算列轴上的平均值
arr.mean(axis=1)
#计算行轴上的累和
arr.sum(axis=0)
```

像cumsum和cumprod并不会聚合，它们会产生一个中间结果

```
arr = np.array([0, 1, 2, 3, 4, 5, 6, 7])
arr.cumsum()
```

```
arr = np.array([[0, 1, 2], [3, 4, 5], [6, 7, 8]])
arr
arr.cumsum(axis=0)
arr.cumprod(axis=1)
```



补充：`numpy.hstack`

- **功能**：用于将多个数组沿水平方向（列方向）堆叠在一起。

- **用法**：传入多个数组（它们的行数必须相同），返回一个新数组。

  ```
  import numpy as np
  a = np.array([[1, 2], [3, 4]]) 
  b = np.array([[5, 6], [7, 8]]) 
  result = np.hstack((a, b)) 
  print(result)
  ```

#### 4.33布尔数组的方法

布尔值被强制为1（True）和0（False），因此，sum通常可以用于计算布尔值数组中的True的个数

```
arr = np.random.randn(100)
(arr > 0).sum()
```

any检查数组中是否至少有一个True，all检查是否每个值都是True

```
bools = np.array([False, False, True, False])
bools.any()
bools.all()
```

这些方法也可以适用于非布尔值数组，所有的非0元素都会按True处理



#### 4.34排序

Numpy数组使用sort的方法按位置排列，可以在多维数组中根据传递的axis值，沿着

轴向每一个一维数据段进行排序

np.sort方法返回的是已经排序好的数据拷贝

```
arr = np.random.randn(6)
arr
np.sort(arr)
```

```
arr = np.random.randn(5, 3)
arr
np.sort(arr, axis=1)
```



#### 4.35唯一值和其它集合逻辑

1，数组的集合操作

unique（x）：计算x的唯一值，并排序

intersect1d（x，y）：计算x和y的交集并排序

union1d（x，y）：计算x和y的并集并排序

in1d（x，y）：计算x中的元素是否包含在y中，返回一个布尔值数组

setdiff1d（x，y）：差集，在x中但不在y中的x的元素

setxor1d（x，y）：异或集，在x或y中，但不属于x，y交集的元素

例子：

```
names = np.array(['bob', 'joe', 'will', 'joe', 'joe'])
np.unique(names)
```



### 4.6伪随机数的生成

numpy.random模块

1，生成随机数

np.random.rand(shape): 返回一个给定形状的数组，其中的元素服从0到1之间的均匀分布。

np.random.randn(shape):返回一个给定形状的数组，其中的元素服从标准正态分布（均值为0，标准差为1）。

np.random.randint(low,high,shape):返回一个给定形状的数组，其中的元素为low（包含）到high（不包含）之间的随机数。

2，生成随机数组

np.random.random(shape):返回一个给定形状的数组，其中的元素服从0到1之间的均匀分布。

np.random.normal(mean,std,shape):返回一个给定形状的数组，其中的元素服从具有给定均值和标准差的正太分布。

np.random.uniform(low,high,shape):返回一个给定形状的数组，其中的元素服从具有给定最小值和最大值的均匀分布。

```
import numpy as np
arr1 = np.random.rand(3, 3)
arr1
arr2 = np.random.randn(3, 3)
arr2
arr3 = np.random.randint(1, 10, (3, 3))
arr3
arr4 = np.random.random((3, 3))
arr4
arr5 = np.random.normal(2, 3, (3, 3))
arr5
arr6 = np.random.uniform(1, 10, (3, 3))
arr6
```



## 第五章：pandas入门

pandas用来处理表格型或异质型数据，numpy则相反，更适合处理同质型数值类型的数据。

pandas经常和其他数值计算工具一起使用。



### 5.1pandas数据结构介绍

入门pandas，需要熟悉两个常用的工具数据结构：Series和DataFrame

#### 5.1.1 Series

1，Series是一种一维的数组型对象，它包含了一个值序列和一个数据标签，称为索引

```
obj = pd.Series([4, 7, -5, 3])
obj
```

索引在左边，值在右边

2，values属性和index属性分别获得Series对象的值和索引

```
obj.values
obj.index
```

3,通常可以自己选择标签来创建一个索引序列

```
obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
obj2.index
```

可以在从数据中选择数据的时候使用标签来进行索引

```
obj2['a']
obj2['d'] = 6
obj2['d']
obj2[['c', 'a', 'd']]
```

['c', 'a', 'd']包含的不是数字而是字符串，作为索引列表。

4，从另一个角度考虑Series，可以认为它是一个长度固定且有序的字典，因为它将索引值和数据值按位置配对

```
'b' in obj2
'e' in obj2
```

如果已经有数据包含在python字典中，你可以使用字典生成一个Series

```
sdata = {'Ohio':35000, 'Texas':71000, 'Oregon':16000, 'Utah':5000}
obj3 = pd.Series(sdata)
```

产生的Series的索引将是排序好的字典键，你可以将字典按照你所想要的顺序传递给构造函数

```
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, index=states)
```

因为'California', 它对应的值是NaN

python中使用isnull和notnull函数来检查缺失数据

```
pd.isnull(obj4)
pd.notnull(obj4)
```

isnull和notnull也是Series的实例方法

```
obj4.isnull()
```

5，自动对齐索引是Series的一个非常有用的特性

6，Series对象自身和其他索引都有name属性，这个属性与pandas其它重要功能集成在一起

```
obj4.name = 'poppulation'
obj4.index.name = 'state'
obj4
```

7,Series的索引可以通过按位置赋值的方式进行改变

```
obj
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
```



#### 5.1.2DataFrame

1，表示的是矩阵的数据表，包含已排序的列集合，每一列可以是不同的值类型（数值，字符串，布尔值等）

DataFrame既有行索引也有列索引。

```
data = {'state':['ohio', 'ohio', 'ohio', 'nevada', 'nevada', 'nevada'],
		'year':[2000, 2001, 2002, 2001, 2002, 2003],
		'pop':[1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```

对于大型的DataFrame，head方法只会选出头部的五行：

```
frame.head()
```

2，如果你指定了列的序列，DataFrame的列将会按照指定顺序排列

```
pd.DataFrame(data, columns=['year', 'state', 'pop'])
```

如果你传的列不包含在字典中，将会在结果中出现缺失值

```
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
		index=['one', 'two', 'three', 'four', 'five', 'six'])
frame2
frame2.columns
frame2.index
```

3，DataFrame中的一列，可以按字典型标记或属性那样检索为Series

```
frame2['state']
frame2.year
```

行也可以通过位置或特殊属性loc进行选取

```
frame2.loc['three']
```

列的引用是可以修改的，例如，空的'debt'列可以赋值为标量值或值数组

```
frame2['debt'] = 16.55
frame2
frame2['debt'] = np.arange(6.)
frame2
```

值的长度必须和DataFrame的长度匹配

如果你将Series赋值给一列时，Series的索引将会按照DataFrame的索引重新排列，

并在空缺的地方填充缺失值

```
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
```

4, del关键字可以像在字典中那样对DataFrame删除列

在del的例子中，我们先添加一列eastern，这一列是布尔值，判断条件是state列是否为'ohio'

```
frame2['eastern'] = frame2.state == 'ohio'
frame2
del frame2['eastern']
frame2.columns
```

5，如果嵌套字典被赋值给DataFrame，pandas会将字典的键作为列，将内部字典的键作为行索引

```
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
		'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = pd.DataFrame(pop)
frame3
```

进行转置操作（调换行和列）

```
frame3.T
```

如果已经显示指明索引的话，内部字典的键将不会被排序

```
pd.DataFrame(pop, index=[2001, 2002, 2003])
```

6，Series的字典也可以用于构造DataFrame

```
pdata = {'Ohio': frame3['Ohio'][:-1],
		'Nevada': frame3['Nevada'][:2]}
pd.DataFrame(pdata)
```

7，DataFrame的索引和列拥有name属性，则这些name属性也会被显示

```
frame3.index.name ='year'; frame3.columns.name = 'state'
frame3
```

8，DataFrame的values属性会将包含在DataFrame中 的数据以二维的ndarray的形式返回。

```
frame3.values
```

如果DataFrame的列是不同的dtypes，则values的dtype会自动选择合适所有列的类型

```
frame2.values
```

#### 5.1.3索引对象

1，pandas中的索引对象是不可变的的

2，pandas索引对象可以包含重复的标签，根据重复标签进行筛选，会选取所有重复标签对应的数据

```
dup_labels = pd.index(['foo', 'foo', 'bar', 'bar'])
dup_labels
```



### 5.2基本功能

#### 5.2.1重建索引

reindex，该方法用于创建一个符合新索引的新对象。

1，在Series中

数据会按照新的索引进行排列，如果某个索引值之前并不存在，则会引入缺失值

```
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
obj2
```

对于顺序数据，比如时间序列，在重建索引时可能会需要进行插值或填值。

method可选参数允许我们使用诸如ffill等方法在重建索引时插值，ffill方法会将值向前填充

```
obj3 = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
obj3
obj3.reindex(range(6), method='ffill')
```

2，DataFrame

在DataFrame中，reindex可以改变行索引，列索引，也可以同时改变二者。当仅传入一个

序列时，结果中的行会重建索引

```
frame = pd.DataFrame(np.arange(9),reshape((3,3)),
		index=['a', 'b', 'd'], columns=['ohio', 'texas', 'california'])
frame
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
frame2
```

列可以使用columns关键字重建索引

```
states = ['texas', 'utah', 'california']
frame.reindex(columns=states)
```

可以使用loc进行更为简洁的标签索引

```
frame.loc[['a', 'b', 'c', 'd'], states]
```



#### 5.2.2轴向上删除索引

drop方法会返回一个含有指示值或轴向上删除值的新对象

```
obj = pd.Series(np.arange(5.), index=['a', 'b', 'c', 'd', 'e'])
obj
new_obj = obj.drop('c')
new_obj
obj.drop(['d', 'c'])
```

在DataFrame中，drop使用标签序列会根据行标签删除值（轴0），

可以通过传递axis=1或axis=‘columns’来从列中删除值。

```
data = pd.DataFrame(np.arange(16).reshape((4,4)),
		index=['ohio', 'colorado', 'utah', 'new york'],
        columns=['one', 'two', 'three', four])
data
data.drop(['colorado', 'ohio'])
data.drop('two', axis=1)
data.drop(['two', 'four'], axis='columns')
```

修改Series或DataFrame的尺寸或形状，这些方法直接操作原对象而不返回新对象。



#### 5.2.3索引，选择与过滤

1，Series的索引（obj[...]）

```
obj = pd.Series(np.arange(4),index=['a', 'b', 'c', 'd'])
obj
obj['b']
obj[1]
obj[2:4]
obj[['b', 'a', 'd']]
obj[[1,3]]
obj[obj < 2]
```

使用这些方法设置时会修改Series相应的部分

```
obj['b':'c'] = 5
obj
```

2，DataFrame

行选择语法data[:2]非常方便，传递单个元素或一个列表可以选择列

```
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
		index=['ohio', 'colorado', 'utah', 'new york'],
		columns=['one', 'two', 'three', 'four'])
data
data['two']
data[['three', 'one']]
data[:2]
```

可以使用布尔值对DataFrame进行索引

```
data[data['three'] > 5]
data < 5
data[data < 5] = 0
```

3,整数索引和标签索引

整数索引是默认的索引方式，切片的索引不包括在内，与python的索引方式相同

标签索引，切片的结束索引是包括在内的

为了保持一致性，如果你有一个包含整数的轴索引，数据选择时请始终选择标签索引，

为了更精确的处理，可以使用loc（用于标签）或iloc（用于整数）

```
import pandas as pd

# 创建一个Series对象，使用隐式默认整数索引
series = pd.Series([10, 20, 30, 40, 50])

# 使用整数索引访问元素
value = series[0]

# 使用整数切片访问多个元素
subset = series[1:4]
```

```
import pandas as pd

# 创建一个Series对象，使用自定义标签索引
series = pd.Series([10, 20, 30, 40, 50], index=['a', 'b', 'c', 'd', 'e'])

# 使用标签索引访问元素
value = series['a']

# 使用标签切片访问多个元素
subset = series['b':'d']
```

```
import numpy as np
import pandas as pd
ser = pd.Series(np.arange(3))
ser
ser[0:1]
ser.loc[0:1]
ser.iloc[0:1]
```

4，loc和iloc的使用

```
data.loc['colorado', ['two', 'three']]
data.iloc[2, [3, 0, 1]]
data.iloc[2]
data.iloc[[1, 2], [3, 0, 1]]
data.loc[:'utah', 'two']
data.iloc[:, :3][data.three > 5]
```



#### 5.2.5算术和数学对齐

当你将对象相加时，如果存在某个索引对不相同，则返回结果的索引是索引对的并集，没有交叠的标签位置上，

内部数据对齐会产生缺失值。

```
s1 = pd.Series([7.3, -2.5, 3.4, 1.5], index=['a', 'c', 'd', 'e'])
s2 = pd.Series([-2.1, 3.6, -1.5, 4, 3.1], index=['a', 'c', 'e', 'f', 'g'])
s1
s2
s1 + s2
```

在DataFrame中，行和列会自动对齐

```
df1 = pd.DataFrame(np.arange(9).reshape((3, 3)),columns=list('bcd'),
		index=['ohio', 'texas', 'colorado'])
df2 = pd.DataFrame(np.arange(12).reshape((4, 3)),columns=list('bde'),
		index=['utah', 'ohio', 'texas', 'oregon'])
df1
df2
df1 + df2
```



##### 5.2.5.1使用填充值的算术方法

在df1上使用add方法，再将df2和一个fill_value作为参数传入

```
df1.add(df2, fill_value=0)
```

灵活算术方法

add， radd：加法（+）

sub，rsub：减法（-）

div，rdiv：除法（/)

floordiv，rfloordiv：整除（//）

mul，rmul：乘法（*）

pow， rpow：幂次方（**）

```
df1.rdiv(1)
```



##### 5.2.5.2DataFrame和Series间的操作

```
arr = np.arange(12).reshape((3, 4))
arr
arr[0]
arr - arr[0]
```

当我们从arr中减去arr[0]时，减法在每一行都进行了操作，这就是所谓的广播机制。

在DataFrame中，默认从列轴进行匹配，可以传递（axis=‘index’或axis=0），对行进行匹配，并进行广播

```
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
		columns=list('bde'), index['utah', 'ohio', 'texas', 'oregon'])
series = frame.iloc[0]
frame
series
frame - series
```

```
series3 = frame['d']
series3
frame.sub(series3, axis='index')
```



#### 5.2.6函数应用和映射

1，numpy的通用函数（逐元素数组方法）对pandas对象也有效

```
frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'),
		index=['utah', 'ohio', 'texas', 'oregon'])
frame
np.abs(frame)
```

2，另一个常用的操作是将函数应用到一行或一列的一维数组上，DataFrame的apply方法

```
f = lambda x: x.max() - x.min()
frame.apply(f)
```

这里的函数f，可以计算Series最大值和最小值的差，会被frame中的每一列调用一次，

axis=‘columns’给apply函数，函数将会被每行调用一次

```
frame.apply(f, axis='columns')
```

3，传递给apply的函数并不一定要返回一个标量值，也可以返回带有多个值的Series

```
def f(x):
	return pd.Series([x.min(), x.max()], index=['min', 'max'])
frame.apply(f)
```

4,根据frame中的每一个浮点数计算一个格式化字符串，可以使用applymap方法

```
format = lambda x: '%.2f' % x
frame.applymap(format)
```



#### 5.2.7排序和排名

1，排序，使用sort_index的方法

```
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index()
```

在DataFrame中

```
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),
		index=['three', 'one'],
		columns=['d', 'a', 'b', 'c'])
frame.sort_index()
frame.sort_index(axis=1)
```

数据默认升序排序但也可以降序排序

```
frame.sort_index(axis=1, ascending=False)
```

如果要根据Series的值进行排序，使用sort_values方法

```
obj = pd.Sereies([4, 7, -3, 2])
obj.sort_values()
```

默认情况下，所有缺失值都会被排至Series的尾部

```
obj = pd.Series([4, np.man, 7, np.nan, -3, 2])
obj.sort_values()
```

在DataFrame中，使用一列或多列作为排序键，传递一个或多个列名给sort_values的可选参数by

```
frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
frame
frame.sort_values(by='b')
frame.sort_values(by=['a', 'b'])
```



#### 5.2.8含有重复标签的轴索引

索引的is_unique属性会告诉你它的标签是否唯一

```
obj = pd.Series(range(5), index['a', 'a', 'b', 'b', 'c'])
obj
obj.index.is_unique
```

根据一个标签索引多个条目会返回一个序列，而单个条目会返回标量值

```
obj['a']
obj['c']
```



### 5.3描述性统计的概率和计算

描述性统计和汇总统计

count：非Na值的个数

describe：计算Series或DataFrame各列的汇总统计集合

min，max：计算最下值，最大值

argmin，argmax：分别计算最小值，最大值所在的索引位置（整数）

idxmin，idxmax：分别计算最小值或最大值所在的索引标签

quantile：计算样本的从0到1间的分位数

sum：加和

mean：均值

median：中位数（50%分位数）

mad：平均值的平均绝对偏差

prod：所有值的积

var：值的样本方差

std：值的样本标准差

skew：样本的偏度（第三时刻）值

kurt：样本的峰度（第四时刻）值

cumsum：累积值

cummin，cummax：累积值的最小值或最大值

cumprod：值的累计积

diff：计算第一个算术方差（对时间序列有用）

pct_change：计算百分比

```
df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],
		[np.nan, np.nan], [0.75, -1.3]],
		index=['a', 'b', 'c', 'd'],
		columns=['one','two'])
df
df.sum()
df.sum(axis='columns')
```

除非整个切片上（在本例中是行或列）都是NA，否则NA值是被自动排除的，可以通过禁用skipna来实现不排除NA值

```
df.mean(axis='columns', skipna=False)
```

```
df.idxmax()
df.cumsum()
```



#### 5.3.2唯一值，计数和成员属性

isin：计算表征Series中每个值是否包含于传入序列的布尔值数组。

match：计算数组中每个值的整数索引，形成一个唯一值数组。有助于

数据对齐和join类型的操作。

unique：计算Series值中的唯一值数组，按照观察顺序返回。

value_counts：返回一个Series，索引是唯一值序列，值是计数个数，按照个数降序排序。

```
 = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])
uniques = obj.unique()
uniques
```

如果需要的话可以进行排序（uniques.sort()）。相应的，value_counts计算Series包含的值的个数。

```
obj.value_counts()
```

isin执行向量化的成员属性检查

```
obj
mask = obj.isin(['b', 'c'])
mask
obj[mask]
```



## 第六章：数据载入，储存及文件格式

### 6.1文本格式数据的读写

1，pandas的解析函数

read_csv:从文件，URL或文件型对象读取分隔好的数据，逗号是默认分隔符。

reas_table:从文件，URL或文件型对象读取分隔好的数据，制表符（‘\t’）是默认分隔符。

read_fwf:从特定宽度格式的文件中读取数据（无分隔符）。

read_clipboard:read_table的剪贴板版本，在将表格从web页面上转换成数据时有用。

read_excel:从Excel的xls或xlsx文件中读取表格数据。

read_hdf:读取用pandas存储的HDF5文件。

read_html:从HTML文件中读取所有表格数据。

read_json:从JSON字符串中读取数据。

read_msgpack:读取MessagePack二进制格式的pandas数据。

read_pickle:读取以python pickle格式存储的任意对象。

read_sas:读取存储在SAS系统中定制存储格式的SAS数据集。

read_sql:将SQL查询的结果读取为pandas的DataFrame。

read_stata:读取Stata格式的数据集。

read_feather:读取Feather二进制格式。

2，pandas.read_csv会进行类型推断

```
!type examples/ex1.csv
a,b,c,d,message
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo
```

```
df = pd.read_csv('examples/ex1.csv')
df
```

也可以使用read_table，并指定分隔符

```
pd.read_table('examples/ex1.csv', seq=',')
```

3，有的文件不包含表头行，考虑以下文件

```
!type examples/ex2.csv
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo
```

你可以允许pandas自动分配默认列名，也可以自己指定列名

```
pd.read_csv('examples/ex2.csv', header=None)
pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
```

4，假设你想要message列成为返回DataFrame的索引，你可以指定位置4的列为索引，获将‘message’传给参数index_col

```
names = ['a', 'b', 'c', 'd', 'message']
pd.read_csv('examples/ex2.csv', naems=names, index_col='message')
```

5,想要从多个列中形成一个分层索引，需要传入一个包含列序号或列名的列表

```
!type examples/csv_mindex.csv
key1,key2,value1,value2
one,a,1,2
one,b,3,4
one,c,5,6
one,d,7,8
two,a,9,10
two,b,11,12
two,c,13,14
two,d,15,16
```

```
parsed = pd.read_csv('examples/csv_mindex.csv',
		index_col=['key1', 'key2'])
```

6,当字段是以多种不同数量的空格分开时，可以向read_table传入一个正则表达式作为分隔符，在本例中，正则表达式为\s+,

```
list(open('examples/ex3.txt'))
result = pd.read_table('examples/ex3.txt', seq='\s+')
result
```

7,对于异常的文件格式，你可以使用skiprows来跳过

```
!type examples/ex4.csv
#嘿！
a,b,c,d,message
#只是为了让你觉得更难
#谁用计算机读取csv文件？
1,2,3,4,hello
5,6,7,8,world
9,10,11,12,foo

pd.read_csv('examples/ex4.csv', skiprows=[0, 2,3])
```

8,缺失值处理是文件解析过程中一个重要且常常微妙的部分，通常情况下，缺失值要么不显示（空字符串），要么用一些标识值，

默认情况下，pandas使用一些常见的标识，例如NA和NULL

```
!type examples/ex5.csv
something, a,b,c,d,message
one,1,2,3,4,NA
two,5,6,8,world
three,9,10,11,12,foo
result = pd.read_csv('examples/ex5.csv')
result
```

```
pd.isnull(result)
```



#### 6.1.1分块读入文本文件

我们可以先对pandas的显示设置进行调整，使之更为紧凑，如果只想读入一小部分，避免读入整个文件，可以指明nrows

```
pd.options.display.max_rows = 10
result = pd.read_csv('examples/ex6.csv')
result
```

```
pd.read_csv('examples/ex6.csv', nrows=5)
```



#### 6.1.2将数据写入文本格式

```
data = pd.read_csv('examples/ex5.csv')
data
```

1，DataFramed的to_csv方法，我们可以将数据导出为逗号分隔的文件

```
data.to_csv('examples/out.csv')

!type examples/out.csv
,something,a,b,c,d,message
0,one,1,2,3,0,4,
1,two,5,6,8,world
2,three,9,10,11,0,12,foo
```

2，其他的分隔符也是可以的，写入到sys.stdout

```
import sys
data.to_csv(sys.stdout, sep='|')
```

3，用其它标识值对缺失值进行标注

```
data.to_csv(sys.stdout, na_rep='NULL')
```

4，行和列的标签都会被写入，不过二者也都可以禁止写入

```
data.to_csv(sys.stdout, index=False, header=False)
```

你也可以仅写入列的子集，并且按照你选择的顺序写入

```
data.to_csv(sys.stdout, index=False,columns=['a', 'b', 'c'])
```

5，Series也有to_csv方法

```
datas = pd.date_range('1/1/2000, periods=7')
ts = pd.Series(np.araange(7), index=dates)
ts.to_csv('examples/tseries.csv')
!type examples/tseries.csv
```



#### 6.1.4 JSON数据

1，将json字符串转换为python形式时，使用json.loads方法

```
import json
result = json.loads(obj)
result
```

2，jspn.dumps可以将python对象转换回json

```
siblings = pd.DataFrame(result['siblings'], columns=['name', 'age'])
siblings
```

3，pandas中可以使用to_json方法将数据导出为json

```
print(data.to_json())
```



#### 6.1.5 XML和HTML:网络抓取

pandas的内建函数read_html可以使用lxml和beautiful soup等库将html中的

表解析为DataFrame对象

```
tables = pd.read_html('examples/fdic_failed_bank_list.html')
len(tables)
failures = tables[0]
failures.head()
```



### 6.2二进制格式

使用python内建的pickle序列化模块进行二进制格式操作是存储数据最高效，最方便的方式之一，

to_pickle可以将数据以pickle格式写入硬盘

pandas.read_pickle可以从内建的pickle中读取文件

```
frame = pd.read_csv('example/ex1.csv')
frame
```

```
pd.read_pickle('example/frame_pickle')
```



#### 6.2.2读取Microsoft Excel文件

1， 使用pandas.read_excel函数来读取存储在Excel文件中的表格数据类型

或使用ExcelFile，通过将xls或xlsx的路径传入

```
pd.read_excel(xlsx, 'Sheet1')
xlsx = pd.ExcelFile('examples/ex1.xlsx')
```

2，如果需要将pandas数据写入到excel格式中，你必须生成一个ExcelWriter，然后使用pandas

对象的to_excel方法将数据写入

```
writer = pd.ExcelWriter('examples/ex2.xlsx')
frame.to_excel(writer, 'Sheet1')
writer.save()
```

你也可以将文件路径传给to_excel,避免直接调用ExcelWriter

```
frame.to_excel('examples/ex2.xlsx')
```



## 第七章：数据清洗与准备

在进行数据分析和建模的过程中，大量的时间花在数据准备上：加载，清理，转换，重新排列。

pandas中的大部分设计和实现都是由真实世界的应用需求所驱动的。



### 7.1 处理缺失值

1，pandas使用浮点值NaN来表示缺失值

```
string_data = pd.Series(['aardark', 'artichoke', np.nan, 'avocado'])
string_data
```

2，在panda中采用R语言的编程惯例，将缺失值当做NA处理，python为内建的None值

```
string_data[0] = None
string_data.isnull()
```

3,NA处理方法

fillna：用某些值填充缺失的数据或使用插值方法（如“ffill“ 或 ”bfill“）。

isnull：返回表明哪些值是缺失值的布尔值。

notnull：isnull的反函数。

dropna：用于过滤DataFrame或Series中的缺失值



#### 7.1.1 过滤缺失值:dropna()的使用

(1)在Series上使用dropna，它会返回Series中所有的非空数据及其索引值

```
data = pd.Series([1, None, 3.5, None, 7])
data.dropna()
```

上面的例子和下面的代码是等价的

```
data[data.notnull()]
```

(2)在处理DataFrame对象时，dropna默认情况下会删除包含缺失值的行

```
data = pd.DataFrame([1, 6.5, 3], [1, None, None],
					[None, None, None], [None, 6.5, 3])
data
data.dropna()
```

传入how=‘all’时，将删除所有值均为NA的行

```
data.dropna(how='all')
```

如果要用同样的方式去删除列，传入参数axis=1

```
data[4] = None
data
data.dropna(axis=1, how='all')
```

thresh参数：用于指定删除行或列的阈值。阈值是非缺失值的最小数量，如果某一行或列中的非缺失值数量小于该阈值，则会被删除。

```
df = pd.DataFrame(np.random.randn(7, 3))
df.iloc[:4, 1] = None
df.iloc[:2, 2] = None
df
df.dropna()
df.dropna(thresh=2)
df.dropna(axis=1, thresh=2)
```



#### 7.1.2补全缺失值：fillna()的使用

(1)填充

调用fillna时，可以使用一个常数来代替缺失值

```
df.fillna(0)
```

也可以使用字典，这样你可以为不同列设定不同的填充值

```
df.fillna({1: 0.5, 2: 0})
```

还可以将Series的平均值或中位数用于填充缺失值

```
data = pd.Series([1, None, 3.5, None, 7])
data.fillna(data.mean())
```

(2)插值方法

```
df = pd.DataFrame(np.random.randn(6,3))
df.iloc[2:, 1] = None
df.iloc[4:, 2] = None
df
df.fillna(method='ffill')
df.fillna(method='ffill', limit=2)
```

注：fillna返回的是一个新的对象，你也可以传入参数（inplace=True）修改已经存在的对象

```
df.fillna(0, inplace=True)
df
```

(3)fillna函数参数

value：标量值或字典型对象用于填充缺失值

method：插值方法，如果没有其他参数，默认是‘ffill’（前向填充）

‘bfill’（后向填充）

axis：需要填充的轴，默认axis=0

inplace：修改被调用的对象，而不是生成一个备份

limit：用于前向或后向填充时最大的填充范围



### 7.2数据转换

#### 7.2.1删除重复值(duplicates()方法)

（1）

```
data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],
					'k2': [1, 1, 2, 3, 3, 4, 4]})
data
```

duplicated方法返回的是一个布尔值Series，这个Series反映的是每一个行是否存在重复

```
data.duplicated()
```

drop_dumplicates返回的是DataFrame，内容是dumplicated返回数组中为False的部分

```
data.drop_duplicates()
```

这些方法默认都是对列进行操作

（2）

指定数据的任何子集来检测是否有重复，

假设我们有一个额外的列，并想基于'k1'列去除重复值

```
data['v1'] = range(7)
data.drop_duplicates(['k1'])
```

duplicated和drop_duplicates默认都是保留第一个观测到的值，传入参数keep=’last‘将会返回最后一个

```
data.drop_duplicates(['k1', 'k2'], keep='last')
```



#### 7.2.2 使用函数或映射进行数据转换(map()方法)

`map`方法用于根据提供的映射关系对Series中的每个元素进行映射转换。下面我将介绍`map`方法的基本使用方法。

```
# 创建一个示例Series
s = pd.Series([1, 2, 3, 4, 5])

# 使用map方法对Series进行映射转换
mapped_s = s.map({1: 'A', 2: 'B', 3: 'C', 4: 'D', 5: 'E'})
print(mapped_s)
```



假设下面是你收集到的关于肉类的数据

```
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon', 'Pastrami', 'coreed beef',
					'Bacon', 'pastrami', 'honey ham', 'nova lox'],
					'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
data
```

现在你想要添加一列用于表明每种食物的动物肉类型，

让我们写一下食物和肉类的映射

```
meat_to_animal = {'bacon': 'pig', 'pulled pork': 'pig'
					'pastrami': 'cow', 'corned beef': 'cow',
					'honey ham': 'pig', 'nova lox': 'salmon'}
```

因为有一些食物的值大写了，所以使用Series的str.lower方法将每个值都转换为小写

```
lowercased = data['food'].str.lower()
lowercased
```

```
data['anima'l = lowercased.map(meat_to_animal)]
data
```



#### 7.2.3替代值(replace()方法)

```
data = pd.Series([1, -999, 2, -999, -1000, 3])
data
```

-999可能是缺失值的标识，repalece会产生新的对象，可以传入（inplace=True）在原数据上修改

```
data.replace(-999, np.nan)
```

如果你想要一次替代多个值，你可以传入一个列表和替代值

```
data.replace([-999, -1000], np.nan)
```

要将不同的值替换为不同的值，可以传入替代值的列表

```
data.replace([-999, -1000], [np.nan, 0])
```

参数也可以通过字典传递

```
data.replace({-999: np.nan, -1000: 0})
```



#### 7.2.4重命名轴索引(rename()方法)

`rename`方法用于重命名DataFrame或Series的行索引标签或列索引标签

```
# 创建一个示例DataFrame
data = {'A': [1, 2, 3], 'B': [4, 5, 6]}
df = pd.DataFrame(data)

# 使用rename方法重命名列标签
renamed_df = df.rename(columns={'A': 'Column1', 'B': 'Column2'})
print(renamed_df)
```

```
# 创建一个示例DataFrame
data = {'A': [1, 2, 3], 'B': [4, 5, 6]}
df = pd.DataFrame(data, index=['row1', 'row2', 'row3'])

# 使用rename方法重命名行标签
renamed_df = df.rename(index={'row1': 'Index1', 'row2': 'Index2', 'row3': 'Index3'})
print(renamed_df)
```

`rename`方法还可以用于同时重命名行和列标签，

rename会产生新的对象，传入（inplace=True）参数，以在原始数据上直接进行修改。



#### 7.2.5 离散化和分箱

连续值经常需要离散化，或分离成“箱子”进行分析

(1)`cut`函数用于将连续型数据进行分箱（binning），将连续的数值型数据划分为离散的区间

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用cut函数对Series进行分箱
bins = [0, 5, 10, 15, 20]  # 划分的区间边界
result = pd.cut(s, bins)
print(result)
```

(2)可以通过向labels参数传递一个列表或数组来传入自定义的箱名

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用cut函数对Series进行分箱
bins = [0, 5, 10, 15, 20]  # 划分的区间边界
labels = ['A', 'B', 'C', 'D']  # 区间的标签
result = pd.cut(s, bins, labels=labels)
print(result)
```

(3)`codes`属性是在使用`cut`函数对连续型数据进行分箱后，返回的一个包含了每个原始数据**所属区间的代号**的属性

categories属性返回一个包含箱名的数组

```
result.cat.codes
result.cat.categories
```

(4)可以使用value_counts()来对箱数量记数

```
pd.value_counts(result)
```

(5)小括号表示边是开放的，中括号表示它是封闭的，可以传递right=False来改变哪一边是封闭的

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用cut函数对Series进行分箱
bins = [0, 5, 10, 15, 20]  # 划分的区间边界
result = pd.cut(s, bins, right=False)
print(result)
```

(6)可以向cat传递整数个的箱来代替显式的箱边，pandas将根据数据中的最小值和最大值计算出等长的箱，

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用cut函数对Series进行分箱
result = pd.cut(s, 5, right=False)
print(result)
```

(7)可以通过precision选项进行十进制精度的设置

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用cut函数对Series进行分箱
result = pd.cut(s, 5, precisionv=2)
print(result)
```

(8)`pd.qcut` 函数基于样本分位数划分

```
# 创建一个示例Series
data = [1, 2, 4, 5, 7, 9, 10, 12, 15, 18]
s = pd.Series(data)

# 使用qcut函数对Series进行分箱
result = pd.qcut(s, 3)
print(result)
```

补充：样本的分位数是将一个数据集按照大小顺序排列后，将其分成几个等份的数值点。这些分割点就是分位数。

比如四分位数：将数据分成四等份，分别是下四分位数（25%分位数）、中位数（50%分位数）、上四分位数（75%分位数）。



#### 7.2.6 检测和过滤异常值

（1）`data.describe()`方法返回了关于DataFrame中数值列的统计摘要，包括均值、标准差、最小值、最大值以及四分位数。

```
data = pd.DataFrame(np.random.randn(1000, 4))
data.describe()
```

(2)`col = data[2]`：这一行代码通过`data[2]`选择了DataFrame中的第三列，并将其存储在名为`col`的变量中。

`col[np.abs(col) > 3]`：这一行代码对`col`中的元素进行条件筛选，`np.abs(col)`表示取`col`中元素的绝对值，然后应用条件筛选操作`col > 3`，找出绝对值大于3的元素。

```
col = data[2]
col[np.abs(col) > 3]
```

(3)`any(1)`表示沿着列的方向进行“或”操作，判断每一行中是否至少有一个元素为True。最终得到一个布尔类型的Series,目的是找出DataFrame中任意一行中至少有一个元素的绝对值大于3的行

```
data[(np.abs(data) > 3).any(1)]
```

(4)语句np.sign(data)根据数据中的正负分别生成1和-1的数值

```
np.sign(data).head()
```



#### 7.2.7 置换和随机抽样

(1)`sample` 方法用于从 Series 或 DataFrame 中随机抽取样本

```
import pandas as pd

# 创建一个示例 Series
s = pd.Series([1, 2, 3, 4, 5])

# 从 Series 中随机抽取 3 个样本，无放回抽样
sampled_data = s.sample(n=3, replace=False)

print(sampled_data)

```

```
import pandas as pd

# 创建一个示例 DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4, 5], 'B': [6, 7, 8, 9, 10]})

# 从 DataFrame 中随机抽取 2 行样本，有放回抽样
sampled_data = df.sample(n=2, replace=True)

print(sampled_data)

```

`frac` 参数用于指定抽取样本的比例，`weights` 参数用于指定抽样时的权重

```
import pandas as pd

# 创建一个示例 DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4, 5], 'B': [6, 7, 8, 9, 10]})

# 从 DataFrame 中随机抽取 30% 的样本
sampled_data_frac = df.sample(frac=0.3)

print(sampled_data_frac)

```

```
import pandas as pd

# 创建一个示例 DataFrame
df = pd.DataFrame({'A': [1, 2, 3, 4, 5], 'B': [6, 7, 8, 9, 10]})

# 从 DataFrame 中根据权重抽取样本
weights = [0.1, 0.1, 0.2, 0.3, 0.3]  # 每行对应的抽样权重
sampled_data_weights = df.sample(n=3, weights=weights, replace=True)

print(sampled_data_weights)

```

(2)`np.random.permutation` 函数用于对数组(一维或多维都适用）进行随机重排

```
import numpy as np

# 创建一个示例数组
arr = np.array([1, 2, 3, 4, 5])

# 对数组进行随机重排
arr_permuted = np.random.permutation(arr)

print(arr_permuted)

```

s = np.random.permutation(5) 是使用 NumPy 中的 random.permutation 函数对长度为 5 的数组进行随机重排，并将结果赋值给变量 s

`take` 方法用于根据索引从 Series 或 DataFrame 中获取元素。它允许您使用索引的列表或数组来获取数据

```
df = pd.DataFrame(np.arange(5 * 4).reshape((5, 4)))
s = np.random.permutation(5)
s
df
df.take(s)
```



#### 7.2.8 计算指标/虚拟变量

`get_dummies` 函数用于将分类变量转换为指示变量的 DataFrame，即将分类变量转换为二进制的指示变量。

```
# 创建一个包含分类变量的 DataFrame
data = {'category': ['A', 'B', 'C', 'A', 'B']}
df = pd.DataFrame(data)

# 使用 get_dummies 函数将分类变量转换为指示变量
dummies = pd.get_dummies(df['category'])
dummies
df
```

参数`prefix`：用于指定生成的指示变量列名的前缀

数据合并

```
df.join(dummies)
dummies = pd.get_dummies(df['category'], prefix= 'key')
dummies
```



### 7.3 字符串操作

#### 7.3.1 字符串对象方法

1，python内建字符串方法

count：返回子字符串在字符串中的非重叠次数

join：使用字符串作为间隔符，用于粘合其他字符串的序列

index：查询字符，如果在字符串中找到，则返回子字符串中第一个字符的位置；如果

找不到则引发ValueError

find：返回子字符串中第一个出现子字符的第一个字符的位置；如果没有找到，则返回-1

replace:使用一个字符串代替另一个字符串

strip，rstrip，lstrip：修建空白，包括换行符，相当于对每个元素进行x.strip()（以及rstrip, lstrip)

split：使用分隔符将字符串拆分为子字符串的列表

lower：将大写字母转换为小写字母

upper：将小写字母转换为大写字母

2，

一个逗号分隔的字符串可以使用split方法拆分成很多块

```
val = 'a,b,  guido'
val.split(',')
```

split常和strip一起使用，用于清除空格（包括换行）

```
pieces = [x.strip() for x in val.split(',')]
pieces
```

这些字符串可以使用加法和两个冒号分隔符连接在一起

```
first, second, third = pieces
first + '::' + second + '::' + third
```

也可以使用join方法

```
'::'.join(pieces)
```

使用python的in关键字是检测子字符串的最佳方法

```
'guido' in val
```

index和find也能实现同样的功能，find和index的区别在于index在字符串没有找到时会抛出一个异常，而find是返回-1

```
val.index(',')
val.find(':')
```

count返回某个特定的子字符串在字符串中出现的次数

```
val.count(',')
```

replace将用一种字符串来代替另一种字符串，它通常也用于传入空字符串

```
val.replace(',', '::')
val.repalece(',', '')
```



#### 7.3.2正则表达式

1，正则表达式是一种用来描述字符串匹配模式的表达式。

python内建的re模块是用于将正则表达式应用到字符串上的库。

re模块主要有三个主题：模式匹配，替代，拆分,

2，假设我们想将含有多种空白字符（制表符， 空格，换行符）的字符串拆分开，

描述一个或多个空白字符的正则表达式是 \s+：

(1)split 函数用于根据正则表达式来分割字符串

```
import re
text = "foo		bar\t baz	\tqux"
re.split('\s+', text)
```

(2)compile 函数用于将正则表达式编译成模式对象，该模式对象可以用于在后续的匹配操作中进行重复使用

```
regex = re.compile('\s+')
regex.split(text)
```

(3)`findall` 函数用于在字符串中查找正则表达式匹配的所有子串，并返回一个包含所有匹配子串的列表。

```
regex.findall(text)
```

```
import re

text = "The cat and the hat sat on the mat."
result = re.findall(r'\b\w{3}\b', text)
print(result)
```

在这个示例中，我们使用 `re.findall` 函数查找了字符串 `text` 中所有包含三个字母的单词，并将结果打印输出。

(4)search函数用于在字符串中搜索匹配给定正则表达式模式的第一个子串。如果找到匹配的子串，则返回一个匹配对象；如果没有找到匹配的子串，则返回 None

```
text = "The cat and the hat sat on the mat."
pattern = r'\b\w{3}\b'
result = re.search(pattern, text)
result
```

(5) `match` 函数用于从字符串的开头开始匹配正则表达式模式。如果找到匹配的子串，则返回一个匹配对象；如果没有找到匹配的子串，则返回 `None`。

```
text = "The cat and the hat sat on the mat."
pattern = r'\b\w{3}\b'
result = re.match(pattern, text)
result
```

(6)

```python
re.sub(pattern, repl, string, count=0, flags=0)
```

其中：

- `pattern` 是要匹配的正则表达式模式。
- `repl` 是用于替换匹配子串的字符串。
- `string` 是要进行替换操作的字符串。
- `count` 是可选参数，用于指定最多替换的次数。如果省略，则默认替换所有匹配的子串。
- `flags` 是可选的标志参数，用于控制正则表达式的匹配方式。

`sub` 函数会在 `string` 中查找所有与 `pattern` 匹配的子串，并用 `repl` 中指定的字符串进行替换。如果指定了 `count` 参数，则最多进行 `count` 次替换。

```
import re

text = "The cat and the hat sat on the mat."
result = re.sub(r'\b\w{3}\b', 'dog', text)
print(result)
```

在这个示例中，我们使用 `re.sub` 函数将字符串 `text` 中所有包含三个字母的单词替换为 "dog"。



#### 7.3.3 pandas中的向量化字符串函数

1. `str.lower()` 和 `str.upper()`: 将字符串转换为小写或大写形式。
2. `str.strip()`: 移除字符串首尾的空格或指定字符。
3. `str.replace()`: 替换字符串中的指定子串。
4. `str.contains()`: 检查字符串是否包含指定的子串。
5. `str.extract()`: 从字符串中提取符合正则表达式模式的子串。
6. `str.split()`: 将字符串拆分为多个子串。
7. `str.join()`: 将字符串数组中的元素连接起来。

```
# 创建一个包含字符串的 Series
data = {'name': ['Alice', 'Bob', 'Charlie', 'David'],
        'email': ['alice@gmail.com', 'bob123@yahoo.com', 'c@d.com', 'david@example.com']}
df = pd.DataFrame(data)

# 使用向量化字符串函数处理 email 列
df['email_upper'] = df['email'].str.upper()
df['email_domain'] = df['email'].str.extract(r'@(.+)')
df['email_contains_gmail'] = df['email'].str.contains('gmail')

print(df)
```



## 第八章：数据规整：连接，联合与重塑

### 8.1 分层索引

1,分层索引是pandas的重要特性，允许你在一个轴向上拥有多个索引层级

```
data = pd.Series(np.random.randn(9),
		index=[['a', 'a', 'a', 'b', 'b', 'c', 'c', 'd', 'd'],
				[1, 2, 3, 1, 3, 1, 2, 2, 3]])
data
data.index
```

2,通过分层索引对象，也可以称为部分索引，允许你简洁地选择出数据的子集

```
data['b']
data['b': 'c']
data.loc[['b', 'd']]
```

在内部层级中进行选择也是可以的

```
data.loc[:, 2]
```

3,unstack方法可以让你在多级索引的数据结构中进行数据的重塑和重新排列

```
data.unstack()
```

unstack的反操作是stack

```
data.unstack().stack()
```

4，在DataFrame中，每个轴都可以拥有分层索引

```
frame = pd.DataFrame(np.arange(12).reshape((4,3)),
		index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
		columns=[['ohio', 'ohio', 'colorado'], ['green', 'red', 'green']],
		names=['state', 'color'])
frame
```

5，分层的层级可以有名称（可以是字符串或python对象），如果层级有名称，这些名称会在控制台输出中显示

```
frame.index.names = ['key1', 'key2']
frame.columns.names = ['state', 'color']
frame
```

通过部分列索引，你可以选出列中的组

```
frame['ohio']
```



#### 8.1.1 重排序和层级排序

1，需要重新排列轴上的层级顺序，swaplevel接收两个层级序号或层级名称，返回一个进行了层级变更的新对象

可以传入axis参数来选择应用的轴

```
frame.swaplevel('key1', 'key2')
```

2，sort_index只能在单一层级上对数据进行排序，通过axis参数来选择轴，索引是按照字典从最外层开始排序，`level` 指定多级索引的级别

```
frame.sort_index(level=1)
frame.swaplevel(0, 1).sort_index(level=0)
```



#### 8.1.2 按层级进行汇总统计

通过sum方法，level选项可以指定你想要在某个特定的轴上进行聚合

```
frame.sum(level='key2')
```

```
frame.sum(level='color', axis=1)
```



#### 8.1.3 使用DataFrame的列进行索引

`set_index` 函数用于将 DataFrame 中的一列或多列作为新的索引。这个函数可以帮助您重新构造 DataFrame

```
frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),
					'c': ['one', 'one', 'one', 'two', 'two', 'two', 'two'],
					'd': [0, 1, 2, 0, 1, 2, 3]})
frame
```

```
frame2 = frame.set_index(['c', 'd'])
frame2
```

默认情况下，这些列会从DataFrame中删除，你也可以将它们留在DataFrame中

```
frame.set_index(['c', 'd'], drop=False)
```

reset_index是set_index的反操作

```
frame2.reset_index()
```



### 8.2 联合与合并数据集

#### 8.2.1 数据库风格的DataFrame连接

`merge` 函数用于将两个 DataFrame 根据一个或多个键（key）进行合并

```
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
                   'data1': range(7)})
df1
df2 = pd.DataFrame({'key': ['a', 'b', 'd'],
                   'data2': range(3)})
df2
pd.merge(df1, df2)
```

1，如果连接的键信息没有指定，merge会自动将重叠列名作为连接的键，但是，显示地指定连接键才是好的实现

```
pd.merge(df1, df2, on='key')
```

如果每个对象的列名是不同的 ，你可以分别为它们指定列名

```
df3 = pd.DataFrame({'lkeye': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
					'data1': range(7)})
df4 = pd.DataFrame({'rkey': ['a', 'b', 'd'],
					'data2': range(3)})
pd.merge(df3, df4, left_on='lkey', right_on='rkey')
```

2，how参数的不同连接类型

‘inner’ ： 只对两张表都有的键的交集进行联合

‘left’ ： 对所有左表的键进行联合

‘right’ ： 对所有右表的键进行联合

‘outer’ ： 对两张表都有的键的并集进行联合

```
pd.merge(df1, df2, how='outer')
pd.merge(df1, df2, how='left')
pd.merge(df1, df2, how='right')
```

多对多连接是行的笛卡尔积。由于左边的DataFrame有三个‘b’行而在右边有2个，因此结果中有6个‘b’行

```
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
					'data1': range(6)})
df1
df2 = pd.DataFrame({'key': ['a', 'b', 'a', 'b', 'd'],
					'data2': range(5)})
df2
pd.merge(df1, df2, on='key', how='left')
pd.merge(df1, df2, how='inner')
```

使用多个键进行合并时，传入一个列名的列表

```
left = pd.DataFrame({'key1': ['foo', 'foo', 'bar'],
					'key2': ['one', 'two', 'one'],
					'lval': [1, 2, 3]})
left
right = pd.DataFrame({'key1': ['foo', 'foo', 'bar', 'bar'],
					'key2': ['one', 'one', 'one', 'two'],
					'rval': [4, 5, 6, 7]})
right
pd.merge(left, right, on=['key1', 'key2'], how='outer')
```

3，关于重叠的列名，merge有一个suffixes后缀选项，用于在左右两边DataFrame对象的重叠列名后指定需要添加的字符串

```
pd.merge(left, right, on='key1')
pd.merge(left, right, on='key1', suffixes=('_left', '_right'))
```



#### 8.2.2根据索引合并

1，如果DataFrame中用于合并的键是它的索引，在这种情况下需要传递

left_index=True或right=True来表示索引需要作为合并的键

```
left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],
                     'value': range(6)})
left1
```

```
right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
right1
```

```
pd.merge(left1, right1, left_on='key', right_index=True)
```

2，在多层索引的情况下，必需以列表的方式指明合并所需多个列

```
lefth = pd.DataFrame({'key1': ['ohio', 'ohio', 'ohio', 'nevada', 'nevada'],
					'key2': [2000, 2001, 2002, 2001, 2002],
					'data': np.arange(5)})
lefth
```

```
righth = pd.DataFrame(np.arange(12).ershape((6, 2)),
					index=[['nevada', 'nevada', 'ohio', 'ohio', 'ohio', 'ohio'],
					2001, 2002, 2000, 2000, 2001, 2002],
					columns=['event1', 'event2'])
righth
```

```
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index= True)
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index= True, how='outer')
```

使用两边的索引来进行也是可以的

```
left2 = pd.DataFrame([[1, 2], [3, 4], [5, 6]],
					index=['a', 'c', 'e'],
					columns=['ohio', 'nevada'])
right = pd.DataFrame([[7, 8], [9, 10], [11, 12], [13, 14]],
					index=['b', 'c', 'd', 'e'],
					columns=['misssouri', 'alabama'])
pd.merge(left2, right2, how='outer', left_index=True, right_index=True) 
```

3，DataFrame有一个方便的join实例方法，用于按照索引合并。该方法也可以用于合并多个索引相同或相似

但没有重叠列的DataFrame对象

```
left2.join(right2, how='outer')
```

对于一些简单的索引—索引合并，你可以向join方法传入一个DataFrame列表

```
another = pd.DataFrame([[7, 8], [9, 10], [11, 12], [16, 17]],
						index=['a', 'c', 'e', 'f'],
						columns=['new york', 'oregon'])
another
left2.join([right2, another])
left2.join([right2, another], how='outer')
```



#### 8.2.3沿轴向连接

`concat` 函数用于沿指定轴将多个 DataFrame 进行连接

1，假设我们有三个不存在重叠的Series

```
s1 = pd.Series([0, 1], index=['a', 'b'])
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
s3 = pd.Series([5, 6], index=['f', 'g'])
```

```
pd.concat([s1, s2, s3])
```

2，默认情况下，concat方法沿着axis=0的轴向生效的，生成另一个Series，如果你传递axis=1，返回的结果是一个DataFrame（axis=1时是列）

```
pd.concat([s1, s2, s3], axis=1)
```

3，join 参数指定连接的方式，可选值包括inner和outer，默认为outer

```
s4 = pd.concat([s1, s3])
s4
pd.concat([s1, s4], axis=1)
pd.concat([s1, s4], axis=1, join='inner')
```

4，可以使用join_axes来指定用于连接其他轴向的轴

```
pd.concat([s1, s4], axis=1, join_axes=[['a', 'c', 'b', 'e']])
```

5，拼接在一起的各部分无法在结果中区分是一个潜在的问题，假设你想在连接轴上

创建一个多层索引，可以使用keys参数来实现

```
result = pd.concat([s1, s2, s3], keys=['one', 'two', 'three'])
result
result.unstack()
```

6，沿着轴向axis=1连接Series的时候，keys则成为DataFrame的列头

```
pd.concat([s1, s2, s3], axis=1, keys=['one', 'two', 'three'])
```

相同的操作在DataFrame中

```
df1 = pd.DataFrame(np.arange(6).reshape(3,2), index=['a', 'b', 'c'],
					columns=['one', 'two'])
df2 = pd.DataFrame(5 + np.arange(4).reshape(2,2), index=['a', 'c'],
					columns=['three', 'four'])
df1
df2
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'])
```

如果传递的是对象的字典而不是列表的话，则字典的键会用于keys选项

```
pd.concat({'leval1':df1, 'level2':df2}, axis=1)
```

我们可以使用names参数命名生成轴层级

```
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'],
		names=['upper', 'lower'])
```

7，ignore_index不沿着连接轴保留索引 ，而产生一段新的索引

```
df1 = pd.DataFrame(np.random.randn(3, 4), columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.random.randn(2, 3), columns=['b', 'd', 'a'])
df1
df2
pd.concat([df1, df2], ignore_index=True)
```



#### 8.2.4 联合重叠数据

1，where函数，这个函数可以进行面向数组的if—else等价操作

```
a = pd.Series([np.nan, 2.5, 0.0, 3.5, 4.5, np.nan],
             index=['f', 'e', 'd', 'c', 'b', 'a'])
b = pd.Series([0, np.nan, 2, np.nan, np.nan, 5],
             index=['a', 'b', 'c', 'd', 'e', 'f'])
np.where(pd.isnull(a), b, a)
```

np.where(pd.isnull(a), b, a):如果`pd.isnull(a)`中的元素为True，则返回对应位置上`b`中的元素；如果`pd.isnull(a)`中的元素为False，则返回对应位置上`a`中的元素

2，combine_first，逐列进行操作，可以认为它是根据你传入的对象来

“修补”调用对象的缺失值

```
df1 = pd.DataFrame({'a': [1, np.nan, 5, np.nan],
				'b': [np.nan, 2, np.nan, 6],
				'c': [range(2, 18, 4)]})
df2 = pd.DataFrame({'a': [5, 4, np.nan, 3, 7],
					'b': [np.nan, 3, 4, 6, 8]})
df1
df2
df1.combine_first(df2)
```



### 8.3 重塑和透视

重排列表格型数据有多种基础操作，这些操作被称为重塑或透视

#### 8.3.1 使用多层索引进行重塑

1，stack():堆叠

unstack():拆堆

```
data = pd.DataFrame(np.arange(6).reshape((2, 3)),
					index=pd.Index(['ohio', 'colorado'], name='state'),
					columns=pd.Index(['one', 'two', 'three'],
					name='number'))
data
result = data.stack()
result
result.unstack()
```

默认情况下，是对最内层进行拆堆的，也可以传入一个层级序号或名称来拆分一个不同的层级

```
result.unstack(0)
result.unstack('state')
```

2， 如果层级中的所有值并未包含于每个子分组中时，拆分可能会引入缺失值

```
s1 = pd.Series([0, 1, 2, 3], index=['a', 'b', 'c', 'd'])
s2 = pd.Series([4, 5, 6], index=['c', 'd', 'e'])
data2 = pd.concat([s1, s2], keys=['one', 'two'])
data2
data2.unstack
```

默认情况下，堆叠会过滤缺失值（可以传入dropna=False）避免，因此堆叠拆堆的操作是可逆的

```
data2.unstack()
data2.unstack().stack()
data2.unstack().stack(dropna=False)
```

3，在DataFrame中拆堆时，被拆分的层级会变为结果中最低的层级

```
df = pd.DataFrame({'left': result, 'right': result + 5},
				columns=pd.Index(['left', 'right'], name='side'))
df
df.unstack('state')
```

在调用stack方法时，我们可以指明需要堆叠的轴向名称

```
df.unstack('state').stack('side')
```



#### 8.3.2将“长”透视为“宽”

1，pivot方法：pivot方法允许我们通过选择一个列用作新的DataFrame的列标签，然后选择另一个列用作行标签，并在最后选择一个列来填充新的DataFrame中的值

```
  date       city  temperature
0  2022-01-01  Beijing  0
1  2022-01-01  Shanghai  2
2  2022-01-02  Beijing  3
3  2022-01-02  Shanghai  5
```

```
pivot_df = df.pivot(index='date', columns='city', values='temperature')
```

```
           Beijing  Shanghai
date
2022-01-01  0       2
2022-01-02  3       5
```

使用pivot方法来将这个数据框转换为以日期为行索引，以城市为列索引，以温度为值的新数据框



#### 8.3.3 将‘宽’透视为‘长’

melt方法，它将多列合并成一列，产生一个新的DataFrame

```
df = pd.DataFrame({'key': ['foo', 'bar', 'baz'],
				'A': [1, 2, 3],
				'B': [4, 5, 6],
				'C': [7, 8, 9]})
df
```

当使用melt方法时，我们必需指出哪些列是分组指标，这里我们让‘key’作为分组指标，其他列均为数据值

```
melted = pd.melt(df, ['key'])
melted
```

使用pivot方法，可以将数据重塑回原先的布局

```
reshaped = melted.pivot('key', 'variable', 'value')
reshaped
```



## 第九章：绘图和可视化

### 9.1简明matplotlib API入门

```
import matplotlib.pyplot as plt
import numpy as np
data = np.arange(10)
data
plt.plot(data)
```



#### 9.1.1 图片与子图

1，可以使用plt.figure()生成一个新的空白图片

```
fig = plt.figure()
```

2，add_subplot方法用于在一个图形窗口中添加子图（subplot）。

add_subplot方法是在一个Figure对象上调用的，用于在该Figure对象中创建一个新的子图，也就是在同一个窗口中创建一个新的绘图区域，以便在这个子图中绘制图形或图表。

```
fig = plt.figure()  # 创建一个新的Figure对象
ax = fig.add_subplot(nrows, ncols, index)  # 在Figure对象中添加子图
```

参数说明：

- nrows: 子图的行数
- ncols: 子图的列数
- index: 子图的索引（从左上角开始，从左到右，从上到下的顺序排列）

```
import matplotlib.pyplot as plt

# 创建一个新的Figure对象
fig = plt.figure()

# 在Figure对象中添加子图
ax1 = fig.add_subplot(2, 2, 1)  # 子图1
ax2 = fig.add_subplot(2, 2, 2)  # 子图2
ax3 = fig.add_subplot(2, 2, 3)  # 子图3
ax4 = fig.add_subplot(2, 2, 4)  # 子图4
```

当你输入绘图命令时，matplotlib会在最后一个子图上进行绘制

```
import matplotlib.pyplot as plt

# 创建一个新的Figure对象
fig = plt.figure()

# 在Figure对象中添加子图
ax1 = fig.add_subplot(2, 2, 1)  # 子图1
ax2 = fig.add_subplot(2, 2, 2)  # 子图2
ax3 = fig.add_subplot(2, 2, 3)  # 子图3
plt.plot(np.random.randn(50).cumsum(), 'k--')
ax1.hist(np.random.randn(100), bins=20, color='k', alpha=0.3)
ax2.scatter(np.arange(30), np.arange(30) + 3 * np.random.randn(30))
```

3，，subplots方法被用来创建一个包含多个子图（subplots）的Figure对象，并返回一个Figure对象和一个包含所有子图的Axes对象数组。

```
fig, ax = plt.subplots(nrows, ncols)
```

参数说明：

- nrows: 子图的行数

- ncols: 子图的列数

- sharex, sharey：布尔值或者’row’, 'col’，用来控制子图之间的x轴（sharex）或者y轴（sharey）的共享情况。如果设置为True，表示子图共享x轴或y轴；如果设置为’row’，表示在同一行的子图共享x轴或y轴；如果设置为’col’，表示在同一列的子图共享x轴或y轴。

  ```
  import matplotlib.pyplot as plt
  
  # 创建一个包含2x2子图的Figure对象和一个包含所有子图的Axes对象数组
  fig, axes = plt.subplots(2, 2)
  ```

4，调整子图周围的间距

subplots_adjust方法用于调整子图之间的间距和位置，以便更好地控制整体图形的布局。通过这个方法，我们可以调整子图之间的距离，以及整个图形区域的边距。

subplots_adjust方法的常用语法如下：

```python
fig.subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)
```

参数说明：

- left, right, top, bottom：用于控制子图区域的边距，以百分比表示，默认为None。
- wspace, hspace：用于调整子图之间的水平间距（wspace）和垂直间距（hspace），以百分比表示，默认为None。

```
import matplotlib.pyplot as plt

fig, ax = plt.subplots(2, 2)

# 调整子图之间的水平间距和垂直间距
fig.subplots_adjust(wspace=0.5, hspace=0.5)

# 调整子图区域的边距
fig.subplots_adjust(left=0.1, right=0.9, top=0.9, bottom=0.1)
```



#### 9.1.2 颜色，标记和线类型

`matplotlib` 库中的 `plot` 方法用于绘制折线图

`plot` 方法的基本语法如下：

```python
import matplotlib.pyplot as plt

plt.plot(x, y, **kwargs)
plt.show()
```

其中，`x` 和 `y` 是要绘制的数据点的 x 和 y 坐标，`**kwargs` 是用于传递其他参数的可选参数，比如颜色、线型、标记等。

下面是一个简单的示例，演示了如何使用 `plot` 方法绘制折线图：

```python
import matplotlib.pyplot as plt

# 定义数据
x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]

# 绘制折线图
plt.plot(x, y, marker='o', linestyle='-', color='b')

# 显示图形
plt.show()
```

在这个示例中，我们首先导入了 `matplotlib.pyplot` 模块，然后定义了要绘制的数据点的 x 和 y 坐标，最后使用 `plot` 方法绘制了折线图，并通过 `marker`、`linestyle` 和 `color` 参数指定了线条的标记、线型和颜色。



#### 9. 1.3刻度，标签和图例

1，在matplotlib中，有几种方法可以设置x轴刻度的位置和标签。以下是一些常用的方法：

使用set_xticks和set_xticklabels方法：

- `set_xticks`方法用于设置x轴刻度的位置，可以传入一个列表或数组来指定刻度的位置。
- `set_xticklabels`方法用于设置x轴刻度的标签，可以传入一个列表或数组来指定刻度的标签文本。

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(0, 10, 1)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)

ax.set_xticks([0, 2, 4, 6, 8, 10])  # 设置x轴刻度的位置
ax.set_xticklabels(['zero', 'two', 'four', 'six', 'eight', 'ten'])  # 设置x轴刻度的标签

ax.set_title('My first matplotlib plot')
ax.set_xlabel('Stages')

plt.show()
```

set_xlabel:会给x轴一个名称

set_title:会给子图一个标题

对于y轴，只需将实例中的x换成y即可。



2，`ax.set()` 方法是用于一次性设置坐标轴属性的快捷方法。通过这个方法，可以在一个语句中设置多个坐标轴的属性，如坐标轴范围、刻度位置、刻度标签等。

`ax.set()` 方法使用关键字参数来设置坐标轴的属性，语法如下：

```python
ax.set(xlim=(xmin, xmax), ylim=(ymin, ymax), xlabel='X轴标签', ylabel='Y轴标签', title='图表标题')
```

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

fig, ax = plt.subplots()
ax.plot(x, y)

# 使用ax.set()方法一次性设置坐标轴的属性
ax.set(xlim=(0, 10), ylim=(-1, 1), xlabel='X轴', ylabel='Y轴', title='正弦函数')

plt.show()
```

在这个例子中，我们首先创建了一个包含正弦函数的简单折线图。然后，我们使用 `ax.set()` 方法一次性设置了x轴和y轴的范围（`xlim` 和 `ylim`）、标签（`xlabel` 和 `ylabel`）以及图表标题（`title`）。



3，添加图例最简单的方法是在添加每个图表时同时传递label参数，最后调用ax.legend或plt.legend自动生成图例

legend有个位置参数（loc=‘best’）会自动选择

```
from numpy.random import randn
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
ax.plot(randn(1000).cumsum(), 'k', label='one')
ax.plot(randn(1000).cumsum(), 'k--', label='two')
plt.legend(loc='best')
plt.show()
```



#### 9.1.4注释和子图加工

1，`annotate` 函数用于在 matplotlib 图表中添加注释，可以包括文字和箭头，以指向特定的数据点或坐标位置。该函数的常见参数包括：

- `s`：要显示的注释文本内容。
- `xy`：被注释点的位置，是一个包含两个值的元组，分别代表 x 和 y 坐标。
- `xytext`：注释文本的位置，也是一个包含两个值的元组，分别代表 x 和 y 坐标。
- `arrowprops`：一个字典，用于设置箭头的样式，包括颜色、箭头形状等。

```
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 2*np.pi, 100)
y = np.sin(x)

fig, axs = plt.subplots(2)

# 在第一个子图中添加注释
axs[0].plot(x, y)
axs[0].annotate('Local Max', xy=(3*np.pi/2, 1), xytext=(2*np.pi, 1.5),
             arrowprops=dict(facecolor='black', shrink=0.05))

# 在第二个子图中添加注释
axs[1].plot(x, np.cos(x))
axs[1].annotate('Local Min', xy=(np.pi/2, -1), xytext=(0, -1.5),
             arrowprops=dict(facecolor='black', shrink=0.05))

plt.show()
```



#### 9.15 将图片保存在文件

`plt.savefig` 方法是 matplotlib 库中用于保存图表的函数。它可以将当前图表保存为各种图像格式的文件，例如 PNG、JPEG、PDF、SVG 等。

该方法的常见用法如下：

```python
plt.savefig('filename.png')
```

在这个示例中，`plt.savefig` 方法将当前图表保存为一个名为 `filename.png` 的 PNG 格式文件。除了文件名之外，`plt.savefig` 方法还可以接受其他一些参数，用于指定图像文件的格式、分辨率、透明度等。

例如，您可以通过传递 `dpi` 参数来指定图像的分辨率：

```python
plt.savefig('filename.png', dpi=300)
```

这将以 300 dpi 的分辨率保存图像。您还可以使用 `format` 参数指定文件格式：

```python
plt.savefig('filename.pdf', format='pdf')
```

这将把图表保存为 PDF 格式文件。

最后，需要注意的是，`plt.savefig` 方法通常应该在 `plt.show()` 方法之前调用，因为在调用 `plt.show()` 后，图表将被显示在屏幕上，而不是保存到文件中。



### 9.2 使用pandas和seaborn绘图

#### 9.2.1 折线图

Series和DataFrame都有一个plot属性，用于绘制基本图形，默认情况下，plot()

绘制的是折线图

1，Series

series对象的索引传入作为绘图的x轴，可以通过use_index=False来禁用这个功能。

```
s = pd.Series(np.random.randn(10).cumsum(), index=np.arange(0, 100, 10))
s.plot()
```

- `kind`：指定要绘制的图形类型，如线图（’line’）、柱状图（’bar’）、散点图（’scatter’）

- `title`：设置图形的标题。

- `grid`：设置是否显示网格线。

- `color`：设置图形的颜色。

- `legend`：设置是否显示图例。

- `xlim`、`ylim`：用于指定 x 轴和 y 轴的范围。

- alpha:透明度

  ```
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  
  # 创建一个 Series 对象
  s = pd.Series(np.random.randn(10), index=np.arange(10))
  
  # 使用 plot 方法创建折线图
  s.plot(kind='line', title='Line Plot', color='g', grid=True, xlim=[0,10], ylim=[-2,2])
  plt.show()
  ```

2，DataFrame

dataframe的plot方法在同一个子图中将每一列绘制为不同折线，并自动生成图例

```
df = pd.DataFsrame(np.random.randn(10, 4).cumsum(0),
				columns=['A', 'B', 'C', 'D'],
				index=np.arange(0, 100, 10))
df.plot()
```



#### 9.2.2柱状图

plot.bar()和plot.barh()可以分别绘制垂直和水平的柱状图

```
fig, axes = plt.subplots(2, 1)
data = pd.Series(np.random.rand(16), index=list('abcdefghijklmnop'))
data.plot.bar(ax=axes[0], color='k', alpha=0.7)
data.plot.barh(ax=axes[1], color='k', alpha=0.7)
```

在DataFrame中，柱状图将每一行中的值分组到并排的柱子中的一组

```
df = pd.DataFrame(np.random.randn(6, 4),
				index=['one', 'two', 'three', 'four', 'five', 'six'],
				columns=pd.Index(['A', 'B', 'C', 'D'], name='Genus'))
df
df.plot.bar()
```

DataFrame的列名称‘Genus’被用作了图例标题，

我们可以通过传递stacked=True来生成堆积柱状图，会使每一行的值堆积在一起

```
df.plot.barh(stacked=True, alpha=0.5)
```



#### 9.2.3 直方图和密度图

1， 直方图是一种条形图，用于给出值频率的离散显示

plot.hist方法，`bins` 参数用于指定直方图的箱子数量，使用参数 `edgecolor` 来指定直方图中每个条之间的边缘颜色

```
import matplotlib.pyplot as plt
import numpy as np

# 创建一些随机数据
data = np.random.randn(1000)

# 绘制直方图
plt.hist(data, bins=30, edgecolor='black')
plt.title('Histogram')
plt.show()
```

2， `plot.kde` 方法可以绘制密度图（Kernel Density Estimation）。密度图是对数据分布的连续估计，可以显示数据的概率密度函数。

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 创建一个包含随机数据的 Series 对象
data = pd.Series(np.random.randn(1000))

# 使用 plot.kde 方法绘制密度图
data.plot.kde()
plt.title('Density Plot')
plt.show()

```

您还可以通过传递其他参数来自定义密度图的样式，例如设置颜色、线型、透明度等。例如：

```python
data.plot.kde(color='green', linestyle='--', linewidth=2, alpha=0.5)
```

3，`seaborn` 库中的 `distplot` 函数可以用来绘制带有直方图和密度估计的单变量分布图。下面是一个简单的示例，演示如何使用 `distplot` 方法绘制单变量的分布图：

```python
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt

# 创建一个包含随机数据的 Series 对象
data = np.random.randn(1000)

# 使用 distplot 方法绘制单变量的分布图
sns.distplot(data, bins=30, kde=True, color='blue')
plt.title('Distribution Plot')
plt.show()
```

在这个示例中，我们首先创建了一个包含随机数据的 numpy 数组，然后使用 `sns.distplot` 方法绘制了数据的分布图。在 `sns.distplot` 中，`bins` 参数用来指定直方图的箱子数量，`kde` 参数用来控制是否显示密度估计，`color` 参数用来指定颜色。



#### 9.2.4 散点图或点图

1，seaborn库中的`regplot`方法可以绘制带有线性回归模型拟合的散点图。下面是一个简单的示例，演示如何使用`regplot`方法绘制散点图并拟合线性回归模型：

```python
import seaborn as sns
import numpy as np
import matplotlib.pyplot as plt

# 创建一些随机数据
x = np.random.rand(100)
y = 2 * x + np.random.normal(0, 0.1, 100)  # 创建一个线性关系的数据，带有一些噪声

# 使用 regplot 方法绘制带线性回归模型拟合的散点图
sns.regplot(x, y)
plt.title('Scatter Plot with Linear Regression Model')
plt.xlabel('X')
plt.ylabel('Y')
plt.show()
```

在这个示例中，我们首先创建了一些随机的 x 和 y 数据，然后使用 `sns.regplot` 方法绘制了带有线性回归模型拟合的散点图。`regplot` 方法会自动拟合出一个线性回归模型，并在散点图上绘制出拟合的直线。

2，`pairplot` 是 seaborn 库中的一个非常有用的函数，用来绘制数据集中每对变量之间的关系图。它会创建一个关联矩阵，其中对角线上是每个变量的直方图，而其他位置则是每对变量之间的散点图或其他可视化方式。

以下是一个简单的示例，演示如何使用 `pairplot` 函数来绘制数据集中各变量之间的关系图：

```python
import seaborn as sns
import pandas as pd

# 创建一个包含随机数据的DataFrame
data = pd.DataFrame({
    'A': np.random.rand(100),
    'B': np.random.rand(100),
    'C': np.random.rand(100)
})

# 使用 pairplot 方法绘制关系图
sns.pairplot(data)
plt.show()
```

在这个示例中，我们首先创建了一个包含随机数据的 DataFrame，然后使用 `sns.pairplot` 方法绘制了各变量之间的关系图。`pairplot` 方法会自动生成关联矩阵，展示出每对变量之间的关系，可以帮助我们理解数据集中的变量之间的相关性和分布情况。



#### 9.2.5分面网格和分类数据

`catplot` 函数可以用于绘制分类数据的图表，包括散点图、箱线图、小提琴图、条形图等。它可以根据不同的参数和子图类型来呈现数据的不同方面。

以下是一个简单的示例，演示如何使用 `catplot` 函数绘制分类数据的图表：

```python
import seaborn as sns
import pandas as pd

# 创建一个包含分类数据的DataFrame
data = pd.DataFrame({
    'Category': ['A', 'A', 'B', 'B', 'C', 'C'],
    'Value': [1, 2, 3, 4, 5, 6]
})

# 使用 catplot 方法绘制分类数据图表
sns.catplot(x='Category', y='Value', data=data, kind='bar')
plt.show()
```

在这个示例中，我们首先创建了一个包含分类数据的 DataFrame，然后使用 `sns.catplot` 方法绘制了分类数据的条形图。



## 第十章：数据聚合和分组操作

数据聚合是指将数据集合中的多个值合并为单个值的过程。在数据聚合中，通常会应用一些函数来对数据进行计算，比如求和、平均值、最大值、最小值等。聚合操作可以帮助我们对数据进行汇总，以便更好地理解数据集的特征和趋势。

分组操作是指根据某些特征或条件将数据集划分成多个组的过程。在分组操作中，我们可以根据某一列或多列的数值或类别特征，将数据集拆分成不同的子集。然后，我们可以对每个子集进行独立的分析、聚合或其他操作，以便更好地理解数据的分布和关系。



### 10.1 GroupBy机制

`groupby` 函数是 Pandas 库中用于数据分组操作的重要工具，它允许我们根据一个或多个键（可以是列名、数组、Series或者索引级别）对数据集进行分组。一旦数据被分组，我们就能够对每个分组进行聚合、转换、过滤等操作。

下面是一个简单的示例，演示如何使用 `groupby` 函数对数据进行分组操作：

```python
df = pd.DataFrame({'key1': ['a', 'a', 'b', 'b', 'a'],
                  'key2': ['one', 'two', 'one', 'two', 'one'],
                  'data1': np.random.randn(5),
                  'data2': np.random.randn(5)})
df
grouped1 = df.groupby('key1')
grouped1.mean()
```

在这个示例中，我们首先创建了一个包含分类数据的 DataFrame，然后使用 `groupby` 方法对数据根据 ‘Category’ 列进行分组。接着，我们可以对每个分组进行聚合操作，比如计算平均值。

```
grouped2 = df.groupby(['key1', 'key2'])
grouped2.mean()
```

我们使用两个键对数据进行分组，得到一个包含唯一键对的多层索引

```
grouped2.mean().unstack()
```



#### 10.1.1遍历各分组

groupby对象支持迭代，会生成一个包含组名和数据块的2维元组序列

```
for name, group in df.groupby('key1'):
    print(name)
    print(group)
```

在多个分组键的情况下，元组中的第一个元素是键值的元组

```
for (k1, k2), group in df.groupby(['key1', 'key2']):
    print((k1, k2))
    print(group)
```

你可以选择在任何一块数据上进行你 想要的操作

```
pieces = dict(list(df.groupby('key1')))
pieces
pieces['b']
```



#### 10.1.2 选择一列或所有列的子集

df.groupby('key1')['data1']

df.groupby('key1')[['data1']]

```
df.groupby('key1')['data1'].mean()
df.groupby(['key1', 'key2'])[['data2']].mean()
```



#### 10.1.3 使用字典和Series分组

```
import numpy as np
import pandas as pd
people = pd.DataFrame(np.random.randn(5, 5),
                     columns=['a', 'b', 'c', 'd', 'e'],
                     index=['joe', 'steve', 'wes', 'jim', 'travis'])
people.iloc[2:3, [1, 2]] = np.nan
people
```

假设我们拥有各列的分组对应关系，并且想把各列按组累加

```
mapping = {'a': 'red', 'b': 'red', 'c': 'blue', 'd': 'blue', 'e': 'red', 'f': 'orange'}
by_column = people.groupby(mapping, axis=1)
by_column.sum()
```

Series也有相同的功能，可以视为固定大小的映射

```
map_series = pd.Series(mapping)
map_series
people.groupby(map_series, axis=1).count()
```



#### 10.1.4 使用函数分组

想根据名字的长度进行分组，可以传递len函数

```
people.groupby(len).sum()
key_list = ['one', 'one', 'one', 'two', 'two']
people.groupby([len, key_list].min())
```



#### 10.1.5根据索引层级分组

分层索引的数据集有一个非常方便的地方，就是能够在轴索引的某个层级上进行聚合

```
columns = pd.MultiIndex.from_arrays([['us', 'us', 'us', 'jp', 'jp'],
									[1, 3, 5, 1, 3]],
									names=['cty', 'tenor'])
hier_df = pd.DataFrame(np.random.randn(4, 5), columns=columns)
hier_df
hier_df.groupby(level='cty', axis=1).count()
```

根据层级分组时，将层级数值或层级名称传递给level关键字



### 10.2 数据聚合

聚合是指所有根据数组产生标量值的数据转换过程

1，常用的groupby方法

- count：分组中的非NA值数量
- sum：非NA值的累和
- mean：非NA值的均值
- median：非NA值的算术中位数
- std，var：无偏的（n-1分母）标准差和方差
- min，max：非NA值的最小值，最大值
- prod：非NA值的乘积
- first，last：非NA值的第一个和最后一个值

2，自定义聚合方法，需要将函数传递给aggregate和agg方法

```
def peak_to_peak(arr):
	return arr.max() - arr.min()
grouped.agg(peak_to_peak)
```



#### 10.2.1 逐列及多函数应用

当使用 Pandas 的 `groupby` 方法进行分组后，我们可以对每个分组应用多个函数，或者对每一列应用不同的函数。这通过 `agg` 方法实现，即应用聚合函数进行计算。

下面是一个简单的示例，演示如何对每个分组应用多个函数：

```python
import pandas as pd

# 创建一个包含分类数据的DataFrame
data = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B', 'A', 'C'],
    'Value1': [1, 2, 3, 4, 5, 6],
    'Value2': [10, 20, 30, 40, 50, 60]
})

# 使用 groupby 方法对数据进行分组
grouped = data.groupby('Category')

# 对每个分组应用多个函数
result = grouped.agg({
    'Value1': 'mean',
    'Value2': ['min', 'max']
})
print(result)
```

在这个示例中，我们首先创建了一个包含分类数据的 DataFrame，然后使用 `groupby` 方法对数据根据 ‘Category’ 列进行分组。接着，我们使用 `agg` 方法对每个分组应用了多个函数，比如计算 ‘Value1’ 列的平均值，以及 ‘Value2’ 列的最小值和最大值。

`agg` 方法允许我们传递一个字典，其中键是要操作的列名，值是要应用的函数。你也可以传递内置的聚合函数名称（如 ‘mean’、’sum’、’min’、’max’ 等）。



#### 10.2.2 返回不含行索引的聚合数据

在不需要索引的情况下，可以通过向groupby传递as_index=False来禁用分组键作为索引的行为



### 10.3 应用：通用拆分-应用-联合

1，Pandas 的 `groupby` 方法进行分组后，我们可以使用 `apply` 方法对每个分组应用自定义的函数。`apply` 方法允许我们将自定义的函数应用于每个分组，并将结果合并为单个 DataFrame。

下面是一个简单的示例，演示如何使用 `apply` 方法对每个分组应用自定义函数：

```python
import pandas as pd

# 创建一个包含分类数据的DataFrame
data = pd.DataFrame({
    'Category': ['A', 'B', 'A', 'B', 'A', 'C'],
    'Value': [1, 2, 3, 4, 5, 6]
})

# 使用 groupby 方法对数据进行分组
grouped = data.groupby('Category')

# 自定义函数
def custom_function(group):
    return group['Value'].mean()  # 计算每个分组的均值

# 对每个分组应用自定义函数
result = grouped.apply(custom_function)
print(result)
```

在这个示例中，我们首先创建了一个包含分类数据的 DataFrame，然后使用 `groupby` 方法对数据根据 ‘Category’ 列进行分组。接着，我们定义了一个自定义的函数 `custom_function`，该函数计算了每个分组的 ‘Value’ 列的均值。最后，我们使用 `apply` 方法将自定义函数应用于每个分组。

`apply` 方法允许我们对每个分组应用任意自定义函数，从而实现灵活的数据处理和分析。

2，在 Pandas 中，`describe` 方法用于生成描述性统计信息，包括计数、平均值、标准差、最小值、25%，50%，75% 百分位数和最大值。它适用于数值型数据列，可以帮助用户快速了解数据的分布和摘要统计信息。

下面是一个简单的示例，演示如何使用 `describe` 方法：

```python
import pandas as pd

# 创建一个包含数值型数据的DataFrame
data = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [5, 10, 15, 20, 25]
})

# 使用 describe 方法生成描述性统计信息
description = data.describe()
print(description)
```

在这个示例中，我们首先创建了一个包含数值型数据的 DataFrame，然后使用 `describe` 方法生成了关于数据列 ‘A’ 和 ‘B’ 的描述性统计信息，包括计数、平均值、标准差、最小值、25%，50%，75% 百分位数和最大值。

`describe` 方法对于快速了解数据的分布和摘要统计信息非常有用，特别是在数据探索和预处理阶段。



#### 10.3.1 压缩分组键

之前的例子，所得到的对象具有分组键所形成的分层索引以及每个原始对象的索引，

可以向groupby传递group_keys=False来禁用这个功能。



#### 10.3.2 分位数和桶分析

pandas中的工具，如cut和qcut，用于将数据按照你选择的箱位或样本分位数进行分桶。与groupby

方法一起使用这些函数可以对数据集更方便地进行分桶或分位分析

```
frame = pd.DataFrame({'data1': np.random.randn(1000),
					'data2': np.random.randn(1000)})
quartiles = pd.cut(frame.data1, 4)
quartiles[:10]
```

```
def get_states(group):
	return{'min': group.min(), 'max': group.max(),
			'count': group.count(), 'mean': group.mean()}
grouped = frame.data2.groupby(quartiles)
grouped.apply(get_states).unstack()
```



#### 10.3.3 示例：使用指定分组值填充缺失值

填充缺失值，fillna是一个可以使用的正确工具

```
s = pd.Series(np.random.randn(6))
s[::2] = np.nan
s
s.fillna(s.mean())
```



#### 10.3.4 示例: 随机采样和排列

`sample` 方法是 Pandas 中用于对 DataFrame 或 Series 进行随机抽样的方法。它可以用于从数据集中随机选择一部分数据进行分析或处理。

下面是一个简单的示例，演示如何使用 `sample` 方法：

```python
import pandas as pd

# 创建一个包含数据的 DataFrame
data = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [6, 7, 8, 9, 10]
})

# 使用 sample 方法对数据进行随机抽样
sampled_data = data.sample(n=3, replace=False, random_state=1)
print(sampled_data)
```

在这个示例中，我们首先创建了一个包含数据的 DataFrame，然后使用 `sample` 方法对数据进行了随机抽样。在这个示例中，我们选择了 3 个样本（通过参数 `n=3`），不允许重复抽样（通过参数 `replace=False`），并且设置了随机种子（通过参数 `random_state=1`）以确保结果的可重现性。

除了上面示例中提到的参数之外，`sample` 方法还有其他一些参数，比如 `frac` 参数用于指定抽样样本的比例（当 `n` 未指定时使用），以及 `axis` 参数用于指定抽样的轴（是对行进行抽样还是对列进行抽样）等。



#### 10.3.5 示例: 分组加权平均和相关性

在 NumPy 中，`average` 方法用于计算数组的加权平均值。它可以接受一个权重数组，用来指定每个元素的权重。以下是一个简单的示例：

```python
import numpy as np

# 创建一个包含数据的数组
arr = np.array([1, 2, 3, 4, 5])

# 创建一个包含对应权重的数组
weights = np.array([1, 2, 3, 4, 5])

# 使用 average 方法计算加权平均值
weighted_average = np.average(arr, weights=weights)
print(weighted_average)
```

在这个示例中，我们使用了 NumPy 的 `average` 方法来计算数组 `arr` 的加权平均值，其中 `weights` 数组指定了每个元素的权重。



### 10.4 数据透视表与交叉表

1，`pivot_table` 是 Pandas 中用于创建透视表的方法。透视表是一种数据汇总工具，可以根据一个或多个键对数据进行聚合，并按照指定的行和列来排列数据。

- `index`: 用于指定在透视表中作为行索引的列名或列名列表。
- `columns`: 用于指定在透视表中作为列索引的列名或列名列表。
- `values`: 需要聚合的列名，默认情况下聚合所有数值型的列。
- `aggfunc`: 用于指定对数值进行聚合的函数，可以是内置的聚合函数（如 ‘sum’、’mean’、’count’ 等），也可以是自定义的聚合函数。
- `fill_value`: 用于指定填充缺失值的值。
- `dropna`: 如果为True，将不含所有条目均为NA的列。

```
import pandas as pd

# 创建一个包含数据的 DataFrame
data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02', '2021-01-01'],
    'Category': ['A', 'B', 'A', 'B', 'A'],
    'Value': [10, 20, 30, 40, 50]
}
df = pd.DataFrame(data)

# 使用 pivot_table 方法创建透视表
pivot_table = df.pivot_table(index='Date', columns='Category', values='Value', aggfunc='sum')
print(pivot_table)
```

2，交叉表，是数据透视表的一个特殊情况，计算的是分组中的频率

`crosstab` 是 Pandas 中用于创建交叉表（cross-tabulation table）的方法

以下是一个简单的示例，演示如何使用 `crosstab` 方法：

```python
import pandas as pd

# 创建一个包含数据的 DataFrame
data = {
    'A': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'],
    'B': ['one', 'one', 'two', 'two', 'one', 'one'],
    'C': ['x', 'y', 'x', 'y', 'x', 'y']
}
df = pd.DataFrame(data)

# 使用 crosstab 方法创建交叉表
cross_tab = pd.crosstab(df['A'], [df['B'], df['C']])
print(cross_tab)
```

在这个示例中，我们创建了一个包含数据的 DataFrame，并使用 `crosstab` 方法根据 ‘A’ 列和 ('B’, ‘C’) 列组合来创建交叉表，其中 ‘A’ 列作为行索引，('B’, ‘C’) 组合作为列索引，统计了各个组合出现的频数。



## 第十一章：时间序列

### 11.1 日期和时间数据的类型及工具

在 Python 的 datetime 模块中，常用的日期和时间类型包括以下几种：

1. **datetime.date**: 用于表示日期的类，包括年、月、日。可以使用 `datetime.date(year, month, day)` 来创建一个日期对象。
2. **datetime.time**: 用于表示时间的类，包括小时、分钟、秒和微秒。可以使用 `datetime.time(hour, minute, second, microsecond)` 来创建一个时间对象。
3. **datetime.datetime**: 用于表示日期和时间的类，包括年、月、日、小时、分钟、秒和微秒。可以使用 `datetime.datetime(year, month, day, hour, minute, second, microsecond)` 来创建一个日期时间对象。
4. **datetime.timedelta**: 用于表示时间间隔或持续时间的类，可以用来对日期时间进行加减操作。

这些类型都是 datetime 模块中的内置类，可以通过导入 datetime 模块来使用。下面是一个简单的示例，演示如何使用 datetime 模块中的类型：

```python
import datetime

# 创建一个日期对象
date_obj = datetime.date(2022, 12, 31)
print(date_obj)

# 创建一个时间对象
time_obj = datetime.time(23, 59, 59)
print(time_obj)

# 创建一个日期时间对象
datetime_obj = datetime.datetime(2022, 12, 31, 23, 59, 59)
print(datetime_obj)

# 创建一个时间间隔对象
delta = datetime.timedelta(days=7)
new_date = date_obj + delta
print(new_date)
```

这些类型提供了丰富的功能，可以满足大部分的日期和时间处理需求

```
from datetime import datetime
now = datetime.now()
now
now.year, now.month, now.day
s = datetime(2011, 1, 7) - datetime(2008, 6, 24, 8, 15 )
s
s.days
s.seconds
datetime(2011, 1, 7) + s
```



#### 11.1.1字符串与datetime互相转换

可以使用`datetime`模块中的`strptime`和`strftime`方法来进行字符串和`datetime`对象之间的转换。

1. **字符串转换为datetime**:

   - 使用`strptime`方法将字符串转换为`datetime`对象。该方法接受两个参数，第一个参数是要转换的字符串，第二个参数是日期时间的格式。

   示例：

   ```python
   from datetime import datetime
   
   date_string = "2022-12-31 23:59:59"
   date_object = datetime.strptime(date_string, '%Y-%m-%d %H:%M:%S')
   print(date_object)
   ```

2. **datetime转换为字符串**:

   - 使用`strftime`方法将`datetime`对象转换为字符串。该方法接受一个格式化字符串作为参数，用于指定输出的日期时间格式。

   示例：

   ```python
   from datetime import datetime
   
   date_object = datetime(2022, 12, 31, 23, 59, 59)
   date_string = date_object.strftime('%Y-%m-%d %H:%M:%S')
   print(date_string)
   ```

这些方法提供了方便的方式来在字符串和`datetime`对象之间进行转换。需要确保在使用`strptime`方法时，提供的格式与输入字符串的格式完全匹配，否则会引发`ValueError`异常。同样，在使用`strftime`方法时，需要使用正确的格式化指令来输出所需的日期时间格式。



### 11.2 时间序列基础

pandas中的基础时间序列种类是由时间戳索引的Series

```
from datetime import datetime
dates = [datetime(2011, 1, 2), datetime(2011, 1, 5),
		datetime(2011, 1, 7), datetime(2011, 1, 8),
		datetime(2011, 1, 10), datetime(2011, 1, 12)]
ts = pd.Series(np.random.randn(6), index=dates)
ts
```

在这种情况下，这些datetime对象可以被放入DatetimeIndex中

```
ts.index
ts + ts[::2]
```



#### 11.2.1 索引，选择，子集

当你基于标签进行索引和选择时，时间序列的行为和其他的pandas.Series类似

```
stamp = ts.index[2]
ts[stamp]
```

为了方便，你还可以传递一个能解释为日期的字符串

```
ts['1/10/2011']
ts['20110110']
```

对一个长的时间序列，可以传递一个年份或一个年份和月份来轻松地选择数据的切片

```
longer_ts = pd.Series(np.random.randn(1000),
					index=pd.date_range('1/1/2000', periods=1000))
longer_ts
longer_ts['2001']
```

字符串‘2001’被解释为一个年份，并选择了相应的时间区间，如果你指定了

月份也是有效的

```
longer_ts['2001-05']
```

使用datetime对象进行切片也是可以的

```
ts[datetime(2011, 1, 7):]
```

因为大部分的时间序列是按时间顺序排序的，你可以使用不包含在时间序列中的时间戳进行切片，

```
ts
ts['1/6/2011':'1/11/2011']
```

在切片上的修改会反映在原始数据上

上面这些操作也都适用于DataFrame，并在其行上进行索引

```
dates = pd.date_range('1/1/2000', periods=100, freq='W-WED')
long_df = pd.DataFrame(np.random.randn(100, 4),
					index=dates,
					columns=['colorado', 'texas',
							'new york', 'ohio'])
long_df.loc['5-2001']
```



#### 11.2.2 含有重复索引的时间序列

```
dates = pd.DatatimeIndex(['1/1/2000', '1/2/2000', '1/2/2000', '1/2/2000', '1/3/2000'])
dup_ts = pd.Series(np.arange(5), imdex=dates)
dup_ts
```

通过检查索引的is_unique属性，我们可以看出索引并不是唯一的

```
dup_ts.index.is_unique
```

对上面的Series进行索引，结果是标量值还是Series切片取决于是否有时间戳是重复的

```
dup_ts['1/3/2000']
dup_ts['1/2/2000']
```

假设你想要聚合含有非唯一时间戳的数据，一种方式就是使用groupby并传递level=0

```
grouped = dup_ts.groupby(level=0)
grouped.mean()
grouped.count()
```



### 11.3 日期范围，频率和移位

resample方法将样本时间序列转换为固定的每日频率数据

```
ts
s = ts.resample('D')
```



#### 11.3.1 生成日期范围

1，pandas.date_range是用于根据特定频率生成指定长度的DatatimeIndex

```
index = pd.date_range('2012-04-01', '2012-06-01')
index
```

默认情况下，date_range生成的是每日的时间戳，如果只传递一个起始或结尾日期，你必需传递一个用于

生成范围的数字

```
pd.date_range(start='2012-04-01', periods=20)
pd.date_range(end='2012-06-01', periods=20)
```

如果你需要一个包含每月最后业务日期的时间索引，你可以传递‘BM’频率

```
pd.date_range('2000-01-01', '2000-12-01', freq='BM')
```

2，在 Python 中，时间序列频率是指在时间序列数据中观测值之间的时间间隔。Pandas 库提供了丰富的时间序列频率处理功能，以下是一些基础的时间序列频率：

1. **秒（Second）**: `S` 或 `second`
2. **分钟（Minute）**: `T` 或 `min`
3. **小时（Hour）**: `H` 或 `hour`
4. **每天（Day）**: `D` 或 `day`
5. **工作日（Business Day）**: `B`
6. **周（Week）**: `W`
7. **月（Month）**: `M`
8. **季度（Quarter）**: `Q`
9. **年（Year）**: `A`

除了以上基础频率之外，Pandas 还支持复合频率，例如可以使用 `2H` 表示每两个小时，`3T` 表示每三分钟，以此类推。此外，还可以使用偏移别名，如 `BQ` 表示每个季度的最后一个工作日。

Pandas 也提供了一些高级的频率，如 `BQ` 表示商务季度结束，`BM` 表示商务月末。还可以使用 `W-MON` 表示每周的周一，`BQS-JAN` 表示每个季度的第一个工作日。

在 Pandas 中，还可以使用`freq`参数或`offset-aliases`参数来指定时间序列的频率，在时间序列的重采样和频率转换中非常有用。

3，默认情况下，date_range保留开始或结束时间戳的时间

```
pd.date_range('2012-05-02 12:56:31', periods=5)
```

如果想要生成标准化为零点的时戳，有一个normalize选项可以实现这个功能

```
pd.date_range(['2012-05-02', '2012-05-03', '2012-05-04', '2012-05-05', '2012-05-06'],
			dtype='datetime64[ns]', freq='D')
```



#### 11.3.2 频率和日期偏置

pandas中的频率是由基础频率和倍数组成的，基础频率通常会有字符串别名，例如’M‘代表月，’H'代表小时，

对于每个基础频率，都有一个对象可以被用于定义日期偏置，例如：每小时的频率可以使用Hour类来表示

```
from pandas.tseries.offsets import Hour, Minute
hour = Hour()
hour
```

你可以传递一个整数来定义偏置量的倍数

```
four_hours = Hour(4)
four_hours
```

在大多数的应用中，你都不需要显示地创建这些对象，而是使用字符串别名，如‘H’或‘4H’

在基础频率前放一个整数就可以生成倍数

```
pd.date_range('2000-01-01', '2000-01-03 23:59', freq='4h')
```

多个偏置可以通过加法进行联合

```
Hour(2) + Minute(30)
```

也可以传递频率字符串，例如‘1h30min’将会有效地转换为同等的表达式

```
pd.date_range('2000-01-01', periods=10, freq='1h30min')
```



##### 11.3.2.1 月中某星期的日期

以‘WOM’开始，它允许你可以获取每月第三个星期五这样的日期

```
rng = pd.date_range('2012-01-01', '2012-09-01', freq='WOM-3FRI')
list(rng)
```



#### 11.3.3 移位（前向和后向）日期

1，`shift()` 是 Pandas 库中的一个方法，用于将时间序列数据按照指定的偏移量进行移动。这个方法非常有用，可以用于计算时间序列数据的差分、滞后值等。

下面是一个简单的示例，演示了如何使用 `shift()` 方法：

```python
import pandas as pd

# 创建一个示例数据
data = {'value': [10, 20, 30, 40, 50]}
df = pd.DataFrame(data)

# 使用 shift() 方法将数据向后移动一行
df['shifted'] = df['value'].shift(1)
print(df)
```

在这个示例中，我们创建了一个包含值为`10, 20, 30, 40, 50`的数据框。然后我们使用 `shift(1)` 方法将 `value` 列向后移动了一行，并将结果保存到一个新列 `shifted` 中。这样，我们就得到了一个滞后一期的数据列。

`shift()` 方法可以接受一个整数作为参数，用于指定移动的距离。如果参数是正数，表示向下移动；如果是负数，表示向上移动。

2，除了基本的移动功能，`shift()` 方法还可以用于计算时间序列数据的差分，例如计算一阶差分（当前值减去上一期的值）：

```python
# 计算一阶差分
df['diff'] = df['value'] - df['value'].shift(1)
print(df)
```

这样就可以得到原始数据的一阶差分列。

3，可以用shift来推移时间戳

```
ts = pd.Series(np.random.randn(4),
			index=pd.date_range('1/1/2000', periods=4, ferq='M'))
ts
ts.shift(2, freq='M')
ts.shift(1, freq='D')
ts.shift(1, freq='90T')
```



## 第十二章：高阶pandas

### 12.1 分类数据

#### 12.1.1 背景和目标

1，unique函数允许我们从一个数组中提取不同值

value_counts允许我们从一个数组提取不同值后分别计算这些不同值的频率

```
import numpy as np
import pandas as pd
values = pd.Series(['apple', 'orange', 'apple', 'apple'] * 2)
values
```

```
pd.unique(values)
```

 ```
 pd.value_count(values)
 ```

2，在数据入库的操作中，使用维度表，维度表包含了不同值，并将主要观测值存储为引用维度表的整数键

```
values = pd.Series([0, 1, 0, o] * 2)
dim = pd.Series(['apple', 'orange'])
values
dim
```

使用take方法来恢复原来的字符串Series

```
dim.take(values)
```

按照整数展现的方式被称为分类或字典编码展现，不同值的数组可以被称为数据的类别，字典或层级



####  12.1.2 pandas中的Categorical类型

python拥有特殊的Categorical类型，用于承载基于整数的类别展示或编码的数据

```
fruits = ['apple', 'orange', 'apple', 'apple'] * 2
N = len(fruits)
df = pd.DataFrame({'fruit': fruits,
				'basket_id': np.arange(N),
				'count': np.random.randint(3, 15, size=N),
				'weight': np.random.uniform(0, 4, size=N)},
				columns=['basket_id', 'fruit', 'count', 'weight'])
df
```

这里，df['fruit']是一个python字符串对象组成的数组，我们可以通过调用函数将它转化为Categorlcal对象

```
fruit_cat = df['fruit'].astype('category')
fruit_cat
```

fruit_cat的值并不是Numpy数组，而是pandas。Categorical的实例

```
c = fruit_cat.values
type(c)
```

Categorical对象拥有categorical和codes属性

```
c.categories
c.codes
```

你可以通过分配已转换的结果将DataFrame的一列转换为Categorical对象

```
df['fruit'] = df['fruit'].astype('category')
df.fruit
```

你也可以从其他python序列类型直接生成pandas。Categorical

```
my_categories = pd.Categorical(['foo', 'bar', 'baz', 'foo', 'bar'])
my_categories
```

如果你已经从另一个数据来源获得了分类编码数据，你可以使用from_codes构造函数

```
categories = ['foo', 'bar', 'baz']
codes = [0, 1, 2, 0 , 0, 1]
my_cats_2 = pd.Categorical.from_codes(codes, categories)
my_cats_2
```

当使用from_codes构造函数时，你可以为类别指定一个有意义的顺序

```
ordered_cat = pd.Categorical.from_codes(code, categories, ordered=True)
ordered_cat
```

输出的[foo<bar<baz]表明‘foo’的顺序在‘bar’之前，一个未排序的分类实例可以使用as_ordered进行排序

```
my_cats_2.as_ordered()
```



### 12.1.4 分类方法

```
s = pd.Series(['a', 'b', 'c', 'd'] * 2)
cat_s = s.astype('category')
cat_s
```

特殊属性cat提供了对分类方法的访问

```
cat_s.cat.codes
cat_s.cat.categories
```

假设我们知道该数据的实际类别集合超过了数据中观察到的四个值，我们可以使用set_categories

方法来改变类别

```
actual_categories = ['a', 'b', 'c', 'd', 'e']
cat_s2 = cat_s.cat.set_categories(actual_categories)
cat_s2
```

```
cat_s.value_counts()
cat_s2.value_counts()
```

可以使用remove_unused_categories方法来去除未观察到的类别

```
cat_s3 = cat_s[cat_s.isin(['a', 'b'])]
cat_s3
cat_s3.cat.remove_unused_categories()
```

pandas中的Series的分类方法

add_categories:将新的类别（未使用过的）添加到已有类别的尾部

as_ordered:对类别排序

as_unordered:使类别无序

remove_categories:去除类别，将被移除的值置为null

remove_unused_categories:去除所有没有出现在数据中的类别

rename_categories:使用新的类别名称代替现有的类别，不会改变类别的数量

reorder_categories:与renaem_categories类似，但结果是经过排序的类别

set_categories:用指定的一组新类别代替现有类别，可以添加或删除类别



### 12.2 高阶GroupBy应用

#### 12.2.1 分组转换和‘展开’Groupby

```
df = pd.DataFrame({'key': ['a', 'b', 'b', 'c'] * 4,
				'value': np.arange(12)})
df
```

按‘key’分组的均值

```
g = df.groupby('key').value
g.mean()
```

假设我们 想要产生一个Series，它的尺寸和df['value']一样，但值都被当‘key’分组的均值替代，我们可以

向transfrom传递匿名函数lamda x：x.mean（）

```
g.transform(lambda x: x.mean())
```

对于内建的聚合函数，我们可以像groupby的agg方法一样传递一个字符串别名

```
g.transform('mean')
```

与apply类似，transform可以返回Series的函数一起使用，但结果必需和输入有想相同的大小，

例如我们可以使用lambda函数给每个组乘以2

```
g.transform(lambda x: x * 2)
```

考虑一个由简单聚合构成的分组转换函数：

```
def normalize(x):
	return (x - x.mean()) / x.std()
```

我们使用transform或apply可以获得等价的结果

```
g.transform(normalize)
g.apply(normalize)
```

内建的聚合函数通常比apply更快

```
g.transform('mean')
normalized = (df['value'] - g.transfomrm('mean')) / g.transform('std')
normalized
```

补充：要将DataFrame转换为Numpy数组，使用.values属性

```
import pandas sd pd
import numpy as np
data = pd.DataFrame({'x0'：[1, 2, 3, 4, 5],
					'x1': [0.01, -0.01, 0.25, -4.1, 0.1],
					'y': [-1.5, 0, 3.6, 1.3, -2.2],})
data
data.columns
data.values
```

