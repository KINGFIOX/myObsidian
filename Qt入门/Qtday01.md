### main.cpp入口程序中的一些东西
```C++
#include "widget.h"
#include <QApplication>. // QApplication 应用程序类

// 程序入口   argc 命令行变量数量   命令行变量数组
int main(int argc, char *argv[])
{
	// a 应用程序类   在 Qt中 应用程序对象   有且只有一个
    QApplication a(argc, argv);
    // 窗口类. Widget继承的是 Qwidget类。
    // 通过 窗口类 实例化对象 w
    Widget w;
    // 窗口是不会默认弹出的，需要调用show方法进行显示
    w.show();

	// 进入一个进入消息循环的机制.  阻塞代码的功能
	// 就是像windows命令行那样，只要程序执行完成以后，就会关闭窗口
    return a.exec();
}
```

### xxx.pro项目工程文件(qmake的makefile)
```C++
QT       += core gui. // Qt包含的模块。Qt包含了很多的模块

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets. // 大于4版本，加入widgets模块

TARGET = xxx  // 生成 可执行程序 的名称
TEMPLATE = app  // 模板

// 包含的源文件和头文件，都是自动生成的
// 不用自己包含。
SOURCES += \
    main.cpp \
    widget.cpp

HEADERS += \
    widget.h
```
- 这里是包含的模板
![[Pasted image 20230308131231.png]]

### 头文件widget.h
```C++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

class Widget : public QWidget
{
    Q_OBJECT. // 支持信号和槽

public:
    Widget(QWidget *parent = nullptr);
    // 这里有默认参数，我们之前学到了，就是默认参数在声明或实现的一个有 
    ~Widget();
};
#endif // WIDGET_H

```

