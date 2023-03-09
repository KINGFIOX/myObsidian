### 关系运算符重载



### 函数运算符重载（仿函数、函数对象）
- 匿名对象，匿名的函数对象
- 特点：当前行执行完立即释放
```C++
class MyClass{
	int operator()(int a,int b){
		return a+b
	}
}
cout<<MyClass(1,1)<<endl;
```
- 仿函数，函数对象



### 不要重载的符号&&，||
短路特性：比方说 p&&q ，如果 p=false，那么这个直接就判定为false了。如果 p||q ，其中 p=True ，那么也是不用判断 q ，直接将整个式子判定为 true

为什么不要重载？因为无法模拟出来短路特性

### 重载的一些建议
![[Pasted image 20230301081350.png]]
- 左移、右移写成全局函数
- 其他符号写成成员函数
- 不要重载 && 和 ||

### 字符串封装
1         强化训练-字符串类封装
1.1        myString类 实现自定义的字符串类
1.2        属性
1.2.1    char * pString; 维护 在堆区真实开辟的字符数组
1.2.2    int m_Size; 字符串长度
1.3        行为
1.3.1    有参构造 MyString(char * str)
1.3.2    拷贝构造 MyString(const MyString & str);
1.3.3    析构  ~MyString();
1.4        重载<< 运算符
1.5        重载 >> 运算符
1.6        重载 =   赋值运算
1.7        重载 []  str[0]  按照索引位置设置获取字符
1.8        重载 +  字符串拼接
1.9        重载 == 对比字符串


### 类的继承
继承的好处：减少重复的代码，提高代码的复用
```C++
class BasePage{
...
}

// 继承
class son1:public BasePage
{
...
}

```
语法格式
```C++
class 子类:继承方式 父类
{

}
```
子类，也叫做派生类
父类，也叫做基类

有不同的继承方式

==注意：这里是在类后面写了冒号，代表继承==
==但是，在构造函数后面也可以写冒号，初始化列表，可以达到快速赋值的目的==

### 继承的方式
![[Pasted image 20230301125730.png]]
继承方式：public、protected、private
就是继承以后的是什么权限
1.1        公共继承
	1.1.1    父类中公共权限，子类中变为公共权限
	1.1.2    父类中保护权限，子类中变为保护权限
	1.1.3    父类中私有权限，子类访问不到
1.2        保护继承
	1.2.1    父类中公共权限，子类中变为保护权限
	1.2.2    父类中保护权限，子类中变为保护权限
	1.2.3    父类中私有权限，子类访问不到
1.3        私有继承
	1.3.1    父类中公共权限，子类中变为私有权限
	1.3.2    父类中保护权限，子类中变为私有权限
	1.3.3    父类中私有权限，子类访问不到

这里sizeof(son)的结果是16，说明了所有的属性都被继承下去了，只不过访问不到
如何跳转盘符，直接 E:
![[Pasted image 20230301132416.png]]

### 继承中的构造与析构
子类继承父类，仍然会调用父类的构造函数和析构函数
并且有调用的顺序，这和入栈出栈类似
![[Pasted image 20230301133245.png]]

假如又有下面这一段代码：
```C++
class father{
public:
	father(){
		cout<<“father构造函数调用”<<endl;
	}
	~father(){
		cout<<"father析构函数调用"<<endl;
	}
}

class son:public father{
public:
	son(){
		cout<<“son构造函数调用”<<endl;
	}
	~son(){
		cout<<"son析构函数调用"<<endl;
	}
	Other oa;
}
class Oterh{
	other(){
		cout<<“other构造函数调用”<<endl;
	}
	~other(){
		cout<<"other析构函数调用"<<endl;
	}
	Other o
}
```
那有一个问题，就是像之前提到的，一般会先调用类下面的类创建的对象。然后也会先继承father调用father的构造函数。那么哪一个是在前面呢？
	结论：先有父类构造->再来调用其他成员的构造->自身构造
![[Pasted image 20230301134006.png]]

还可以有这样的代码，就是使用列表初始化的操作
在子类有参构造的同时，调用了父类的有参构造
![[Pasted image 20230301160519.png]] ^f49bec

