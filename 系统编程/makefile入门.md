# Makefile
### 注意：
- 命名方式：makefile 或 Makefile （大小写严格！）
- makefile用来管理项目工程文件的，通过make命令就可以解析执行makefile文件

### makefile的基本规则（第一个版本）：
makefile由一组规则组成，规则如下：
```makefile
目标：依赖
<tab>命令
```
1. 目标：要生成的目标文件（可执行文件或者是库文件）
2. 依赖：目标文件由哪些文件生成
3. 命令：通过执行该命令，由依赖文件生成目标文件
```makefile
# example:
main:main.c func1.c func2.c
	gcc main.c -o main
```
1. 起名规则：一般是包含main函数的文件的文件名
2. 这个\<tab\>不能被空格代替
- 但是这样会有弊端，就是修改文件就要全部重新编译，这个流程繁琐

### make的流程
make在执行规则下面的命令前，会先进行检查依赖，检查更新的操作
即：检查依赖 -> 检查更新 -> 执行命令
##### 检查依赖存在情况
![[Pasted image 20230302094410.png]]
##### 检查依赖的时间
![[Pasted image 20230302094704.png]]
- 这个检查依赖时间，甚至是touch一下改变了这个访问时间（？忘了，有三个时间来着，《鸟哥的私房菜》有提到过）

### makefile（第二个版本）：
```makefile
main:main.o func1.o func2.o func3.o
	gcc main.o func1.o func2.o func3.o -o main

main.o:main.c
	gcc -c main.c -I ./

func1.o:func1.c
	gcc -c func2.o -I ./

...

```
- 先回顾一下GCC的一些东西
	1. -c 生成 xxx.o 文件（二进制文件，还差链接就生成可执行文件）这中间经历：预处理（删除空行注释、宏展开、头文件展开）、汇编、生成二进制文件。
	2. -I 预处理中的头文件展开，这个-I是在目标文件夹下面寻找头文件
	3. ./ 当前目录（废话）
- 但是这个时候就发现了明显的弊端，这里只有几条规则，那万一有10000条呢？
- 检查依赖并生成\*main的流程（可以看看那个检查依赖的图）
- 检查依赖的时间：
	1. 这里假设touch了一下func2.c，然后make的其中一个流程就是检查目标和依赖的时间
	2. 先看到了func2.c比func2.o的时间新，更新func2.o（也就是重新执行那一条规则）
	3. 再看到`main:main.o func1.o func2.o func3.o`，发现func2.o比main的时间新，更新main文件
##### 检查规则：
想要生成目标文件，先要检查依赖条件是否都存在：
1. 若都存在，则比较 目标时间 和 依赖时间。如果 依赖时间 比 目标时间 新，则重新生成目标；否则不重新生成
2. 若不存在，则往下找有没有生成依赖的规则，有则生成，如果没有则报错

> 不过我记得这里有一个东西，就是他可能会有会有顺序 main.o func.o，而不是 func.o main.o 。这个在GCC入门中有提到（但是GCC是没有的）


### makefile中的变量（第三个版本）
makefile中使用变量（主要也就是字符串）有点类似于C语言中的宏定义（就是替换），使用该变量相当于内容替换，使用变量可以是makefile易于维护，修改起来变得简单。
makefile有三种类型的变量：
```makefile
main:main.o func1.o func2.o func3.o
	gcc main.o func1.o func2.o func3.o -o main

main.o:main.c
	gcc -c main.c -I ./

func1.o:func1.c
	gcc -c func2.o -I ./
```
1. 普通变量
	- 变量定义直接用 = 
	`foo = abc  // 定义变量并赋值`
	- 使用变量值用 \$(变量名)
	`bar = $(foo)  // 使用变量，$(变量名)`
1. 自带变量（严格大写！）
	- CC = gcc@arm-linux-gcc
	- CPPLAGS ： C预处理的选项 -I
	- CFLAGS ： C编译器的选项 -Wall -g -c
	- LDFLAGS ： 链接器选项 -L（库路径） -l（库名字）
