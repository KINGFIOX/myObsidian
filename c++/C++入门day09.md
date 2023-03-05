### 类型转换
##### 1. 静态类型转换static_cast
- 比方说，像之前提到的向上转换、向下转换，父转子，子转父
- 允许内置的数据类型转换
- 允许父子之间的指针、引用是可以转换的

静态类型转换static_cast
```C++
char a = 'a';
double d = static_cast<double>(a);
```
- 类指针的转换
- 非父子之间的类型是不能互相转换的
- 引用之间的转换也是可以成功的
```C++
class base {};
class son:public base{};
class other{};
base* b = NULL;
son* s = NULL;
other* o = NULL;
// 父类指针转子类指针（不安全）
son* s2 = static_cast<son*>(b);
// 子类指针转父类指针（安全）
base* b2 = static_cast<base*>(s);

// 下面这段代码会报错
other* o2 = static_cast<other*>(b);
```


##### 2.动态转换dynamic_cast
- 动态类型转换 严格！（安全）
- 假如想要严格的防止寻址越界，那么就可以用dynamic_cast

- 内置数据类型不允许！
```C++
// 下面这段代码会报错
char a = 'a';
double d = dynamic_cast<double>(a);
```

- 向上转换、向下转换
- 也是无法执行非父子之前的指针、引用转换
- 但是向下转换的时候，报错不是报例如内存越界之类的错误，而是：dynamic_cast必须包含多态类型！
```C++
class base {};
class son:public base{};
class other{};
base* b = NULL;
son* s = NULL;
other* o = NULL;
// 父类指针转子类指针（不安全）
// 直接报错
son* s2 = static_cast<son*>(b);
// 子类指针转父类指针（安全）
base* b2 = static_cast<base*>(s);

// 下面这段代码会报错
other* o2 = static_cast<other*>(b);
```

