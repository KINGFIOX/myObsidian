如果在编译过程中，出现了乱码提示，需要转换语言标注
```Shell
$ export LANG=C
```

```Shell
$ ./a.out
```
- 假如没有加 `./`，那么就是在环境目录下面的可执行文件，而不是执行当前文件夹里面的文件
```Shell
$ gcc -Wall xxx.c -o xxx 
```
- 这个 `-o` 可以指定输出的文件名
- 这个 `-W` 表示会报 warning ，但是还是会生成可执行文件

下面两种包含头文件的方式的规定
`#include "FILE.h"` 是现在当前目录去找
`#include <FILE.h>` 是去系统同文件库中找
当然还有可以去其他文件夹中找的方法

### verbos compilation
`gcc -v` 可以打印详细信息

### 多文件编译
多文件编译，不能一次编译成一个可执行文件，但是会生成一个object文件，这个object在 `Windows` 是 `.obj`；在`unix`是`.o`
`Linker`（链接器），输入object，输出可执行文件
- 注意：gcc一个外壳，实际上的链接器是`ld`
在这里可以找到：
```Shell                                                                                                  
-rwxr-xr-x  76 root  wheel  167136  2  9 17:39 /usr/bin/ld
```

### 怎样输出object文件
```Shell
$ gcc -c xxx.c xxx.c
```
编译文件的时候，永远不要包含头文件`.h`

### 从object产生可执行文件
```Shell
$ gcc xxx1.o xxx2.o
```

### 链接的次序
![[B84A792F-8AAB-4C47-B55B-EFF9370A17FF_1_201_a.jpeg]]
1. 这个大概意思就是：从做到右读。也就是，如果在 `main.c`中有`#include "hello.c"`，那么在链接的时候，就要有。（调用者在前面，定义者在后面）
```Shell
$ gcc main.o hello.o
```
2. 但是现在大多数编译器都会自己寻找头文件包含了，当然gcc肯定是比较先进和智能的
3. 假如真的没发现什么错误，你可以考虑一下使用编译器的时候，颠倒编译的顺序
4. 事实上，链接的速度比编译快很多

### 链接外部的库
```Shell
$ gcc -Wall main.o /usr/lib/libm.a -o calc
```
- 还可以使用下面这种方式链接
```Shell
$ gcc -Wall main.o -lm -o calc
```
- 这里 `-l` 表示 lib 或者是 link ，就是一种快捷选项
- 这里 `(-m)` 表示用的是系统的标准库（mac的目录在/usr/local/lib下的）
```Shell
$ ls -ltr *.a                                                                                                           
-rw-r--r--  1 root  wheel   399392  1 19 13:51 liblua.a
-rw-r--r--  1 root  wheel  1403176  1 20 02:11 libsodium.a
-rw-r--r--  1 root  wheel   285000  1 20 02:27 libmbedtls.a
-rw-r--r--  1 root  wheel    67792  1 20 02:27 libmbedx509.a
-rw-r--r--  1 root  wheel   774216  1 20 02:27 libmbedcrypto.a
-rw-r--r--  1 root  wheel  1128704  1 20 02:35 libuv.a
```
假如要链接指定的库，那就要`gcc -l...`，比方说要链接`liblua.a`的话，就可以使用`-llua`，或者是`-luv`
- 参数 `-i` 表示指定一个头文件
- 参数 `-l` 表示指定一个链接的目录
- 参数 `-I` 表示指定头文件的目录
- 参数 `-L` 表示指定链接文件的目录
- 不要在头文件中使用绝对路径（这样会降低程序的可移植性），但是可以通过 `-I` 来告诉编译器去寻找头文件的目录
- 不过可以使用相对路径
- 还可以在 `.zshrc` 或者是 `.bash_profile` 中添加环境变量
```zsh
# 当然这里只是头文件的环境变量
# for C header files
$ export C_INCLUDE_PATH=xxx:xxx:xxx:...
# for C++ header files
$ export CPLUS_INCLUDE_path=xxx:xxx:xxx:...
# 如果是库的话，可以使用下面的代码
# 这里尾部加上$LIBRARY_PATH是防止原本的环境变量被覆盖掉
$ export LIBRARY_PATH=xxx:$LIBRARY_PATH
```

^f9359a

优先顺序：
1. 命令行参数（推荐！！！） ^8b9d69
2. 设置环境变量
3. 默认的，本地库目录（最不推荐！！！）

