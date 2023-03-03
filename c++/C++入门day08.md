### 泛型编程的概念
首先，C++有三种编程思想：
1. 面向过程（C语言）
2. 面向对象
3. 泛型编程
这里先介绍泛型编程中的模板技术
![[Pasted image 20230303144449.png]]
有两种模板：
1. 类模板
2. 函数模板

##### 什么是模板？
比方说这种就是模板：
![[Pasted image 20230303144705.png]]
- 只要把我的脸给贴上去，就可以算是我的一张照片
- 但是：模板不能直接使用。就比方说，我给人事部说这个就是我的照片

先想一下，之前我们是怎样用一个函数来完成两个变量的交换的？
```C++
void mySwapInt(int& a,int& b){
	int temp=a;
	a=b;
	b=temp;
}
```
当然其实还可以使用只针对版本。
但是这样会有一个问题，就是不够通用。比方说，我这里是想要交换double的值呢？
你会发现这个代码非常的像，但是就是要存在int和double——两种版本！
想一下？假如我能够把这个类型，当成一个参数传进去，那我是不是就可以只要有一套代码就行了。

### 利用模板实现通用交换函数
```dictionary
template
template | BrE ˈtɛmpleɪt,ˈtɛmplɪt, AmE ˈtɛmplət | noun
① (pattern, outline) 型板 xíngbǎn 
② figurative (model) 样板 yàngbǎn
③ Computing 模板 múbǎn
```
像上面要交换两个数，那么就可以有下面这一段代码
```C++
// T代表一个通用的数据类型
// 告诉编译器如果下面紧跟着的函数或者类中出现T不要报错
template<typename T>
void mySwap(T& a,T& b){
	T temp=a;
	a=b;
	b=temp;
}
```
- 约定俗成：T 是自动类型
下面两种使用方式：
1. 自动类型推导
	- 必须推导出一致的T数据类型，才可以正常的使用模板
2. 显示指定数据类型
- 在上面代码的基础上，使用下面的方式调用
```C++
int a,b;
mySwap<int>(a,b);
```
但是，假如是下面的这段代码
```C++
int a;
char b='c';
mySwap<int>(a,b);
```
这样肯定是会报错的。但是报错的原因不是这里指定的是int，而是引用出的问题。假如swap是下面这一段代码，是不会有任何报错的（把引用去掉）
```C++
template<typename T>
void mySwap(T a,T b){
	T temp=a;
	a=b;
	b=temp;
}
```
因为实际上是不能出现这样一段代码：
```C++
int b = 'c';
int& p = b;
```
- char可以转换为int类型，但是char不能成为int类型的一个引用
- 引用必须要类型一致才能取别名

##### 注意：
1. template必须是==紧跟着==模板函数或者是模板类
2. 模板不能单独使用，必须指定出T才可以使用
	如果出现下面这样的代码也会报错
```C++
template<typename T>
void mySwap(){
	T a;
	...
}
int main(void){
	swap();
}
```
因为编译器是不知道T到底是什么类型的，所以也不知道应该给里面的变量分配多少内存（这里应该说是自动推导数据类型失效的一种情况）
但是我可以指定T是什么类型。这样就不会报错
```C++
template<typename T>
void mySwap(){
	T a;
	...
}
int main(void){
	swap<int>();
}
```
其实*自动类型推导*和*显示指定类型*都是告诉编译器T到底是什么。
这种编程思想：类型参数化（这里T就是一个参数）
这里可以是 `template<typename T>` 也可以是 `template<class T>`

### 案例：通过通用的数据类型
回忆一下三目运算符
1. 作为右值，和C的效果一样
2. 作为左值，放回的是变量的引用。

### 函数模板和普通函数的区别
1. 如果使用自动类型推导，是不能使用隐式类型转化的（会有二义性）
```C++
void mySwap(T a,T b){
	T temp=a;
	a=b;
	b=temp;
}
int main(void){
	char b='c';
	int a=10;
	mySwap(a,b);
}
```
这个时候，编译器是不知道给我们int还是char，会报错。但是可以显示指定T

2. 函数模板和普通函数的调用规则
发生重载时。如果普通函数可以调用，优先使用普通函数
- 如果普通函数只有声明，没有实现。这个时候还是走普通函数，这个时候就会报错，报错内容：一个无法解析的外部命令！
```C++
template<class T>
void myPrint(T a,T b){
	cout<<"函数模板调用"<<endl;
}

void myPrint(int a,int b){
	cout<<"普通函数调用"<<endl;
}

```
如果想要优先使用函数模板，可以使用空参数列表（这个时候用的还是自动类型推导）
```C++
template<class T>
void myPrint(T a,T b){
	cout<<"函数模板调用"<<endl;
}

void myPrint(int a,int b){
	cout<<"普通函数调用"<<endl;
}

int main(void){
	myPrint<>(1,2);
}
```

