# python 常用库和函数

## 一，python 常用函数和方法

 ### 1，数据类型

#### （1）数字类型及操作

max(x1, x2, x3...) :返回最大值

min(x1, x1, x3...) :返回最小值

round(x, d) :四舍五入，d是保留小数位数，默认值为0

pow(x, y) :函数计算X^y

int(x) :转换为整数类型

float(x): 转换为浮点数类型

补充：python中无穷大的表示方法：

float('inf')

输出结果为inf表示无穷大

complex(x): 转换为复数类型

#### （2）字符串类型及操作

len(x): 返回字符串x的长度

str(x): 转换为字符串类型

hex(x): 将整数x转换为十六进制的字符串形式

oct(x): 将整数x转换为八进制的字符串形式



str.split(sep): 根据分隔符返回一个列表，sep可指定分隔符，默认空格

str.replace(old, new): 返回字符串str的副本，所有old子串替换为new子串

str.count(sub): 返回子串sub在str中出现的次数

str.center(width, fillchar): 对字符串进行操作，根据宽度width居中，fillchar填补可选

eg："python".center(8, '=')

"=python="

str.strip(chars): 从str中去掉在其左侧和右侧chars中列出的字符

eg: "=python=".strip("=np")

"ytho"

str.jion(iter): 在iter变量除最后元素外每个元素后增加一个str#主要用于字符串分隔

eg: ','.jion("12345")

"1, 2, 3, 4, 5"

补充：`print()` 函数的 `end` 参数用于指定打印完数据后的结尾字符。它的作用是控制 `print()` 函数在输出数据后如何结束，而默认情况下，`print()` 会在输出数据后添加一个换行符 `'\n'`。通过设置 `end` 参数，可以改变这个默认行为。

### `end` 参数的基本用法

```
print('hello', end=" ")
print('world')

输出：
hello world
```

- **`end`**：指定在输出结束后添加的字符或字符串，默认为换行符 `'\n'`。



### 2，组合数据类型

#### (1)字典

del d[k]: 删除字典d中键k对应的数据值

k in d: 判断键k是否在字典d中，在返回true，否则false

d.keys(): 返回字典d中所有键的信息

d.values():              ----------值的信息

d.items():               -----------键值对信息

d.get(k, <default>): 键k存在，则返回相应值，否则返回<default>值

d.pop(k, <default>): 键k存在，则取出相应值，否则返回<default>值

d.popitem(): 随机从字典d中取出一个键值对以元组形式返回

d.clear(): 删除所有键值对

len(d): 返回字典d中元素的个数

#### (2)集合

s.copy(): 返回集合s的一个副本

len(s): 返回集合s的元素个数

x in s: 判断s中的元素x，x 在s中返回true否则返回false

x not in s: 判断s中的元素x，x 不在s中返回true否则返回false

set(x): 将其他类型变量x转换为集合类型

s.add(x): 如果x不在集合s中，将x增加到s

s.discard(x): 移除s中的元素x，如果x不在集合s中，不报错

s.remove: 移除s中的元素x，如果x不在集合s中，产生异常

s.clear(): 移除s中所有元素

s.pop(): 随机返回s的一个元素，更新s，若s为空产生异常

#### (3)列表

len(s): 返回序列s的长度，即元素 个数

min(s): 返回序列中的最小值

max(s): 返回序列中的最大值

s.index(x) or s.index(x, i, j): 返回序列s从i开始到j位置中第一次出现元素x的位置

s.count(x): 返回序列s中出现x的总次数

ls.append(x): 在列表ls最后增加一个元素x

ls.clear(): 删除列表ls中所有元素

ls.copy(): 生成一个新列表，赋值ls中所有元素

del ls[i]: 删除ls中的元素i

del ls[i:j:k]: 根据指定的步长k从ls的i到j中删除元素

ls.insert(i, x): 在列表ls的第i位置增加元素x

ls.pop(i): 将列表ls中第i位置元素取出

ls.remove(x): 将列表ls中出现的第一个元素x删除

ls.reverse(): 将列表ls中的元素反转

补充：

enumerate函数

将一个可遍历的数据对象（如列表，字符串等）组合为一个索引序列，同时列出数据和数据下标

```
data = ['apple', 'banana', 'pear']
for idx, datum in enumerate(data):
	print(f"索引:{idx}, 数据:{datum}")
```