### ar创建库、搜索路径
作用：简单来说，ar 就是将一大堆的 object 文件捏在一起，形成一个库文件（这样好管理）
当然这个前提是要有一堆的 object 文件
```Shell
$ ar cr libNAME.a file1.o file2.o ... filen.o
```
这里参数的解释：
- 参数 c 表示creat创建
- 参数 r 表示replace覆盖

```Shell
$ ar t /usr/local/lib/liblua.a 
__.SYMDEF SORTED
lapi.o
lcode.o
lctype.o
ldebug.o
ldo.o
ldump.o
lfunc.o
lgc.o
llex.o
```

- 如果我们要使用库中的内容，那也同样要引用头文件`#include "mylib.h"`
- 那么在使用gcc编译的时候，可以有
```Shell
# 这样就可以生成可执行文件a.out
$ gcc -Wall main.c libMYLIB.a
```
这里的时候，就要考虑次序，一定要把调用者放到前面，被调用者放在后面。（这里是两个不同类型的文件放在一起进行编译）
假设libMYLIB.a里面有函数func1.o和func2.o，那么还是可以写成
```Shell
# 这个时候是同一种类型的，就可以不用考虑次序
$ gcc -Wall main.o func1.o func2.o
```
当然这里还可以写成：
```Shell
# 这里就是用命令行参数的方法[[GCC入门#^8b9d69]]
$ gcc -Wall main.o -L . -lMYLIB
```

### 等等，我这里意识到了一个问题
对于参数 `-I` 是对 main.c 来说的，这取决于编译的流程。因为头文件展开是在第一阶段“预处理”时期执行的。对应的编译代码也就是下面这样
```Shell
$ gcc -Wall -Iinclude -c main.c
```
这里对于编译的流程给下面一张图
![[gcc编译的4步骤.jpeg]]
刚才可能没有特别理解这个参数 `-c` 是什么意思，这里给出解释，这里 `-c` 是经历了 `预处理去掉注释多余行、宏展开、头文件展开` `编译成汇编` `汇编再变成二进制文件` 这一些列流程，从一个 `.c` 变成一个 `.o` 目标文件（或者是二进制文件）。
==然后在这里的时候，才涉及到链接，有Shell指令：==
```Shell
$ gcc -Wall -Llib 
```
一般情况下，一个工程文件会建立这样的结构
```Shell
$ ls -l
drwxr-xr-x@ 2 wangfiox  staff    64B  2 23 19:27 include
drwxr-xr-x  2 wangfiox  staff    64B  2 23 19:27 lib
-rw-r--r--  1 wangfiox  staff     0B  2 23 19:27 main.c
```

