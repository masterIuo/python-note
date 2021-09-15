## 内置函数与第三方库函数

### 1.split( )

​		常用的字符串处理函数，作用是给字符串切片并返回一个列表。如：

```python
suits = 'spades diamonds clubs hearts'.split()
print(suits)
['spades', 'diamonds', 'clubs', 'hearts']
```

参数：

1. str：分隔符。默认为所有的空字符，即空格，换行（\n），制表符（\t）等。

2. num：分割次数。默认为 -1，即分隔所有。

### 2.reversed( )

​		传入一个对象seq，seq可以是元组，列表，字符串或range（）函数构造的迭代对象。返回值为一个反转的迭代器。如：

```python
print([x for x in reversed([1,2,3,4,5])])
[5,4,3,2,1]
```

### 3.items( )

​		字典处理函数。把字典中的每对键值组成一个元组，并把这些元组放在列表中返回。如：

```python
country_code = {'China': 86, 'India': 91, 'United States': 1}
print(country_code.items())
dict_items([('China', 86), ('India', 91), ('United States', 1)])
```

### 4.upper( )

​		常用的内置函数，将传入的对象中的小写字母全部转换为大写。

```python
str1 = 'this is string example'
print(str1.upper())
THIS IS STRING EXAMPLE
```

### 5.isinstance( )

​		isinstance(object, classinfo)

​		该函数用来判断一个对象是否是一个已知的类型，类似type( )。

```python
a = 2
isinstance(a, int)
True
isinstance(a, str)
False
isinstance (a,(str,int,list))    # 若是元组中的一个则返回True
True
```

参数：

​		classinfo：可以是直接或间接类名、基本类型或者由它们组成的元组

isinstance( )和type( )的区别：

​		type( )不会认为子类是一种父类类型，不考虑继承关系。而isinstance( )考虑继承关系。

### 6.map( )

​		map(function, iterable, ...)

​		Python内置高阶函数。该函数会根据提供的函数对指定的序列做映射。返回一个迭代器(需要使用可迭代数据类型转换)。例如：

```python
def square(x) :
	return x ** 2
map(square, [1,2,3,4,5])    # 计算列表各个元素的平方
<map object at 0x100d3d550>     # 返回迭代器
list(map(square, [1,2,3,4,5]))   
[1, 4, 9, 16, 25]
```

参数：

1. function：传入的函数
2. iterable：一个或多个序列

### 7.rstrip( )

​		str.rstrip([chars])

​		字符串函数，用于删除字符串末尾的指定字符，返回新生成的字符串如：

```python
str1 = 'this is string example     '
print(str1.rstrip())
this is string example
str2 = '88888this is string example..8888'
print(str2.rstrip('8'))
88888this is string example..
```

参数：

chars：指定删除的字符（默认为空格）

### 8.reduce( )

​		reduce(func,iterable[, initializer])

​		functools模块函数。reduce()函数用于实现迭代的运算。即：先对集合中的第1，2个元素进行操作，得到的结果再与第三个数据用func函数运算，最后得到一个结果并返回。

```python
from functools import reduce
def add(x,y)
	return x+y
sum1 = reduce(add,[1,2,3,4,5])
sum1
15
```

参数：

​		func：含两个参数的函数

​		iterable：可迭代对象

​		initalizer：（可选）初始参数

### 9.itemgetter( )

​		operator模块提供的itemgetter()函数用于获取对象的哪些维的数据，参数为一些序号。（即需要获取的数据在对象中的序号）。该函数获取的不是值，而是**定义了一个函数**，通过该函数作用到对象上才能获取值。（如果把多个参数传给它，它构建的函数会返回提取的值构成的元组）如：

```python
import operator
a = [1,2,3]
b = operator.itemgetter(1) #定义函数b，获取对象的第一个域的值
b(a)
2
b = operator.itemgetter(1,0) #定于函数b，获取对象第1，0个域的值
b(a)
(2,1)
```

### 10.partial( )

​		functools模块提供的partial()函数用于部分应用的一个函数。部分应用是指基于一个函数创建一个新的可调用对象，把原函数的某些参数固定。如：

