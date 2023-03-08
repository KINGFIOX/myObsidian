### 栈容器
1. 复合先进后出的数据结构
2. 不提供遍历功能（不提供迭代器）
3. 对外接口
	1. 入栈puSh
	2. 出栈pOp
	3. 栈顶tOp
	4. 是否为空empty
	5. 栈大小size

### 队列
1. 不提供遍历功能（不同迭代器）
2. 符合先进先出的数据结构
3. 对外接口
	1. 入队puSh
	2. 队头front
	3. 是否为空empty
	4. 

### liST
1. 双向循环链表
2. list对空间资源的控制非常精确，不会造成一点浪费
3. 链表灵活，但是空间和时间的额外消耗很大 
4. liST插入元素和删除元素，迭代器不会失效。但是vector不一样
5. list可以双向遍历，但是不能随机访问
6. 接口
	1. 

liST和vector是STL中最常用的
链表反转 l.reverse

### 链表反转实现的原理：
其实挺简单的，就是将连得线给反过来就行了


### 链表排序
sort链表会报错
- sort的规则：如果容器的迭代器支持随机访问，就可以使用sort
- 不支持随机访问的迭代器的容器，内部会提供算法的接口
就是直接l.sort就行了
- 当然这里默认是从小到大
- 加入要从大到小，提供回调函数
当然，假如是自定义数据类型的排序，也是用回调函数

- 多级排序，假如年龄相同，那么身高升序排序
在回调函数中添加
```C++
bool myCompare(Person p1, Person p2){
	if (p1.m_Age == p2.m_Age){
		return p1.Height > p2.Height;
	}
	return p1.m_Age > p2.m_Age;
}
```

### remove按值删除
假如是自定义数据类型，那么就要重载\=\=号，不然编译器不知道到底是什么


### set集合
特性：
1. 插入的时候自动排序，set的key值也是value值
2. set的迭代器不能改变set元素的值，也就相当于是const_iterator
3. set和multiset底层的实现原理都是红黑树


### multiset多重集合
- 与set的差别，允许有重复的value

### 二叉搜索树
![[Pasted image 20230306133038.png]]
因为这个二叉搜索树的原理，是不能用迭代器来修改值的，否则就会打乱顺序

### 平衡二叉树
![[Pasted image 20230306133144.png]]

### upper_bound(keyElem)返回第一个key>keyElem元素的迭代器
### lower_bound(keyElem)返回第一个key>=keyElem元素的迭代器


### equal_range(keyElem)
- 返回容器中key与keyElem相等上下限的两个迭代器
- 这里的返回类型是对组：pair<iterator, iterator> _Paircc
- equal_range是上面两种的结合。就是 取等 和 不取等 的的两个

### 创建对组
- 不需要引用任何的头文件就可以使用
1. 构造函数
```C++
pair<string, int> p("tom", 18);
```
2. 创建对组（方法）
```C++
pair<string, int> p2 = make_pair("jerry", 18);
```

### 对组
```C++
typedef pair<iterator, iterator> _Paircc
```

### set插入重复的东西不会报错
- 但是会插入不成功
- 但是实际上，插入是有返回值的，返回值类型是pair
- pair里面装的是 迭代器 和 布尔值
![[Pasted image 20230306183525.png]]
- 下面代码可以判断有没有插入成功
![[Pasted image 20230306183630.png]]

### 如何让set从大到小排序
set默认只能从小到大排序。一旦插入了元素以后，就不能修改里面的值了
所以，假如想要指定set从小到大排序，只能一开始就指定他的规则
那怎么指定规则？
set<int,func>s;
但是这里，func不能是函数，因为模板里面只能放数据类型。这个时候，仿函数就派上用场了。
![[Pasted image 20230306185422.png]]
但是这个时候，以后的话，迭代器就得使用set<int,func>::iterator
![[Pasted image 20230306185523.png]]

同样，这里还可以通过 仿函数 实现对 自定义类型 的数据进行排序

### map
- 所有元素都会根据元素的兼职自动排序
- map中所有的元素都是pair
- map不能通过迭代器修改key值，只能修改value值
- map的key不允许重复，multimap
- 底层原理也是红黑树

### map插入方式
1. 匿名插入
```C++
map<int,int> m;
m.insert(pair<int,int>(1,10));
```

2. make_pair匿名插入
```C++
m.insert(make_pair(2,20));
```

3. 第三种（长）（不推荐）
```C++
m.insert(map<int,int>::value_type(3,30));
```

4. 中括号（不推荐）
```C++
m[4] = 40;
```
- 比方说，可以这样的操作（很容易误操作）
- 这样会输出m[5]=0;
```C++
m[5];
```

### erase要传入一个kEY

### find(key)

### lower_bound(keyElem) 
返回第一个key大于等于keyElem的迭代器

### upper_bound(keyElem)
返回第一个key大于keyElem的迭代器

### equal_range()返回上面两个的对组
```C++
pair<map<int,int>::iterator, map<int,int>::iterator> ret2 = equal_range(3);
```
上面这个是接收。
那么要怎么获取key和value呢？
ret2.first。就是ret点出来，得到的是一个迭代器。
其中这个迭代器指向的是一个pair地址（不一定是地址。这里统称吧），然后pair的第一个元素是key，第二个元素是value
ret2.first->first。

### 改map排序的规则
仿函数！


### STL使用的时机！
![[Pasted image 20230306192600.png]]
![[Pasted image 20230306192602.png]]
1. vector单向数组。非常适合做遍历的操作
2. deque排队购票
3. list频繁的插入删除，并且是随机插入
4. set比方说手机游戏的个人得分
5. map想要快速通过id找到东西，比方说身份证查找




