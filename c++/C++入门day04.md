### 静态成员变量
##### 1.所有对象都共享同一份数据
##### 2.编译阶段就分配内存
##### 3.类内声明、类外访问
就是说，类内要有`static int a;`然后在类的外面进行初始化数据。为什么？在类内初始化数据的时候，只有在创建对象以后才能初始化，甚至有的在创建完对象以后还不会初始化。比方说初始化这个静态成员变量可能有用一些接口，但是我们始终没有调用这个接口，那这个静态成员变量始终都没有初始化。那假如是在构造函数中初始化呢？那么创建一个对象就会初始化一次，数据不安全。在类外初始化只用`int myClass::a=100;`就可以了。
##### 4.访问方式有两种：通过对象访问、通过类型访问
##### 5.静态成员变量也是有权限的

### 静态成员函数
##### 所有对象都共享同一个func函数
##### 静态成员函数 不能 访问非静态成员变量
为什么？想一下，因为只有一份func，因为用的是内存中同一个func，无法区分不同对象之前的非静态成员变量，
![[Pasted image 20230226233434.png]]
##### 静态成员函数也是有访问权限的
##### 静态的成员函数也是有两种访问方式：对象、类名

### 单例模式
所谓单例模式，也就是一个对象只能有一个实例（对象），这个就是单例。并且这个对象是共享的。举个例子，比方说：中国只有一个国家主席。（可能不太恰当，但这是事实）

### 主席类
- 一般来说，构造函数都是写在public下的，但是假如写在了私有private下就无法创建对象，要的就是这样的效果。虽然不能创建多个对象，但不至于一个也不能创建，因此要提供类似于接口的东西
```C++
class ChairMan{
private:
	// 将构造函数私有化，不可以创建多个对象
	ChairMan(){};  // 这个一定要加花括号，要不然就会当成是函数的声明
public:
	// 主席是共享的，大家口中的主席都是同一个
	// 拿到一个主席的话，其实只要拿到一个主席的指针就好了
	// sigleMan是一个指针，他的类型是ChairMan
	static ChairMan * sigleMan;  
}
// 类内声明，类外初始化
// 注意：这个new返回来的是sizeof(ChairMan)大小的指针
// 那为什么还能创建一个对象呢？因为ChairMan就相当于是在类内写的
ChairMan * ChairMan::sigleMan = new ChairMan

int main(void){
	// 那么该如何访问这个静态成员变量呢？通过对象是不行的，因为无法创建对象，但是通过类可以
	ChairMan* c1 = ChairMan::sigleMan;  // 通过类名来访问
	ChairMan* c2 = ChairMan::sigleMan;
	if (c1==c2){
		cout<< "c1=c2" <<endl;
	}
	else{
		cout<< "c1!=c2" <<endl;
	}
	// 但是这段代码是有问题的甚至可以进行 ChairMan::sigleMan = NULL; 的操作
}
```
解决这个安全隐患的方法如下：
设置将`static ChairMan* sigleMan`私有化，然后对外提供一个只读的结构即可
```C++
class ChairMan{
public:
	// 这个还是要static，要保证类型相同
	static ChairMan* getInstance(){
		return sigleMan;
	}

private:
	// 将构造函数私有化，不可以创建多个对象
	ChairMan(){};  // 这个一定要加花括号，要不然就会当成是函数的声明
//public:
private:
	// 主席是共享的，大家口中的主席都是同一个
	// 拿到一个主席的话，其实只要拿到一个主席的指针就好了
	// sigleMan是一个指针，他的类型是ChairMan
	static ChairMan * sigleMan;  
}
// 类内声明，类外初始化
// 注意：这个new返回来的是sizeof(ChairMan)大小的指针
// 那为什么还能创建一个对象呢？因为ChairMan就相当于是在类内写的
ChairMan * ChairMan::sigleMan = new ChairMan

int main(void){
	ChairMan* c1 = Chair::getInstance();
	// 这样看似是没有问题的，但看一下下面这一段代码
	ChairMan * c3 = new ChairMan(*c1);
	// 上面这段代码的意思就是：将c1解引用，得到一个ChairMan的实体（而不是一个指针）。
	// 通过拷贝构造函数，在堆区上开辟一个c1解引用出来的实体的拷贝这样。然后将地址传给c3
	if (c1==c3){
		cout<< "c1=c3" <<endl;
	}
	else{
		cout<< "c1!=c3" <<endl;
	}
}
```
为了防止这样的出现拷贝构造函数，直接把拷贝构造函数屏蔽掉，即：
```C++
class ChairMan{
public:
	// 这个还是要static，要保证类型相同
	static ChairMan* getInstance(){
		return sigleMan;
	}

private:
	// 将构造函数私有化，不可以创建多个对象
	ChairMan(){};  // 这个一定要加花括号，要不然就会当成是函数的声明
	ChariMan(const ChairMan &){};  // 这个就直接是一个占位参数，我们也暂时用不到这个值
//public:
private:
	// 主席是共享的，大家口中的主席都是同一个
	// 拿到一个主席的话，其实只要拿到一个主席的指针就好了
	// sigleMan是一个指针，他的类型是ChairMan
	static ChairMan * sigleMan;  
}
// 类内声明，类外初始化
// 注意：这个new返回来的是sizeof(ChairMan)大小的指针
// 那为什么还能创建一个对象呢？因为ChairMan就相当于是在类内写的
ChairMan * ChairMan::sigleMan = new ChairMan

int main(void){
	ChairMan* c1 = Chair::getInstance();
	// 这样看似是没有问题的，但看一下下面这一段代码
	ChairMan * c3 = new ChairMan(*c1);
	// 上面这段代码的意思就是：将c1解引用，得到一个ChairMan的实体（而不是一个指针）。
	// 通过拷贝构造函数，在堆区上开辟一个c1解引用出来的实体的拷贝这样。然后将地址传给c3
	if (c1==c3){
		cout<< "c1=c3" <<endl;
	}
	else{
		cout<< "c1!=c3" <<endl;
	}
}
```

