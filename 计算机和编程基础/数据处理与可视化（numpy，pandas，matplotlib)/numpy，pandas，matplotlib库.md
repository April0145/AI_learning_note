# numpy，pandas，matplotlib库

## 一，numpy

`NumPy` 是 Python 中一个强大的数值计算库，广泛应用于科学计算、数据分析、机器学习等领域。`NumPy` 主要提供了以下功能：

- **多维数组**（`ndarray`）：提供高效的存储和操作大规模数据的方式。
- **数学函数**：包括线性代数、傅里叶变换、随机数生成等。
- **广播机制**：支持不同形状数组之间的操作。



#### 1. **`numpy.ndarray`**

- `ndarray` 是 `NumPy` 中的核心数据结构，用于存储多维数组。它支持快速的向量化操作。
- 每个 `ndarray` 对象都有一个 `dtype`（数据类型）和 `shape`（形状）属性。

```
import numpy as np
arr = np.array([1, 2, 3, 4])  # 创建一维数组

print(arr.shape)  # 数组形状
print(arr.dtype)  # 数组数据类型
print(arr.ndim)   # 数组的维度
```



#### 2. 数组的创建

- `np.array()` 用于创建数组，可以通过列表、元组、其他 `ndarray` 等数据结构来创建 `ndarray`。

```
arr = np.array([1, 2, 3, 4])
```

- `np.zeros()`创建一个元素全为零的数组。

```
arr = np.zeros((3, 4))  
```

- `np.ones()`创建一个元素全为一的数组。

```
arr = np.ones((2, 3)) 
```

- `numpy.arange()`创建一个指定范围内的数组，类似于 Python 中的 `range`，但返回的是 `ndarray` 对象。

```
arr = np.arange(0, 10, 2)  # 创建一个从 0 到 10，步长为 2 的数组
```



#### 3. 随机数生成

`np.random()` ：用于生成随机数，包括均匀分布、正态分布等

```
# 创建一个包含 5 个随机数的数组（0 到 1 之间）
rand_arr = np.random.rand(5)

# 创建一个标准正态分布的随机数数组
normal_arr = np.random.randn(5)

# 从给定的范围（0，10）内随机选择5个整数
randint_arr = np.random.randint(0, 10, 5)

```



#### 4，数据类型

（1）数据类型的命名

```
arr1 = np.array([1, 2, 3], dtype=np.float64)
arr1.dtype
```

（2）数据类型的转换`.astype()`

```
arr = np.array([1, 2, 3, 4, 5])
float_arr = arr.astype(np.float64)
```



#### 5，数组形状

（1）数组形状的转换`.reshape()`

```
arr = np.arange(12)
reshaped_arr = arr.reshape(3, 4)
```

（2）数组转置`.T`

```
arr = np.arange(15).reshape((3, 5))
arr.T
```



## 二，pandas 

`Pandas` 提供了非常强大的功能来处理数据，主要的两种数据结构是：

- **`Series`**：一维数组，类似于列表或字典的结合。
- **`DataFrame`**：二维表格数据结构，类似于电子表格或 SQL 表，它由多列组成，每一列可以是不同的数据类型。



#### 1，基础数据结构：`Series` 和 `DataFrame`

（1）series

```
import pandas as pd

# 从列表创建 Series
s = pd.Series([10, 20, 30, 40, 50])

# 创建 Series 时添加自定义索引
s2 = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'])
print(s2)
```

（2）dataframe

```
# 从字典创建 DataFrame
data = {'Name': ['Alice', 'Bob', 'Charlie'], 'Age': [25, 30, 35], 'City': ['New York', 'Los Angeles', 'Chicago']}
df = pd.DataFrame(data)
print(df)
```



#### 2，数据查看

```
# 查看前几行数据
print(df.head())  # 默认查看前 5 行数据

# 查看数据的维度
print(df.shape)   # 输出 (行数, 列数)

# 查看数据类型
print(df.dtypes)

# 查看数据的摘要统计信息
print(df.info())

```



#### 3，数据访问

通过 `loc` 和 `iloc` 进行行选择：

- `loc`：基于标签选择行和列。
- `iloc`：基于位置选择行和列。

```
print(df.loc[0, 'Name'])  # 选择第 1 行的 'Name' 列

print(df.loc[:, ['Name', 'City']])  # 选择所有行，'Name' 和 'City' 列

print(df[df['Age'] > 30])  # 输出 Age 大于 30 的行
```



