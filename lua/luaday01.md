### 定义变量
- 因为是动态脚本语言，直接变量名就能用了`a=100`
- 但是这样只是一个全局变量
- 如果想要用局部的话，就可以加上关键字`local a = 100'
- 两个`--`切换注释

### 定义一个方法
```lua
funcion max(a,b)
	if a>b then
		return a
	else 
		return b
	end
end
```

### 循环
```lUa
for var =1,100 do
	print(var)
end
```

### 定义一个表table
- 这个感觉就是字典，python里面的字典
```lUa
config = {}  -- 新建一个空表
config.words = "hello"
config.num = 100
config["name"]="zhangshan"
print(config["words"])
print(config.name)
```
在创建表的时候初始化
```lUa
config = {hello="hello lua", wordld="world"}
```
遍历表
下面这种遍历方式，输出了所有的键值对
```lUa
for key,var in pairs(config) do 
	print(key,var)
end
```


### 数组
```C++
arr = {1,2,3,4,"hello"}

for key,var in pairs(arr) do
	print(key,var)
end
```
这个输出结果是
```Shell
1 1
2 2
3 3
4 4
5 hello
```
这个key也就是1,2,3,4,5也就是数组的索引。
这里索引居然是从1开始的，比较迷惑

### lua语言面向对象
- 实际上就是将table看成一个类
- 然后通过clone这个table，并修改里面的一些属性，就当做是创建对象
- 实际上lua并没有真正定义的面向对象，都是我们自己实现的
##### 定义成员函数
```lUa
people.sayHi = function()
	print("People say hi")
end
```
还可以这样定义
```lua
function people.sayHi()
	print("People say hi")
end
```

##### 通过复制类来创建对象
```lUa
-- 这里是复制类
function clone(tab)
	local ins = {}
	for key,var in pairs(tab) do
		ins[key] = var
	end
	return ins
end 
-- 下面是一个例子
Person = {}
People.sayHi = function()
	print("people say hi")
end

p = clone(People)
p.sayHi
```
上面这一段语句执行的结果就是
```shll
people say hi
```
当然，这个还可以传入参数
这里就是在创建对象了
```lUa
people.new = function(name)
	local self = clone(poeople)
	self.name = name

	return self
end
```
简单来说，就是people下面的一个key-value，然后key是new，value是一个地址。
`local p = people.new("张三")`。显示拷贝了一份people的所有key-val，再将people中的name重新赋值，然后返回一个table。并将这个table赋值给了p

##### 调用成员函数
这里先定义一个成员函数
```lua
people.sayHi = function(self)
	print("people say hi:"..self.nam)
end
```
- 这里的两个点==..== 就是==字符串拼接==
下面是调用成员函数了
- 这里因为并不是真的意义上的面上对象，所以还是要把对象(table)传进去
```lUa
local p = people.new("张三")
p.sayHi(p)
```
当然，还可以写的更加的直观一些
```lUa
p:sayHi()
```

##### 继承
那么该如何手动的实现继承呢？
什么时候可以体现出继承呢？创建对象的时候！
所以说这里会发现，我们都是使用有参构造创建对象的
```lUa
-- 将父类的东西拷贝到子类中
function copy(dist,tab)
	local ins = {}
	for key,val in pairs(tab) do 
		dist[key] = vAL
	end
	return ins
end

Man = {}  -- 这里创建了一个类
Man.new = function(name)
	local self = people.new(name)
	copy(self,Man)  -- 子类要加上父类的东西
	return self
end
```

##### 私有属性，用闭包设置（但是不私有）
```lUa
function People(name)
	-- 创建一个空的表
	local self = {}
	-- 一般把初始化的操作写成一个函数
	-- 其实不写成函数也行，只是写成函数更加清楚一些
	-- 并且在最后的时候执行这个init函数
	local function init()
		self.name = name;
	end
	
	-- 定义一个成员函数
	self.sayHi = function()
		print("hello:"..self.name)
	end
	
	init()
	return self
