### gdb
##### 注意
- 是在程序运行产生的结果与预期不符合的时候使用的gdb调试
- 一定要在编译的时候添加调试的参数

##### gdb的一些基本命令
- 设置命令行参数 `set args xxx xxx xxx`
- 显示命令行参数 `show args`
- 执行程序到下一个断点 `run`
- run 简称r
- 程序向下执行一行（不完全只是向下执行一行，而是在第一条语句以后暂停） `start`
- 直接回车，执行上一条命令

##### 命令行参数的个数？
命令行参数的个数 = 1（程序本身） + 输入的参数的个数

##### liST
- liST 默认显示10行
- list - 回到当前文件开头（就是后面有显示了其他文件）
- list 5 显示第5行，第5行在中间，然后其他的几行就上下分布
- liST function 显示函数主体
- list 简称 l
- list func1.c:1，能够查看某一个文件里面的东西
- list func1.c:func4 也是可以的
- set listsize 5 这样一次就显示5行，就是把默认的10行改成了5行

##### break断点
- break 简称 b
- b 10 在第10行设置断点
- info b 就可以看见有多少断点
- info 简称 i，info显示的信息
	![[Pasted image 20230302081355.png]]
	1. num 断点的编号
	2. type 类型，这里是breakpoint
	3. Enb, enable有效的，这里是y，也就是yeS
	4. address 断点的内存地址（当然还可以有其他的type）
	5. what 在哪设置的？在main.c的第10行
- 多文件设置断点
	![[Pasted image 20230302081734.png]]
	这里比较神奇，就是我在main.c的fun1中设置的断点，居然是设置在了func1.c中
	
- print xxx 可以打印变量的值
- next 继续执行，在下一个断点处暂停
- b func2.c:3 在func2.c中的第3行设置断点
- disable 1 将第1个断点禁用
- disable 2-4 将2~4个断点设置为禁用
	单走一个disable，禁用所有断点
- enable 1 3 将1、3断点启用
- delete 1 删除第1个端点
- delete 简称 d
	d 单走一个d，删除全部断点
- b 9 if i\==9 当i=9的时候，这个断点才有效

##### 流程控制
- run 运行程序，简称 r
- next 单步跟踪，把函数的调用当成一条简单语句执行，简称 n
- step 单步跟踪，调用函数，会进入函数体中，简称 s
- finish 退出进入的函数，如果出不去，看一下函数体的循环中是否有断点
- until 跳出循环， 简称u
	如果出不去，看一下循环体中是否有断点
- continue 程序运行至下一个断点

##### 查看
- print i 打印值
- print &i 打印地址
- display i 可以在每一次执行完流程控制的语句之后显示 i的值 和 n的值
	但是这个不能一下设置俩
- info display 查看自动显示的信息
	![[Pasted image 20230302090032.png]]
	1. num 编号
	2. enb enable有效的，y->yes，n->no
	3. expression auto-display的变量名
- undisplay 1 删除自动显示的变量，其编号为1
- delete display 2 3 删除 2，3 的自动显示的变量
- delete display 删除所有自动显示
- disable diskpy 2 将自动显示的变量设为无效，其中编号为2
	但是一个disalbe 2 是对断点操作
- enable display 2 启用断点
- ptype i 打印变量类型，输出结果：tpye = int 
- set var i=8 在程序运行的过程中，将i修改为8





