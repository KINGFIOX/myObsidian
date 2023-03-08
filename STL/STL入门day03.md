### 仿函数、函数对象
- 几元函数对象，就表示有仿函数有几个参数
- 仿函数主要内容也就是重载小括号，调用就像是使用函数一样
- 可以拥有自己的状态，比方说可以统计调用的次数
##### 函数对象可以作为函数的参数传递
```C++
void doPrint(MyPrint myPrint, int num){
	myPrint(num);
}

void test(){
	// 这里传进去的是一个匿名对象
	doPrint(MyPrint(),1000);
}
```
##### 函数对象通常不会不定义构造函数和析构函数
##### 函数对象可以内联接，性能好
这个内联接inline。因为成员函数基本上前面都隐式的加了inline，性能好一些
模板函数可以使函数对象具有通用性。像之前set的排列规则的指定

### 谓词
定义：普通函数，或者是仿函数，返回是一个bool类型，这个函数就叫做谓词
几个参数，就叫做几元谓词

### 一元谓词
- find_if 条件查找，这个时候要穿一个谓词进去
```C++
class GreaterThan20{
public:
	bool operator()(int num){
		return num>20;
	}
}
ret = find_if(v.begin(), v.end(),GreaterThan20());
```

### 二元谓词
- 之前就用到了，比方说像快速排序操作
```C++

```


### lambda表达式
- `[]` 是lambda的表示
```C++
for_each(v.begin(), v.end(), [](int val){cout<<val<<" ";})
```

### 内建函数对象
实际上就是类模板+仿函数
要包含头文件
```C++
#include <functional>
```

1. 加法
```C++
plus<int> p;
cout<<p(10,10)<<endl;
```
- 为什么这个只用是一个int就行了呢？因为这个是人家自己设置的模板。但是实际上，因为必须是同一种数据类型才能进行相加操作，所以只用写一个int就行了
- 当然，其实上面那个还可以更简单（谢邀，不简单）一些:
```C++
cout << plus<int>()(10, 10) << endl;
```

2. 大于
```C++
greater<int>
```
![[Pasted image 20230306223416.png]]
这里可以看到这个排序的默认参数就是functional中的lower

### 仿函数（函数对象）适配器
举个例子，比方说像下面这个代码
```C++
class MyPrint{
public:
	void operator()(int val){
		cout<<val+1000000<<endl;
	}
}

void test01(){
	vector<int> v;
	for (int i =0; i<10; i++){
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), MyPrint());
}
```
- 这个代码中有1000000，像这里，假如这个起始值不断的改，或者是说想要用户操作。这里不是已经将这个值写死了嘛，这个值也确实没法改，除非使用全局变量。
那么还有什么办法可以将这个值改动呢？
能不能这样？
```C++
class MyPrint{
public:
	void operator()(int val, int start){
		cout<<val+start<<endl;
	}
}

void test01(){
	vector<int> v;
	for (int i =0; i<10; i++){
		v.push_back(i);
	}
	cout<<"请输入起始的值？"<<endl;
	int num = 0;
	cin>>num;
	// 结果在这一步就出现了问题
	for_each(v.begin(), v.end(), MyPrint());
}
```
结果在for_each这一步就出现了问题，因为是这个for_each只帮我们传一个参，这个我们确实控制不了。
那是不是没招了呢？（自己写一个for_each，笑）
即答：适配器！（依托答辩）
1. 上面那段代码在for_each处添加bind2nd
- 绑定！
```C++
for_each(v.begin(), v.end(), bind2nd(MyPrint(), num));
```
- 但是这个只是一个内建的函数对象，要加头文件`#include <functional>`
2. 继承！
- 在定义类的时候，改成`void operator()(int val, int start):public binary_function`
- 但是这还没有完。要对参数、返回值类型进行萃取
- `void operator()(int val, int start):public binary_function<int, int, void>`
3. 加const修饰。因为这个`operator()`是重写小括号，重写要求类型是一样，甚至对于const等修饰也要完全一样。那就`void operator()(int val, int num) const`
- 实际上那个bin2nd(MyPrint(), num)就是创建一个匿名的函数对象

注：
- bind2nd和bind1st区别：就是将绑定的参数，赋在了 第一个参数 和 第二个参数 的差别
- 这里回顾一下const，像上面那种用法就是常函数，不能用来修改成员变量

### 一元取反操作
1. not1(GreaterThan5());
2. 继承GreaterThan5:public unary_function<int, bool>
3. bool operator(int val) const

