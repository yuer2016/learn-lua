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

```