end
```
这个时候，在people外面是无法修改这个name的？实验一下？
事实上，还是可以改的。感觉这是在放屁
但是没关系

- 首先看看 闭包创建对象 与上面 复制类创建对象的区别
上面复制类创建对象，本质上类就是一个对象，只不过复制过来，然后修改里面的成员变量罢了
- 这里的类，本质上是一个函数

##### 继承
- 这里继承还是发生在创建对象的过程中的
- 但是，我感觉，是不是能用装饰器来继承
```lUa
function Man(name)
	local self = people(name)
	self.sayHello = function()
		print("hello:"..self.name)
	end
	return self
end
```

### nil 类型
- 简单来说，就表示错误，然后占位
- 在boolean中表示false

### 接收可变数量的参数
`...` 三个点表示可以接受可变数量的参数
```lUa
function add(...)  
local s = 0  
  for i, v in ipairs{...} do   --> {...} 表示一个由所有变长参数构成的数组  
    s = s + v  
  end  
  return s  
end  
print(add(3,4,5,6,7))  --->25
```
可以使用`#`来获取可变参数的数量
- 简单来说 `#table` 其中table是表，就可以统计出数量 
- 还可以使用 select("#",...) 来获取可变参数的数量
- 这里可以说一个函数里面 ... 就代表传进来的参数
```lUa
function average(...)  
   result = 0  
   local arg={...}    --> arg 为一个表，局部变量  
   for i,v in ipairs(arg) do  
      result = result + v  
   end  
   print("总共传入 " .. #arg .. " 个数")  
   return result/#arg  
end  
  
print("平均值为",average(10,5,3,4,5,6))
```

##### select函数
```lUa
function f(...)  
    a = select(3,...)  -->从第三个位置开始，变量 a 对应右边变量列表的第一个参数  
    print (a)  
    print (select(3,...)) -->打印所有列表参数  
end  
  
f(0,1,2,3,4,5)
```

### for 循环
```lUa
for <start>,<end>,<step> do
end
```

### 算术运算符
| 操作符 | 描述 | 实例 |
| --- | --- | --- |
| + | 加法 | A + B 输出结果 30 |
| \- | 减法 | A - B 输出结果 -10 |
| \* | 乘法 | A \* B 输出结果 200 |
| / | 除法 | B / A 输出结果 2 |
| % | 取余 | B % A 输出结果 0 |
| ^ | 乘幂 | A^2 输出结果 100 |
| \- | 负号 | \-A 输出结果 -10 |
| // | 整除运算符(>=lua5.3) | 5//2 输出结果 2 |
- 特殊地：整除，幂运算

### 关系运算符
| 操作符 | 描述 | 实例 |
| --- | --- | --- |
| \== | 等于，检测两个值是否相等，相等返回 true，否则返回 false | (A == B) 为 false。 |
| ~= | 不等于，检测两个值是否相等，不相等返回 true，否则返回 false | (A ~= B) 为 true。 |
| \> | 大于，如果左边的值大于右边的值，返回 true，否则返回 false | (A > B) 为 false。 |
| < | 小于，如果左边的值大于右边的值，返回 false，否则返回 true | (A < B) 为 true。 |
| \>= | 大于等于，如果左边的值大于等于右边的值，返回 true，否则返回 false | (A >= B) 返回 false。 |
| <= | 小于等于， 如果左边的值小于等于右边的值，返回 true，否则返回 false | (A <= B) 返回 true。 |
- 特殊地：不等于

### 逻辑运算符
| 操作符 | 描述 | 实例 |
| --- | --- | --- |
| and | 逻辑与操作符。 若 A 为 false，则返回 A，否则返回 B。 | (A and B) 为 false。 |
| or | 逻辑或操作符。 若 A 为 true，则返回 A，否则返回 B。 | (A or B) 为 true。 |
| not | 逻辑非操作符。与逻辑运算结果相反，如果条件为 true，逻辑非为 false。 | not(A and B) 为 true。 |