#### (4)元组

在Python中，`tuple()` 函数用于创建一个新的元组。元组是不可变的序列数据类型，类似于列表，但一旦创建，就不能修改其内容。`tuple()` 函数可以接受一个可迭代对象，并将其转换为元组。如果没有传入参数，`tuple()` 函数将返回一个空的元组。

语法

```
python
复制代码
tuple([iterable])
```

- `iterable`（可选）：一个可迭代对象，例如列表、字符串或其他序列。`tuple()` 函数将这个可迭代对象的元素转换为元组的元素。

示例

1. 从列表创建元组

```
my_list = [1, 2, 3, 4, 5]
my_tuple = tuple(my_list)
print(my_tuple)
# 输出: (1, 2, 3, 4, 5)
```

2. 从字符串创建元组

```
my_string = "hello"
my_tuple = tuple(my_string)
print(my_tuple)
# 输出: ('h', 'e', 'l', 'l', 'o')
```

3. 空元组

```
empty_tuple = tuple()
print(empty_tuple)
# 输出: ()
```

4. 单元素元组

创建一个单元素的元组时，需要在元素后面加一个逗号：

```
single_element_tuple = (1,)
print(single_element_tuple)
# 输出: (1,)

# 不加逗号会被解释为普通括号中的值
not_a_tuple = (1)
print(not_a_tuple)
# 输出: 1
```

常见用途

1. **不可变数据**：元组用于存储不可变的数据，适合需要保护数据不被修改的场景。
2. **字典键**：由于元组是不可变的，可以作为字典的键，而列表则不能。
3. **多返回值**：函数可以通过返回元组来返回多个值。

示例：多返回值

```
def get_min_max(numbers):
    return (min(numbers), max(numbers))

min_val, max_val = get_min_max([3, 1, 4, 1, 5, 9])
print("Min:", min_val)
print("Max:", max_val)
```

## 二，python常用库

### 1，常用标准库

1，time模块：处理时间



时间获取：

（1）,time.time()

返回当前的时间戳（从1970年1月1日到现在的秒数）

（2）,time.ctime()

获取当前时间，并以易读方式表示

（3），time.gmtime()

获取当前时间，表示为计算机可处理的时间格式



时间格式化：

（1），strftime(tpl, ts)

tpl是格式化模板字符串，用来定义输出结果

ts是计算机内部的时间变量

%Y：年份

%m：月份-----%B：月份名称-----%b：月份名称缩写

%d：日期

%A：星期-----%a：星期缩写

%H：小时（24H）-----%I：小时（12H）

%p：上/下午

%M：分钟

%S：秒

eg：time.strftime("%Y-%m-%d %H:%M:%S")

（2），strptime(str, tpl)

str是字符串形式的时间值

eg：timestr = '2018-01-26'

time.strptime(timestr, '%Y-%m-%d')



程序计时

（1），perf_counter()

返回一个cpu级别的精确时间计数值，单位为秒，连续调用差值才有意义

eg：star = time.perf_counter()

end = time.perf_counter()

end - star

（2），sleep(s)

使程序休眠s秒数

eg：def wait():

​			time.sleep(3.3)

​		wait()





3，random模块:使用随机数

基本随机函数

（1），seed(a = None): 初始化给定的随机数种子，默认为当前系统时间

random.seed(10)      #产生种子10对应的序列

（2），random():生成一个[0.0, 10]之间的随机小数

random.random()



拓展随机函数

（1），randint(a, b):生成一个[a, b]之间的整数

random.randint(10, 100)

（2），randrange(m, n, k):生成一个(m， n)之间以k为步长的随机数

random.randrange(10, 100, 10)

（3），uniform(a, b):生成一个[a, b]之间的随机小数

random.uniform(10, 100)

（4），choice(seq):从序列seq中随机选择一个元素

random.choice([1, 2, 3, 4, 5])

（5），shuffle(seq):将序列seq中元素随机排列，返回打乱后的序列

random.shuffle([1, 2, 3, 4, 5])





4，collections模块:拓展和补充内置数据结构

（1），deque：提供了双端队列的实现

``` 
from colltions import deque
d = deque([1, 2, 3])
d.append(4)		#从右侧添加
d.appendleft(0)		#左侧添加
d.pop()		#从右侧删除
d.popleft()		#从左侧删除
print(d)
```