3. 函数模板也可以发生重载

4. 如果函数模板能产生更好的匹配，优先使用函数模板
```C++
template<class T>
void myPrint(T a,T b){
	cout<<"函数模板调用"<<endl;
}

void myPrint(int a,int b){
	cout<<"普通函数调用"<<endl;
}

int main(void){
	myPrint('a','b')
}
```
比方说，这里普通函数可以调用（发生隐式类型转换），模板函数也可以调用。但是这里会调用函数模板，因为能产生更好的匹配。

### 函数模板的实现机制
- 编译器并不是把函数模板处理成能够处理仍和类型的函数。（只对一些内置的数据类型适用）
- 函数模板通过具体的类型产生不同的函数（实际上是将 T 替换过后，产生了一个新的函数）（也就是帮我们创建了一个新的函数）
	1. 函数模板，就是上面的东西
	2. 模板函数：由函数模板产生的函数。
- 编译器会对函数模板进行两次编译
	1. 检测一下写的模板代码有没有错误
	2. 生成了模板函数以后，又进行了一次编译

### 模板的局限性（具体化！）
就是对一些特殊的数据类型（字符串常量），或者一些自定义的数据类型（结构体、类）不适用
通用化是有意义的，那么我该怎么做？（模板具体化）
例如：显示两个变量 对比 函数
```C++
class person{
public:
	person(int age, string name){
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age
}

template<class T>
bool myCompare(T& a,T& b){return a==b;}

int main(void){
	person p1 = {"tom", 18};
	person p2 = {"jerry", 20};
	bool ret = myCompare(p1,p2)
}
```
像上面这段代码就会报错。怎么解决？实际上只要重载运算符是可以解决的。
这里提供另一种方法：利用==具体化==的技术，实现对自定义数据类型提供特殊模板
```C++
template<> bool myCompare(Person& a, Person& b){
	return a.m_Name==b.m_Name && b.m_Age == b.m_Age;
}
```

### 类模板
- 类模板会在STL中非常常见
```C++
template<class NAMETYPE, class AGETYPE>
class person{
public:
	person(NAMETYPE name, AGETYPE age){
		this->m_Name = name;
		this->m_Age = age;
	}
	NAMETYPE m_Name;
	AGETYPE m_Age;
}
```
这里是使用了两个typename/class，其实在函数模板中也可以这么用
ok，下面我们来看一下类模板和函数模板的区别
1. 自动类型模板？类模板禁用《自动类型模板》
```C++
person p1("孙悟空", 999);
```
但是，实际上，上面这一段代码会报错的，这个报错是非常诡异的：缺少类模板person的参数列表！但是这里显然是没有缺少类模板的参数列表的。就是比方说，这里的 "孙悟空"，我们可能想要的是string类型的，但是说不定他自己推导的是char\*类型的。也就是说有二义性。所以，类模板禁用《自动类型模板》！

2. 显示指定类型
```C++
Person<string,int> p1("孙悟空", 999);
```
在类模板中可以有默认参数（函数模板中不可以有默认参数，因为会有二义性）
```C++
template <class NAMETYPE, class AGETYPE=int>
```

### 类模板中成员函数的创建时机——运行时期
下面我们来创建一个很魔幻的代码：
```C++
class person1{
public:
	void showPerson1(){
		cout<<"person1 show调用"<<endl;
	}
};

public:
	void showPerson2(){
		cout<<"person2 show调用"<<endl;
	}
};

template<class T>
class MyClass{
	void func1(){
		obj.showPerson1();
	}
	void fun2(){
		obj.showPerson2();
	}

	T obj;
}

```
到此为止。
1. 上面这段代码能不能生成？至少是能编译通过，并且编译器没有报错。
2. 编译器能不能创建出来 func1 和 func2 这两个函数？不能，即便编译器看得到这两个函数的实现，但由于不知道T的数据类型，所以是不能创建这两个函数的。那什么时候可以创建这两个函数？运行阶段！但是，有可能创建成功，有可能创建失败。
但是，假如还有下面的代码
```C++
int main(void){
	MyClass<person1> p1;
	// 上面这一行能正常执行
	p1.func1();
	// 但是下面这一样不能正常执行
	p1.func2();
}
```
##### 但是，我还是有点没懂，就是这个函数是什么时候创建的，这个意思我没有概念