### 其他运算符
| 操作符 | 描述 | 实例 |
| --- | --- | --- |
| .. | 连接两个字符串 | a..b ，其中 a 为 "Hello " ， b 为 "World", 输出结果为 "Hello World"。 |
| # | 一元运算符，返回字符串或表的长度。 | #"Hello" 返回 5 |
- 这两个都比较特殊

### 运算的优先级
从高到低
```lua
^
not    - (取相反数)
*      /       %
+      -
..
<      >      <=     >=     ~=     ==
and
or
```

### 左连接/右连接
除了 ^ 和 .. 外所有的二元运算符都是左连接的。
```lUa
a+i < b/2+1          -->       (a+i) < ((b/2)+1)
5+x^2*8              -->       5+((x^2)*8)
a < y and y <= z     -->       (a < y) and (y <= z)
-x^2                 -->       -(x^2)
x^y^z                -->       x^(y^z)
```
- 其实左连接/右连接，从最下面那一行就能看出是啥意思

### 字符串的三种表示方法
```lUa
string1 = "Lua"  
print("\"字符串 1 是\"",string1)  
string2 = 'runoob.com'  
print("字符串 2 是",string2)  
  
string3 = [[Lua 教程]]  
print("字符串 3 是",string3)
```

### 字符串操作
1. string.upper(argument)转成大写
2. string.lower(argument)字符串全部转为小写字母
3. string.gsub(mainString,findString,replaceString,num)在字符串中替换。
	- mainString为要操作的字符串
	- findString 为被替换的字符
	- replaceString 要替换的字符
	- num 替换次数（可以忽略，则全部替换）
	- example:`string.gsub("aaaa","a","z",3);` -> `zzza`
4. string.find (str, substr, [init, [plain]])
	- 在一个指定的目标字符串 str 中搜索指定的内容substr，如果找到了一个匹配的子串，就会返回这个子串的起始索引和结束索引，不存在则返回 nil。
	- init 指定了搜索的起始位置，默认为 1，可以一个负数，表示从后往前数的字符个数。
	- plain 表示是否使用简单模式，默认为 false，true 只做简单的查找子串的操作，false 表示使用使用正则模式匹配。
	- example:`string.find("Hello Lua user", "Lua", 1)` -> `7 9`起始/结束的位置
5. string.reverse(arg)字符串反转
6. string.format(...)返回一个类似printf的格式化字符串
	- example:`string.format("the value is:%d",4)` -> `the value is:4`
7. string.char(arg) 和 string.byte(arg[,int])
	- char 将整型数字转成字符并连接
	- byte 转换字符为整数值(可以指定某个字符，默认第一个字符)
	- example:
		- `string.char(97,98,99,100)` -> `abcd`
		- `string.byte("ABCD",4)` -> `68`
		- `string.byte("ABCD")` -> `65`
8. string.len(arg)计算字符串长度
9. string.rep(string, n)返回字符串string的n个拷贝
	- example: `string.rep("abcd",2)` -> `abcdabcd`
10. .. 连接两个字符串
	- example: `print("www.runoob.".."com")` -> `www.runoob.com`
11. string.gmatch(str, pattern)
	- 返回一个迭代器函数，每一次调用这个函数，返回一个在字符串
	- str 找到的下一个符合
	- pattern 描述的子串。如果参数 pattern 描述的字符串没有找到，迭代函数返回nil。
	- example:这里输出结果是`Hello\n Lua\n user`
	- 其中%a，就是匹配任意字母，+就是多个，这里有点像正则表达式