（2），namedtuple：创建具名元组，具名元组可以让你用字段名而不是位置来访问元组的元素

```
from collections import namedtuple
Person = namedtuple('Person', ['name', 'age'])
p = Person(name='Alice', age=30)
print(p.name)
print(p.age)
```

可以用于表示各种结构化数据

```
# 表示一个学生记录
Student = namedtuple('Student', ['id', 'name', 'grade'])
student1 = Student(id=1, name='John', grade='A')
student2 = Student(id=2, name='JAne', grade='b')
#打印学生记录
print(student1)
print(student2)
```



6，os模块:提供了一种与操作系统交互的方式



7，re模块：提供了对正则表达式的支持

补充:正则表达模式

正则表达式使用一些特殊的字符来定义模式，

. :匹配任何字符（除了换行符）

^ :匹配字符串的开始

$ :匹配字符串的结束

\* :匹配前面的元素零次或多次

\+ :匹配前面的元素一次或多次

? :匹配前面的元素零次或一次

{n} :匹配前面的元素恰好多次

{n, m} :匹配前面的元素至少n次，但不超过m次

[] :匹配括号内的任意字符

\d :匹配任何数字，相当于[0-9]

\D :匹配任何非数字字符

\w :匹配任何字母数字字符，相当于[a-z A-Z 0-9]

\W :匹配任何非字母数字字符

\s :匹配任何空白字符（如空格、换行符）

\S :匹配任何非空白字符



（1），re.match()

从字符串开头匹配一个模式

```
import re
result = re.match(r'\d+', '123abc')
if result:
    print(result)
```

（2），re.search()

在整个字符串中搜索一个模式，返回第一个匹配的结果

```
import re
result = re.search(r'\d+', 'afa123abc')
if result:
    print(result)
```

（3），re.findall()

返回字符串中所有匹配的结果，以列表形式返回

```
import re
result = re.findall(r'\d+', 'afa123abc6887hfaihf986hai')
if result:
    print(result)
```

（4），re.sub()

用于替换匹配的字符串

```
import re
result = re.sub(r'\d+','www', 'afa123abc6887hfaihf986hai')
if result:
    print(result)
```

（5），re.split()

按照匹配的模式分隔字符串

```
import re
result = re.split(r'\d+', 'afa123abc6887hfaihf986hai')
if result:
    print(result)
```





8，itertools模块: 处理迭代器的工具



9，json模块：处理json数据



10，math模块：数学计算模块

1. **数学常量**:
   - `math.pi`：圆周率 π，约等于 3.14159。
   - `math.e`：自然对数的底数 e，约等于 2.71828。
2. **基本数学函数**:
   - `math.sqrt(x)`：返回 x 的平方根。
   - `math.exp(x)`：返回 e 的 x 次方。
   - `math.log(x, base)`：返回以 base 为底的 x 的对数，如果省略 base，则返回自然对数（以 e 为底）。
   - `math.log10(x)`：返回以 10 为底的 x 的对数。
   - `math.factorial(x)`：返回 x 的阶乘。
3. **三角函数**:
   - `math.sin(x)`：返回 x（弧度）的正弦值。
   - `math.cos(x)`：返回 x（弧度）的余弦值。
   - `math.tan(x)`：返回 x（弧度）的正切值。
   - `math.asin(x)`：返回 x 的反正弦值（弧度）。
   - `math.acos(x)`：返回 x 的反余弦值（弧度）。
   - `math.atan(x)`：返回 x 的反正切值（弧度）。
4. **角度和弧度转换**:
   - `math.degrees(x)`：将弧度转换为角度。
   - `math.radians(x)`：将角度转换为弧度。
5. **其他函数**:
   - `math.ceil(x)`：返回大于等于 x 的最小整数。
   - `math.floor(x)`：返回小于等于 x 的最大整数。
   - `math.fabs(x)`：返回 x 的绝对值。
   - `math.modf(x)`：返回 x 的小数部分和整数部分。

示例代码:

