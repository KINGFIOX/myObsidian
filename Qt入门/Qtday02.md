### QmainWindow
注：这里铆接部件，也叫做：浮动窗口
![[Pasted image 20230309231105.png]]

### 菜单栏
```C++
// 创建对象
QMenuBar *bar = menuBar();
// 设置菜单栏
setMenuBar(bar);
// 创建菜单，要包含头文件 #include <QMenuBar>
QMenu *fileMenu = bar->addMenu("文件");
```

### 工具栏
```C++
// 创建工具栏对象
QToolBar *toolbar = new QToolBar(this);
// 创建工具栏
addToolBar(toolbar);
```
这个addToolBar还有几种不同的重载版本，可以查看帮助文档
![[Pasted image 20230310092848.png]]
设置窗口的位置，这里连接的话，用位运算：或
这个和枚举类型有关。并且，这里Qt::LeftToolBarArea实际上就是0x01，主不过是枚举
```C++
toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea)
```

```C++
  QToolBar *toolBar = new QToolBar(this);   // 挂到对象树上
  addToolBar(Qt::LeftToolBarArea, toolBar); // 默认左侧
  toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea);
  toolBar->setFloatable(false); // 设置浮动
  toolBar->setMovable(false);   // 设置允许移动(总开关)
  toolBar->addAction(newAction);
  toolBar->addSeparator(); // 设置分割线
  toolBar->addAction(openAction);
```

### 状态栏
```C++
QStatusBar *stBar = statusBar();
setStatusBar(stBar);
QLabel *label1 = new QLabel("左侧信息", this);
QLabel *label2 = new QLabel("右侧信息", this);
stBar->addWidget(label1);          // 添加左侧信息
stBar->addPermanentWidget(label2); // 添加右侧信息
```

其实可以看到，就是假如是set的话，就是唯一的。假如是add的话，那就是可以有多个

### 铆接部件（浮动窗口）与 核心部件
```C++
// 铆接部件
QDockWidget *dock = new QDockWidget("aaa", this);
addDockWidget(Qt::LeftDockWidgetArea, dock);

// 核心部件
QTextEdit *edit = new QTextEdit(this);
setCentralWidget(edit);
```
核心部件只有一个


### design

##### 添加资源
怎样才能添加图标？图标icon。
添加资源的文件
![[Pasted image 20230310170259.png]]
用编辑的方式打开res.qrc

##### 添加图标
将文件导入到qrc中后，使用api，actionNew->setupUi(QIcon(""));
其中这个QIcon里面的东西是":/image/butterfly.png"

### 点击新建，创建对话框
triggered触发

##### 对话框分类
1. 模态对话框，不能对其他窗口操作
2. 非模态对话框。与上面相反

1. 模态对话框
对话框要包含头文件`#include <QDialog>`
其中，执行模态对话框的时候，还要使用dlg.exec()阻塞代码
报warning，对话框太小了
```C++
  connect(ui->actionNew, &QAction::triggered, this, [=]() {
    // 创建对话框
    // 模态对话框不允许对其他窗口操作
    QDialog dlg(this);
    // 调整大小
    dlg.resize(400, 300);
    dlg.exec();
  });
```
2. 非模态对话框。可以看到差别，一个是show，一个是exec。这个同样可以创建在堆区或栈区
```C++
// 非模态对话框
QDialog *dlg2 = new QDialog(this);
// 调整大小
dlg2->resize(400, 300);
dlg2->show();
```
但是，栈区和堆区会有差别
栈区创建的非模态对话框，会一闪而过。一个原因是代码没有阻塞。还有一个就是，因为这个是创建在栈区上的，执行完代码以后就会被立即释放掉
但是，假如这个创建在堆区上，也就是挂到对象树上，那么假如一直有人触发信号，那么也就一直创建在对象树上，直到代码崩溃，或者是关闭mainWindows。
为了阻止一直创建对象，那么就要设置属性——delete on close!这个枚举类型是55
![[Pasted image 20230310183338.png]]
```C++
dlg2->setAttribute(Qt::WA_DeleteOnClose);
```


### 标准对话框
QMessageBox

1. 错误对话框
```C++
QMessageBox::critical(this,"错误","critical");
```
- 模态的
- 标题是：“错误”
- 内容是：“critical”

2. 信息提示对话框
```C++
QMessageBox::information(this,"信息","info");
```
- 模态的
- 标题是：“信息”
- 内容是：“info”

3. 询问提示的对话框
```C++
QMessageBox::question(this,"询问","question");
```
- 模态的
- 标题是：“询问”
- 内容是：“question”
上面这条的参数都是默认参数，这里的是使用一些自己的参数，上面的选项是yes或no
```C++
    QMessageBox::question(this, "询问", "question",
                          QMessageBox::Save | QMessageBox::Abort,
                          QMessageBox::Abort);
```
这里的效果图如下
这里的选项是save或abort（当然，不一定是二选一，还可以是多选一）
然后默认选项，我设置成了abort
![[Pasted image 20230310185224.png]]
- 这里的返回值是StandandButton，也就是像QMessageBox::Abort之类的

### 颜色选择对话框
```C++
connect(btn, &QPushButton::clicked, this, [=]() {
		QColor colorName = QColorDialog::getColor(Qt::red);
	});
```
- 默认选取看红色