### 再看一下widget.cpp实现部分
```C++
#include "widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    // 这里就是对应的声明中有默认参数，实现中没有默认参数
    // QWidget是Widget的父类，这里是用到了父类。
    // 然后这里是将传进来的参数，交给父类QWidget初始化
{
}

Widget::~Widget()
{
}

```
在列表初始化的时候，初始化的同时，调用了父类的有参构造[[C++入门day06#^f49bec]]

### Qt中的命名规范
1. 类名：首字母大写。单词和单词之间，首字母大写
2. 变量/函数名：首字母小写。单词和单词之间，首字母大写

### Qt中的快捷键
1. command+R 运行
2. command+B 编译
3. command+F 查询
4. command+/ 注释
5. 

### QPushButton
- inhert 继承
	- 继承关系：QPushButton->QAbstractButton->QWidget->QObject（顶到头了）
```C++
// 创建创建 按钮 对象
QPushButton *btn = new QPushButton
```
但是这样并不会显示出来，因为我们没有show
```C++
btn->show();
```
不过，这种是从顶层弹出的方式。
如果想要显示到当前窗口，那么就需要做依赖
```C++
btn->setparent(this)
```
里面要求传递的是父类的指针。
因为我们这个是直接定义在了widget里面的，所以直接传this就行了
```C++
// 设置文本
btn->setText("德玛西亚");
```
但是，其实还是可以在创建按钮的时候就设置好，文本 继承
```C++
QPushButton *btn = new QPushButton("第二个",this);
```
但是这个都是只在左上角，需要知道怎么移动，有两种重载的版本
```C++
btn->move(100,100);
```

### 窗口
如何重置窗口的大小
```C++
resize(600, 400);
```
重置按钮的大小
```C++
btn->resize(300, 200);
```
指定窗口的标题
```C++
setWindowTitle("第一个");
```
设置窗口的固定大小
```C++
setFixedSize(600, 400);
```

### 对象树
- 这里都new出来了对象，但是没有些delete，因为这里会给我们回收
由QObject创建出来的子类，都是当我们的窗口关闭以后，都会帮我们回收东西

如何验证这种操作呢？
自己写一个按钮类，然后验证。
![[Pasted image 20230308222115.png]]
在新建对象中，没有AbstractButton和PushButton的类
只能找一个最近的类：QWidget（其实QObject也行）
然后再继承手动改为继承QPushButton。
- 一个是头文件
![[Pasted image 20230308222729.png]]
- 一个是列表初始化
![[Pasted image 20230308222734.png]]
- 写一个析构函数检查是否调用，打印用qDebug，就像cout一样正常用就行了
![[Pasted image 20230308223153.png]]
- 调用
```C++
// 创建自定义的按钮
MyPushButton *myBtn = new MyPushButton;
myBtn->setParent(this);
myBtn->setText("我的按钮");
myBtn->move(200, 200);
```
- 然后关闭窗口的同时输出了这一段东西
![[Pasted image 20230308223810.png]]
说明，我们没有手动释放的东西，这个会帮我们释放

- 下面是对象树的结构图
![[Pasted image 20230308223841.png]]
- 那我要如何验证这个new的顺序，和delete的顺序呢？
	就直接加上析构的qDebug就行了
但是这个输出是这样的
![[Pasted image 20230308224342.png]]
先打印出了widget，再打印出了MyPushButton
那是不是结论错了呢？不是！这里是先执行了除了delete以后的代码，再往下找子类，如果子类下面没有对象了，那就先从子类的对象开始释放，然后一步一步往回释放

1. 优点：一定程度上的优化了对象的回收机制
2. 当创建的对象，指定的父亲是有QObject或者Object派生的类时候，这个对象被加载到对象树上，当窗口关闭掉时候，树上的对象也都会被释放掉

### Qt中的坐标系
![[Pasted image 20230308224905.png]]
左上角是原点


### 信号槽
举个例子
connect(信号的发出者， 发送的信号， 信号的接受者， 处理的槽函数)
就是谁发出信号，发送的什么信号才起反应。接收的人是谁，这个接收的人要有什么反应
- 槽：主要指这个槽函数
```C++
// connect(信号发送者,发送信号,信号接受者,处理的槽函数)
connect(myBtn, &QPushButton::clicked, this, &QWidget::close);
```
- 信号的发送者：myBtn（这是一个对象）
- 调用的是QPUshButton中的成员函数clicked，然后取成员的函数的地址
- 信号由widget接收，这是在widget里面定义的，所以this就可以了
- 最后调用槽函数。关闭窗口，QWidget里面的close
- 这里其实可以发现，所有的参数都是指针类型的（都是传地址）

### 帮助文档的使用
- 这里可以查找帮助文档
![[Pasted image 20230309110333.png]]
- 然后找到了signal
![[Pasted image 20230309110512.png]]
- 在QWidget中可以找到slots，然后可以看到关闭之类的
![[Pasted image 20230309110712.png]]

### 自定义信号 和 槽函数
- 创建C++类
	如果不知道应该继承哪一个类，就直接继承最上面的QObject就行了
	![[Pasted image 20230309132520.png]]
1. 自定义型号需要写在Signalss下
	1. 返回值是void
	2. 只需要声明，不需要实现
	3. 可以有参数，可以发生重载
2. 自定义槽函数，可以写在public或者全局函数 或者 public slot 或者 lambda表达式
	1. 返回值是void
	2. 需要有声明，也需要有实现
	3. 可以有参数，可以发生重载

##### 这里来一个案例：下课后，老师饿了，学生请老师吃饭
这里的信号，和学生的行为，都是自定义的。
老师是信号的发出者，他发送了一个信号，肚子饿了
学生是信号的接受者，然后并且提出了请老师吃饭这件事
- 老师头文件中，类包含
```C++
signals:
	void hungery();
```
不需要实现
- 学生头文件，类包含
```C++
public slots:
	void treat();
```
头文件中只是声明，cpp中做实现
```C++
void student::treat(){
	qDebug()<<"请老师吃饭";
}
```
然后在widget.h中有：teacher/student创建的成员变量
```C++
teacher *zt;
student *st;
```
在wideget.cpp中创建分配内存（因为上面创建的是指针）
因为teacher/student是继承QObject的，所以也可以挂在对象树上
```C++
this->zt = new teacher(this);
this->st = new student(this);
```
![[Pasted image 20230309133839.png]]
这里好像是给我们整了一个有参构造，可以传入父类，然后挂在对象树上。就可以帮我们释放内存。
- 连接信号和槽
```C++
connect(zt, &teacher::hungery, st, &student::treat);
```
但是这样就能出现：学生请老师吃饭吗？不能！要有状语！下课后
之前，像关闭窗口，也只是：点击了关闭窗口的按钮以后
要有触发事件！触发自定义信号

##### 这里在上面的案例修改
上面提到：信号可以重构/槽函数也可以重构
这里引入一个有参信号/有参槽
但是，这里很明显就暴露问题了，就是这个没办法分辨hungery
![[Pasted image 20230309141619.png]]

这里要传入的是函数的地址，其实只用一个指针接收就行了。函数指针！
当自定义信号发生重载的时候，必须使用函数指针，明确的指出来是connect哪一个重载的版本
```C++
// 连接信号和槽 // 函数指针
void (teacher::*teacherSignal)(QString) = &teacher::hungery;
void (student::*studentSlot)(QString) = &student::treat;
connect(zt, teacherSignal, st, studentSlot);
```
这里在emit触发中可以写参数
```C++
emit this->zt->hungery("奥利给");
```
这里的输出结果是
```Shell
请老师吃饭，老师要吃：“奥利给”
```
但是这样，会有引号。
原因：看源代码
```C++
void student::treat(QString foodName) {
  qDebug() << "请老师吃饭,老师要吃" << foodName;
}
```
这里是将char* 和 QString 拼接。但是，所以会有引号。那要怎么去掉呢？
![[Pasted image 20230309144419.png]]
这里返回值是QByteArray，返回的是字节数组，那么再找，怎么样将QByteArray转成char*
![[Pasted image 20230309144602.png]]
！`.data（）`

1. 信号可以连接信号
```C++
// 2.无参的
void (student::*studentSlot2)() = &student::treat;
void (teacher::*teacherSignal2)() = &teacher::hungery;
connect(zt, teacherSignal2, st, studentSlot2);
// 构造函数中就调用,但是这样肯定是不行的
//  classIsOver();
// 创建按钮
btn = new QPushButton("下课", this);
setFixedSize(600, 400);
// connect 信号可以连接信号
connect(btn, &QPushButton::clicked, zt, teacherSignal2);
```
这样把三个时间连接在一起了
![[Pasted image 20230309161941.png]]
点击按钮，触发了老师饿了的事件，老师饿了，触发学生请客

2. 信号是可以断开连接的
```C++
disconnect(zt, teacherSignal2, st, studentSlot2);
```
参数还是和原本一样，只不过connect变成了disconnect

3. 一个信号可以连接多个槽函数
比方说，加入我一发了工资，我就可以买iphone，买ipad，买mac，买iwatch。这后面的买，都是以同一个槽函数，但是只有一个触发的事件：发工资！

4. 多个信号可以连接同一个槽函数
比方说：有人想要买手机的原因不同，但是只要满足一个即可。对，这里的逻辑关系是：或！

5. 信号和槽函数的参数类型，必须==一一对应==，信号的参数个数 可以多余槽函数的参数个数，反之不可以。就是==信号的参数个数>=槽函数的参数个数==。就是你的参数我可以不接收。但是我要的参数，你必须要传。

### Qt4版本的信号与槽（不推荐）
```Qt
connect(zt, SIGNAL(hungery(QString)), st, SLOT(treat(QString)))
```
1. 优势：看起来更加直观，也不用写一堆的函数指针
2. 劣势：不做参数类型的检测，运行阶段才报错，而且错误不易发现。
3. 本质：直接将SIGNAL和SLOT里面的东西看成是字符串。

### lambda表达式（匿名函数）C++11以后的
大概模式如下：\[函数对象参数\]\(操作符重载函数参数\)mutable ->返回值{函数体}
```C++
[capture](parameters) mutable ->return-type
{
	statement
}
```
首先，看一下，下面这一段代码能不能实现，其中btn2在外面就已经有声明和定义了
```C++
[](){
	btn2->setText("bbb");
}
```
但是这一段代码是直接报错的，因为没有btn2的定义
但是改成下面你这段代码，就是在中括号中加了=。
```C++
[=](){
	btn2->setText("bbb");
}
```
但是，这段代码并没有起作用。原因：这里只有定义，没有调用——加上括号！
```C++
[=](){
	btn2->setText("bbb");
}
```

1. 如果在中括号中写了一个变量，那么lambda中只能有这个变量起作用
```C++
[btn2](){
	btn2->setText("bbb");
	btn3->setText("ccc");
}
```
这个时候，这个btn3会报错
2. 中括号中写了一个&，那么局部的所有变量都将以应用的方式传递到这个lambda函数中
3. &btn，那么btn以引用的方式传递
4. a,&b。就是 a值传递，b引用传递
5. \[=,&a,&b\]除了a和b是引用传递，其他都是值传递
6. \[&,a,b\]除了a和b值传递，其他都是引用传递
那是不是 引用传 递就一定效率高呢？可能会有问题！比方说有下面这一段代码
```C++
QPushButton *btn = new QPushButton("aaa",this);
connect(btn,&QPushButton::clicked,this,[=](){
		btn->setText("bbb");
	};  // 这里最后不加括号，就是这里不是调用
)
```
这一段代码，假如将 = 改成了 & 。就会崩溃
原因：当进行信号和槽连接的时候，控件内就会进入一个锁的状态。假如用了引用传递，相当于是就是const里面直接操作了，这肯定不好。但是，假如值传递，给他传递了里面的指针的东西（实际上就是控件的地址）。那么，就能传递成功，并操作成功（这里实际上又创建了一个指针，指向这个控件）。

1. mutable，想一下这个mutable在哪里出现过？常函数！常对象！
- lambda加上了mutable就可以修改：值传递进来的拷贝
```C++
int m=10;
connect(btn1, &QPushButton::clikcked, this, [m]() mutable{m=20; qDebug()<<m;});
connect(btn2, &QPushButton::clikcked, this, [m]() mutable{qDebug()<<m;});
```
上面这段代码。当按下btn1的时候，输出的值是20。当按下btn2的时候，输出的值是10。就是说这个时候，就是值传递，只是一个简单的拷贝。但是假如把mutable去掉以后。这个程序执行就会报错。（感觉lambda就是一个常函数，常函数不能修改成员变量，除非成员变量前面加上mutable）

2. 加上->，设置返回值类型。没有就是void型。这样写。当然，这里要的是这个匿名函数的实现
```C++
int num = [=]()->int{
	return 1000;
}();
```







### invaild use of xxx：没有包含头文件！







