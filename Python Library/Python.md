### 列表的基本操作

> ### 增
>
> > * 用extend方法
> >
> >   ```python
> >   >>> l = [1, 2, 3]
> >   >>> j = [4, 5, 6]
> >   >>> l.extend(j)
> >   >>> l
> >   [1, 2, 3, 4, 5, 6]
> >   ```
> >
> > * 用运算符+对列表直接进行拼接
> >
> >   ```python
> >   >>> l = [1, 2, 3]
> >   >>> j = [4, 5, 6]
> >   >>> l + j
> >   [1, 2, 3, 4, 5, 6]
> >   ```
> >
> >   **extend 方法是直接在原列表上直接新增，a 列表直接被新增，extend 方法返回的是 None。而用运算符相加的话，相加的结果新开辟一个内存来存放新的列表。**
> >
> > * 用Insert方法再对应的位置上插入新的对象
> >
> >   ```python
> >   >>> num = [1, 2, 4, 5]
> >   >>> num.insert(2, 'three')
> >   >>> num
> >   [1, 2, 'three', 4, 5]
> >   ```
> >
> > * 用切片的方式将一个对象插入到列表中
> >
> >   ```python
> >   >>> num = [1, 2, 4, 5]
> >   >>> num[2:2] = ['three']
> >   >>> num
> >   [1, 2, 'three', 4, 5]
> >   ```
>
> ### 删
>
> > * 用pop方法来删除列表中的元素，pop方法默认删除最后一个元素,也可以指定索引位置删除元素
> >
> >   ```python
> >   # 默认删除最后一个元素
> >   >>> num = [1, 2, 4, 5]
> >   >>> num.pop()
> >   5
> >   >>> num
> >   [1, 2, 4]
> >   
> >   
> >   # 指定索引位置删除元素
> >   >>> num = [1, 2, 4, 5]
> >   >>> num.pop(1)
> >   2
> >   >>> num
> >   [1, 4, 5]
> >   ```
> >
> > * remove()方法删除元素,如果删除元素多的话，指挥删除先出现的那个元素，其他的不会删除
> >
> >   ```python
> >   >>> num = [1, 2, 4, 5, 4]
> >   >>> num.remove(4)
> >   >>> num
> >   [1, 2, 5, 4]
> >   ```
> >
> > * 如果需要清空列表元素，用clear方法
> >
> >   ```python
> >   >>> l = [1, 2, 3]
> >   >>> l.clear()
> >   >>> l
> >   []
> >   ```
>
> ### 改
>
> > * 修改列表中的元素，直接用下标复制替换就可以了
> >
> >   ```python
> >   >>> num = [1, 2, 4, 5]
> >   >>> num[1] = 'two'
> >   >>> num
> >   [1, 'two', 4, 5]
> >   ```
>
> ### 查
>
> > * 查询列表中的元素我们也可以用下下标直接查看
> >
> >   ```python
> >   >>> num = [1, 2, 4, 5]
> >   >>> num[0]
> >   1
> >   ```
> >
> > * 我们也可以查询具体元素对应的索引值，用index方法
> >
> >   ```python
> >   >>> l = ['are', 'you', 'ok']
> >   >>> l.index('you')
> >   1
> >   ```
> >
> > * 排序
> >
> >   * **对列表中的元素进行升降序的排列**
> >
> >     这个可以用sort和sorted方法。sort() 直接返回为 None，它直接在原列表上进行排序，原列表改变了，sorted 会开辟一个新的内存空间来存放排序好的列表。sort 和 sorted 默认都是升序排列的，如果想降序呢？那也简单，把 reverse 参数改成 True就搞定，这个参数默认为 False。
> >
> >   * **让列表元素顺序颠倒**
> >
> >     > * 用reverse()方法
> >     >
> >     >   ```python
> >     >   >>> num = [1, 2, 3, 4, 5]
> >     >   >>> num.reverse()
> >     >   >>> num
> >     >   [5, 4, 3, 2, 1]
> >     >   
> >     >   ```
> >     >
> >     > * 用切片的方法
> >     >
> >     >   ```python
> >     >   >>> num = [1, 22, 45, 99, 49]
> >     >   >>> num[::-1]
> >     >   [49, 99, 45, 22, 1]
> >     >   ```

### Python判断字符串是否包含特定的子字符串的方法



#### 使用 in 和 not in

`in`和`not in`在 Python 中是很常用的关键字，我们将它们归类为`成员运算符`。

使用这两个成员运算符，可以很让我们很直观清晰的判断一个对象是否在另一个对象中，示例如下：

```python
>>> "llo" in "hello, python"
True
>>>
>>> "lol" in "hello, python"
False
```





*****



#### 使用 find 方法

使用 字符串 对象的 find 方法，如果有找到子串，就可以返回指定子串在字符串中的出现位置，如果没有找到，就返回`-1`

```python
>>> "hello, python".find("llo") != -1
True
>>> "hello, python".find("lol") != -1
False
>>
```





****



#### 使用 index 方法