```
for word in string.gmatch("Hello Lua user", "%a+") do 
	print(word) 
end
```
12. string.match(str, pattern, init) 
	- string.match()只寻找源字串str中的第一个配对
	- 参数init可选, 指定搜寻过程的起点, 默认为1。  
	- 在成功配对时, 函数将返回配对表达式中的所有捕获结果; 
	- 如果没有设置捕获标记, 则返回整个配对字符串. 
	- 当没有成功的配对时, 返回nil。
	- example: `string.match("I have 2 questions for you.", "%d+ %a+")` 
										-> `2 questions`
13. string.sub(s, i [, j])字符串截取
	- s：要截取的字符串
	- i：截取开始位置
	- j：截取结束位置，默认为 -1，最后一个字符
	- example:`local first_sub = string.sub(sourcestr, 4, 15)` -> `fix--runoobg`

### 格式化字符串string.format( )
- %g(%G) : 接受一个数字并将其转化为%e(%E, 对应%G)及%f中较短的一种格式
- %q : 接受一个字符串并将其转化为可安全被Lua编译器读入的格式
- exapmle:`string.format("%+d", 17.0)              -- 输出+17`

### 字符串与整型互换string.byte
`print(string.byte("Lua",-1))` -> `97`

### 一小部分正则表达式
-   .(点): 与任何字符配对
-   %a: 与任何字母配对
-   %c: 与任何控制符配对(例如\n)
-   %d: 与任何数字配对
-   %l: 与任何小写字母配对
-   %p: 与任何标点(punctuation)配对
-   %s: 与空白字符配对
-   %u: 与任何大写字母配对
-   %w: 与任何字母/数字配对
-   %x: 与任何十六进制数配对
-   %z: 与任何代表0的字符配对
-   %x(此处x是非字母非数字字符): 与字符x配对. 主要用来处理表达式中有功能的字符(^$()%.\[\]\*+-?)的配对问题, 例如\%\%与\%配对
-   \[数个字符类]: 与任何[]中包含的字符类配对. 例如[%w_]与任何字母/数字, 或下划线符号(_)配对
- \[^数个字符类]: 与任何不包含在[]中的字符类配对. 例如\[^%s]与任何非空白字符配对
上面的写成了大写，就表示取反
例如：\%A 表示非字母的字符
`print(string.gsub("hello, up-down!", "%A", "."))` -> hello..up.down.    4
这里的数字4，就表示替换的次数，是gsub的第二个返回值
##### 模式条目
-   单个字符类匹配该类别中任意单个字符；
-   单个字符类跟一个 '`*`'， 将匹配零或多个该类的字符。 这个条目总是匹配尽可能长的串；
-   单个字符类跟一个 '`+`'， 将匹配一或更多个该类的字符。 这个条目总是匹配尽可能长的串；
-   单个字符类跟一个 '`-`'， 将匹配零或更多个该类的字符。 和 '`*`' 不同， 这个条目总是匹配尽可能短的串；
-   单个字符类跟一个 '`?`'， 将匹配零或一个该类的字符。 只要有可能，它会匹配一个；
-   `%_n_`， 这里的 _n_ 可以从 1 到 9； 这个条目匹配一个等于 _n_ 号捕获物（后面有描述）的子串。
-   `%b_xy_`， 这里的 _x_ 和 _y_ 是两个明确的字符； 这个条目匹配以 _x_ 开始 _y_ 结束， 且其中 _x_ 和 _y_ 保持 _平衡_ 的字符串。 意思是，如果从左到右读这个字符串，对每次读到一个 _x_ 就 _+1_ ，读到一个 _y_ 就 _-1_， 最终结束处的那个 _y_ 是第一个记数到 0 的 _y_。 举个例子，条目 `%b()` 可以匹配到括号平衡的表达式。
-   `%f[_set_]`， 指 _边境模式_； 这个条目会匹配到一个位于 _set_ 内某个字符之前的一个空串， 且这个位置的前一个字符不属于 _set_ 。 集合 _set_ 的含义如前面所述。 匹配出的那个空串之开始和结束点的计算就看成该处有个字符 '`\0`' 一样。
##### 模式
_模式_ 指一个模式条目的序列。 在模式最前面加上符号 '`^`' 将锚定从字符串的开始处做匹配。 在模式最后面加上符号 '`$`' 将使匹配过程锚定到字符串的结尾。 如果 '`^`' 和 '`$`' 出现在其它位置，它们均没有特殊含义，只表示自身。
##### 捕获
模式可以在内部用小括号括起一个子模式； 这些子模式被称为 _捕获物_。 当匹配成功时，由 _捕获物_ 匹配到的字符串中的子串被保存起来用于未来的用途。 捕获物以它们左括号的次序来编号。 例如，对于模式 `"(a*(.)%w(%s*))"` ， 字符串中匹配到 `"a*(.)%w(%s*)"` 的部分保存在第一个捕获物中 （因此是编号 1 ）； 由 "`.`" 匹配到的字符是 2 号捕获物， 匹配到 "`%s*`" 的那部分是 3 号。

