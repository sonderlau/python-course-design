# 字典

## 字典的特征

- 包含若干数据项，每一项是 **键-值** 对。

- 元素之间无顺序，没有索引和位置的概念。

- 键 可以几乎可以是任何值，但不是`hashable 可哈希`[^1]的值
  
  不能用作键，如：
  - 列表 List
  - 字典 Dict



[^1]: Hashable: [术语-Hashable](https://docs.python.org/zh-cn/3/glossary.html#term-hashable)

## 字典的操作

### 创建

- 使用花括号包裹键-值对

  ```python
  foo = {'one': 1, 'two': 2}
  ```

- 使用类型构造器

  ```python
  foo = dict()
  
  foo = dict(one = 1, two = 2)
  
  # 产生的效果与第一种一样
  ```

- 使用字典推导式

  ```python
  foo = {x: x ** 2 for x in range(10)}
  ```




#### 扩展

考虑如下情况：

> 我需要统计某个只有小写字母的字符串中 26 个字母的出现次数，输出每个字母的出现次数

如果我们不对字典进行初始化，则可能会出现 `KeyError` 的错误，即字典中找不到这个键。

在 [获取](###获取)  一节中会介绍如何解决找不到键的问题。

如果我们在存储之前判断一下键是否存在，再进行储存，后面输出的时候有可能有的键是没有初始化过的，最后可能不会输出 26 个字母的结果。

因此我们需要初始化一个 26个字母作为键 且每个字母对应的值为 0 的字典。后面依次读取待统计的字符串中的字母进行存储。

```python
store = { chr(letter): 0 for letter in range(ord('a'), ord('z')) }
```

> - `chr()` 返回 Unicode 码位为对应值的字符串
> - `ord()` 返回单个 Unicode 字符码点的整数
>
> ```python
> chr(97)
> # >> 'a'
> 
> chr(8364)
> # >> '€'
> 
> ord('a')
> # >> 97
> 
> ord('€')
> # >> 8364
> ```
>
> 这两个函数互相为逆函数。



或者我们使用内置的方法 `fromkeys()` 

> `fromkeys(iterable[, value])`
>
> - `iterable` 可遍历的值
> - `value` 将值设为该值 默认为 None
>
> 这个方法会让所有 `iterable` 中的值作为 字典的键，且所有的键的值都设置为`value`
>
> 因此如果想对不同的键取不同的值，该方法则不太适合。



```python
import string

store = dict.fromkeys(string.ascii_lowercase, 0)
```

> 这里使用了 Python 内置的常量 `string.ascii_lowercase` 该值即为 `'abcdefghijklmnopqrstuvwxyz'` 字符串，使用之前需要导入 `string` 包。
>
> 其他的还有如：
>
> - `string.ascii_letters` 
>
>   'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
>
> - `string.digits`
>
>   '0123456789'
>
>   
>
> 更多信息请访问 [PythonDocs - string](https://docs.python.org/zh-cn/3/library/string.html)
>

上述的两种方法会产生相同的效果。



### 插入与修改

```python
Teledic = {"Wang": "88919112", "Liu": "89919111", "Zhang": "13091918232"}

Teledic["Wang"] = "15319198668"

Teledic["Xu"] = "88367065"

# Teledic
# 输出: {'Wang': '15319198668', 'Liu': '89919111', 'Zhang': '13091918232', 'Xu': '88367065'}
```

**存在修改，不存在插入**



### 获取

```python
Teledic = {"Wang": "88919112", "Liu": "89919111", "Zhang": "13091918232"}

print(Teledic['Zhang'])

# 13091918232

print(Teledic['han'])
```

报错： 键错误

> Traceback (most recent call last):
>
>  File "<pyshell#32>", line 1, in <module>
>
>   print(Teledic['han'])
>
> KeyError: 'han'



#### 解决方案

- 查看键是否在字典中

  ```python
  # 从键盘输入
  s = input()
  
  # 存在则返回 True
  if s in Teledic:
      print(Teledic[s])
  else:
      print("查无此人")
  
  ```

- 使用默认值

  ```python
  s = input()
  
  # 若键不存在则输出 0
  print(Teledic.get(s, 0))
  ```

  `get(key[, default])` 若不填写 `default` 则默认`default = None` 
  
  ```python
  # 获取 存在的键 的值
  Teledic.get('Wang', -1)
  # >> 88919112
  
  # 获取 不存在的键 的值
  Teledic.get('Zhao')
  # >> None
  # 返回了 default 的默认值
  
  Teledic.get('Zhao', -1)
  # >> -1
  # 返回了设定的 default 值
  ```



### setdefault

如果我想获取某个键的值，该键存在则获取值，不存在则插入一条新的键值对。

这个过程自己编写会有点复杂，我们可以使用  `setdefault` 方法简化这一操作

> `setdefault(key[, default])`
>
> 如果字典存在键 `key` 则返回它的值。若不存在，则插入值为 `default` 的键 `key`，并返回 `default` 。
>
> `default` 默认为 `None` 



```python
foo = {'one': 1, 'two': 2}

# 不指定 default 的值
three = foo.setdefault('three')
print(three)
# >> None
print(foo)
# >> {'one': 1, 'two': 2, 'three': None}


# 指定 default 的值
four = foo.setdefault('four', 4)
print(four)
# >> 4
print(foo)
# >> {'one': 1, 'two': 2, 'three': None, 'four': 4}
```





#### 题目 - 四则运算（用字典实现）

> 输入：
>
> 在一行中输入一个数字 在一行中输入一个四则运算符(+,-,*,/) 在一行中输入一个数字。

> 输出：
>
> 运算结果（小数保留2位）

```python
x = float(input())
op = input()
y = float(input())
try:
    res = {"+": "x+y", "-": "x-y", "*": "x*y", "/": "x/y"}
    print("{:.2f}".format(eval(res[op])))
except:
    print("divided by zero")

```

==`eval` 可以用于解决当前的问题，但是不是一个好的方案，因为会有安全问题==

另解：

```python
opts = {
    "+": lambda x, y: x + y,
    "-": lambda x, y: x - y,
    "*": lambda x, y: x * y,
    "/": lambda x, y: x / y,
}


a = float(input())
op = input()
b = float(input())

print("{:.2f}".format(opts[op](a, b)))

```



### 删除

```python
del Teledic['Wang']

# 删除不存在的键

del Teledic['Zhao']
```

会引发 `KeyError`

> Traceback (most recent call last):
>   File "<stdin>", line 1, in <module>
>
> KeyError: '1'



#### 解决方案

先判断键是否存在，再删除，写法类似获取

```python
# 从键盘输入
s = input()

# 存在则返回 True
if s in Teledic:
    del Teledic[s]
else:
    print("查无此人")

```

同样的，可以使用`pop(key[, default])` 方法

```python
# 删除存在的键 会返回该键
wang = Teledic.pop('Wang', -1)
# >> 88919112

# 若删除不存在的键 会返回 设置的 值
nonexistent = Teledic.pop('Zhao', -1)
# >> -1
```

若不设置`default`值且不存在该键，则会引发 `KeyError`





## 字典的遍历

- `keys()` 返回字典的 **键 key** 组成的字典视图[^1]
- `values()` 返回字典的 **值 value** 组成的字典视图[^1]
- `items()` 返回字典的 **键值对 key-value** 组成的字典视图[^1]

> 对于初学者来说，可以把 字典视图[^1] 看作是一个 List 列表。
>
> ```python
> foo = {'one': 1, 'two': 2}
> 
> print(foo.keys())
> # >> dict_keys(['one', 'two'])
> 
> print(foo.values())
> # >> dict_values([1, 2])
> 
> print(foo.items())
> # >> dict_items([('one', 1), ('two', 2)])
> ```

因此想要遍历字典，可根据需要遍历键，值，或键值对

```python
foo = {'one': 1, 'two': 2}

# 遍历键
for key in foo.keys():
    print(key)
    
# 遍历值
for value in foo.values():
    print(value)

# 遍历键值对
for key,value in foo.items():
    print(key, value)

for kv in foo.items():
    print(kv, type(kv), kv[0], kv[1])
# >> ('one', 1) <class 'tuple'> one 1
# >> ('two', 2) <class 'tuple'> two 2

# 每次输出了一个键值对组成的 Tuple
# 对 Tuple 进行索引可分别输出 键 值
```







## 题目 - 统计字符数量

输入一个字符串，输出其中出现次数最多的字符及其出现的次数。

> 输入样例：
>
> abcdsekjsiejdlsjdiejsl

> 输出样例：
>
> s 4



将未完成的程序填空：

```python
s = input()
d = ___________
for letter in s:
    if letter in d:
        ___________   
    else: 
        d[letter] = 1
        
max_value = s[0]

for key in d:
    if d[key]>d[max_value]:
        max_value = k
        
print(c, ___________)

```



参考解答

```python
s = input()
d = {}
for letter in s:
    if letter in d:
        d[letter] += 1
    else:
        d[letter] = 1

max_key = s[0]

for key in d:
    if d[key] > d[max_key]:
        max_key = key

print(max_key, d[max_key])

```

改进：

```python
s = input()
d = {}

for letter in s:
    d[letter] = d.get(letter, 0) + 1

max_key = s[0]

for key in d:
    if d[key] > d[max_key]:
        max_key = key

print(max_key, d[max_key])

```





## 题目 - 统计单词数量

> 现需要统计若干段文字(英文)中的不同单词数量。
>
> 如果不同的单词数量不超过10个，则将所有单词输出(按字母顺序)，否则输出前10个单词。
>
> 单词大小写敏感，即'word'与'WORD'是两个不同的单词 。
>
> 输入有若干行英文，最后以`!!!!!`为结束。
>
> 输出不同单词数量。 然后输出前10个单词（按字母顺序），如果所有单词不超过10个，则将所有的单词输出。



输入：

> Failure is probably the fortification in your pole
>
> It is like a peek your wallet as the thief when you
>
> are thinking how to spend several hard-won lepta
>
> when you Are wondering whether new money it has laid
>
> background Because of you, then at the heart of the
>
> most lax alert and most low awareness and left it
>
> godsend failed
>
> !!!!!

输出：

> 49
>
> Are
>
> Because
>
> Failure
>
> It
>
> a
>
> alert
>
> and
>
> are
>
> as
>
> at



参考解答

```python
d = dict()
while True:
    s = input()
    # 结束字符
    if s == "!!!!!":
        break
    # 将一行的内容分成单词 遍历每个单词
    for x in s.split():
        # 字典中已有该单词
        if x in d:
            d[x] = d[x] + 1
        # 无该单词
        else:
            d[x] = 1

# Dict 转成 List
word = list(d.items())
# 排序
word.sort()
print(len(word))
n = min(10, len(word))
for i in range(n):
    print(word[i][0])

```



解析：

- `s.split()`
  - 不指定分隔符，连续的空格会被视为单个分隔符。
  
  - ```python
    >>> '1 2 3'.split()
    ['1', '2', '3']
    
    >>> '1 2 3'.split(maxsplit=1)
    ['1', '2 3']
    
    >>> '   1   2   3   '.split()
    ['1', '2', '3']
    ```
  
- `d.items()`

  - 返回的值是一个由 `键，值` 对组成的 [字典视图 - Dict Views](https://docs.python.org/zh-cn/3/library/stdtypes.html#dict-views) 

  - 字典视图是随着字典的更改而动态更新的，字典视图是可以迭代的。

  - ```python
    dishes = {'eggs': 2, 'sausage': 1, 'bacon': 1, 'spam': 500}
    keys = dishes.keys()
    values = dishes.values()
    key_values = dishes.items()
    
    list(keys)
    # ['eggs', 'sausage', 'bacon', 'spam']
    
    list(values)
    # [2, 1, 1, 500]
    
    list(key_values)
    # [('eggs', 2), ('sausage', 1), ('bacon', 1), ('spam', 500)]
    ```





## 提高 - collections

`collections` 是 Python 中内置的一个模块，该模块里有很多有用的工具类，与字典有关如：

- `Counter` 
  - Dict 的子类，键是元素，值是元素的计数。
- `OrderedDict`
  - Dict 的子类，记录了键值插入的顺序，看起来既有链表的特性，也有字典的特性。
- `defaultdict`
  - 类似于字典，但是可以通过默认的工厂函数获得键对应的默认值，相比上面介绍的`setdefault()`方法更加高效



```python
"""
找出序列中出现次数最多的元素
"""
from collections import Counter

words = [
    'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
    'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around',
    'the', 'eyes', "don't", 'look', 'around', 'the', 'eyes',
    'look', 'into', 'my', 'eyes', "you're", 'under'
]
counter = Counter(words)
print(counter.most_common(3))
```

使用 `Counter` 可以减少很多的代码量完成统计的工作，因此上面的统计字符数量的题目可以非常简单使用该类完成。



```python
from collections import OrderedDict

d = OrderedDict.fromkeys('abcde')

# 移动键值对到末尾
d.move_to_end('b')
print(d.keys())
# >> odict_keys(['a', 'c', 'd', 'e', 'b'])

# 移动键值对到开头
d.move_to_end('b', False)
print(d.keys())
# >> odict_keys(['b', 'a', 'c', 'd', 'e'])

# 删除最后一个元素
tmp = d.popitem()
print(d.keys())
# >> odict_keys(['b', 'a', 'c', 'd'])
print(tmp)
# >> ('e', None)

# 删除第一个元素
tmp = d.popitem(False)
print(d.keys())
# >> odict_keys(['a', 'c', 'd'])
print(tmp)
# >> ('b', None)
```



```python
from collections import defaultdict

s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]

# 对于字典中没有的键，会自动执行工厂函数
# 这里指定工厂函数为 list 即会新建一个 List
d = defaultdict(list)

for k, v in s:
    # 键不存在即会自动新建，不用担心不存在的问题
    d[k].append(v)

sorted(d.items())
# >> [('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]

```



更多内容请访问: [PythonDocs - collections](https://docs.python.org/zh-cn/3/library/collections.html)



## 修改意见

PPT中相对应页码的内容

- P2

  - 扩展 Key 的可选范围
  - 扩展 `hashable` 术语
- P3

  - 字典初始化的多种方式
- P5

  - 添加 `get` 方法
- P6
  - 添加 不使用 `eval` 函数的安全方法
- P7
  - 添加 `pop` 方法
- P8
  - 修改待填空程序中变量的名字 便于阅读
- P9
  - 添加非优化版的解答
- P10
  - 添加 `split()` `items()` 方法的解析
  - 添加 ==字典视图== 的额外扩展内容
- 额外扩充
  - 初始化的各种方式
  - `collections` 模块的简单介绍