如果发生多态，类型转化总是安全的[[[C++入门day07#^17772e]]]
那要怎么发生多态[[C++入门day07#^7f3474]]

##### 3.常量转换
- 常量转换只适用于：指针、引用
```C++
const int * p = NULL;
int * pp = const_cast<int*>(p)
```
有点不懂上面这段代码的意思？回顾一下：常量指针是什么？
[[[C++入门day01#^9247d4]]]
突然好想明白了，之前的那些命名的意义！
- 函数模板（强调的是模板！）
- 模板函数（强调的是函数！）就是函数模板创建出来的函数
- 常量指针，就是一个指向常量的指针！指针的解引用能不能改？不能！指针的指向能不能改？能！
- 指针常量，就是类型是指针的常量，指针的指向不能改！
这个常量，非常量，就是取决于const修饰的位置

为什么常量转换不能有下面这段代码？
```C++
const int a = 0;
int* b = const_cast<int*>(a);
```
想一下，a是存放在哪里的？常量表里面的！有内存吗？没有！
所以自然也就不能将指针指向那里！

可以用引用转换
```C++
int num = 10
int &a = num;
const int &b = const_cast<int&>(a);
```

##### 4.重新解释转换（不建议使用）
- 最不安全的一种转换

甚至可以将 int 转换为 int* 
```C++
int a = 10;
int *p = reinterpret_cast<int*>(a);
```

```C++
class base {};
class son:public base{};
class other{};
base* b = NULL;
son* s = NULL;
other* o = NULL;

// 下面这个转换可以转换成功
other* o2 = reinterpret_cast<other*>(b);
```


### 异常
##### 先看一下C语言中的异常处理方式
C语言中两种异常处理的方式：
![[Pasted image 20230304083735.png]]

缺点：
1. 异常提示不统一
![[Pasted image 20230304083226.png]]
2. 异常和返回结果有二义性
![[Pasted image 20230304083216.png]]

##### C++异常机制的优势！
1. ![[Pasted image 20230304083819.png]]
2. ![[Pasted image 20230304083833.png]]
3. ![[Pasted image 20230304083851.png]]
4. ![[Pasted image 20230304083922.png]]
什么是调用跳级？
- 比方说有这样的调用结构：A调用B，B调用C。也就是C最先执行
- 但是假如C返回了一个异常，比方说是`return -1`，结果B把`-1`当成正确结果继续运行。
- 那么最终A运算的结果肯定是错的一万八千里的

1. 在调用函数中写这段代码捕获异常
```C++
try{
	// 这里是一段可能会出现问题的代码
	func1();
}
catch(int){
	...
}
```
- 这个catch里面的数据类型与throw要相同，不然是不处理的
2. 在被调函数中，写报错代码
```C++
if (b == 0){
	throw -1;
}
```
3. 如果要使用除此之外的其他类型，可以在catch里面写`catch(...)`
4. 假如捕获到了异常，但是不想处理，可以继续向上抛出处理
```C++
try{
	func1();
}
catch(...){
	throw;
}
```
- 异常在C++中必须处理，如果不进行处理，那么程序就会崩掉（调用了terminate函数）

还可以自定义异常类
```C++
class myException{
public:
	void printError(){
		cout<<"我自己的异常"<<endl;
	}
}

int myDivision(int a, int b){
	if(b==0){
		throw myException();
	}
}
```
- 这里先将一下，异常的英文单词是exception
- 这里myException加上一个小括号()是什么意思？返回的是一个匿名对象。当然，这期间，实际上会使用两次构造函数，一次是创建匿名函数的时候构造，一次是在返回的时候构造
继续上面的代码：
```C++
int main(void){
	try{
		myDivision();
	}
	catch(myException e){
		e.printError();
	}
}
```
- 如果try没有发生异常，那么catch语句就不会执行

### 栈解旋
1. 异常被抛出后
2. 从进入try块起，到异常被抛出前，这期间在站上构造的所有对象，都会被自动析构
3. 析构的顺序与构造的顺序相反
4. 这个过程称为栈解旋

### 异常接口声明
```C++
// 这里是只运行抛出int和char类型
void func()throw(int, char)
{
	
}
```
- 但是假如这里抛出了其他的，错误提示是调用了terminate函数
- 假如是`void func()throw()`，这个小括号里面什么都没写，那就是不允许抛出

### 异常对象的生命周期
```C++
class MyException{
	MyException(){
		cout<<"myexception默认构造函数调用"<<endl;
	}

	MyException(const MyException& e){
		cout<<"myexception拷贝构造函数调用"<<endl;
	}
	~MyException(){
		cout<<"myexception析构函数调用"<<endl;
	}
}

void doWork(){
	throw MyException();
}

int main(void){
	try{
		doWork();
	}
	catch(MyException &e){
		cout<<"自定义异常捕获"<<endl;
	}
}
```
- 上面代码可以优化。就是一般传参的话，假如是类的话，不推荐使用按值传参，可以使用引用传递，减少资源的开销。因为假如是按值传递，传递的同时会使用拷贝构造。增大了资源的开销。
按值传参和引用传参打印的结果是一样的：
```Shell
myException默认构造调用
自定义异常捕获
myException析构函数调用
```
有了引用传参和值传参后，还有地址传参:
- 也就是将上面写成`throw &myExption();`（这里依然要写小括号，为了创建匿名对象）然后下面写`catch(MyException *e)`以及`e->printError`
- 这个时候输出的结果和上面两种方式不一样了!
```Shell
myException默认构造调用
myException析构函数调用
自定义异常捕获
```
说明什么？就已经调用了析构函数，说明这个匿名对象在调用e->printError之前就已经被释放掉了，后面再调用对象的其他成员函数e->funcOther肯定是不行的。甚至，其实不应该有这个`自定义异常处理`的输出。

上面的三种传参方式总结：
1. 抛出的是 throw MyException(); catch (MyException e) 调用拷贝构造函数 效率低
2. 抛出的是 throw MyException(); catch (MyException &e) 只调用默认构造函数，效率高，推荐！！！
3. 抛出的是 throw MyException(); catch (MyException &e) 对象会提前释放掉，不能再非法操作

为什么？这个和匿名对象有关！
为什么引用可以使用？
1. 看一下引用的语句：throw MyException(); catch (MyException &e)。这里的操作也就是：MyException &e = MyException();右边是一个匿名对象，左边是一个引用的创建。实际上就是给匿名对象起名的一个操作。有了名字，匿名对象就不在是匿名对象。
- 回顾一下匿名函数的三种创建方式：
	1. Person p1("tom",18);
	2. Person p1 = "tom"; 当然这里是有参构造写成只有一个值的形式
	3. Person p1 = Person("tom", 18);
	- 实际上，第三种就像是上面那样，使用匿名对象来创建对象（但是经过编译器的优化以后，最终只创建出来了一个对象，只调用了一次构造函数）
那为什么地址不可以使用？
1. 匿名对象的一大特性：改行创建，改行释放！因为这里没有给匿名函数起名。
2. 上面也就是变成了这样的语句：MyException\* e = &MyException();
3. 改行创建，改行释放。然后 e 就成了野指针，就不行了

当然，只要解决了匿名对象的释放问题，这个还是可以按照地址传参的！
直接在堆区上面创建一个匿名对象!`throw new MyException()`就行啦！
- 堆区手动开辟手动释放（包括匿名对象也是如此）

### 异常的多态使用
这里先回顾一下什么是多态？
多态分为静态和动态：
1. 静态就是重载之类的
2. 主要是动态：
	1. 有继承关系
	2. 父类中有虚函数
	3. 子类重写父类的虚函数
	- 重写的概念
		1. 返回值相同
		2. 函数名相同
		3. 参数：类型、顺序、名字、修饰词、个数相同
动态：函数地址的早绑定还是晚绑定。vfptr -> vftable
大概回顾完了!

比方说编译器报错的底层实际上就是多态实现的
```C++
class BaseException{
public:
	virtual void printError() = 0;
}

// 空指针异常
class NULLPointerException{
public:
	virtual void printError(){
		cout<<"空指针异常"<<endl;
	}
}

// 寻址越界
class OutOfRangException{
public:
	virtual void printError(){
		cout<<"越界异常"<<endl;
	}
}

void doWork(){
	throw NULLPointerException();
	// 或者是
	throw OutOfRangException();
}


int main(void){
	try{
		doWork();
	}
	catch(BaseException& e){
		e.printError();
	}
}
```
多态两种使用方式：
- 父类指针接受子类对象
- 父类引用接受子类对象
这里当让使用引用传参，因为简单。（考虑到匿名函数问题！）

顿时有点理解多态的含义了！
就是同一段代码有多种含义，当然这个是良定义的，不能有二义性。
如何改变含义？就是让他在不同的情况下，调用的东西不一样。
- 比方说重载，不同的情况下，就是传参不同嘛。
- 比方说动态的多态：就是实际上调用的函数不同。当然，这里是要用基(父类）类型创建对象！然后只是传参方面的小改变，就可以让他调用不同的函数

### 系统标准异常类
包含系统标准的异常头文件`#include <stdexcept>`
```C++
#include <stdexcept>
class Person{
public:
	Person(int age){
		if (age<0 || age>150){
			throw out_of_range("年龄必须在0~150之间!");
		}
		else{
			this->m_Age = age;
		}
	}
private:
	int m_Age;
}

void test01(){
	try{
		Person p1(180);
	}
	catch(exception &e){
		cout<<e.what()<<endl;
	}
}
```
系统标准异常的父类：exception
出了什么错误？e.what();
一般来说，我们不用打印错误信息，一般都是系统来提供的。
我们只用负责捕获异常就行了。

### 编写自己的异常类
就是我想要扩展系统的标准异常。
1. 我应该重写哪些东西？
![[Pasted image 20230304215024.png]]
	- 可以看到析构函数是virtual的
	- what也是virtual的
2. 重写的注意事项？
	- 上面我们也看到了，这个what的返回值是char\*，这点是要注意的
	- 其次，这个是一个常函数[[C++入门day04#^64f459]]，常函数中不可以修改成员变量，除非这个成员变量前面有mutable修饰
- 补充，C++可以将const char\*隐式转换为string。但是反之不成立
- 不过，可以通过string类中的.c_str()来实现转换
```C++
class MyOutOfRangeException:public exception{
public:
	// 构造函数
	MyOutOfRangeException(const char* str){
		this->m_errorInfo = str;  // 这里发生char*隐式转化为string
	}
	MyOutOfRangeException(string str){
		this->m_errorInfo = str;  // 这里就直接都是string类型的了
	}
	
	const char* what() const{
		return this->m_errorInfo.c_str();
	}

private:
	string m_errorInfo;
}
```
下面是尝试使用我们这个
```C++
class Person{
public:
	Person(int age){
		if (age<0 || age>150){
			throw MyOutOfRangeException(string("年龄必须在0~150之间!"));
		}
		else{
			this->m_Age = age;
		}
	}
private:
	int m_Age;
}

void test01(){
	try{
		Person p1(180);
	}
	catch(exception &e){
		cout<<e.what()<<endl;
	}
```
上面的调用中`MyOutOfRangeException(string("年龄必须在0~150之间!"));`是什么意思？
- 这里往首先这是创建一个类为：MyOutOfRangeException的匿名对象，方式是：有参构造。
- 这个参数也是一个匿名构造，类是string。有参构造，参数是"年龄..."

### 标准输入流
- 我们看一下标准输入流
![[Pasted image 20230304225358.png]]

1. cin.get() 无参数，可以读取一个字符。可以吸收缓冲区的换行符。和get的效果类似

2. cin.get(xxx,xxx) 就是cin.get()有两个重载的版本
- 下面这个程序可以验证，用cin.get(buffer,1024)的这个版本，会把 换行符 遗留在缓冲区中。
- 假如不想要用到这个换行符，那么就需要一个cin.get()来吸收掉这个换行符
```C++
void test03(){
	char buf[1024] = {0}
	// 这个可以获取一个字符串
	// 这样可以把所有输入的数据放到一个buffer中
	cin.get(buf,1024);

	char c = cin.get();
	if (c=='\n'){
		cout<<"换行符遗留在了缓存区中"<<endl;
	}
	else{
		cout<<"换行符不在缓冲区中"<<endl;
	}

	cout<<buf<<endl;
}
```

3. cin.getline(buf,1024) 这个版本的cin.getline不会遗留换行符在缓冲区中。并且换行符不会保留在buf中，而是直接扔掉了。
- 同样，可以通过下面程序来验证
```C++
void test03(){
	char buf[1024] = {0}
	// 这个可以获取一个字符串
	// 这样可以把所有输入的数据放到一个buffer中
	cin.getline(buf,1024);

	char c = cin.get();
	if (c=='\n'){
		cout<<"换行符遗留在了缓存区中"<<endl;
	}
	else{
		cout<<"换行符不在缓冲区中"<<endl;
	}

	cout<<buf<<endl;
}
```

4. cin.ignore(int x) 忽略前x个字符（不填参数，默认忽略1个字符）
```C++
void test04()
{
	cin.ignore(2);
	char c = cin.get();
	cout <<"c ="<<c<<endl;
}
```

5. cin.peek() 偷窥
```C++
void test05(){
	char c = cin.peek();
	cout<<"c ="<<c<<endl;

	c = cin.get();
	cout<<"c = "<<c<<endl;

	c = cin.get();
	cout<<"c = "<<c<<endl;
}
```
就是假如输入as，那么cin.peek()输出是a，就是偷看了第一个字符，但是没有取走。


6. cin.putback() 放回
```C++
void test06(){
	char c = cin.get();
	cin.putback(c);

	char buf[1024] = {0};

	cin.getline(buf,1024);
	cout<<buf<<endl;

}
```
想几个问题：既然这里都叫做放回了，那是放回原位？还是尾插？
- 通过验证是放回原位。

### 标准输入流案例：
##### 案例1：判断用户输入的内容是字符串还是数字
- cin.peek
- cin.putback


##### 案例2：让用户输入一个0~10之间的一个数字，如果有误，请重新输入

```C++
int main(void){
	cout<<"请输入0~10之间的数字"<<endl;
	int num;
	while (true){
		cin>>num;
		if (num>=0 && num<=10){
			cout<<"输入正常，输入值为："<<num<<endl;
			break;
		}
		cout<<"输入有误，请重新输入！"<<endl;
	}
	

}
```
这个时候代码的健壮性是有问题的：就是我不知道这个用户到底输入的是什么东西，万一他输入的是 a 呢？这个时候就会死循环了。
- 为什么会进入死循环？因为这个是缓冲区始终有 a ，这个 a 一直进不去num变量。所以 a 一直停留在缓冲区中。cin从缓冲区开始读入数据，就又开始循环。
- 标志位cin.fail()，这个时候输入任一数字，cin.fail()=0，意味着：这个时候缓冲区的东西可以被读取。
- 标志位cin.fail()=1的时候，缓冲区就异常了
我们要做的是什么？
- 清空缓冲区 cin.clear()
- 重置标志位 cin.sync()
- cin.ignore()这个在vs2013以后要加，原因：缓冲区没有真的清空？

不过，我好像有点明白了这个输入缓冲区是什么意思了。之前想scanf，或者是get，就一直不是很清楚。尤其是有一个突然出现的get()。不过我这里也忘记了scanf和get，gets的区别了。

### 标准输出流
##### cout.put()向缓冲区写字符
链式编程的思想
```C++
cout.put('h').put('e');
```

##### cout.write()从buffer中写num个字节到当前输出流
```C++
char buf[] = "hello world";
cout.write(buf, strlen(buf));
```

##### 格式化输出
1. 使用控制符的方法
2. 使用流对象的有关成员函数

##### 2.使用流对象的有关成员函数
```C++
int main(void){
	int number = 99;
	cout.width(20);  // 指定宽度20
	cout.fill('*');  // 用18个*填充
	cout.setf(ios::left);  // 左对齐
	cout.unsetf(ios::dec);  // 卸载十进制
	cout.setf(ios::hex);  // 安装10进制
	cout.setf(ios::showbase);  // 显示进制数
	cout.unset(ios::hex);  // 卸载16进制
	cout.set(ios::oct);  // 安装八进制
	cout<<number<<endl;  // 输出，输出方式是上面的设置
}
```

##### 2.使用控制符的方式
```C++
#include <iomanip>. // 控制符格式化输出 头文件
int main(void){
	int number = 99;
	cout << setw(20)  // 设置宽为20
		 << setfill('*')  // 设置填充为*
		 << setiosflags(ios::showbase)
		 << setiosflags(ios::left)
		 << hex
		 << number << endl;

}
```
这个标准输出，标准输入博大精深，有本书《标准C++输入输出流与本地化》。深入研究的时候可以看一下。不过，就是这种感觉，很奇妙。实际上我们都是在研究别人写的代码的特性，我们编程实际上都是在调用别人的接口。最nb的人，写了一套底层的0101，然后又有人，写了一套汇编。然后到C语言。最后，又有人写了一套C++。然后我们都是在探究别人写的代码的特性

### 文件流
包含头文件`#include <fstream>`
##### 写文件
```C++
// 创建一个对象，名字叫ofs
// ios::out以输出的方式打开文件
// 结合：ios::trunc，就是如果文件存在，重新写入
ofstream ofs("./test.txt", ios::out | ios::trunc)
if (!ofs.is_open()). // 判断打开是否成功
{
	cout<<"文件打开失败"<<endl;
}
ofs << "姓名：孙悟空"<<endl;
```

##### 读取文件
```C++
#include <fstream>
ifstream ifs("./test.txt", ios::in);

if (!ifs.open()){
	cout<<"文件打开失败"<<endl;
}

// 第一种方式，通过右移运算符
char buf[1024] = {0};

while(ifs>>buf){
	cout<<buf<<endl;
}

// 第二种方式，通过函数
while(isf.getline(buf,sizeof(1024))){
	cout<<buf<<endl;
}

// 第三种方式，通过string的全局函数
string buf;
while(getline(ifs,buf)){
	cout<<buf<<endl;
}

// 第四种（不推荐）
// 一个字符一个字符的读取
char c;
while((c = ifs.get())!=EOF){
	cout<<c;
}

```