作为一个特例，空的捕获 `()` 将捕获到当前字符串的位置（它是一个数字）。 例如，如果将模式 `"()aa()"` 作用到字符串 `"flaaap"` 上，将产生两个捕获物： 3 和 5 。


### 一维数组 
lua里面的数组，是从1开始的，但是也可以指定从0开始
`for i=0,4 do arr[i] = i end`

### 多维数组
```lUa
-- 初始化数组  
array = {}  
for i=1,3 do  
   array[i] = {}  
      for j=1,3 do  
         array[i][j] = i*j  
      end  
end  
  
-- 访问数组  
for i=1,3 do  
   for j=1,3 do  
      print(array[i][j])  
   end  
end
```

### 迭代器
for的迭代函数，实际上保存了三个值：迭代函数，状态常量，控制变量
```lUa
array = {"Google", "Runoob"}  
  
for key,value in ipairs(array)  
do  
   print(key, value)  
end
```
输出：
```Shell
1 Google
2 Runoob
```
以上实例中我们使用了 Lua 默认提供的迭代函数 ipairs
下面我们看看泛型 for 的执行过程：
1. 首先，初始化，计算 in 后面表达式的值，表达式应该返回泛型 for 需要的三个值：迭代函数、状态常量、控制变量；与多值赋值一样，如果表达式返回的结果个数不足三个会自动用 nil 补足，多出部分会被忽略。
2. 第二，将状态常量和控制变量作为参数调用迭代函数（注意：对于 for 结构来说，状态常量没有用处，仅仅在初始化时获取他的值并传递给迭代函数）。
3. 第三，将迭代函数返回的值赋给变量列表。
4. 第四，如果返回的第一个值为nil循环结束，否则执行循环体。
5. 第五，回到第二步再次调用迭代函数

##### 迭代器类型
在Lua中我们常常使用函数来描述迭代器，每次调用该函数就返回集合的下一个元素。Lua 的迭代器包含以下两种类型：
1. 无状态的迭代器
2. 多状态的迭代器

### table
example:
```lUa
-- 简单的 table  
mytable = {}  
print("mytable 的类型是 ",type(mytable))  
  
mytable[1]= "Lua"  
mytable["wow"] = "修改前"  
print("mytable 索引为 1 的元素是 ", mytable[1])  
print("mytable 索引为 wow 的元素是 ", mytable["wow"])  
  
-- alternatetable和mytable的是指同一个 table  
alternatetable = mytable  
  
print("alternatetable 索引为 1 的元素是 ", alternatetable[1])  
print("mytable 索引为 wow 的元素是 ", alternatetable["wow"])  
  
alternatetable["wow"] = "修改后"  
  
print("mytable 索引为 wow 的元素是 ", mytable["wow"])  
  
-- 释放变量  
alternatetable = nil  
print("alternatetable 是 ", alternatetable)  
  
-- mytable 仍然可以访问  
print("mytable 索引为 wow 的元素是 ", mytable["wow"])  
  
mytable = nil  
print("mytable 是 ", mytable)
```
输出结果：
```lUa
mytable 的类型是     table
mytable 索引为 1 的元素是     Lua
mytable 索引为 wow 的元素是     修改前
alternatetable 索引为 1 的元素是     Lua
mytable 索引为 wow 的元素是     修改前
mytable 索引为 wow 的元素是     修改后
alternatetable 是     nil
mytable 索引为 wow 的元素是     修改后
mytable 是     nil
```

