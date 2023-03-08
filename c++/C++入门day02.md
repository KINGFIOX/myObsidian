### 设计一个类

^5ace05

- 突然有点明白了，上一章[[C++入门day01#^0f3ea0]]。就是讲结构体的时候，就说到C++中对结构体有增强，结构体中可以包含函数。这个类和结构体很像，有点像是一个自定义的数据类型，但是又不完全是一个数据类型
```C++
class circle
{
public:  // 公共权限

	// 求圆周长
	double calculateZC()
	{
		return 2*PI*m_R;
	}
	void setR(int r)
	{
		m_R = r;
	}
	int getR()
	{
		return m_R;
	}
	// 半径
	int m_R;
};
int main(void)
{
	circle c1;  // 通过类创建一个对象（实例化对象）
	// 给c1半径赋值
	c1.m_R=10;
	ca.setR(10);
	cout<< c1.calculateZC() << endl;
}
```
##### 专业术语：
- 实例化对象：通过类创建一个对象
- 成员函数、成员方法：类中的函数
- 成员变量、成员属性：类中的变量 ^bacaf2

### 内联函数
##### 宏缺陷：
1. 必须要加括号保证运算的完整性
2. 即使加了括号，有些运算依然与预期不符，比方说
```C
#define MYCOMPARE(a,b) (((a) < (b)) ? (a) : (b))
int main(void)
{
	int a = 10;
	int b = 20;
	int ret = MYCOMPARE(++a,b);  // 我们的预期是 11
	cout<<"ret = "<<ret<<endl;   // 但是这里输出的结果是12
	// 宏函数会出现这样的问题
	// 但是普通函数不会出现这样的问题
}
```
##### 内联函数：
![](Pasted%20image%2020230225094301.png)
##### 内联函数的声明和实现必须同时加关键字inline
![](Pasted%20image%2020230225094442.png)
##### 类内部的成员函数都隐式地加上关键字inline
##### 编译器与内联函数
![](Pasted%20image%2020230225094719.png)
- 不能对函数取地址，因为内联函数会展开，展开后都没有函数了，所以也就没有地址
![](Pasted%20image%2020230225094732.png)
- 假如对所有的函数都加上inline 实际上并不一定会内联，假如不加，编译器也可能会主动转化为内联（不用特意去加）

### 函数的默认参数和占位参数
- 如果不是从左到右都有默认参数，那么传参就有问题
- 突然记得，好像python也确实有默认参数
- 因为python传参甚至可以是无序的，是用键值对的形式传参的
![](Pasted%20image%2020230225100211.png)
- 不然就会产生冲突，比方说上下不知道用哪一个默认参数
- 声明和实现中，只有一个能有默认参数！
![](Pasted%20image%2020230225100415.png)
- 想一下，在 C 语言中，好像也有这个占位参数，不不不！C 语言的“占位参数”是在函数声明时的，但是这个占位参数却是在函数定义的时候有的
- 占位参数暂时没有用，在后面函数重载的时候有用。用到的地方，比方说，这个后缀递增，就要加这个占位参数，来区分前缀与后缀
- 这个占位参数甚至还可以有默认参数`void func(int a, int = 10){}`
![](Pasted%20image%2020230225100744.png)

### 函数重载
- 比方说这样的情况，假如是由两个 `void test()` 和 `void test()` 。这个函数名字相同，并且传的参数相同，就会报错。但是假如函数名相同，但是传的参数不同，有 `void test()` 和 `void test(int a)`就不会报错，这个就叫做函数重载
- 不同的语境导致调用的函数不同
![](Pasted%20image%2020230225101756.png)
##### 函数重载的条件
- 在同一个作用域（注意全局函数、类的成员函数，这两个作用域是不同的，所以就不算是重载）
- 函数的名称相同
- 参数的个数不同，类型不同，顺序不同（这三个满足一个就行了）
##### 返回值可以不以作为函数重载的条件？
这是不可以的！！！
*为什么不可以？*
因为返回值可以接收，也可以不接收。
##### 这个参数不同，甚至可以是一个有const修饰的，另一个是没有const修饰的
那假如比方说是这样一种呢？那究竟是调用哪一个？
- 像这一种，`a` 是一个变量，给变量创建引用，就是 `int &a` ，所以调用的就是上面的
- 但是，如果是 `10` ，这是一个常量，想要给常量创建应用，就要是 `const int &a` ，所以也就是会调用下面的，[常量的引用](C++入门day01.md#^c571f0)
![](Pasted%20image%2020230225104340.png)
- 下面这种情况，虽然三种版本可以同时存在，但是还是有问题，需要避免二义性的存在
![](Pasted%20image%2020230225133002.png)
- 当使用默认参数的时候函数重载也要防止出现二义性
![](Pasted%20image%2020230225133359.png)
##### 函数重载的实现原理（编译器）
- 编译器帮我们把函数的名字修改了
![](Pasted%20image%2020230225135009.png)

### externC浅析
应用场景，就是假如我要在C++里面引用C语言里面的东西
比方说下面在一段代码
```C++
#include "func.h"
void test()
{
	show();  // 其中这个show是在func.c中的
}
```
这个时候会报错，错误内容是：一个无法解析的外部命令！！！
因为C++有重载的特性，所以就会在函数加一些修饰的，比方说这里的`test`，被修饰了以后可能就`_Z4testv`，然而在C源文件中没有这样一个`_Z4testv`的函数，这里就是要通过externC来处理
例如
```C++
// 其中这里原本的 #include "func.h" 不用了
// 如果保留这个头文件引用，就会导致头文件重复包含，下面已经有extern了
extern "C" void test();
// 这里extern表示从外部找，然后"C"就表示C语言
// 告诉编译器，用C语言方式做链接
void test()
{
	show();  // 其中这个show是在func.c中的
}
```
但是，这样就太麻烦了，那岂不是每次写C++程序的时候，这样的C语言的代码就要有好多行？
这就要两个步骤
```C++
// main.cpp

#include "func.h"  // 依然包含C语言的头文件
void test()
{
	show();  // 其中这个show是在func.c中的
}
```
C语言头文件中，如下
```C
// func.h

#ifdef __cplusplus
extern "C" {
#endif

#include <stdio.h>
void func();

#ifdef __cplusplus
}
#endif
```

### 类
##### 面向对象的三大特性：
- 封装
- 继承
- 多态
先说一下C语言中不好的现象：属性和行为（函数/方法）不能分开
比方说下面这张图的情况，因为`struct Person`和`struct Dog`的属性是相同的，所以在调用DogEat的时候，人也能传进来，这就不是很好。所以需要封装
![](Pasted%20image%2020230225144909.png)
##### 封装
- 概念：将属性和行为作为一个整体，来表现生活中的事物
- 第二层理念：将属性和行为加以权限进行控制
##### struct和class区别
```C++
struct dog{
	char name[64];
	int age;
	void dogEat()
	{
		printf("%s在吃狗粮",name);
	}
}

class dog{
	char name[64];
	int age;
	void dogEat()
	{
		printf("%s在吃狗粮",name);
	}
}

int main(void)
{
	struct dog d;
	// 如果没有在class里面加public的话
	// 这两段代码会报错：xxx不可访问！！！
	strcpy(d.name,"老王");
	d.dogEat();
}
```
回想一下上面的代码[](C++入门day02.md#^5ace05)，对比一下，上面的class有`public`公共权限。假如将代码改成这样，就不会报错
- 因为class默认的权限是`私有权限`(private)
- 而struct的默认权限是`公共权限`
```C++
class dog{
public:
	char name[64];
	int age;
	void dogEat()
	{
		printf("%s在吃狗粮",name);
	}
}
```
##### C++中的三种权限
成员变量、成员函数[](C++入门day02.md#^bacaf2)
|权限|关键字|区别|
|---|---|---|
|公共|public|成员在类内、类外都能访问到|
|私有|private|成员在类内可以访问，在类外不能访问|
|保护|protected|成员在类内可以访问，在类外不能访问|
- 这里会感觉私有和保护是一样的，但是实际上不一样，在继承上表型的就不一样
- 二字可以访问父亲的protected权限里面的东西，但是儿子不能访问private权限里面的东西
![](Pasted%20image%2020230225152852.png)

- 之前学python的对象的时候，C语言还没有学结构体。现在我对结构体有了一些更加深刻的认识。当然同时，我现在也对这个类和对象有了一定的认识，感觉定义起来确实比较自然。
##### 尽量将成员的属性设置为私有
为什么这么做?因为可以自己控制读写的权限
```C++
class Person
{
private:
	string m_name;   // 姓名 读写
	int m_age;       // 年龄 只读
	string m_lover;  // 情人 只写
public:
	string getName()
	{
		return m_name;
	}
	void setName(string name)
	{
		m_name = name;
	}
	void setLover(string lover)
	{
		m_lover = lover;
	}
	int getAge()
	{
		return m_age;
	}

}
int main(void)
{
	Person p;
	// 这里实际上将字符数组转换成了string类型
	p.setName("张三");
	cout<<”姓名： “<<p.getName()<<endl;
}

```
- 不过我又开始惊奇于C++，因为在C语言中，string就是char*，并且加入要对字符串进行操作的话要strcpy(dist,src)。并且肯定是不能通过返回局部自动变量的形式来返回字符串的，也就是不能返回char*