### 内建+绑定+取反
1. 首先，我们是有了greater这个内建的函数
2. 我们又有了绑定，说明我们可以直接调控里面的值，我们这里调控第二个值
- 这里创建了一个greater\<int\>的匿名对象
- 这里的意思就是，return val > 5
```C++
bind2nd(greater<int>(), 5)
```
3. 最后取反，就是可以用于判断小于等于5
```C++
not1( bind2nd(greater<int>(), 5) )
```

### 二元取反适配器
- 比方说二元谓词less
- sort会传两个参数
- 这里是less接收两个参数
- 然后因为是两个参数，所以就用二元取反not2()
```C++
sort(v.begin(), v.end(), not2(less<int>()))；
```

### 函数适配器
- 与仿函数适配器做区分
- 因为实际上，只有仿函数才能做继承之类的操作。这里需要先将函数变成仿函数！
- 这里就直接是一个函数对象（匿名函数）（仿函数）
```C++
ptr_fun(myPrint3)
```
然后可以用 仿函数适配器
bind2nd(ptr_fun(myPrint3), 1000)
这个还是上面那个例子

### 成员函数适配器
需求：上面已经讲了真正的函数，仿函数
还有一类函数：成员函数！！！
下面做一下成员函数的适配
比方说：Person下面有showPerson的成员函数
Person::showPerson就是成员函数
然后去地址 &Person::showPerson
然后加上适配器mem_fun_ref

- 下面是取地址的理由：
- 简单来说，这是语法
> 为什么要取成员函数的地址? 因为可以通过一定的手段 **让成员函数作为回调函数**, 而不再使用全局的静态函数

下面引用一段话（不行不行，这里函数名取地址还不是很清楚！）（看一下《C和指针》！）

> 《C和指针》这本书中，”初始化表达式中的＆操作符是可选的，因为函数名被使用时总是由编译器把它转换为函数指针。＆操作符只是显式地说明了编译器将隐式执行的任务“。

>《C和指针》这本书中，“取一个数组名的地址所产生的是一个指向数组的指针，而不是一个指向个指针常量值的指针。”。

> 结论：在之前arr和&arr虽然结果相同，但二者之间的意义却截然不同；
> 实际上&arr取的是数组的地址，而不是数组首元素的地址；
> arr+1指的是加一个元素内存大小（加4），&arr+1指跳过整个数组，即加40，存在本质区别；

> test是函数的首地址，它的类型是void ()，&test表示一个指向函数test这个对象的地址，  它的类型是void (*)()


- 函数名是函数指针，我觉得不完全是这样，我觉得函数名是 函数地址的解引用（这个就暂且认为是这样吧，因为我确实有点懵了）




> 一个函数在编译时被分配一个入口地址，这个地址就称为函数的指针，函数名代表函数的入口地址


### for_each
1. 有返回值
![[Pasted image 20230307093541.png]]
可以将里面的匿名对象返回出来。

2. 可以绑定参数
就可以 bind2nd(MyPrint(),num)

### transform算法，将指定容器搬运到另一个容器里
```C++
class MyTransform{
	int operator()(int val){
		return val;
	}
}

void test(){
	vector<int>v;
	for (int i=0; i<10; i++){
		v.push_back(i);
	}
	vector<int> v2;
	v2.resize(v1.size());
	transform(v.begin(), v.end(), MyTransform());
}
```
上面就可以完成搬运的操作。
这里为什么要resize()?
- 因为，这里加入直接传入迭代器，然后往里面硬塞数据的话，肯定是不行的。因为里面都没用容量，怎么给里面塞数据呢？这个是不满足动态扩容的操作的
- 这个提供了时机，让我们在搬运的过程中进行一些操作

### find
```C++
vector<int> v;
for (int i=0; i<10; i++){
	v.push_back();
}
vector<int>::iterator pos = find(v.begin(), v.end(), 5);
if (pos!=v.end()){
	cout<<"找到了"<<endl;
}
else{
	cout<<"未找到"<<endl;
}
```
注意find返回的是一个迭代器
但是，这样的代码是有局限性的，就是内置数据类型肯定是不满足我们的需求的
- 我们其实可以看到，就是find底层是要用到 \=\= 号。这就需要重载这个符号了 
```C++
class Person{
public:
	string m_name;
	int age;
	bool operator==(const Person &p){
		return p.m_name == this->m_name
			&& p.m_age == this->m_age;
	}
}
vector<Person> v;
// 插入数据就先省略了
vector<Person>::iterator pos = find(v.begin(), v.end(), p);
```
但是，这样还不够。