### 打印机
```C++
class Printer{
public:
	static Printer* getInstance(){
		return printer;
	}
private:
	Printer(){cout<<"打印机的构造调用"<<endl;};
	Printer(const Printer& ){};
	static Printer* printer;
}
Printer* Printer::printer=new Printer;
int main(void){
	cout<<"main函数调用"<<endl;
}
```
上面这段代码的输出结果是：
```Shell
打印机的构造调用
mina函数的调用
```
因为 静态成员函数是编译阶段就分配内存，所以编译阶段就会跑printer的构造函数的代码。

### 类与内存分配
##### 空类开辟的对象，大小是一个字节sizeof(Person)=1
##### 类中变量和函数是分开存储的
```C++
class Person{
	int m_A;
	void func(){}; // 成员函数 不属于对象身上
	static int m_B;  // 静态成员变量 不属于对象身上
	static void func2(){};  //静态成员函数 不属于对象身上
}
```
实际上，经过sizeof(Person)，这个结果始终是4。只有非静态的成员变量属于对象上，因为只有非静态成员变量是有很多分的，成员函数、静态成员变量、静态成员函数都是只有一份的

### this指针
回想一下，就是 非静态成员函数 和 静态成员函数 都是在内存中只有一份的，那为什么非静态成员函数中可以使用成员变量，而静态函数中不能使用成员变量（只能使用静态成员变量）？就是为什么非静态成员函数能区分出不同的对象呢？因为实际上，在使用对象成员函数的时候，传参传了一个this指针。

有一个东西
```C++
class Person{
public:
	Person(int age){
		age = age;
	}
	int age;
}

int main(void){	
	Person p1(18);
}
```
这样不能把p1.age赋值成18，因为这里实际上，把
```C++
Person(int age){
	age = age;
}
```
这里的三个age看成是同一个age，所以在基本上在所有的成员变量之前写上m_age，是一个好的编码习惯：m_,members。那怎么办呢？改成下面的代码
```C++
Person(int age){
	this->age = age;
}
```
this指针指向：被调用的成员函数所属的对象
##### 用途1：解决名称冲突
##### this指针，隐式加在每个成员函数中
##### 链式编程的思想
比方说cout
```C++
cout<< "1" << "2" << "3" <<endl;
```
像上面这一串,`cout<<"1"`又返回了一个cout，设为是cout1，cout1<<"2"<<"3"<<endl;像这样就进行连续的操作，就是链式变成。
我们现在想要设计一个函数，他能够对年龄进行连续相加，并且赋值
```C++
Class Person{
public:
	Person(int age){
		this->age = age;
	}
	int age;
	void PersonAddPerson(Person &p){
		
	}
}
int main(void){
	Person p1 = Person(10);
	Person p2 = Person(10);
	p1.PersonAddPerson(p2).PersonAddPerson(p2).PersonAddPerson(p2);
}
```
比方说像这样，我希望最后输出的结果是p1.age=40
那怎么处理？首先我们是希望能够返回出来的还是一个对象，并且这个对象还是p1。所以返回值不能是一个对象，假如返回值是一个对象，那么就相当于创建了一个临时变量。返回指针？有些接近，但是指针的话，中间的符号就不能是`.`，而是`->`，返回对象的引用! `Person& PersonAddPerson(Person&)` ！然后返回的是 p1 本身，就要用到this指针，但是假如直接 `retrun this` 肯定是不行的，因为返回值要求的是对象的引用，而不是一个内存地址（指针），那就讲this解引用`return *this`就行了

