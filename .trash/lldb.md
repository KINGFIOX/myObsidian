### 断点
1. 按照函数的名字设置断点，这个可以一次设置多个断点
2. 当然也可以指定作用域（我有点印象，但是我忘记了）
```lldb
(lldb) breakpoint set --name foo
(lldb) breakpoint set -n foo
```


```

(lldb) breakpoint set --name foo --name bar
```




### 为命令取别名
1. 取消别名
```lldb
(lldb) command unalias b
```

2. 设置别名
```lldb
(lldb) command alias b breakpoint
```

3. 设置带有参数的别名
```lldb
(lldb) command alias bfl breakpoint set -f %1 -l %2
(lldb) bfl foo.c 12
```

```lldb
(lldb) help -h

Debugger commands:

  	_regexp-attach    	# 根据进程 ID 或进程名称，附加到指定的进程

  	_regexp-break     	# 使用几种速记格式之一设置断点

  	_regexp-bt        	# 显示当前线程的调用栈
     				  	# 数字参数用于指定要显示的帧数，all 参数用于指定显示所有线程的调用栈

  	_regexp-display   	# 在每次程序暂停时，执行指定的表达式(请参阅 'help target stop-hook')

  	_regexp-down      	# 选择一个较新的栈帧
     				  	# 默认情况下移动一帧，指定数字参数以控制要移动的帧数

  	_regexp-env       	# 用于查看和设置环境变量

  	_regexp-jump      	# 将程序计数器(PC 寄存器)设置为新地址

  	_regexp-list      	# 使用几种速记格式之一列出相关源代码

  	_regexp-tbreak    	# 使用几种速记格式之一设置一次性断点

  	_regexp-undisplay 	# 在每次程序暂停时，停止执行指定的表达式(通过 stop-hook 索引指定，请参阅 'help target stop-hook')

  	_regexp-up        	# 选择一个较旧的栈帧
     					# 默认情况下移动一帧，指定数字参数以控制要移动的帧数

	apropos				# 用于列出与给定单词或者给定主题相关的调试命令

	breakpoint			# 用于操作断点的命令（参见 'help b' 以查看速记格式）

	command				# 用于管理自定义 LLDB 命令的命令

	disassemble			# 反汇编当前 target 中的指定指令
						# 默认为当前线程的当前栈帧中的当前函数

	expression			# 用于在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	frame				# 用于选择和检视当前线程的栈帧的命令

	gdb-remote			# 用于通过远程 GDB server 连接到一个进程
						# 如果没有指定 host，则假设为 localhost

	gui					# 用于切换到基于 curses 的 GUI 模式

	help				# 用于显示所有调试器命令的列表，或者提供有关特定命令的详细信息

	kdp-remote			# 用于通过远程 KDP server 连接到一个进程
						# 如果没有指定 UDP 的端口，则假设为该端口为 41139

	language			# 用于指定要在运行时操作的语言（C++、OC、RenderScript、Swift）

	log					# 用于控制 LLDB 内部日志记录的命令

	memory				# 用于操作当前目标进程中的内存的命令

	platform			# 用于管理和创建操作系统平台的命令

	plugin				# 用于管理 LLDB 插件的命令

	process				# 用于与当前平台上的进程进行交互的命令

	quit				# 用于退出 LLDB 调试器

	register			# 用于访问当前线程和栈帧的寄存器的命令

	reproducer			# 用于操作复制器的命令
					  	# 复制器可以捕获具有所有依赖项的完整调试会话
					  	# resulting 复制器用于在调试调试器时重放调试会话
						# 因为复制器需要从头到尾的整个调试会话，所以需要以捕获模式(capture mode)或者重放模式(replay mode)启动调试器，通常通过命令行驱动程序
						# 因为复制器是不相关的记录-重放调试，所以在重放期间您不能与调试器交互

	script            	# 使用所提供的代码调用脚本解释器，并显示调用结果
						# 如果没有提供任何代码，则启动交互式解释器

	settings          	# 用于管理 LLDB 设置的命令

	source            	# 用于检查由当前目标进程的调试信息所描述的源代码的命令

	statistics        	# 用于打印有关调试会话的统计信息

	target            	# 用于对调试器 target 进行操作的命令

	thread            	# 用于操作当前进程中的一个或者多个线程的命令

	type              	# 用于在类型系统(type system)上操作的命令

	version           	# 用于显示 LLDB 调试器的版本

	watchpoint        	# 用于操作内存断点的命令

Current command abbreviations (type 'help command alias' for more info):
# 当前已经存在的命令别名（输入 'help command alias' 以获取更多信息）：

	add-dsym  			# 通过指定调试符号文件的路径，或使用选项指定要下载符号的模块，将调试符号文件添加到 target 的当前模块之一

	attach    			# 通过进程 ID 或者进程名称附加到进程

	b         			# 使用几种速记格式之一设置断点

	bt        			# 显示当前线程的调用栈
						# 指定数字参数将用于控制要显示的调用栈的数量，指定 all 参数将显示所有线程的调用栈

	c         			# 继续执行当前进程中的所有线程

	call      			# 在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	continue  			# 继续执行当前进程中的所有线程

	detach    			# 脱离当前的目标进程

	di        			# 反汇编当前 target 中指定的汇编指令
						# 默认为当前线程的当前栈帧中的当前函数

	dis       			# 分解当前 target 中指定的汇编指令
						# 默认为当前线程的当前栈帧中的当前函数

	display   			# 在每次进程暂停时都执行一个给定的表达式（请参阅 'help target stop-hook'）

	down      			# 选择一个较新的栈帧
						# 默认情况下移动一帧，指定数字参数以控制要移动的帧数

	env       			# 用于查看和设置环境变量

	exit      			# 退出 LLDB 调试器

	f         			# 通过给定的索引在当前线程中选择当前栈帧（请参阅 'thread backtrace'）

	file      			# 使用给定的参数作为主可执行文件(main executable)来创建一个 target

	finish    			# 完成当前栈帧的执行，并在返回后暂停
						# 除非指定，否则默认为当前线程

	image     			# 用于访问一个或者多个目标模块的信息的命令

	j         			# 将程序计数器(pc 寄存器)设置为一个新的地址

	jump      			# 将程序计数器(pc 寄存器)设置为一个新的地址

	kill      			# 终止当前的目标进程

	l         			# 使用几种速记格式之一列出相关的源代码

	list      			# 使用几种速记格式之一列出相关的源代码

	n         			# 源码级别的单步执行，以黑盒的方式执行一行代码
						# 除非指定，否则默认为当前线程
						# 遇到子函数不会进入，而是会执行这个子函数，然后继续

	next      			# 源码级别的单步执行，以黑盒的方式执行一行代码
						# 除非指定，否则默认为当前线程
						# 遇到子函数不会进入，而是会执行这个子函数，然后继续

	nexti     			# 汇编指令级别的单步执行，以黑盒的方式执行一条汇编指令
						# 除非指定，否则默认为当前线程
						# 遇到汇编函数不会进入，而是会执行这个汇编函数，然后继续

	ni        			# 汇编指令级别的单步执行，以黑盒的方式执行一条汇编指令
						# 除非指定，否则默认为当前线程
	          			# 遇到汇编函数不会进入，而是会执行这个汇编函数，然后继续

	p         			# 在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	parray    			# parray <COUNT> <EXPRESSION> 
						# LLDB 会执行 EXPRESSION 所标识的表达式以在内存中获取一个指向数组的类型指针
						# 并将显示数组中 COUNT 个该类型的元素

	po        			# 在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	poarray   			# poarray <COUNT> <EXPRESSION>
	          			# LLDB 会执行 EXPRESSION 所标识的表达式以在内存中获取数组中 COUNT 个元素的地址
	          			# 并且对获取到的这些元素调用 po 命令

	print     			# 在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	q         			# 退出 LLDB 调试器

	r         			# 在调试器中启动可执行文件

	rbreak    			# 在可执行文件中设置一个或者一组断点

	re        			# 用于访问当前线程和栈帧的寄存器的命令

	repl      			# 在当前线程上执行给定的表达式，并以 LLDB 的默认格式显示返回值

	run       			# 在调试器中启动可执行文件

	s         			# 源码级别的单步执行，以白盒的方式执行一行代码
						# 除非指定，否则默认为当前线程
						# 遇到子函数就进入并且继续单步执行

	shell     			# 在主机上执行 shell 命令

	si        			# 汇编指令级别的单步执行，以白盒的方式执行一条汇编指令
						# 除非指定，否则默认为当前线程
	          			# 遇到汇编函数就进入并且继续单步执行

	sif       			# 单步执行当前的 block，如果直接进入名称与 TargetFunctionName 匹配的函数，则暂停

	step      			# 源码级别的单步执行，以白盒的方式执行一行代码
						# 除非指定，否则默认为当前线程
						# 遇到子函数就进入并且继续单步执行

	stepi     			# 汇编指令级别的单步执行，以白盒的方式执行一条汇编指令
						# 除非指定，否则默认为当前线程
	          			# 遇到汇编函数就进入并且继续单步执行

	t         			# 更改当前选定的线程

	tbreak    			# 使用几种速记格式之一设置一次性断点

	undisplay 			# 取消 stop-hook（通过 stop-hook index 指定）
						# hcg 注：stop-hook 用于在每次进程暂停时都执行一个给定的表达式（请参阅 'help target stop-hook'）

	up        			# 选择一个较旧的栈帧
						# 默认情况下移动一帧，指定数字参数以控制要移动的帧数

	v         			# 显示当前栈帧中的变量
						# 默认为作用域内的所有参数和局部变量
						# 可以指定参数、局部变量、文件静态变量和文件全局变量的名称
						# 可以指定聚合变量的子变量，例如：'var->child.x'
						# 'frame variable' 指令中的操作符 -> 和 [] 不会调用操作符重载，而是直接访问指定的元素
						# 如果要触发操作符的重载，则请使用 expression 命令来打印变量
						# 值得注意的是，除了重载操作符外，当打印局部变量时，'expr local_var' 和 'frame var local_var' 会产生相同的结果
						# 然而，'frame variable' 更具效率，因为它使用调试信息和直接读取内存，而不是解析和执行一个表达式（这可能涉及 JITing 和在目标程序中运行代码）

	var       			# 显示当前栈帧中的变量
						# 默认为作用域内的所有参数和局部变量
						# 可以指定参数、局部变量、文件静态变量和文件全局变量的名称
						# 可以指定聚合变量的子变量，例如：'var->child.x'
						# 'frame variable' 指令中的操作符 -> 和 [] 不会调用操作符重载，而是直接访问指定的元素
						# 如果要触发操作符的重载，则请使用 expression 命令来打印变量
						# 值得注意的是，除了重载操作符外，当打印局部变量时，'expr local_var' 和 'frame var local_var' 会产生相同的结果
						# 然而，'frame variable' 更具效率，因为它使用调试信息和直接读取内存，而不是解析和执行一个表达式（这可能涉及 JITing 和在目标程序中运行代码）

	vo        			# 显示当前栈帧中的变量
						# 默认为作用域内的所有参数和局部变量
						# 可以指定参数、局部变量、文件静态变量和文件全局变量的名称
						# 可以指定聚合变量的子变量，例如：'var->child.x'
						# 'frame variable' 指令中的操作符 -> 和 [] 不会调用操作符重载，而是直接访问指定的元素
						# 如果要触发操作符的重载，则请使用 expression 命令来打印变量
						# 值得注意的是，除了重载操作符外，当打印局部变量时，'expr local_var' 和 'frame var local_var' 会产生相同的结果
						# 然而，'frame variable' 更具效率，因为它使用调试信息和直接读取内存，而不是解析和执行一个表达式（这可能涉及 JITing 和在目标程序中运行代码）

	x         			# 从当前目标进程的内存中读取数据

For more information on any command, type 'help <command-name>'.
# 有关任何命令的更多信息，请输入 'help <command-name>'

```