字符串对象有一个 index 方法，可以返回指定子串在该字符串中第一次出现的索引，如果没有找到会抛出异常，因此使用时需要注意捕获。

```python
def is_in(full_str, sub_str):
    try:
        full_str.index(sub_str)
        return True
    except ValueError:
        return False

print(is_in("hello, python", "llo"))  # True
print(is_in("hello, python", "lol"))  # False
```





*****



#### 使用 count 方法

利用和 index 这种曲线救国的思路，同样我们可以使用 count 的方法来判断。

只要判断结果大于 0 就说明子串存在于字符串中。

```python
def is_in(full_str, sub_str):
    return full_str.count(sub_str) > 0

print(is_in("hello, python", "llo"))  # True
print(is_in("hello, python", "lol"))  # False
```





***



#### 通过魔法方法

在第一种方法中，我们使用 in 和 not in 判断一个子串是否存在于另一个字符中，实际上当你使用 in 和 not in 时，Python 解释器会先去检查该对象是否有`__contains__`魔法方法。

若有就执行它，若没有，Python 就自动会迭代整个序列，只要找到了需要的一项就返回 True 。

示例如下；

```python
>>> "hello, python".__contains__("llo")
True
>>>
>>> "hello, python".__contains__("lol")
False
>>>
```

这个用法与使用 in 和 not in 没有区别，但不排除有人会特意写成这样来增加代码的理解难度。



***



#### 借助 operator

operator模块是python中内置的操作符函数接口，它定义了一些算术和比较内置操作的函数。operator模块是用c实现的，所以执行速度比 python 代码快。

在 operator 中有一个方法`contains`可以很方便地判断子串是否在字符串中。

```python
>>> import operator
>>>
>>> operator.contains("hello, python", "llo")
True
>>> operator.contains("hello, python", "lol")
False
>>> 
```





***



#### 使用正则匹配

说到查找功能，那正则绝对可以说是专业的工具，多复杂的查找规则，都能满足你。

对于判断字符串是否存在于另一个字符串中的这个需求，使用正则简直就是大材小用。

```python
import re

def is_in(full_str, sub_str):
    if re.findall(sub_str, full_str):
        return True
    else:
        return False

print(is_in("hello, python", "llo"))  # True
print(is_in("hello, python", "lol"))  # False
```



***



### Python常用模块

#### os

```python
os.getcwd() #获取当前的工作目录
os.listdir() #获取当前文件夹内文件内所有的名称
print(__file__) #获取当前工作目录下的所有文件名（也就是说以绝对路径显示）
os.path.split(文件路径) #在最后一个斜杠处分割
os.path.join(os.getcwd(),文件名) #拼接路径
```

#### time

```python
time.localtime() #打印当地时间
time.gmtime() #打印世界时间
time.strftime("%Y-%m-%d %A %H:%M:%S",时间对象) #格式化时间字符串
time.time() #Python time time() 返回当前时间的时间戳（1970纪元后经过的浮点秒数）。
time.sleep(秒数) #让程序睡眠相应的时间
```

#### copy

```python
a = [1,2,3,4,5,6]
b = a # 指针仍然只向a
print(b) #[1,2,3,4,5,6]
c = copy(a) #此时是浅拷贝，值拷贝一维数据，但不拷其他维度的数据
d = deepcopy(a) #此时是深拷贝
```



***



### Python中的魔法方法

#### "\_\_call\_\_"

```python
class Sum:
    def __init__(self, x, y):
        self._x = x
        self._y = y

    def add(self):
        return self._x + self._y

    def __call__(self):
        return self.add()

# 初始化sum类
sum = Sum(1, 2)
# 常规方法调用add方法
print(sum.add())
# 使用Python的魔法方法来调用add方法
print(sum())
# callable方法是检验一个对象是否可以被调用。如果返回 True，object 仍然可能调用失败；但如果返回 False，调用对象 object 绝对不会成功。对于函数、方法、lambda 函式、 类以及实现了 __call__ 方法的类实例, 它都返回 True。
print(callable(sum))
```



*****



### Python中的冷知识

#### python中的inf

Python中可以用如下方式表示正负无穷：

```python
float("inf"), float("-inf")
```

***

#### set_printoptions方法

**Python3中想要打印完整的numpy数组a而不截断，通过numpy中的set_printoptions()方法可以实现**

set_printoptions方法的定义如下：

```python
set_printoptions(precision=None, threshold=None, edgeitems=None, 
				linewidth=None, suppress=None, nanstr=None, infstr=None)
```

* precision : int, 可选，float输出的精度，即小数点后维数，默认8。
* threshold : int, 可选，当数组数目过大时，设置显示几个数字，其余用省略号。
* edgeitems : int, 可选，边缘数目，每个维度开始和结束时摘要中的数组项数，默认3
* linewidth : int, 可选，用于确定每行多少字符数后插入换行符，默认为75。
* suppress : bool, 可选，是否压缩由科学计数法表示的浮点数，默认False。
* nanstr : str, 可选，浮点非数字的字符串表示形式，默认nan
* infstr : str, 可选，浮点无穷大的字符串表示形式，默认inf