### 数组类封装
```C++
// 下面这个是一个拷贝构造函数
MyArray::MyArray(const MyArray& arr){
	this->m_Capacity=arr.m_Capacity
}
```
这段代码比较神奇，m_Capacity是MyArray下面的私有属性，但是这里arr.m_Capacity能直接点 `.` 出来，这就比较神奇了，原因：在相同类的作用域下可以这样操作。
下面这个是myArray.cpp文件
```C++
#include "myArray.h"

#include <stdlib.h>

#include <cstring>
#include <iostream>
using namespace std;
MyArray::MyArray()  // 默认构造
{
    // 默认创建一个容量为100的数组
    cout << "调用默认构造" << endl;
    this->m_Capacity = 100;
    this->m_Size = 0;
    this->pAddress = new int[this->m_Capacity];
}
MyArray::MyArray(int capacity)  // 有参构造
{
    cout << "调用有参构造" << endl;
    this->m_Capacity = capacity;
    this->m_Size = 0;
    this->pAddress = new int[this->m_Capacity];
}
MyArray::MyArray(const MyArray& arr)  // 拷贝构造
{
    cout << "调用拷贝构造" << endl;
    this->m_Capacity = arr.m_Capacity;
    this->m_Size = arr.m_Size;
    this->pAddress = new int[m_Capacity];
    memcpy(this->pAddress, arr.pAddress, this->m_Size);
}
void MyArray::pushBack(int val)  // 尾插
{
    this->pAddress[this->m_Size] = val;
    this->m_Size++;
}
void MyArray::setData(int pos, int val)  // 按位置设置数据
{
    if (pos >= this->m_Size) {
        return;
    }
    this->pAddress[pos] = val;
}
int MyArray::getData(int pos)  // 按位置获取数据
{
    if (pos >= this->m_Size) {
        return EOF;
    }
    return this->pAddress[pos];
}
int MyArray::getCapacity()  // 获取容量
{
    return this->m_Capacity;
}
int MyArray::getSize()  // 获取数组长度
{
    return this->m_Size;
}
MyArray::~MyArray()  // 析构函数,用来释放堆区空间
{
    // 析构函数,释放内存
    if (this->pAddress != NULL) {
        cout << "调用析构函数" << endl;
        // 释放内存
        delete[] this->pAddress;
        // 指针置空
        this->pAddress = NULL;
    }
}

```
下面这个是myArray.h文件
```C++
#ifndef __MYARRAY_H__
#define __MYARRAY_H__
// 设计类
class MyArray {
   public:                           // 对外提供的接口
    MyArray();                       // 默认构造
    MyArray(int capacity);           // 有参构造
    MyArray(const MyArray& array);   // 拷贝构造
    void pushBack(int val);          // 尾插
    void setData(int pos, int val);  // 按位置设置数据
    int getData(int pos);            // 按位置获取数据
    int getCapacity();               // 获取容量
    int getSize();                   // 获取数组长度
    ~MyArray();                      // 析构函数,用来释放堆区空间

   private:
    int m_Capacity;  // 用来记录数组的容量
    int m_Size;      // 用来记录数组的长度
    int* pAddress;   // 用来记录数组的首地址
};

#endif

```
我这里命名有点问题，但是我突然悟了一点东西，就是假如是名词，就要大驼峰，假如是动词就要用小驼峰，当然我这个是看出来的，不一定在正确

### 运算符重载
这里要注意，假如使用全局的运算符重载，那么重载运算的变量必须是有公用权限的，当然也可以设置全局的运算符重载函数为友函数。
```C++
#include <iostream>
using namespace std;

class Person {
    friend Person operator+(const Person& p1, const Person& p2);

   public:
    Person(){};  // 构造函数空实现,注意一下这个,后面要考
    Person(int a, int b) {
        this->m_A = a;
        this->m_B = b;
    }
    void printClass() {
        cout << this->m_A << endl;
        cout << this->m_B << endl;
    }

    Person MyPlus(const Person& p) {
        // 能完成这一步说明有默认构造
        // 但是有了有参构造以后编译器就不会给我们提供默认构造了
        // 所以要不自己写一个默认构造,要不Person temp(0,0);
        Person temp;
        temp.m_A = this->m_A + p.m_A;
        temp.m_B = this->m_B + p.m_B;
        cout << "调用了MyPlus" << endl;
        return temp;
    }

#if 0  // 把这段代码注释掉
    // 这里使用运算符号的重载
    Person operator+(const Person& p) {
        Person temp;
        temp.m_A = this->m_A + p.m_A;
        temp.m_B = this->m_B + p.m_B;
        cout << "调用了MyPlus" << endl;
        return temp;
    }
#endif

   private:
    int m_A;
    int m_B;
};

// 全局的运算符重载
Person operator+(const Person& p1, const Person& p2) {
    Person temp;
    temp.m_A = p1.m_A + p2.m_A;
    temp.m_B = p1.m_B + p2.m_B;
    return temp;
}

int main(void) {
    Person p1(10, 20);
    Person p2(985, 211);
    // 像下面这种运算肯定是不行的,但是我想让他变的行起来
    // Person p3 = p1 + p2;
    // 但是只有这样还不够,木大大
    Person p3 = p1.MyPlus(p2);
    Person p4 = p1 + p3;
    Person p5 = operator+(p3, p4);
    p3.printClass();
    p4.printClass();
    p5.printClass();
}

```
运算符重载非常有趣，但是一定程度上降低了代码的可读性
- 内置的数据类型是不能重载的

