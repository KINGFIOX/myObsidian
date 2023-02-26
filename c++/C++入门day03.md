### 案例：比较两个立方体
传入两个结构体（对象），假如是直接传值的话，内存拷贝的效率很低
这里推荐使用引用传递（这里是全局函数）
```C++
bool compareCube(cube &c1, cube &c2)
{
    return c1.getL()==c2.getL() && c1.getW()==c2.getW() && c1.getH()==c2.getH();  
}
```
这里是使用成员函数（好像python中用的是self.xxx）
```C++
class cube {
   private:
    int m_L;
    int m_W;
    int m_H;

   public:
    // 设置长/宽/高
    void setL(int l) { m_L = l; }
    void setW(int w) { m_W = w; }
    void setH(int h) { m_H = h; }
    int getL() { return m_L; }
    int getW() { return m_W; }
    int getH() { return m_H; }
    // 成员函数进行比较
    bool compareInClass(cube &c) {
        return m_L == c.getL() && m_W == c.getW() && m_H == c.getH();
    }
};

```

### 案例：点与圆的关系
在分文件编写的时候要注意命名空间，出现没有定义的情况一定要加命名空间
```C++
#include "circle.h"
void circle::setCenter(int x, int y) {
    center.setX(x);
    center.setY(y);
}
void circle::setR(int R) { r = R; }
point circle::getCenter() { return center; }
int circle::getR() { return r; }
void circle::whereByClass(point &m) {
    int rCircle2 = pow(r, 2);
    int dist2 =
        pow(center.getX() - m.getX(), 2) + pow(center.getY() - m.getY(), 2);
    if (rCircle2 == dist2) {
        cout << "在圆上" << endl;
    } else if (rCircle2 > dist2) {
        cout << "在圆内" << endl;
    } else if (rCircle2 < dist2) {
        cout << "在圆外" << endl;
    }
}

```

### 对象的构造和析构
提出背景，比防说我们在买一个手机的时候，他总是有一个出厂设置，而且对于中国人来说，买的手机应该初始化是英文的。然后我们在用了几年手机以后，我们假如要二手转出去，那么也要将这个手机恢复出厂设置，清除里面的数据，这个是安全问题。
基于以上情况，面向对象的变成思想也是这样的。创建对象的时候，这个对象有一个初始状态；对象销毁之前应该先销毁自己创建的一些数据。于是就有了：
- 构造函数：初始化
- 析构函数：销毁
上面这两个函数会被编译器自动调用，完成对象的初始化和清理的工作
无论你是否喜欢，对象的初始化和清理工作是编译器强制我们要做的事情，即使你不提供初始化操作和清理操作，编译器也会给你增加默认的操作，只是这个默认初始化操作不会做任何事（相当于是花括号里面的内容是空的），所以编写类就应该顺便提供初始化函数。
##### 那为什么是自动调用，而不是手动调用呢？
既然是必须操作，那么自动调用会更好，如果靠程序员自觉，那么就会存在遗漏初始化的情况出现。

只要创建一个对象就会使用到构造函数和析构函数，只不过系统提供的构造函数和析构函数是空实现，我们并不能看到什么效果
如果要写构造函数和析构函数，那么作用域必须是全局下面
- 对象被创建的时候调用构造函数
- 对象被释放的时候调用析构函数
```C++
// 构造函数
class Person{
	public:  // 构造和析构必须要声明在全局作用域
		// 构造函数
		// 没有返回值，不用写void
		// 函数名 与 类名 相同
		// 可以有参数，可以发生重载
		// 构造函数 由编译器自动调用一次 无须手动调用
		Person(){
			cout<<"Person的构造函数调用"<<endl;
		}
		// 析构函数
		// 没有返回值，不用写void
		// 函数名 与 类名 相同，函数名前加~
		// 不可以有参数，不可以发生重载
		// 析构函数 也是由编译器自动调用一次，无须手动调用
		~Person(){
			cout<<"Person的析构函数调用"<<endl;
		}
		
};
```
如果我们不写这两个函数，系统也会提供一个空实现，也就是里面什么代码也不执行

### 构造函数的分类
有两种方式分类
1. 按照参数
	- 有参
	- 无参（也叫做默认构造函数，因为系统默认的构造是空实现，也没有参数）
2. 按照类型
	- 普通构造函数（除了拷贝构造函数都是普通构造函数）
	- 拷贝构造函数（难点）
##### 拷贝构造函数
为什么下面的构造函数要使用引用传参？
这里要用const修饰引用，防止被误操作
假如不使用使用引用构造，假如是按值传参，那么实际上是在栈区里面再创建一个Person，那么就又有这样的拷贝构造函数，结果就进行了一个无限递归，所以肯定是不能这样的。可以通过引用传参，给外面的p取一个别名就行了。
所谓拷贝构造函数，就是在初始化传参的时候，传一个同一类的对象，然后可以调用传进去的这个对象的一些属性，来做一些事情，比方说像赋值
```C++
class Person{
	public:
		person(){
			cout<<"Person的默认构造函数"<<endl;
		}
		Person(int age){
			m_age=age;
			cout<<"Person的有参构造函数调用"<<endl;
		}
		// 拷贝构造函数
		Person(const Person &p){
			m_Age=p.m_age;
			cout<<"Person的拷贝构造函数调用"<<endl;
		}
	private:
		int m_age;
};

int main(void){
	// 因为有了带有参数的构造函数
	// 就可以在创建对象的时候初始化一些属性
	// 就可以在创建一些只读的对象的时候使用
	Person p1(18);  // 括号法调用构造函数
	Person p2(p1);
}
```