### 动态库
先说一下静态库的缺点：
- 假如我们用了别人的库，而且用了很多别人的不同库，而且这些库里面的函数利用率低，那么就有大问题，因为使用了很多库，所有就导致工程文件就比较大，而且还会占用比较大的内存。所以就要解决这样的问题。
- 除此之外，还比如，如果有多个进程，使用了同一个著名的库，那么在内存中会占用很大一部分资源，或者是在硬盘里面储存占了很大一部分空间。
为了解决这样的问题，提出了动态库(shared object)，拓展名是 `.so`（笨人用的是mac）
- 可执行文件里面是不包含真正的动态库的执行代码。最后只用把一个动态库放到内存里，然后这多个执行程序都可以共享，就是动态库
- 动态库在链接的时候，实际上包含的是一个小表，表里面包含的信息足够我们去定位需要调用的函数，而不是真正的将库里的机器代码copy到可执行文件里面。
- 在执行程序之前，会将动态库的机器码 copy 到内存中
- 动态库还可以，就是比方说动态库版本更新了，不必重新编译源文件
- gcc会优先使用动态库
- 当可执行文件执行的时候，必须要有一种方法能发现并且把动态库加载到内存里面去
==这个动态库那么好，但是需要定位：==（几种方法：类比gcc寻找头文件、库文件）
1. 缺省目录里面（本地标准库）:"/usr/local/lib" 或者是 "/usr/lib"
2. 添加路径到环境变量 `$ export LD_LIBRARY_PATH=xxx:xxx:$LD_LIBRARY_PATH` （推荐！！！）
- 回顾一下上面的[[GCC入门#^f9359a]]
3. 命令行指定动态库所在的目录


### C 语言标准
##### GNU 和 ANSI/ISO 方言的差别
![[Pasted image 20230223200754.png]]
##### ANIS/ISO
![[Pasted image 20230223200728.png]]
下面写一个方言的例子：
```C
#include <stdio.h>
int main(void)
{
	const char asm[]="6502";
	printf("the string is %s\n",asm);
	return 0;
}
```
这里asm都变色了，不对劲。这里asm是一个关键字，在ANSI C中是没有的，但是在GNU C中有，意思是在 C 语言中嵌套汇编语言。然后这里使用的是 gcc ，这里就默认他是关键字了，众所周知，关键字是不能作为变量名的，这个时候可以使用参数 `-ansi` 来编译
```Shell
$ gcc -Wall -ansi main.c
```
再比如：
```C
#include <stdio.h.
int main(int argc, char* argc[])
{
	int i,n=argc;
	double x[n];
	for (i=0;i<n;i++)
	{
		x[i]=i;
	}
	return 0;
}
```
这里肯定是有问题的，因为x[n],这里 n 明明是一个变量。但是
```Shell
$ gcc -Wall -ansi main.c
```
这个是可以编译通过的，但这在C89肯定是不允许的
首先，这个可变数组，虽然不是标准 C 的一个特性，但是不与标准 C 冲突，就可以编译过
下面是严格的ANSI，加入参数`-pedantic`
```Shell
$ gcc -Wall -ansi -pedantic main.c
```
> 想要很好的学习 C 语言，很好的案例是 mysql 等著名可移植性非常好的开源软件

### -Wall 参数（作为初学者一定要加上）
![[Pasted image 20230223203908.png]]
`-Wall` 表示选择了全部的warning选项
##### `-Wcomment`
检测注释是不是嵌套的，注意！！严格地，注释不能嵌套
![[Pasted image 20230223204024.png]]
为什么会出现初始嵌套？比方说我们不想要删掉代码，代码里面可能会有其他注释，所以这个注释可能嵌套（如下图），这有安全隐患
![[Pasted image 20230223204253.png]]
这个时候怎么办呢？可以使用宏定义（宏实际上是写给 `预处理器` 的）
![[Pasted image 20230223204344.png]]

##### `-Wformat`
![[Pasted image 20230223204613.png]]
就是格式化输出，格式化输入的时候可能会犯错，这里就是用来报warning的

##### `-Wunused`
![[Pasted image 20230223204729.png]]
这个是用来检测没用到的变量的

##### `-Wimplicit`
![[Pasted image 20230223204831.png]]
这个是使用了一个函数或变量，但是没有定义。常见情况，使用了一个函数，但是没有包含他的头文件

##### `-Wreturn-type`
![[Pasted image 20230223205015.png]]
检测 return 或者是没 return

### GCC预处理器
GCC pre processor ，简称 cpp
用 `gcc -Wall -DTEST dtest.c` 编译的时候，就是用到了这个 TEST ，然后就会走 \#ifdef TEST ... \#endif 里面的内容，否则不会走里面的内容
![[Pasted image 20230223210812.png]]
可以通过输入指令，看到很多系统默认定义好的东西
```Shell
$ cpp -dM /dev/null                                                                                                     
#define _LP64 1
...
#define __ARM_FEATURE_COMPLEX 1
...
```
当然还可以使用宏参数
```C
#include <stdio.h>
int main(void)
{
	printf("NUM is %d",NUM);
}
```
这个代码显然是由问题的
但是可以使用宏参数，就能够正常通过编译，并且输出"NUM is 123"
```Shell
$ gcc -Wall -DNUM=123 main.c
```
因为这里只是一个简单的宏替换，甚至可以这样操作
```Shell
$ gcc -Wall -DNUM="1+2" main.c
```
下面这个就是只进行预处理的操作
```Shell
$ gcc -E main.c
```
下面加入参数 `-save-temps` 就可以保存编译过程中产生的临时文件到当前文件夹
预处理后的`main.i` ，汇编后的`main.s` ，生成的二进制文件`main.o`
```Shell
$ gcc -Wall -c -save-temps main.c
```

```Shell
# 可以用来查看core dump
$ ulimit -c 
# 如果输出 0，则表示不允许产生core dump
# 那要怎么才能产生core dump呢?
$ ulimit -c unlimited
```
程序崩溃了，操作系统把程序kill之前，把该程序的所有内存信息都拷贝到一个文件中，也就是core文件，一般情况下是不允许产生core文件的，因为这个core文件可能会很大（相对于源码而言）

下面就是要调试文件，把可执行文件和core文件都输入到gdb中
```Shell
# 第一个是可执行文件名，第二个是core文件
$ gdb a.out core.2534
```
下面输出的这个可执行文件a.out记录了debug信息，他将函数名称，行号等都放入了一个符号表里面，当然在release版本的时候，编译肯定是不含 `-g` 的。含有debug信息的a.out，运行的时候会占内存之类的
```Shell
$ gcc -Wall -g main.c
```

### 编译器优化（源码的两种常见优化）

^8bb7f8

##### Common Subexpression Elimination（公共的子表达式的消除）
举个例子：
```math
f = cox(x)*(1+sin(x/2)+sin(x)*(1-sin(x/2)))
```
就想计算这个，出现了两次 `sin(x/2)` ，作为人类，我们理所当然的会复用 `sin(x/2)`，因为这样可以减少计算量，作为计算机也是如此
```math
int t = sin(x/2);
f = cos(x)*((1+t)+sin(x)*(1-t))
```
##### Function Inlinging（内嵌式函数）
- 这个和函数调用相关（这个我不会）（这个也好像和汇编有关）
- 简单来说，这个编译器优化就是减小这个函数调用时的开销

举个例子：
```C
#include <stdio.h>
double sq(double x)
{
	return (x*x);
}

int main (void)
{
	int x=10;
	int y=sq(x);
}

```
像上面这种，计算一个平方这么简单的事情，但是却调用一个函数，奢侈
所以编译器会这样帮我们优化，写到了同一行（内嵌式）（类似的，实际上更复杂的内嵌式，编译器也会帮我们操作）（我笨人感觉这里肯定是有一种类似于复杂程度的标准的）
```C
#include <stdio.h>
int main(void)
[
	int x=10;
	int y=(int*)(x*x);
]
```
这不经让我想起来了宏函数
不过，除了编译器的自动操作，还可以我们的手动操作
```C
#include <stdio.h>
inline double sq(void)
{
	...
}
int main(void)
{
	...
}
```
当然，实际上优化不总是十全十美的，比防说PC机上，通常追求速度牺牲空间（内存），而在嵌入式开发中，受制于物理，不得不降低速度，减少内存上的开销。`Tradeoffs权衡`
![[53BE16B4-5AC1-4AE5-824A-E4A4A0DE8EA7_1_201_a.jpeg]]
##### Loop Unrolling（这是一个好东西）
```C
for (int i=0;i<8;i++)
{
	y[i]=i;
}
```
像这样一个代码，而且肯定也只能在一个线程中执行（因为是单个循环）
这样的代码，首先只能是单线程的，其次：每执行完一次循环就要进行一次判断，优化：
```C
y[0]=0
...
y[7]=7
```
这样可以多并发，但是增大了代码量，增加了内存占用
再举个例子：
```C
/*假如终止条件是一个变量呢？可能就无法改成像上面那一种代码了*/
for (int i=0;i<n;i++)
{
	y[i]=i;
}
```
这样的代码可以改写成（当然前提是能够正确判断开始和终止条件）
```C
/*先消去奇数*/
int i;
for (i=0;i<(n%2);i++)
{
	y[i]=i;
}
for (;i+1<n;i+=2) /*no initializer*/
{
	y[i]=i;
	y[i+1]=i+1;
}
```

##### Scheduling（机器码层面的优化）（调度问题）
![[45702B14-F0B8-4FE8-999B-368D9EDFDC4A_1_201_a.jpeg]]
实际上就是一种调度算法（但是这样会增加编译所需要的时间）

### Optimization Levels优化等级
![[722CB6AC-3F2B-4D0E-98FF-BC98D1926967_1_201_a 1.jpeg]]
- `-O0` 就表示的没有优化
- 但是优化不是等级越高运行时间就越短
- 优化和调试通常是矛盾的，因为优化会将代码改的面目全非，反而不利于调试
- 一般工业化生产的是使用 O2，但是像阿帕奇这种软件，编译的时候使用的就是 O3，非常有自信，或者是已经进行过充分的调试了
- 除了上述三种优化等级以外，还有 `-funroll-loops` 就是针对循环的优化
##### 但是GCC的优化和调试并不矛盾
- GCC在编译优化的时候，会扫描变量是不是会有无定义的情况
![[75BC23F2-84F9-4DD3-A3D7-355F27A24A5B_1_201_a 1.jpeg]]


### C++的编译器G++
![[68AEB2FC-94F0-4857-B736-26282F902DD9_1_201_a.jpeg]]
- 这里 G++ 和 GCC 一定要区分
- G++是少数的真正的 C++编译器，因为他将 C++文件直接编译成汇编，而很多 C++编译器会将 C++先转换为 C 语言，再进行一系列编译流程。首先这样编译的时间变长，其次，降低了可执行文件的性能
- 文件后缀
| C | C++ |
| --- | --- |
| .c | .cc/.cpp |
| .i | .ii |



### 编译流程
##### The PreProcessor
- 头文件展开的时候
- `#include <xxx.h>` 是寻找系统的库(/usr/lib:/usr/local/lib)
- `#include "xxx.h"` 是寻找本地的库

##### The Compiler
```Shell
# -S 将预处理以后的文件转换成汇编文件
# 其中参数-Wall就是在这一阶段发挥作用的
# 这个是最复杂的一步
$ gcc -Wall -S hello.i
```

##### assembler汇编器
而这里可能会空一些东西，这些空的东西是没有定义的。而这些空的东西需要使用外部库的东西来填充，这一步后面需要使用链接器
```Shell
# 这一步将汇编语言转换为机器码
# 也就是生成object
$ as hello.s
```

##### The Linker
（这个比较复杂）

### GCC辅助命令
##### file
```Shell
# file可以查看文件的一些属性
$ file a.out
```
![[644F6257-5C1A-4DDF-A213-78200463CBAB_1_201_a.jpeg]]
##### nm
```Shell
# 使用 nm 命令分析可执行文件
$ nm a.out                                                                     
0000000100003a58 T _Pop_SeqStack
                 U ___memset_chk
0000000100000000 T __mh_execute_header
0000000100003bc0 T _destroy_SeqStack
                 U _free
0000000100003998 T _init_SeqStack
0000000100003b68 T _isEmpty_SeqStack
0000000100003e30 T _main
                 U _malloc
0000000100003bfc T _nonRecursion
                 U _printf
00000001000039dc T _push_SeqStack
0000000100003b24 T _size_SeqStack
0000000100003d10 T _test01
0000000100003ac0 T _top_SeqStack
```
![[A97B35AE-521F-4B7F-82BA-73BCCE4365BF_1_201_a.jpeg]]
- T表示自包含的，就是在可执行文件里面就包含了库的可执行文件
- U表示没有定义的，通常是动态链接库
##### ldd
```Shell
# 这个命令mac没有 
# 这个命令可以查到上述nm中U标志的指向的
$ ldd a.out
```
![[94CA0F7A-0825-4C79-9CB6-B1B2DCD5D44A_1_201_a.jpeg]]

##### gprof
这个工具可以找出程序运行时，开销最大的函数，然后就可以针对这个函数进行优化
- 当然这里只是入门
```Shell
# 这个是将编译器的gprof工具内嵌到了可执行文件中
# 可以帮忙我们检查每个函数运行的时间
$ gcc -Wall -pg main.c
$ ./a.out
# 然后就会生成一个gmon.out文件
$ ls
a.out* gmon.out* main.c
# 这个时候使用gprof，就会读取gmon.out里面的信息，然后就会输出很多东西
$ gprof a.out
```

##### gcov（覆盖度测试）
可以逐行检测代码运行的次数
```Shell
$ gcc -Wall -fprofile-arcs -ftest-coverage main.c
$ ls
a.out main.c cov.gcno
# 执行文件
$ ./a.out
$ ls
# 可见产生了新文件cov.gcno
a.out main.c cov.gcda cov.gcno
$ gcov main.c
$ ls
# 这个时候产生了一个新的文件cov.c.gcov
a.out main.c cov.c.gcov cov.gcda cov.gcno
# 这个时候可以查看cov.c.gcov，第一列就是可以看到执行次数
$ echo cov.c.gcov
```
- gprof 、gcov 是在排完错误，已经检测完潜在的漏洞的时候，就可以开启优化选项，提高运行效率