### table操作
1. table.concat (table [, sep [, start [, end]]])
	- concat是concatenate(连锁, 连接)的缩写.
	- table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素
	- 元素间以指定的分隔符(sep)隔开。
2. table.insert (table, [pos,] value)插入
	- 在table的数组部分指定位置(pos)插入值为value的一个元素
	- pos参数可选, 默认为数组部分末尾
3. table.remove (table [, pos])删除
	- 返回table数组部分位于pos位置的元素. 其后的元素会被前移
	- pos参数可选, 默认为table长度, 即从最后一个元素删起。
4. table.sort (table [, comp])快速排序
注意：当我们获取 table 的长度的时候无论是使用 # 还是 table.getn 其都会在索引中断的地方停止计数，而导致无法正确取得 table 的长度。可以使用如下代码计数
```lUa
function table_leng(t)
  local leng=0
  for k, v in pairs(t) do
    leng=leng+1
  end
  return leng;
end
```

### 模块
```lUa
-- 文件名为 module.lua  
-- 定义一个名为 module 的模块  
module = {}  
   
-- 定义一个常量  
module.constant = "这是一个常量"  
   
-- 定义一个函数  
function module.func1()  
    io.write("这是一个公有函数！\n")  
end  
   
local function func2()  
    print("这是一个私有函数！")  
end  
   
function module.func3()  
    func2()  
end  
   
return module
```
这就是创建了一个table。然后这里是将key=constant/value="这是一个常数"假如到table
然后也是在module下创建了一个函数。
最后return module

### 调用
require("<模块名>")
常规调用
```lUa
-- test_module.lua 文件  
-- module 模块为上文提到到 module.lua  
require("module")  
print(module.constant)    
module.func3()
```
给调用取别名
```lUa
-- test_module2.lua 文件  
-- module 模块为上文提到到 module.lua  
-- 别名变量 m  
local m = require("module")     
print(m.constant)    
m.func3()
```

### metatable元表 与 元方法
这个很想运算符的重载
比方说像
```lUa
t = {a = 1}  -- 键值对
```
但是假如要这样：一个table与int做运算，这个肯定是会报错的
```lUa
t = {a = 1}
print(a+1)
```
但是，我就是要怎么做，咋整？
```lUa
t = {a = 1}
mt = {
	__add = function(a,b)
		return a.a+b
	end,
}
setmetatable(t,mt)
print(t+1)
```
就能够实现

### 错误处理
1. pcall
```lUa
if pcall(function_name, ….) then
-- 没有错误
else
-- 一些错误
end
```
- 调用实例，其实可以看到
```lua
local ok, mymod = pcall(require, 'module_with_error')
if not ok then
  print("Module had an error")
else
  mymod.function()
end
```
可以看到函数和传的参数。这里的第一个参数是判断对错，mymod就是调用的内容
2. xpcall
```lUa
function myfunction ()  
   n = n/nil  
end  
  
function myerrorhandler( err )  
   print( "ERROR:", err )  
end  
  
status = xpcall( myfunction, myerrorhandler )  
print( status)
```
上面这一段程序的输出结果如下
```shell
ERROR:    test2.lua:2: attempt to perform arithmetic on global 'n' (a nil value)
false
```
这个xpcall会将第一个函数的错误，传入第二个函数中，第二个函数也就是错误处理函数