### 文件选择对话框
```C++
connect(btn, &QPushButton::clicked, this, [=]() {
		QString fileName = QFileDialog::getOpenFileName(this, "打开文件", 
												"./test.txt", "(*.txt)");
	});
```
- 默认选取 ./test.tXT
- 文件选取的类型是 .txt

### 字体选择对话框
```C++
connect(btn, &QPushButton::clicked, this, [=]() {
		bool flag;
		QFont fontName = QFontDialog::getFont(&flag, QFont("华文彩云",36));
	});
```
- QFont("华文彩云",36)就是默认：华文彩云，36号字体
- &flag，别人就是要求传一个bool型的指针

### 界面布局
1. 灵活运用弹簧spacer进行界面布局
	- 弹簧有9像素
2. Containers->widget
3. label标签，可以显示文本
4. line edit单行编辑框
	- password模式
5. windowsTitle
6. 窗口大小minimalSize/maximalSize
7. 水平对齐/垂直对齐

### 常用控件
1. 按钮可以添加图片
	- icon图标
2. toolButton 
	- 设置完图片以后，发现文字被覆盖掉了
	- 要改风格：toolButtonStyle，默认是toolButtonIconOnly
	- autoRaise，选中时突起效果
1. radioButton 单选按钮
	- 发现，结果会出现不同的2组相悖的选项，但是只能选一个，要分组！
	- 用GroupBox进行分组
	- 假如想要有一个默认选中，那就要代码控制`ui->rbtn_Man->setChecked(true);`
2. checkBox 多选按钮
	- 如何捕捉多选的信号呢？`&CheckBox::stateChanged`。这个CheckBox::stateChanged是有参数的，这个参数是int state。那么该如何接收呢？槽函数中写\[=\](int state)\{\}
	- state有三种状态：
		1. 0 未选中
		2. 2 选中
		3. 1 半选中（ QCheckBox 中 tristate ）

### QListWidget控件
1. 创建条目QListWidgetItem，然后可以用addItem();
2. 设置对齐模式：setTextAlignment();
	- 传参，枚举值
3. 除了有addItems（这里多了一个s）
	- 之类传参的类型是QStringList 
	- 这种类型的本质是：list\<string\>.[[STL入门day02#^58fc7f]]list就是链表
```C++
QStringList list;
list<<"床前明月光"<<"疑似地上霜"<<"举头望明月"<<"低头思故乡"<<endl;
ui-listWidget->addItems(list);
```

### QTreeWidget树控件
1. 在设计中是 Tree widget
2. 设置头（可以使用匿名对象）`ui->treeWidget->setHeaderLabels(QStringList()<<"英雄"<<"英雄简介");`
3. 创建力量的根`QTreeWidgetItem *liItem = new QTreeWidgetItem(QStringList()<<"力量");`
4. 但是只是创建还不行，还要绑定到头`ui->treeWidget->addTopLevelItem(liItem);`
5. 根后面的节点，类型也是QTreeWidgetItem。（操作如下）
	- 创建子节点`QTreeWidgetItem *li = new QTreeWidgetItem((QStringList()<<"刚被猪"<<"前排坦克"));`
	- 将子节点挂载到父节点上`liItem->addChild(li)`

### QTableWidget表格控件
0. 设置列数`ui->tableWidget->setColumnCount(3)`这里设置了3列
1. 设置水平表头`ui->tableWidget->setHorizontalHeaderLabels(QStringList()<<)`
2. 设置行数`ui->tableWidget->setRowCount(4)`这里设置了4行
3. 添加单个格儿`ui->tableWidget->setItem(0,0,new QTableWidgetItem(QString("亚瑟")));`这里还是使用了匿名对象。第一个0表示row，第二个0表示col。这里是将这个QString转换成了QTableWidgetItem
- QStringList和QList\<QString\>是一样的
- QString::number(18);可以将18从int转成QString
- 除了中括号访问，还可以用.at(i)来访问

### ScrollArea滚动区域

### ToolBox工具区
- 插入列
- 改名

### Tab Widget
- 插入列
- 改名

### Stacked Widget
stackedWidget并不是像预览那样，有一个小的箭头，而是需要触发才能换页

### comboBox下拉菜单
- 这个下拉菜单，假如想要设置默认，或者是触发槽函数的话
下拉菜单的定位可以是:index，也可以是 名字

### Text Edit 和 Plain Text edit
- Text Edit 可以设置字体/颜色/格式
- Plain Text Edit 就非常的平凡

### spin Box和double spin bOX
就是可以选择数量，比方说京东那种
- double就是可以有小数

### Time Edit / Time edit / Data Edit / DataTime Edit

### Vertical Scroll Bar / Horizontal Scroll bar
水平竖直滚动条

### Horizontal Slider / Vertical Slider
水平竖直滑块，就是假如设置好的话，可以做到滚动的同时，spinBox的数字增加

### label
- 不止可以显示文字
- 还可以显示图片
`ui->label_img->setPixmap(QPixmap(":/images/butterfly"));`
- 还可以显示动态的图片
```C++
QMovie *movie = new QMovie(":/xxx.gif");
ui->label_movie->setMovie(movie);
// 这个时候还没完。要start以后才有用
movie->start();
```