### 类模板做函数的参数
先有上面这个代码
```C++
template<class T1,class T2 =i nt>
class person{
public:
	Person(T1 name, T2 age){
		this->m_Name = name;
		this->m_Age = age;
	}

	void showPerson(){
		cout<<"姓名： "<<this->m_Name<<"年龄： "<<this->m_Age<<endl;
	}
	T1 m_Name;
	T2 m_Age;
}
```
怎么传？（下面这几个，抽象的等级逐步提高，但也更加自由）
1. 指定传入类型
- 突然想反问一下：这里为什么要使用引用传递？
	- 地址传递：麻烦，还有解引用之类的
	- 值传递：相当于在栈上又创建出来了一个对象，增大开销
- 这里就是只用传进去对象就行了。其他的东西都写死了
```C++
void doWork01(person<string,int>&p){
	p.showPerson();
}

void test01(){
	Person<string,int>p("孙悟空", 999);
	doWork(p);
}

```

2. 参数模板化
- 上面是将类型参数直接写死了。这里要更加的灵活一些，那就将类型参数模板化！
- 将原本的string和int替换为T1、T2。但是这里T1、T2有吗？没有！template!
```C++
template<class T1,class T2>
void doWork02(person<T1, T2>&p){
	p.showPerson();
}

void test02(){
	Person<string,int>p("孙悟空", 999);
	doWork(p);
	doWork<string, int>(p);
}

```
- 实际上函数模板的使用
- 这里实际上使用的是自动推导
![[Pasted image 20230303183803.png]]

3. 整个类模板化
- 上面是将person中的两个属性（成员变量的类型）进行了模板化。这里直接将整个类都进行模板化
- 这里是将 Person<string,int>变成了模板
```C++
template<class T>
void doWork03(T &p){
	p.showPerson();
}

void test03(){
	Person<string,int>p("孙悟空", 999);
	doWork(p);
	doWork<person>(p);
}

```

##### 参数模板化，整个类模板化：
这两种模板化都是：函数模板+类模板的结合。并且都用到了自动推导！

对类模板和函数模板的一些思考：
- 实际上都是将数据类型参数化
- 函数模板：将函数的 形参 的数据类型变成一个参数
- 类模板：将 成员变量 的数据类型变成一个参数
- 查看数据类型：`template<class T1>`中的数据类型：`typeid(T1).name()`

### 类模板碰到继承问题机器解决
比方说有下列代码
```C++
template<class T>
class Base{
public:
	T m_A;
}

class Son:public Base{

};
```
这个时候会报错
因为在继承的时候，编译器不知道要给son分配多少内存空间。
解决方法，在继承后面
`class Son:public Base<int>`
必须要指定出来父类的数据类型，才能继承
但是这样有问题，就是这里内存是写死了的。降低了用户在使用过程中的拓展性。
解决方案：
```C++
template<class T>
class Base{
public:
	T m_A;
}

// 这里的T1给son使用，这里T2给base使用
template<class T1, class T2>
class son:public base<T2>{
	T1 m_B;
}

void test(){
	son<int,double> s1;
}
```
突然好像又有点明白了，就是我假如要使用类模板，然后这个类模板有n个参数，在创建变量的时候就要传递n个参数（除非有默认参数）

### 类模板中成员函数的类外实现
```C++
template<class T1, class T2>{
public:
    Person(T1 name, T2 age){
        this->m_Name = name;
        this->m_Age = age;
    }

    void showPerson(){
        cout<<"姓名:"<<this->m_Name<<"年龄:"<<this->m_Age<<endl;
    }

    T1 m_Name;
    T2 m_Age;
}

```
如果要将构造函数Person(T1 name, T2 age)类外实现呢？
```C++
template<class T1, class T2>
person::person(T1 name, T2 age){
	...
}
```
但是这个时候还没有完，还要加上
```C++
template<class T1, class T2>
person<T1, T2>::person(T1 name, T2 age){
	...
}
```

### 类模板的分文件编写问题以及解决
1. 在头文件中只声明，不实现
2. 在.c文件中，包含头文件，并且按照 类外实现 来写实现
3. 在main.cpp中包含头文件
```C++
void test(){
	Person<string, int>P("Jerry",20);
	P.showPerson();
}
```
结果，这个时候报错：一个无法解析的外部命令！（链接阶段的问题）
并且这个错误非常的神奇之处：在包含头文件中，不包含.h，包含.cpp却生成成功
原因：假如包含.h，那么访问 xxx.h 的时候，只能看得到声明，并且这个声明并没有指定类型，编译器也不知道是什么，并且，类模板中的成员函数并不是一开始就创建的。。。（这里说不清楚，反正主要的原因就是：类模板中的成员函数不是一开始就创建的）（看视频！）
那怎么解决？直接包含.cpp肯定是不好的。那怎么解决？