### find_if
像上面的find，肯定只能比较实体，不能比较指针。
比较指针嘛，就是两个地址比一比。这样的比较显然是没有意义的
那咋整？find_if！用条件的方式进行查找
```C++
class MyComparePerson{
public:
	bool operator()(const Perons *p){
		return p->m_name == "bbb"
			&& p->m_age == 20;
	}
}



vector<Person *> v;
Person p1("aaa",10);
Person p1("bbb",20);
Person p1("ccc",30);
Person p1("ddd",40);
Person p1("eee",50);

v.push_back(&p1);
v.push_back(&p2);
v.push_back(&p3);
v.push_back(&p4);
v.push_back(&p5);

Person* p = new Person("bbb",20);
vector<Person*>::iterator ret = find_if(v.begin(), v.end(), MyComparePerson());
```
这样就可以比较自定义的数据类型
然后，假如想要指定的话，可以使用 bind2nd


### adjacent_find算法，查找相邻重复元素
```C++
	vector<int>::iterator ret = adjacent_find(v.begin(), v.end());
```
- 找到的是第一次出现的位置
![[Pasted image 20230307104604.png]]


### binary_search二分查找
- 只能在有序序列中使用，不能在无序序列中使用
- 返回值是bool


### count和count_if
count只能数 固定数 的个数

count_if，条件查找，传入仿函数


### merge合并
作用：将两个容器合并，放入一个新的容器中。
- 目标容器一定要先resize
- 原本两个容器必须要是有序的，而且顺序是相同的（同降序、升序）
![[Pasted image 20230307110045.png]]
![[Pasted image 20230307105904.png]]

### random_shuffle算法，指定范围内的元素重新调整次序
```dictionary
shuffle
v.洗牌;拖着脚走;(笨拙或尴尬地)把脚动来动去;
	坐立不安;洗(牌);把（纸张等）变换位置，打乱次序
n.洗牌;曳步舞;拖着脚走
```
![[Pasted image 20230307110622.png]]
- 这套算法在使用的时候，每次都是一样的，因为底层还是用了random
- 要使用随机数种子

### reverse算法，反转指定范围的元素

### copy算法
同样要resize
![[Pasted image 20230307111125.png]]

copy其他的用法：通过copy打印东西
```C++
copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
```
这个要记得包含头文件`#include <iterator>`

### replace算法，将容器中指定范围的旧元素修改为新元素
```C++
replace(v.bedin(), v.end(), oldValue, newValue);
```
打印
```C++
copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
```

### replace_if算法
按条件替换，将大于3的值，替换为3000
```C++
class MyReplace{
	bool operator()(int a){
		return a>3;
	}
}
replace_if(v.begin(), v.end(), MyReplace(), 3000);
```
打印
```C++
copy(v.begin(), v.end(), ostream_iterator<int>(cout, " "));
```
当然，同样可以使用适配器

### swap算法，俩容器交换全部元素
```C++
vector<int> v1(10,100);  // 这种构建方式：10个100
vector<int> v2(10,200);

swap(v1, v2);

copy(v1.begin(), v1.end(), ostream_iterator<int>(cout, " "));

copy(v2.begin(), v2.end(), ostream_iterator<int>(cout, " "));
```


### accumulate算法，计算容器元素累计总和
包含头文件`#include <numeric>`（小型算法的头文件）
![[Pasted image 20230307113918.png]]
就是可以不用自己求和那么麻烦
- 第三个参数，累计起始值

### fill算法，向容器中添加元素
```C++
vector<int> v;
v.resize(10);
fill(v.begin(), v.end(), 100);  // 全部用100填充
								// 如果之前的值存在
								// 覆盖!
```

### set_intersection算法，求交集
- 先将目标容器扩容成两个容器容量的最小值
```C++
vTarget.resize(min(v1.size(),v2.size()));
```
set_intersection使用
```C++
set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
```
但是这个set_intersection有返回值
@return 目标容器的最后一个元素的迭代器地址
```C++
vector<int>::iterator itEnd = set_intersection(v1.begin(), v1.end(), 
													 v2.begin(), v2.end(), 
													 vTarget.begin());
```
假如要遍历的话，必须使用这个返回值

### set_union并集
- resize最坏的情况：两集合完全不同，size相加
```C++
vTarget.resize(v1.size()+v2.size());
```
然后使用set_union
```C++
vector<int>::iterator itEnd = set_union(v1.begin(), v1.end(), 
										v2.begin(), v2.end(), 
										vTarget.begin());
```
- 假如不是用返回的迭代器呢？
![[Pasted image 20230307124416.png]]
就会把最后几个无效的也输出出来。最后面无效的，是resize剩余的容量


### set_difference差集A-B
- resize最会的情况：两个集合完全不同，那就取大的
```C++
vTarget.resize(max(v1.size(),v2.size()));
```
然后使用
```C++
vector<int>::iterator itEnd = set_difference(v1.begin(), v1.end(), 
										v2.begin(), v2.end(), 
										vTarget.begin());
```


### 注意：集合运算必须是有序的集合