### 空指针访问成员函数
比方说有下面的代码：
```C++
class Perons{
public:
	void showClass(){
		cout<<"Class Name is Person"<<endl;
	}
	void showAge(){
		m_age = 0;
		cout<<m_age<<endl;
	}
	int m_age;
}
int main(void){
	Person* p=NULL;
	p->showClass();
	p->showAge();
}
```
如果运行这段代码就会崩溃。发现showClass()是可以运行的，但是showAge()是会报错的。问题就出在这个空指针这里。这段代码实际上隐式的有this
```C++
class Perons{
public:
	void showClass(){
		cout<<"Class Name is Person"<<endl;
	}
	void showAge(Person* this){
		this->m_age = 0;
		cout<<this->m_age<<endl;
	}
	int m_age;
}
```
而这里穿进去的是空指针，相当于有`NULL->m_age`的操作，这当然是不允许的。至于为什么这个showClass()没有报错，是因为没有给它传this指针。为什么不会在下面`int m_age`中写成`int this->m_age`呢？一个是没有这样的语法，还有一个是，类的非静态成员变量和成员函数之间是封开存储的。
如何提高代码的健壮性?
```C++
class Perons{
public:
	void showClass(){
		cout<<"Class Name is Person"<<endl;
	}
	void showAge(Person* this){
		if (this==NULL){ return; }
		this->m_age = 0;
		cout<<this->m_age<<endl;
	}
	int m_age;
}
```
如果成员函数中用到了this，那么这个this需要加判断，防止代码崩溃

### 常函数和常对象
这里回顾一个东西：const int * p 和 int* const p 的区别
- const int* p 表示p的指向可以发生改变，但是\*p的内容不能发生改变
- int* const p 表示p的指向不能发生改变，但是\*p的内容可以发生改变
- const int* const this 就可以确保：p指向不会发生改变，\*p的内容也不会改变
this指针的本质：myClass* const this
```C++
class Person{
public:
	void showPerson(){
		cout<<"person age="<<endl;
	}
private:
	int m_age;
}
```
比方说像这样一段代码，我们想要让showPerson()真的就只能是只读的。但是很显然，这个除了只读，还可以修改里面的值，比方说`m_age = 100;`这样的操作，为了防止这样的操作，就引入了常函数，可以做到类似于`const Person* const this`这样的效果。就需要再函数后面加上`void showPerson() const {.  }` 但是假如我想要在常函数中有一个特殊的成员变量想要改掉，就需要在定义声明的时候加上`mutable int m_A`即可
- void func() const {}
- mutable int m_a;
除了有常函数(在类里面的），还有常对象（通过类来创建的对象）。创建常对象`const Person p1(10)` 这样就不能修改其成员变量，不能有这种操作`p1.m_age=100;`。但是对于其成员属性中有`mutable`修饰的话，这样的操作是允许的`p1.m_a=100;` 
==注意：常对象只能调用常函数==，假如能够调用普通函数的话，那假如有这样一个函数`void func(){m_age=100;}`能够修改里面的变量，那这个常对象也没意义了。
==注意：==但是实际上，还是可以使用指针将他的成员变量改掉。毕竟都是分配在内存上的。其中`mutalbe`可能就是使用了类似于指针的方式间接处理的

### 友元
类的其中一个特点就是能够隐藏信息：有些信息是不能在类的外部访问到的。但是有时候又需要再类的外面访问私有成员怎么办？比方说，一般的朋友来我家做客，一般不会让他们进入卧室，但是好闺蜜可能得就可以。引入了==友元==！（另外提一句，私有成员依然可以使用指针来访问）
友元只是一个概念，友元的实现可以是：全局函数、某个类中的函数、甚至整个类
##### 全局函数做友元函数
```C++
#include <iostream>
#include <string>
using namespace std;
class Building {
   public:
    Building() {
        this->m_SittingRoom = "客厅";
        this->m_BedRoom = "卧室";
    }

   public:
    string m_SittingRoom;

   private:
    string m_BedRoom;
};

// 好基友全局函数,可以访问Building的私有属性
void goodGay(Building* Building) {
    cout << "好基友正在访问:" << Building->m_SittingRoom << endl;
}
void test01() {
    Building building;
    goodGay(&building);
}

int main(void) {
    test01();
    return 0;
}
```
假如想要好基友访问私有权限的内容，只需要在类的最上方写`friend void goodGay()`。这是一个特殊的声明。

##### 类作为友元类
先插播一个其他的知识，函数在类外实现
```C++
#include <iostream>
#include <string>
using namespace std;
class Building;  // 告诉编译器有这个类,下面在定义

class GoodGay {
   public:
    GoodGay();  // 这里只是做一个构造函数的声明,以后再实现
    void visit();
    Building* m_building;
};

class Building {
    friend class GoodGay;

   public:
    Building();

   public:
    string m_SittingRoom;

   private:
    string m_BedRoom;
};

// 做一个类外的实现
Building::Building() {
    this->m_BedRoom = "卧室";
    this->m_SittingRoom = "客厅";
}

GoodGay::GoodGay() { this->m_building = new Building; }

void GoodGay::visit() {
    cout << "好基友正在访问" << this->m_building->m_SittingRoom << endl;
    cout << "好基友正在访问" << this->m_building->m_BedRoom << endl;
}
```
然后这里友类和友函数是类似定义的

友类是没有 传递、对称的，假如想要传递和对称，必须定义

##### 类中的函数作为友元
```C++
friend void GoodGay::visit();
```