#### 4，处理缺失值

```
# 创建包含缺失值的数据
data = {'Name': ['Alice', 'Bob', 'Charlie', None], 'Age': [25, None, 35, 40]}
df = pd.DataFrame(data)
```

（1），查看缺失值

```
# 查看缺失值
print(df.isnull().sum())  # 检查每个元素是否为 NaN
```

（2），方法一：删除缺失值

```
# 删除缺失值
print(df.dropna())  # 默认删除包含 NaN 的行
```

（3），方法二：填充缺失值

```
# 填充缺失值
train_data['HomePlanet'] = train_data['HomePlanet'].fillna(method='ffill')  # 填充缺失值
```

```
#前向填充: 用前一个有效值填充缺失值。
df.fillna(method='ffill', inplace=True) # 参数直接在原数据上修改，默认FalseF返回一个新对象

#后向填充: 用后一个有效值填充缺失值。
df.fillna(method='bfill', inplace=True)

#均值填充: 对于数值型数据，可以用该列的均值填充缺失值。
df.fillna(df.mean(), inplace=True)

#中位数填充: 如果数据分布不对称，中位数是一个更稳健的选择。
df.fillna(df.median(), inplace=True)
```

```
import pandas as pd

data = {'Name': ['Alice', 'Bob', 'Charlie', None], 'Age': [25, None, 35, 40]}
df = pd.DataFrame(data)

print(df.isnull().sum())

df['Name'] = df['Name'].fillna(method='ffill')
df['Age'] = df['Age'].fillna(df['Age'].mean())
print(df)
```



#### 5，处理重复数据

```
import pandas as pd

# 创建一个包含重复数据的 DataFrame
df = pd.DataFrame({
    'A': [1, 2, 2, 4, 5, 5],
    'B': [10, 20, 20, 40, 50, 50]
})
```

（1），检测重复数据

```
# 查找重复数据: 识别重复的行
print(df.duplicated().sum())
```

（2），删除重复数据

```
# 删除重复数据: 删除重复的行
df.drop_duplicates(inplace=True)
```



#### 6,数据载入和存储

```
df = pd.read_csv('examples/ex1.csv')

read_excel:从Excel的xls或xlsx文件中读取表格数据。
read_html:从HTML文件中读取所有表格数据。
read_json:从JSON字符串中读取数据。
```



#### 7，数据删除

```
data = data.drop('', axis=1)
```



## 三，matplotlib

#### 1，绘制折线图

```
import matplotlib.pyplot as plt

# 数据
x = [0, 1, 2, 3, 4, 5]
y = [0, 1, 4, 9, 16, 25]

# 创建一个折线图
plt.plot(x, y)

# 添加标题和标签
plt.title("Square Numbers")
plt.xlabel("X axis")
plt.ylabel("Y axis")

# 显示图形
plt.show()

```



#### 2，绘制散点图

```
import matplotlib.pyplot as plt

# 数据
x = [1, 2, 3, 4, 5]
y = [2, 4, 5, 4, 5]

# 创建散点图
plt.scatter(x, y)

# 添加标题和标签
plt.title("Scatter Plot Example")
plt.xlabel("X axis")
plt.ylabel("Y axis")

# 显示图形
plt.show()

```



#### 3，绘制柱状图

```
import matplotlib.pyplot as plt

# 数据
categories = ['A', 'B', 'C', 'D']
values = [3, 7, 2, 5]

# 创建柱状图
plt.bar(categories, values)

# 添加标题和标签
plt.title("Bar Plot Example")
plt.xlabel("Categories")
plt.ylabel("Values")

# 显示图形
plt.show()

```



#### 4，绘制直方图

```
import matplotlib.pyplot as plt
import numpy as np

# 随机数据
data = np.random.randn(1000)

# 创建直方图
plt.hist(data, bins=30)

# 添加标题和标签
plt.title("Histogram Example")
plt.xlabel("Value")
plt.ylabel("Frequency")

# 显示图形
plt.show()

```



#### 5，绘制饼图

```
import matplotlib.pyplot as plt

# 数据
sizes = [40, 30, 20, 10]
labels = ['A', 'B', 'C', 'D']

# 创建饼图
plt.pie(sizes, labels=labels)

# 添加标题
plt.title("Pie Chart Example")

# 显示图形
plt.show()
```