### 左移运算符的重载
比方说
```C++
class Person;
Person p1;
cout<<p1<<endl;
```
这样的代码，这个cout是不知道怎么去拼接的，但是加入我就是想要通过这样的
	试图利用成员函数去 做<<重载
```C++
void operator<<(Person& p){
	
}
// 假如真的是这样的话，调用的方式
p1.perator<<(cout)
// 可以简化成 p1<<cout，这个和想象中的是不一样的

```
首先，我们通过跳转到定义可以知道
cout的类型是ostream //out stream
同样，为了保证返回的还是cout，所以返回值是ostream&
其次，如果要在全局下访问对象中的成员，要用到友元friend

突然发现我这个vim，可以使用gd来跳转到定义
然后可以通过<S-\[>来进行向上跳转，可以通过<S-\]>向下跳转

### 前置后置递增运算符重载
- 如何区分 前置递增 和 后置递增？占位参数！！！
- 后置函数 先返回，后自增，这个怎么整
- 前置的效率比较高，因为实际上后置会调用拷贝构造创建新的数据

在视频中，visual studio对于后置递增是可以编译过的，但是我的电脑上clang是编译不通的
![[Pasted image 20230228182023.png]]
```C++
#include <iostream>
using namespace std;
class Person {
   public:
    Person(int age) {
        cout << "构造函数调用" << endl;
        this->m_age = age;
    }
    void showAge() { cout << "age=" << this->m_age << endl; }
    ~Person() { cout << "析构函数调用" << endl; }

   private:
    int m_age;
};

class SmartPoint {
   public:
    SmartPoint(Person* person) { this->m_person = person; }
    ~SmartPoint() {
        if (this->m_person != NULL) {
            delete this->m_person;
        }
    }
    // 使用运算符重载来使sp真正成为一个指针的形式``
    Person* operator->() {
        // 返回person的地址
        // 大概得形式是p->showAge();
        return this->m_person;
    }
    Person& operator*() {
        // 这里就是能够有那种解引用的形式
        // (*p).showAge();
        return *this->m_person;
    }

   private:
    Person* m_person;
};

void test01() {
    // 比方说像这样一段代码
    // 就是要写一个delete
    // Person* p = new Person(18);
    // (*p).showAge();
    // p->showAge();
    // delete p;
    // 有参构造,创建了一个Person,并将指针的地址传给sp
    // sp的类型是SmartPoint
    SmartPoint sp(new Person(18));
    (*sp).showAge();
    sp->showAge();
}

int main(void) {
    test01();
    return 0;
}

```
上面程序中
```C++
SmartPointer sp(new Person(18));
```
就表示：用有参构造在堆区上面创建出来了一个对象，返回了对象的地址，然后用SmartPointer的有参构造，用sp来维护这个地址。
但是，我还是希望使用(\*p).和p->的操作，那就需要重载
```C++
Person& operator*(){
	return *this->m_person
}
```
因为这里要返回的(\*p)出来的是本身，而不是一个地址，所以在return的时候返回的就是\*this。但是这样还不够，如果返回的知识person，那就相当于返回的是一个新床架你的一个临时变量，如果想要放回本身，就要使用引用
还有一点要注意
```C++
...
class SmartPoint{
public:
Person* operator->() {
        // 返回person的地址
        // 大概得形式是p->showAge();
        return this->m_person;
    }
private:
Person* m_person;
}
...
SmartPoint sp(new Person(18));
sp->showAge();

```
这里的sp->showAge();其实不应该这样写，其实应该写成这样sp->->showAge();因为注意一下，这个sp->是什么？实际上是sp.perator->，这里返回的是一串地址，这个地址是m_person的地址，然后这个地址再指向showAge()才能发挥作用。所以也就有sp->->showAge();但是为什么这里还能编译运行通过呢？编译器帮我们做了一步优化!

但是实际上指针指针在实际开发中很少使用

### operator=
编译器至少给类添加了4个函数！默认构造，有参构造，拷贝构造，operator=
当然operator=只是一个简单的浅拷贝
所以说，这里肯定会出现深浅拷贝的问题，以及堆区重复释放的问题
也就是说，编译器是允许这样的代码出现的：
```C++
Person p1(18);
Person p2;
p2 = p1
```

刚才我这段重构的代码不是很严谨
```C++
    Person& operator=(const Person& p) {
        if (this->m_name != NULL) {
            delete[] this->m_name;
            this->m_name = NULL;
        }
        this->m_name = new char[strlen(p.m_name) + 1];
        strcpy(this->m_name, p.m_name);
        this->m_age = p.m_age;
        return *this;
    }

```
首先我没重新分配内存，其实我这个赋值是直接让他们相等了，而不是strcpy，这个肯定是不对的