不仅如此，还可以给使用默认参数
像这样的语法 son(int a=100):father(a)，在qt中非常常见
要知道这个是怎么调用的
![[Pasted image 20230301160644.png]]

除此之外，子类是不会继承父类的：构造函数、operator=函数

### 继承中同名成员的处理

假如父类和子类都有构造函数对同名成员进行初始化：就近原则！！！
但是只是同名成员，但实际上在内存中是两份变量
下面这个得到的m_A的结果是20
![[Pasted image 20230301162228.png]]

可以通过作用域访问父类中的成员
```C++
cout<<s1.Base::m_A<<endl;
```

当然假如是同名的函数，也可以通过作用域来访问
```C++
s1.Base::func();
```

假如在父类中出现了重载
```C++
class Base{
public:
	void func(){
		cout<<"base中func的调用"<<enld;
	}
	void func(int a){
		cout<<a<<endl;
	}
}
class son:public Base{
	void func(){
		cout<<"son中func的调用"<<endl;
	}
}
int main(){
	son s1;
	s1.func(10);
}
```
其中这个`s1.func(10)`会报错
当子类重新定义了父类中的同名成员函数，子类的成员函数会 隐藏掉父类中的所有重载版本的成员函数，可以利用作用域来访问父类中的重载成员函数

### 继承中的同名静态成员的处理
回想一下静态成员的性质：
1. 所有成员共享数据
2. 编译阶段分配内存
3. 类内声明，类外初始化
4. 可以通过类名来访问

假如出现了下面的代码
```C++
class base{
public:
	static int m_A;
}
int base::m_A=10;

class son:public base
{
public:
	static int m_A;
}
int son::m_A=20;

int main(void){
	son s;
	cout<<s.m_A<<endl;
	cout<<s.base::m_A<<endl;
}
```
静态成员的两种访问方式：
1. 对象访问（代码如上）
2. 类名访问:
- 访问son中的static int m_A当然是轻而易举的
```C++
cout<<son::m_A<<endl;
```
- 问题是怎么访问father中的static int m_A呢？
1. 当然也可以使用直接作用域来找
```C++
cout<<base::m_A<<endl;
```
2. 当然可以通过子类去访问
```C++
cout<<son::base::m_A<<endl;
```
- 前面这个son::表示的意思是：我要用类名的方式去访问成员
- 后面这个base::就表示：作用域

### 多继承
语法：
```C++
class son:public base1,public base2{

}
```

### 菱形继承与虚继承
如图：
![[Pasted image 20230301170200.png]]
![[Pasted image 20230301170221.png]]
1. 第一个问题只要写作用域就行了
2. 但是第二个问题主要是需要我们去解决的

构建出来了菱形继承
![[Pasted image 20230301170502.png]]
![[Pasted image 20230301170749.png]]
这个时候已经出现了二义性问题
查看一下类的内存结构图
![[Pasted image 20230301170814.png]]
这个既浪费了资源，又出现了二义性

如何解决这样的问题？虚继承！！！
![[Pasted image 20230301171012.png]]

这个时候cout一下代码
```C++
cout<<st.m_Age<<endl;
cout<<st.Sheep::m_Age<<endl;
cout<<st.Tuo::m_Age<<endl;
```
这个时候的输出是同一个值。也就是，打开了这个虚继承以后，这些数据都只有一份，这一份从哪来？显然：虚基类Animal

这个时候内存结构就已经发生了变化
![[Pasted image 20230301171407.png]]
实际上，我们可以看到，这个时候已经继承了一个东西，这个东西叫做vbtpr
- v: virtual
- b: base
- ptr: pointer
- vbprt: 虚基类指针 -> vbtable
- table: 表
这个虚基类指针指向
大概原理就是：sheep 和 tuo 继承的都是animal的一个指针，所以数据实际上也就只有一份，然后sheep和tuo的指针都有一个自己的表，vbptr指的是一块表，这个表都有唯一的偏移量，也就能找到自己的数据，这个时候访问数据的时候，不管是通过sheep、tuo、sheepTuo访问，都是找到的是唯一一份数据

### 虚继承的内部工作原理
这张图比较有意思，我试一下lldb有没有这个
![[Pasted image 20230301173315.png]]

上面这些虚继承，似懂非懂，有点意思。
我没有调用出来他这个函数