```
import math

# 计算平方根
print(math.sqrt(16))  # 输出: 4.0

# 计算自然对数
print(math.log(math.e))  # 输出: 1.0

# 计算正弦值
print(math.sin(math.radians(30)))  # 输出: 0.49999999999999994

# 计算圆周率
print(math.pi)  # 输出: 3.141592653589793

# 计算阶乘
print(math.factorial(5))  # 输出: 120

# 使用角度和弧度转换
print(math.degrees(math.pi))  # 输出: 180.0
print(math.radians(180))  # 输出: 3.141592653589793

```

补充：`if __name__ == "__main__":` 的作用是判断当前模块是否是主程序。如果是主程序，则执行下方的代码；如果是作为模块被导入，则不执行下方的代码。



## 三，python进阶积累

### 1,`typing` 模块

(1)`typing` 是 Python 提供的用于 **类型标注（type hinting）** 的标准库

(2)`from typing import TypedDict`

`TypedDict` 相比普通的dict,可以直接声明一个字典应该包含哪些字段，以及它们的类型

eg:

```
class UserInfo(TypedDict):
    name: str
    age: int
    
"""
UserInfo 定义的是一个数据类型，而不是一个传统意义上的类。

这个 UserInfo 类型要求：
必须是一个字典，并且：
有字段 name，类型是 str
有字段 age，类型是 int
"""
```

正确示例：

```
user: UserInfo = {"name": "Alice", "age": 30}
表示变量 user 的类型是 UserInfo
```

错误示例：

```
user = {"name": "Bob", "age": "not a number"}  # ❌ age 应该是 int
```

(3)`from typing import Annotated`

- `Annotated` 允许为类型注解添加附加的“标签”或“元数据”。
- **附加的信息不会影响类型本身**，它只是为类型注解提供更多上下文或额外信息。
- 元数据通常会被外部工具（比如类型检查器、框架、库）读取和利用

```
from typing import Annotated

# type 是你想注解的类型。 
# annotations 是你希望附加的附加元数据（可以是一个或多个）。

Annotated[type, *annotations]
```

eg:

```
from typing import Annotated

# 使用 Annotated 为 int 类型添加元数据
score: Annotated[int, "score of the student"]
```

### 2，python异步编程

（1）异步和同步

**同步（Synchronous）**

同步是指代码按顺序执行，当前一行代码执行完成后才会执行下一行。每次执行的操作都会“阻塞”程序的进一步执行，直到当前操作完成。

**异步（Asynchronous）**

异步是指代码的执行不必等待某个任务完成，而是可以继续执行其他任务。当遇到需要等待的操作（如网络请求、文件读取等）时，程序可以暂时让出控制权，去执行其他操作，而不是一直停在那里等着。



（2）异步

导入模块

```
import asyncio
```

`async def`：用于定义一个异步函数。

`await`：用于调用一个异步操作（只能在异步函数内使用），会暂停当前协程的执行，直到等待的任务完成。(相当于告诉事件循环"这个操作需要时间，你先去处理其他任务")

```
# 例子
import asyncio

async def task_1():
    print("Task 1 started")
    await asyncio.sleep(2)
    print("Task 1 completed")

async def task_2():
    print("Task 2 started")
    await asyncio.sleep(1)
    print("Task 2 completed")

async def main():
    # 同时运行 task_1 和 task_2
    await asyncio.gather(task_1(), task_2())

# 执行
asyncio.run(main())

# 输出
Task 1 started
Task 2 started
Task 2 completed
Task 1 completed

```

### 3,`hasattr` 函数

 Python 内置的函数，用来检查一个对象是否具有某个属性。

```
if hasattr(message, "tool_calls")
# 这里检查 message 对象是否有名为 tool_calls 的属性
```



### 4，列表推导式

[表达式 for 变量 in 可迭代对象 if 条件]

**Python 会先读取“我对每一项要干嘛”（即表达式），然后才去一项一项地迭代执行**。

```
docs = [
    Document(
        page_content=row["description"],
        metadata={
            "culture_id": row["culture_id"],
            "area_id": row["area_id"],
            "area_name": row["area_name"],
            "culture_name": row["culture_name"]
        }
    )
    for row in rows if row["area_id"] == "yn_4"
]

```

创建一个列表 `docs`，用于存储多个 `Document` 对象。每一个 `Document` 对象包含两部分：

- `page_content`：正文内容，这里用的是 `description` 字段，即文化描述。
- `metadata`：附带的元数据，比如文化ID、地区ID、名称等。