### 构造函数的调用
##### 1.括号法
```C++
// 无参构造
Person p;
// 有参构造
Person p(10);
Person p2(p1);
```
注意事项：
- 不要在无参构造后面加括号 `Person p();` 这样做，无异于 `void test();` 相当于在一个函数里面再对另一个函数进行了声明。

##### 2.显示法（感觉这种比较好）
```C++
Person p3 = Person(10);  // 有参构造
Person p4 = Person(p3);  // 拷贝构造
```
比方说像上面那一段代码，释放的话会调用两次析构函数，假如输出的内容是
> person的析构函数
> person的析构函数
- 像上面这两条输出，那哪一条是p3的？哪一条是p4的？（注意栈区）
- 还有一点，上面这种`Person(10);`是匿名对象，特点：当前执行完后立即释放
注意事项：
- 不要用拷贝构造函数初始化匿名对象，例如`Person(p3)`，编译器会认为`Person p3`对象实例化，那么就会导致`p3`重定义

##### 3.隐式法（不是很推荐使用，让人读不懂）
```C++
Person p5 = 10;  // Person p5 = Person(10);
Person p6 = p5;
```


### 调用到拷贝构造的三种方式
1. 用已经创建好的对象来初始化对象
```C++
Person p2(p1);  // 括号
person p2 = person(p1);  // 显示
Person p2 =p1;  // 隐式
```
2. 值传递的方式 给函数传值
- 这里会调用两次构造函数和析构函数
```C++
void doWork(Person p)
{
	
}
int main(void)
{
	Person p1(100);
	doWork(p1);
}
```
3. 以值的方式来返回局部对象
- 这里会调用两次构造函数和析构函数
- （不要返回局部变量的引用）
```C++
Person doWork()
{
	Person p;
	return p;
};
int main(void)
{
	Person p = doWork();
}
```
但是实际上，假如这里调高了优化等级，可能会出现：只调用了一次构造函数和析构变量，编译器会帮我们做这件事。[[GCC入门#^8bb7f8]]

### 编译器与构造函数

^e7aaa4

1. 编译器会给一个类至少添加3个函数
- 默认构造（空实现）
- 析构函数（空实现）
- 拷贝构造（值拷贝）
所以说，如果没有定义拷贝构造函数，还是可以进行这样的操作
```C++
Person p2=p1;
```
当然，这种拷贝方式是值拷贝

2. 如果我们自己提供了有参构造函数，编译器就不会提供默认构造函数，但依然会提供拷贝构造函数（值拷贝）
3. 如果我们自己提供了拷贝构造函数，那么编译器就不会提供其他的构造函数

### 深拷贝与浅拷贝问题以及解决
- 析构函数一个很好的应用，假如我们有一些属性是创建在堆区，那么就可以在释放对象的同时，释放掉堆区的资源
但是这样还是有一个问题，看下面这段代码
```C++
class Person{
	public:
		// 构造函数
		Person(char* name,int age){
			m_Name = (char*)malloc(strlen(name)+1);
			// strlen(name) 不包括结尾的 \0，要手动+1
			// 因为c++对类型要求很严格，要类型转换
			if (m_Name==NULL){ return; }
			strcpy(m_Name,name);
			m_Age=age;
		}
		// 析构函数
		~Person(){
			// 释放堆区
			if (m_Name!=NULL){
				free(m_Name);
				m_Name=NULL;
			}
		}

	// 对了，注意一下：这里是私有属性
	// 因为是默认私有属性的
	char* m_Name;  // 姓名
	int m_Age;  // 年龄
};

int main(void){
	Person p1(”德玛西亚“,18);
	Person p2(p1);  // 拷贝构造
}
```
但是这么写，这个程序会崩溃，崩溃原因：堆区内存的重复释放。这里只是一个简单的浅拷贝，p1.m_Name的秩只是一串内存地址（假设是0x01），然后这里通过系统默认的构造拷贝（浅拷贝），然后p2.m_Name也只是一串相同的地址（0x01）。当然这个对象是创建在栈区上的，然后先释放p2，再释放p1。这里0x01是由p2的析构函数进行了释放，然后p1再次释放的时候就出问题了。
![[Pasted image 20230226132121.png]]
##### 深拷贝解决这类问题（自己写一套拷贝构造函数）
```C++
class Person(const Person &p){
	m_Name=(char*)malloc(strlen(p.m_Name)+1);
	strcpy(m_Name,p.m_Name);
	m_Age=p.m_Age;
};
```
浅拷贝只是简单的拷贝地址，深拷贝是拷贝地址对应的内容

### 初始化列表（给成员属性赋初值）
```C++
class Person{
	// 当然这里不一定都要赋初值
	Person(int a,int b,int c):m_A(a),m_B(b),m_C(c){
		// 空实现
	}

private:
	int m_A;
	int m_B;
	int m_C;
};
```
这是一种==语法==
可以通过初始化列表的方法，给成员属性赋初值

### 类对象做类中成员
当其他类对象作为本类成员，先构造其他类对象，再构造自身，析构的顺序和构造相反
```C++
#include <string>
class Phone{
	Phone(string pN){
		m_phoneName=pN;
		cout<<"phone的有参构造调用"<<endl;
	}
	~Phone(){
		m_gameName=gN;
		cout<<"phone的析构函数调用"<<endl;
	}
	string m_phoneName;
};
class Game{
	Game(){
		cout<<"Game的有参构造调用"<<endl;
	}
	~Game(){
		cout<<"Game的析构函数调用"<<endl;
	}
	string m_gameName;
};
class Person{
	Person(string name,string pN,string gN):m_personName(name),m_phone(pN),m_game(gN){.    }
	string m_personName;
	Phone m_phone;
	Game m_game;
};
```
这个时候入栈顺序是 phone -> game -> person 就相当于是person有一堆零件phone和game，然后要先有零件，才能组装好

### explicit关键字
```dictionary
explicit | BrE ɪkˈsplɪsɪt,ɛkˈsplɪsɪt, AmE ɪkˈsplɪsət | adjective
① (precise) 详述的 xiángshù de ‹instructions, reasons, plan›; 明确的 míngquè de ‹denial, purpose, prohibition, warning›
▸ to be explicit about or on sth; 对某事很明确 
② (open) 直截了当的 zhíjié-liǎodàng de ‹criticism, opposition, support›
▸ to be explicit about sth; 对某事直言不讳 
③ (sexually graphic) 有露骨性爱场面的 yǒu lùgǔ xìng'ài chǎngmiàn de ‹film, description›
```
感觉我这里应该翻译成 显式的
比方说像下面场景
```C++
class MyString{
	MyString(char* str){
	
	}
	MyString(int len){
	
	}
};

int main(void)
{
	MyString str1=10;
}
```
上面这段代码可读性比较差，可能会有人误解成`MyString str1="10"`，但显然不是这样。所以可以防止 隐式法调用构造函数，可以改写成`explicit MySring(int len){.  }`这样的形式

### 动态创建对象
回想一下，就是在C语言中是怎样动态分配内存的？用malloc、calloc、realloc，然后还要配合free函数进行释放
C语言下动态分配内存的一些缺点：
1. 必须知道对象的长度，因为malloc里面的东西是要分配多少字节
2. malloc返回的是void*，在C++下必须使用强转来赋值给其他指针
3. 要判断分配内存是否成功
4. C语言中要自己写一些函数来完成初始化（构造）和释放（析构）的操作，忘了写，那就从入门到内存泄漏了
- 因此就提供了new和delete的方法来管理内存
```C++
// new和malloc
Person* p = new Person;
相当于
Person* p = (Person* )malloc(sizeof(Person));
if (p == NULL){
return 0;
}
Person->Init();  // 其中这个init是构造函数，而且要我们自己来写
```
#### new注意事项：
##### 1.不要用void* 去接受new出来的对象，否则delete无法调用析构，而且无法释放，除非强转成(person* )，那实际上也没有意义了
##### 2.利用new开辟自定义数据类型的数组
注意一定，new出来的对象数组一定会调用默认构造，假如有这样一段代码会报错，因为[[C++入门day03#^e7aaa4]]如果存在有参构造，编译器就不会提供默认构造函数，但是new出来的对象数组一定会调用默认构造，就有问题了，这个时候哪怕我们假如一个默认构造（空实现）也好
```C++
class Person{
	Person(int x){
	
	}
};

int main(void){
Person* p = new Person[10];
delete p;
}
```
但是这样还是不够的，我们会在delete出现问题，这个问题就是大名鼎鼎的内存泄漏问题。不过编译器通常会记下来我们创建的元素的个数，所以只需要告诉编译器我们这个是数组就好了，也就是把delete语句改写成`delete [] p `就行了
##### 3.想一个问题，用new开辟数组会调用默认构造，那在栈区开辟的数组能不能没有默认构造？大难是：可以的
```C++
Person Array[10] = {Person(10),Person(20)};
```
##### 4.那new的时候能不能同时赋初值？不一定（为了兼容性，不要这么做）
意思就是，能不能存在这样的代码
```C++
Person* p = new Person[10]{Person(10),Person(20)};
```
对了，这里返回的是一个一级指针，不用二级指针去接收，用以及的就好了

这里使用编译的时候是有一个坑的
```Shell
$ gcc main.cpp    # 包括这里使用clang也会报相同的错误
Undefined symbols for architecture arm64:
  "operator new[](unsigned long)", referenced from:
      _main in main-e302b7.o
ld: symbol(s) not found for architecture arm64

# 这个一看就是链接出现了错误，解决方案，用错编译器了
# 可以使用g++或者是clang++来处理
# 蠢了，前几天学了编译的流程，这里我还搞错了
$ clang++ main.cpp
```