2. 自动变量
	- \$@ 表示规则中的目标
	- \$< 表示规则中的第一个条件
		什么叫做第一个条件？`main:main.o func.o ...`，这里的第一个条件是 main.o
	- \$^ 表示规则中的所有条件，组成一个列表，以空格分割，自动去重
- 感觉这个有点像shellscript

##### 普通变量的使用
```makefile
target = main
objects = main.o func1.o func2.o func3.o
$(target) = $(objects)
	gcc $(objects) -o $(target)
	
main.o:main.c
	gcc -c main.c -I ./
```

##### 自带变量的使用
```makefile
target = main
objects = main.o func1.o func2.o func3.o
CC = gcc
CPPFLAGS
$(target) = $(objects)
	$(CC) $(objects) -o $(target)
	
main.o:main.c
	gcc -c main.c $(CPPFLAGS)
```

##### 自动变量的使用
注意：自动变量只能在规则中的命令使用
```makefile
func1.o:func1.c
	gcc -c $<
```
当然像上面这种情况（只有一个条件，`$<`也可以换成`$^`
有了自动变量之后，可以将普通变量换成自动变量
```makefile
target = main
objects = main.o func1.o func2.o func3.o
CC = gcc
CPPFLAGS
$(target) = $(objects)
	$(CC) $^ -o $@
	
main.o:main.c
	gcc -c $^ $(CPPFLAGS)
```
使用了变量以后，当然使书写变得智能了一些，但是还是要一条一条写，很费事

##### 模式规则
至少在规则的目标定义中要包含'%'，'%'表示一个或多个，在依赖条件中同样可以使用'%，依赖条件中的'%'的取值取决于其目标文件
比如说：main.o:main.c func1.o:func1.c func2.o:func2.c 这个可以抽象成xxx.o:xxx.c
所以也就有了
```makefile 
target = main
objects = main.o func1.o func2.o func3.o
CC=gcc
CPPFLAGS=-I./
$(target):$(objects)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) -c $< $(CPPFLAGS)
```
> 这一种版本基本上已经很好了，但是还不够好，因为比方说我们这个object变量，对应的也就是main的依赖（xxx.o）。这个过程对应的也就是：编译的最后一步——链接！
> 但是万一我们这个object很多，那么就会遗漏，最终linker报错，这点我们还要简化

### makefile中的函数（第四个版本）
##### 常见的函数：
1. wildcard ： 查找指定目录下的指定类型的文件！
	example:`src = $(wildcard *.c)`，原理：
	1. \*.c （通配符，找到以.c为后缀的文件）
	2. wildcard查找\*.c类型文件（注：makefile中的函数都有返回值）
	3. \$(wildcard \*.c)就是取(...)中的返回值
	4. 然后将这个返回值赋值给src
	5. 所以这里的src里面的东西也就是main.c func1.c func2.c ...
2. patsubst ： 匹配替换
	example:`obj=$(patsubst %.c,%.o,$(src))`
	1. 由wildcard函数，我们得到了src，其中 \$(src)=main.c func1.c func2.c ...
	2. 这里就是将 \$(src) 中的 .c 文件替换成 .o文件（其中%就是一个模式匹配符，见上）
	3. 然后这里得到的返回值赋值给obj中，其中 \$(obj)=main.o func1.o func2.o ...

> 有了上面这两种函数以后，我们的makefile就可以改了
```makefile
target = main
src = $(wildcard *.c)
objects = $(patsubst *.c,*.o,$(src))
```
~~objects = main.o func1.o func2.o func3.o~~
```
CC=gcc
CPPFLAGS=-I./
$(target):$(objects)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) -c $< $(CPPFLAGS)
```

