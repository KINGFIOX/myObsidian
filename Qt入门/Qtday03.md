### 自定义控件封装
##### spinBox/horizontalSlider联动
首先，加入是要自己设计界面的话，不只是要.h/.cpp。还需要有.ui。这个时候，c/c++的class就已经满足不了我们了，所以就需要有创建一个Qt的文件，选择widget就行了
![[Pasted image 20230311170327.png]]
这里可以看到，我们的控件是属于QWidget。这个只是在myWidget.ui里面的，并不是在mainWindows.ui里面的。这个时候，在左侧的控件栏也没有我们的这个控件。这个时候，用一个等价的控件来接收。并将这个新创建的Widget提升为myWidget，并使用全局包含（因为后面可以快捷操作）
我们现在的需求是：修改spinBox 右侧滚动条会跟着移动
信号和槽
valueChange有两个版本，说明，需要使用函数指针
![[Pasted image 20230311174607.png]]
联动
```C++
void (QSpinBox::*pChange)(int) = &QSpinBox::valueChanged;
connect(ui->spinBox, pChange, this,
	  [=](int val) { ui->horizontalSlider->setValue(val); });
connect(ui->horizontalSlider, &QSlider::valueChanged, this,
	  [=](int val) { ui->spinBox->setValue(val); });
```

### 监听鼠标
- 小技巧，左括号(可以补齐参数
- frameshape可以给label加一个框
1. 首先要自己封装一个label。
2. 要继承Qlabel
3. 提升类
4. 然后对于鼠标的监听，这个是一个虚函数，要发生重写
5. Reimplemented重新实现。类型都是virtual
```c++
// 鼠标进入事件
void enterEvent(QEvent *);
void leaveEvent(QEvent *);

// 鼠标移动事件
void mouseMoveEvent(QMouseEvent *ev);
// 鼠标按下事件
void mousePressEvent(QMouseEvent *ev);
// 鼠标松开事件
void mouseReleaseEvent(QMouseEvent *ev);
```
1. 假如要设置左右键按下
```C++
if(ev->button() == Qt::LeftButton){}
```
2. 但是，假如这样设置的话，那个鼠标移动就有问题（注：这里会出现，按下左键，拖动鼠标并没有发生移动）
3. 想一个问题，既然是按下才移动的话，可能会出现歧义，可能是按下左键，突然又换成右键移动，或者是两个按键同时按下，这样意义并不明确。
4. 改成
```c++
if(ev->button() & Qt::LeftButton){}
```
5. 鼠标移动事件，能不能让他不是点击以后才移动，要设置状态，在构造函数中写
```c++
setMouseTracking(true);
```

### 定时器事件
需求：在label中设置一个数字，并让这个数字随着时间而增加
- QString::number(num) 可以将num从int转换为QString
```c++
void Widget::timerEvent(QTimerEvent *) {
  // 每隔1秒,让数字自增
  static int num = 1;
  ui->label->setText(QString::number(num++));
}
```
- static，就是每次都会重新调用这个num，所以每一次都重新赋值
- 想要阻止这样的事情，就要加上static
- 这个时候，还是要在构造函数中设置，开始计时
```C++
startTimer(1000); // 单位 毫秒
```

但是假如我想要有两个计时器呢？
帮助文档这里有写：会返回一个identifier
![[Pasted image 20230313114303.png]]
怎样用上这个identifier呢?
```C++
// widget的构造函数中
int id1 = startTimer(1000);
int id2 = startTimer(2000);

// widget::timerEvent中
if (e->timerId() == id1)
{
	static int num =1;
	ui->label_1->setText(QString::number(num++))
}
if (e->timerId() == id2)
{
	static int num =1;
	ui->label_2->setText(QString::number(num++))
}
```
- 另一种创建定时器的方法
定时器类\<QTimer\>
```C++
// 创建对象，挂在对象树上
QTimer *timer1 = new QTimer(this);
timer->start(500);
// 每隔0.5秒会发出一个信号
connect(timer,&QTimer::timeout,[=](){
	static int num = 1;
	ui->label_4->setText(QString::number(num++));
});
```
- 点击暂停的按钮 停止计时器
1. 每隔xxx ms会抛出一个timeout信号
2. 可以对这个timeout信号进行处理
4. timer->start(xxx)开始
5. timer->stop()停止

### 事件分发器
1. 要做一个app
2. app中有很多功能，对应的就是不同的事件
3. 但是，实际上并不是app来分发，而是app下面有一个事件分发器
下面是模式图：
![[Pasted image 20230313130928.png]]









