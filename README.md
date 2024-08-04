# Lua 编程语言

- [Lua 编程语言](#lua-编程语言)
  - [过程抽象](#过程抽象)
    - [基本表达式](#基本表达式)
      - [数值常量](#数值常量)
      - [字符串](#字符串)
      - [关系运算](#关系运算)
      - [控制流结构](#控制流结构)
        - [do ... end](#do--end)
      - [表](#表)
      - [表构造器](#表构造器)
      - [数组、列表和序列](#数组列表和序列)
      - [遍历表](#遍历表)
    - [复合表达式](#复合表达式)
      - [函数](#函数)
        - [pcall](#pcall)
      - [多返回值](#多返回值)
      - [可变长参数函数](#可变长参数函数)
      - [模块](#模块)
      - [元表和元方法](#元表和元方法)
      - [面向对象编程](#面向对象编程)
      - [协程](#协程)

Lua 语言将执行的每一段代码（一个文件或交互模式下的一行）称为一个程序段 (Chunk),即一组命令或表达式组成的序列。

Lua 语言中的标识符(或名称) 是由任意字母、数字和下画线组成的字符串(不能以数字开头)。

Lua 语言是对大小写敏感的

Lua 语言中使用两个连续的连字符 -- 表示单行注释的开始 从 -- 之后直到此行结束都是注释; 使用两个连续的连字符加两对连续左方括号表示长注释或多行注释的开始

```lua
--[[
 lua 语言多行注释示例
--]]
```

在 Lua 语言中,连续语句之间的分隔符并不是必需的,如果有需要的话可以使用分号来进行分隔。
在 Lua 语言中,表达式之间的换行也不起任何作用。

常量 函数 tables

## 过程抽象

### 基本表达式

#### 数值常量

在 Lua 5.2 及之前的版本中，所有的数值都以双精度浮点格式表示

```lua
type(3)
--> number
type(3.5)
--> number
type(3.5)
--> number
1 == 1.0
--> true
13 + 15
--> 28
13.0 + 15.0
--> 28.0
13 + 15.0
--> 28.0
3.0 / 2.0
--> 1.5
-- floor 除法会对得到的商向负无穷取整，从而保证结果是一个整数 
3 // 2
--> 1
```

#### 字符串

Lua 语言中的字符串是不可变值（immutable value）,使用双引号和单引号声明字符串是等价的。

```lua
a = "one stirng"
-- # 获取字符串的长度
print(#a)
-- # 可以使用连接操作符 ..（两个点）来进行字符串连接
"result:" .. 3
--> result:3
"10" + 2
--> 12
10 .. 20
--> 1020
```

#### 关系运算

Lua 语言提供了下列关系运算

```lua
 1 > 2
 1 < 2
 1 <= 2
 1 >= 2
 1 == 2
 1 ~= 2
```

#### 控制流结构

##### do ... end

do...end 是 Lua 中的语法结构，用于创建一个局部作用域。它的主要作用是将一段代码块包裹起来，在该代码块内定义的变量和函数只在该作用域内可见，超出作用域范围后就会被销毁。

```lua
do
  -- 在这里可以定义局部变量和函数
  local x = 10
  local function foo()
    print("Hello, world!")
  end

  -- 对定义的变量和函数进行操作
  print(x)  -- 输出: 10
  foo()     -- 输出: Hello, world!
end

-- 这里无法访问到上面定义的变量和函数
print(x)   -- 报错: attempt to call global 'x' (a nil value)
foo()      -- 报错: attempt to call global 'foo' (a nil value)
```

#### 表

表（Table）是 Lua 语言中最主要（事实上也是唯一的）和强大的数据结构。
使用表，Lua 语言可以以一种简单、统一且高效的方式表示数组、集合、记录和其他很多数据结构。

当调用函数 math.sin 时，对于 Lua 语言来说，其实际含义是 **以字符串 "sin" 为键检索表 math**。

Lua 语言中的表本质上是一种辅助数组（associative array），这种数组不仅可以使用数值作为索引，也可以使用字符串或其他任意类型的值作为索引（nil除外）

Lua 语言中的表要么是值要么是变量，它们都是对象（object）。

```lua
a = {}

k = "x"
a[k] = 20
a["x"]
--> 10

-- 形如 a["name"] 的字符串索引形式表可以使用任意字符串作为键
a.x
--> 20

a[20] = "great"
a[a[k]]
--> great

function a.echo(argment)
  print(argment)
end

a["echo"]("hello")
```

由于可以使用任意类型索引表，所以在索引表时会遇到相等性比较方面的微妙问题。
虽然确实都能用数字 0 和字符串 "0" 对同一个表进行索引，但这两个索引的值及其所对应的元素是不同的。

当被用作表索引时，任何能够被转换为整型的浮点数都会被转换成整型数 2.0 和 2 是一个键。

#### 表构造器

表构造器（Table Constructor）是用来创建和初始化表的表达式，也是 Lua 语言中独有的也是最有用、最灵活的机制之一。

```lua
days = {"sunday","monday","tuesday","wednesday","thursday","friday","staurday"}
days[4]
--> wednesday

a = {x = 10 , y= 20}
a.x
--> 10
a.x = nil 
--> 删除 x 的值
```

创建嵌套表（和构造器）以表达更加复杂的数据结构

```lua
polyline = {
  color = 'bule',
  thicknes = 2,
  nponits = 4,
  {x= 1.0 , y=2.0}
}

polyline[1].x
--> 1.0
```

#### 数组、列表和序列

用表表示数组（array）或列表（list），只需要使用整型作为索引的表即可

```lua
a = {}

for i = 1,10 do
  a[i] = io.read()
end

for i=1,#a do
  print(a[i])
end  
```

#### 遍历表

可以使用 pairs 迭代器遍历表中的键值对

```lua
t = {10, print, x = 12 , k = 'hi'}

for k,v in pairs(t) do
  print(k,v)
end

for k=1,#t do
  print(k,t[k])
end  
```

### 复合表达式

#### 函数

在 Lua 语言中，函数（Function）是对语句和表达式进行抽象的主要方式。

函数调用时都需要使用一对圆括号把参数列表括起来。

即使被调用的函数不需要参数，也需要一对空括号（）。

当函数只有一个参数且该参数是字符串常量或表构造器时，括号是可选的。

Lua 语言为面向对象风格的调用（object-oriented call）提供了一种特殊的语法，即冒号操作符。
形如 o:foo（x）的表达式意为调用对象 o 的 foo方法。

```lua
--[[
  一个函数定义具有一个函数名（ name ，本例中的add）、一个参数（ parameter ）组成的列表和由一组语句组成的函数体（ body ）。
  参数的行为与局部变量的行为完全一致，相当于一个用函数调用时传入的值进行初始化的局部变量
]]
function add(a)
  local sum = 0
  for i = 1,#a do
    sum = sum + a[i]
  end
  return sum
end

function f(a,b) 
  print(a,b)
end

f()
--> nil,nil
f(3)
--> 3,nil
f(3,4)
--> 3,4
f(3,4,5)
--> 3,4
```

##### pcall

pcall 是 Lua 中的一个函数，用于以保护模式（protected mode）调用另一个函数。

```lua
success, result1, result2, ... = pcall(function_name, arg1, arg2, ...)
```

pcall: 函数名，表示以保护模式进行调用。
function_name: 要调用的函数名或函数对象。
arg1, arg2, ...: 可选参数，传递给要调用的函数的参数。
pcall 的执行过程如下：

在保护模式下，调用指定的函数 function_name，并传递相应的参数。
如果函数调用成功且没有发生任何错误，则 pcall 返回 true 作为第一个结果，后跟函数的返回值（如果有）。
如果函数调用过程中出现错误，pcall 返回 false 作为第一个结果，后跟错误消息作为第二个结果。
如果函数有多个返回值，它们将依次作为 pcall 的后续结果返回。
以下是一个示例：

```lua
function divide(a, b)
    if b == 0 then
        error("Attempt to divide by zero")
    else
        return a / b
    end
end

local success, result = pcall(divide, 10, 2)
if success then
    print("Result:", result)
else
    print("Error:", result)
end
```

通过使用 pcall，可以在 Lua 中进行函数调用时捕获错误并采取相应的处理措施，而不会导致程序崩溃。

#### 多返回值

Lua 语言允许一个函数返回多个结果 (Multiple Results)。

```lua
function maximum(a)
  local mi = 1
  local m = a[mi]
  for i = 1, #a do
    if a[i] > m then 
      mi = i
      m = a[i]
    end
  end
  return mi, m
end

maximum({2,10,23,12,18})

```

#### 可变长参数函数

Lua 语言中的函数可以是可变长参数函数。

```lua
function add(...)
  local s = 0
  for _,v in pairs{...} do
    s = s + v
  end
  return s 
end 

add(3,4,10,25,12,15)
--> 69
```

多重返回值涉及一个特殊的函数 **table.unpack** 。
该函数的参数是一个数组，返回值为数组内的所有元素。

#### 模块

一个模块（ module ）就是一些代码（要么是Lua语言编写的，要么是 C 语言编写的），这些代码可以通过函数 require 加载，然后创建和返回一个表。
这个表就像是某种命名空间，其中定义的内容是模块中导出的东西，比如函数和常量。

```lua
local m = require "math"
m.sin(3.14)
```

#### 元表和元方法

Lua 语言中的每种类型的值都有一套可预见的操作集合。
元表可以修改一个值在面对一个未知操作时的行为。

Lua 语言中的每一个值都可以有元表。

每一个表和用户数据类型都具有各自独立的元表，而其他类型的值则共享对应类型所属的同一个元表。

字符串标准库为所有的字符串都设罝了同一个元表，而其他类型在默认情况中都没有元表：

```lua
getmetatable("hi")
```

#### 面向对象编程

Lua 语言中的一张表就是一个对象。
表与对象一样，可以拥有状态。拥有一个与其值无关的的标识。

两个具有相同值的对象（表）是两个不同的对象，而一个对象可以具有多个不同的值

```lua
Account = {balance = 0}

function Account.withdraw(v)
  Account.balance = Account.balance - v
end

-- 冒号的作用是在一个方法调用中增加一个额外的实参，或在方法的定义中增加一个额外的隐藏形参。
function Account:withdraw(v)
  self.balance = self.balance - v
end

function Account:deposit(v)
  self.balance = self.balance + v 
end

function Account:new(o)
  o = o or {}
  self.__index = self
  setmetatable(o,self)
  return o
end
```

#### 协程