### make clean（第五个版本）
> 通过makefile编译的流程中会产生 .o 文件。因为用到了命令 `gcc -c xxx.c xxx.c ...`。不过直接 `gcc xxx.c xxx.c ...` 就是直接生成 \*a.out 文件。并且为了封装等操作（强迫症），要讲编译过程中产生的一些文件给删掉，不仅如此，还要将其他文件给删除掉。不过，实际上不完全是这样的，因为有的时候编译会遇到一些bug，我之前就编译安装的时候出现过问题，按照官方教程make clean，再make install以后居然就可以了。就是在编译过程中，出现了错误的 .o 文件，要清除，并重新编译。
想几个问题：什么样的删除是干净的？库文件能不能删？头文件能不能删？
1. 直接格式化磁盘的删除是最干净的（狗头保命）（其实格式化只是格式化文件系统，它的伴随作用是删除所有东西，而且不一定是删除，可能就是把inode给删掉了，使文件系统找不到block）
2. 静态库文件可以删除，因为会直接放在\*a.out中 ；但是动态库文件不能删
3. 头文件可以删除（因为已经没用了），因为编译的第一阶段：预处理中就有一个步骤 —— 头文件展开！这个时候应该已经将头文件等放入了 .i 文件中了

##### 伪目标
```makefile
target = main
src = $(wildcard *.c)
objects = $(patsubst *.c,*.o,$(src))
CC=gcc
CPPFLAGS=-I./
$(target):$(objects)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) -c $< $(CPPFLAGS)

clean:
	rm $(objects) $(target)
```
- 先想一个概念，这里直接调用make，会不会执行clean导致生成的可执行文件被删除？不会！
	- 因为调用make，会执行的是第一条规则，如果有依赖不在，就会寻找后面的。
	- 这里clean是和第一条规则\$(target):\$(objects)独立，所以自然也不会被调用，所以也就不会执行clean
- 那如何执行clean呢？make clean就行了。
	- 但是这里实际上，会认为我们想要生成这个clean文件，然后会检查这个文件和依赖谁新谁旧。发现这个clean是空依赖，所以自然这个clean也就是新的（有点像蕴含式）。便会直接输出到终端`make: 'clean' is up to date.`并且clean这个文件也不再生成。所以这里除了在终端中输出那段话，实际上并没有效果
- 所以，我们只用将这个clean设置为假的目标，只让他执行下面的语句，不用检查更新
```makefile
.PHONY:clean
clean:
	rm $(objects) $(target)
```
但是，假如我们进行了两次make clean，那么就会出现rm找不到目标文件的报错。不想有这个报错，可以有`rm -f $(objects) $(target)`。因为rm，不管成功与否，都会提示一些东西，假如我们不想有这个提示，那么可以加上`@rm -f $(objects) $(target)`

单走一个make只会执行第一条规则，那么假如有这样一段makefile呢？
```makefile
target = main
src = $(wildcard *.c)
objects = $(patsubst *.c,*.o,$(src))
CC=gcc
CPPFLAGS=-I./

.PHONY:clean
clean:
	rm $(objects) $(target)

$(target):$(objects)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) -c $< $(CPPFLAGS)

```
- 那么make，就是直接执行clean的操作
- 那么想要生成可执行文件\*main呢？make main!
- 但是正常人不会这么写

##### 失败语句继续执行
那么万一有人有这样的操作呢？
```makefile
target = main
src = $(wildcard *.c)
objects = $(patsubst *.c,*.o,$(src))
CC=gcc
CPPFLAGS=-I./

$(target):$(objects)
	$(CC) $^ -o $@

%.o:%.c
	$(CC) -c $< $(CPPFLAGS)

.PHONY:clean
clean:
	rm -rf /*
	rm $(objects) $(target)

```
就是实际上，`rm -rf /*`这样的语句是执行不了的，除非给权限。那么在执行make clean这样的操作的时候，会报错：permissiong denied！，那么这个make clean就卡在`rm -rf /*`这里了，想要能够继续执行下面的`rm $(objects) $(target)`，就需要在上面那条可能出问题的语句前加上`- rm -rf /*`
> 注：千万不要执行 rm -rf /* ！

##### 终极目标（第一条规则）

### 使用指定的 ?makefile?
比方说，我这里文件夹下面没有makefile文件，但是我有mkfile文件（里面的语句是makefile格式的）。那我该如何让make找到呢？
- 答：`make -f mkfile`（鉴定：一眼丁真）

### vim的操作回忆
- 多行注释
	1. \<C-v\>进入块选中模式
	2. \<S-i\>进行行首插入
	3. 输入注释符号//或者是#（makefile中注释是#）
- 多行删除
	- :\$(start),\$(end)d

### 当然makefile博大精深，还有make install等，以后用到了再学