```python
from operator import mul
from functools import partial
triple = partial(mul,3) #mul函数被封装，3为冻结参数
tuiple(7) # 函数返回x*3
21
list(map(triple,range(1,10))) 
[3,6,9,12,15,18,21,24,27]

def display(name,age):
    print("name:",name,"age:",age)
Gfunc = partial(display,name='Gary') # name关键字参数被冻结为Gary
Gfunc(age=13) # 传入关键字参数age
name: Gary age: 13
```

​		partial( )作用为对原始函数的二次封装，将现有函数的部分参数预先绑定为指定值，从而得到一个新的函数，这个新函数就称为**偏函数**。被绑定的参数称为**参数冻结**。具体用法：

​		pfunc = partial(func, *args, **kwargs)

参数：

pfunc：使用partial()函数得到的偏函数名，可直接调用。

func：要封装的原函数

*args，**kwargs：接收无关键字实参和关键字实参。

### 11.repr( )

​		repr(obj)

​	repr( )函数将对象转化为供python解释器读取的形式（字符串）。调用该函数返回一个对象的string格式。

```python
s = "dhcp protrol"
print(repr(s))
'dhcp protrol'
```

### 12.format( )

​		常见的Python内置的字符串处理函数。这里主要说明调用 __ format(sp) __

特殊方法的format( )函数。如果类没有定义这个特殊方法，那么从object继承的方法会返回str(my_object)。

format (my_obj, format_spec)

参数：		

1. format_spec：格式说明符，它是str.format( )方法的格式字符串的大括号 { } 里代换字段中冒号后面的部分。
2. my_obj：字段名，冒号中前面的部分。可以在函数中作为关键字参数传入。

```python
brl = 1/2.43
format(brl,'0.4f')	#output '0.4115'
'BRL={rate:.2f}USD'.format(rate=brl)	#output 'BRL=0.41USD'
```

### 13.zip( )

​		zip( )函数可以生成一个由元组构成的生成器，元组中的元素来自参数传入的各个可迭代对象。但是，**一旦有一个输入耗尽，zip函数会立即停止生成值**且不发出警告。zip( )函数可以接收多个参数，参数的个数决定了生成器元素（元组）的大小。例如：

```python
list(zip(range(3), 'ABC'))	#传入两个参数，返回含有两个元素的元组
[(0, 'A'), (1, 'B'), (2, 'C')]
list(zip(range(3), 'ABC', [0.0, 1.1, 2.2, 3.3]))	#三个同理
[(0, 'A', 0.0), (1, 'B', 1.1), (2, 'C', 2.2)]	
```

## 函数的参数处理机制

​		Python提供了极其灵活的参数处理机制。调用函数时使用* 和** 符号指定可迭代对象，映射到单个参数。

示例：构造HTML标签的tag函数。

```python
def tag(name, *content, cls=None, **attrs):
    '''生成一个或多个HTML标签'''
    if cls is not None:
        attrs['class'] = cls
    if attrs:
        attrs_str = ''.join(' %s="%s"' % (attr, value)
                            for attr, value in sorted(attrs.items()))
    else:
        attrs_str = ''
    if content:
        return '\n'.join('<%s%s>%s</%s>' % (name, attrs_str, c, name) for c in content)
    else:
        return '<%s%s />' % (name, attrs_str)
```

针对传入不同的参数单独进行分析，了解Python中的参数处理。

1. 传入单个定位参数name，生成一个指定名称的空标签

```python
tag('br')
'<br />'
```

2. 传入定位参数name，后面的任意个参数被 *content 捕获，存入一个**元组**。

```python
tag('p', 'hello')
'<p>hello</p>'
print(tag('p', 'hello', 'world'))
<p>hello</p>
<p>world</p>
```

3. 没有指定名称的关键字参数会被 **attrs 捕获，存入一个字典。同时也能传入一个字典作为参数。（关键字参数简单来说就是带等号(=)的参数）即便第一个定位参数也能作为关键字参数传入。

```python
tag('p', 'hello', id=33)
'<p id='33'>hello</p>'
tag(content='testing', name='img')
'<img content="testing" />'
```

