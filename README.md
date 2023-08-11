## Lua 编程

Lua 语言将执行的每一段代码（一个文件或交互模式下的一行）称为一个 **程序段** (Chunk),即一组命令或表达式组成的序列。

### 注释

```lua
--单行注释 

--[[
  多行注释
--]]
```
### 标识符
Lua 语言是对大小写敏感的。

Lua 语言中的标识符(或名称) 是由任意字母、数字和下画线组成的字符串(不能以数字开头)。
一般约定，以下划线开头连接一串大写字母的名字（比如 _VERSION）被保留用于 Lua 内部全局变量, 最好不要使用下划线加大写字母的标示符。

```lua
mohd         zara      abc     move_name    a_123
myname50     _temp     j       a23b9        retVal
```

在 Lua 语言中,连续语句之间的分隔符并不是必需的,如果有需要的话可以使用分号来进行分隔。
在 Lua 语言中,表达式之间的换行也不起任何作用。

```lua
a = 1
b = a * 2

a = 1;
b = a * 2

a = 1; b = a * 2
a = 1  b = a * 2
```

### 全局变量
在 Lua 语言中，全局变量 (Global Variable) 无须声明即可使用，使用未经初始化的全局变量也不会导致错误。

当使用未经初始化的全局变量时，得到的结果是 nil

当把 nil 赋值给全局变量时，Lua 会回收该全局变量(就像该全局变量从来没有出现过一样)。
Lua 语言不区分未初始化变量和被赋值为 nil 的变量。
在赋值语句执行后，Lua 语言会最终回收该变量占用的内存。

```lua
print(b)
=> nil

b = 10
print(b)
=> 10

b = nil
=> nil
```

### 类型和值

Lua 语言是一种 **动态类型语言** (Dynamically-typed-language)，在这种语言中没有类型定义(type-definition)，每个值都带有其自身的类型信息。

Lua 语言中有 8 种基本类型：nil (空)、boolean (布尔)、number (数值)、string (字符串) 、userdata (用户数据)、function (函数)、 thread (线程) 和 table (表)。

使用函数 type 可获取一个值对应的类型名称：

```lua
type(nil)
=> nil

type(true)
=> boolean

type(2.0 * 3.6)
=> number

type("lua")
=> string

type(io.stdin)
=> userdata

type(print)
=> function

type(coroutine.create(function() 
  print("Hello From Thread") 
end))
=> thread

type({})
=> table

-- 不管 X 是什么，最后一行返回的永远是 "string"。这是因为函数 type 的返回值永远是一个字符串。
type(type(X))
=> string
```

### 数值

在 Lua 5.2 及之前的版本中，所有的数值都以双精度浮点格式表示。

从 Lua 5.3 版本开始，Lua 语言为数值格式提供了两种选择：
**64 位整型** integer 和被称为 **双精度浮点** float 类型。

```lua
type(3)
=> number

type(3.5)
=> number

1 == 1.0
=> true

math.type(3)
=> number

math.type(3.5)
=> float
```

### 关系运算

\> 大于 < 小于 <= 小于等于 >= 大于等于 == 等于 ~= 不等

### 字符串

Lua 语言中的字符串是不可变值 (immutable value)。

```lua
a = "one string"
b = string.gsub(a,"one","another")

print(a)
=> one string

print(b)
=> another stirng

-- 使用长度操作符  (#) 获取字符串的长度
a = "hello"
print(#a)
=> 5

print(#"hello world")
=> 11

-- 可以使用连接操作符 ..（两个点）来进行字符串连接
print("result is:" .. 3)
=> result is:3

-- 连接操作符 .. 如果操作数中存在数值，那么 Lua 语言会先把数值转换成字符串
3 .. 2
=> 32
```

### 表

表(Table) 是 Lua 语言中最主要 (事实上也是唯一的) 和强大的数据结构。

使用表，Lua 语言可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构。
Lua 语言也使用 **表** 来表示包（package）和其他对象。

当调用函数 ***math.sin*** 时，我们可能认为是调用了 math 库中函数 ***sin***；
而对于 Lua 语言来说, 其实际含义是 **以字符串 sin 为键检索表 math**

Lua 语言中的表本质上是一种 **辅助数组(associative array)**，这种数组不仅可以使用数值作为索引，也可以使用字符串或其他任意类型的值作为索引(nil除外)

Lua 语言中的 **表** 要么是值要么是变量，它们都是对象(object)。表是一种动态分配的对象，程序只能操作指向表的引用（或指针）。
除此以外，Lua 语言不会进行隐藏的拷贝(hidden copies)或创建新的表。

**表永远是匿名的，表本身和保存表的变量之间没有固定的关系。**

```lua
-- 创建一个表，然后用表的引用赋值
a = {}
k = "x"

a[k] = 10
a[20] = "great"
a["x"]
=> 10

-- 表本身和保存表的变量之间没有固定的关系
b = a
b["x"]
=> 10

-- 对于一个表而言，当程序中不再有指向它的引用时，垃圾收集器会最终删除这个表并重用其占用的内存
b = nil
a = nil
```

同一个表中存储的值可以具有不同的类型索引,并可以按需增长以容纳新的元素。
如同全局变量一样，未经初始化的表元素为 nil，将 nil 赋值给表元素可以将其删除。
因为 Lua 语言实际上就是使用表来存储全局变量的。

当把表当作结构体使用时，可以把索引当作成员名称使用 (a.name 等价于 a["name"])

```lua
a = {}

for i = 1, 1000 do a[i] = i*2 end

a[9]
=> 18

a["x"] = 10
a["x"]
=> 10

a["y"]
=> nil

-- 当把表当作结构体使用时，可以把索引当作成员名称使用 (a.name 等价于 a["name"])
a["name"] = "tom"

a.name
=> tom
```