##### 类模板不建议使用分文件编写！
所以一般都写在同一个文件里，并且是写在头文件中。但是头文件怎么又有声明，又有实现呢？将文件的后缀名改为.hpp即可！

### 三步走：
1. 类内实现
2. 类外实现
3. 分文件编写(不行)

### 全局友元函数类内实现
```C++
class Person{
public:
	// 友元函数类内实现
	friend void printPerson(){
		cout<<"友元函数调用！"<<endl;
	}
}
```
- 之前将友元函数是类外实现，当然，类外实现要添加作用域（实际上也就和类内实现差不多）
- 同样，友元函数也要考虑到私有、公共的问题

### 类模板碰到友元问题以及解决
```C++
template<class T1, class T2>
class Person{
public:
	// 友元函数类外实现
	friend void printPerson(Person<T1,T2> &p);
public:
	Person(T1 name, T2 name){
		this->m_Name = name;
		this->m_Age = age;
	}
private:
	T1 m_Name;
	T2 m_Age;
}

// 首先，声明和实现的那一条函数，一定是要相同的
// 因为有<T1，T2>，所以要跟上template
template<class T1, class T2>
void Person::printPerson(Person<T1,T2> &p){
	cout<<"友元函数调用！"<<endl;
	cout<<p.m_Name<<endl;
	cout<<p.m_Age<<endl;
}

void test01(){
	Person<string,int> p("tom",100);
	printPerson(p);
}
```
1. 首先，上面这段代码是无法运行的，错误原因：一个无法解析的外部命令！（说明是链接阶段出的问题）
想？这个时候printPerson是一个函数模板？还是一个普通函数？普通函数！
- 首先，看看这个T1,T2是谁给的？看声明那里！是类模板给的！
- 类模板里面的函数都是函数模板。但是友元函数实际上不属于类，毕竟在类外调用的时候并不需要加上类的作用域
2. 所以，这个函数本质上是一个普通函数（只看声明）
3. 但是在下面的实现是一个什么函数？函数模板！因为有，并且不得不加template<class T1,class T2>
4. 链接的时候，在找普通函数的时候，没有找到！
5. 报错：一个无法解析的外部命令！
6. 由（3），只能将这个普通函数改成一个模板函数！
7. 怎么改？在声明的时候写`friend void printPerson<>(Person<T1,T2> &p);`
8. 但是，这个时候继续报错！但是不报：一个无法解析的外部命令。
9. 说明不再是申明和实现的错误了
10. 既然这是一个函数模板，那就要让编译器看一下这个函数模板的声明。（原因可能是：模板在运行阶段才会生成：模板类、模板函数。但是，函数模板的声明是在类模板中，然而这个时候编译器才看到这个函数模板的声明，已经错过了编译器看声明的实际）
11. 在类模板前面添加：
```C++
template<class T1, class T2>
void printPerson(Person<T1,T2> &p);
```
（既然加上了这条声明了，那么在类模板中的那条就只是设置为友元了，注意，类模板里面的那条，依然要有`friend printPerson<>(Person<T1,T2> &p)`
12. 这个时候还是报错！因为在`class Person`的前面却已经有了`void printPerson(Person<T1,T2> &p)` ，就已经有了 Person 这个字，编译器不认识。所以只再声明这个`class Person`。
经过了上述的步骤，才能够正常运行，非常的繁琐。代码如下
```C++
// 先有Person存在，并告诉编译器这个是一个类模板
template<class T1, class T2>
class Person;

template<class T1, class T2>
void printPerson(Person<T1,T2> &p);

template<class T1, class T2>
class Person{
public:
	// 友元函数类外实现
	friend void printPerson<>(Person<T1,T2> &p);
public:
	Person(T1 name, T2 name){
		this->m_Name = name;
		this->m_Age = age;
	}
private:
	T1 m_Name;
	T2 m_Age;
}

template<class T1, class T2>
void Person::printPerson(Person<T1,T2> &p){
	cout<<"友元函数调用！"<<endl;
	cout<<p.m_Name<<endl;
	cout<<p.m_Age<<endl;
}

void test01(){
	Person<string,int> p("tom",100);
	printPerson(p);
}
```
经过上面的分析，我大概感觉有几个结论：
1. 模板类中的函数（成员函数除外）要先声明
2. 模板类中的函数，如果包含class T参数，那么就要声明称函数模板

### 还是三步走：
1. 友元类内实现
2. 友元类外实现（复杂）
	- 声明和实现分离
3. 友元类外实现（稍微简单一些）
	- 声明和实现一起实现

### 项目：MyArray的任意数据类型



> 笨人认为，一般来说，一个无法解析的外部命令！简单来说，就是编译器不认识









