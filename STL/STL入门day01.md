### 目的：为了提高代码的复用性
1. 重载
2. 继承
3. 模板
4. 多态

### STL广义上分为：
1. container容器
2. algorithm算法
3. iterator迭代器
迭代器将：容器+算法 进行无缝链接

### STL 六大组件
1. 容器：如vector/list/deque/set/map，从实现角度看，STL是class template
2. 算法
3. 迭代器：重载了指针等。原生的指针也算是一种迭代器
4. 仿函数：重载()。可以协助算法来完成一些策略
5. 适配器：修饰容器和仿函数或迭代器接口的东西
6. 空间配置器：用于分配内存，释放内存

### 算法
1. 质变算法：拷贝，添加，删除
2. 非质变算法：查找、计数、遍历、寻址

### 迭代器的分类
![[Pasted image 20230305151011.png]]

### 容器划分
1. 序列式容器
2. 关联式容器

### 原生的指针也是迭代器


### vector容器
```C++
#include <vector>
vector<int> v;  // 创建一个vector容器，容器里面存放的是int类型

// 插入元素
v.push_back(10);
v.push_back(20);
v.push_back(30);
v.push_back(40);

vector<int>::iterator itBegin = v.begin();
// v.begin() 起始迭代器，指向容器中第一个数据
vector<int>::iterator itEnd = c.end();
// v.end() 借宿迭代器，指向的是容器中最后一个元素的下一个位

// 遍历元素
while (itBegin!=itEnd){
	cout<<*itBegin<<endl;
	itBegin++;
}
```
上面是第一种遍历方式，下面是第二种遍历方式
```C++
for (vector<int>::iterator it = v.begin(); it!=v.end(); it++){
	cout<<*i<<endl;*
}
```
第三种遍历
```C++
#include <algorithm>. // 标准算法头文件
for_each(v.begin(),v.end(),myPrint); //  first, end, 回调函数
```

### 案例：容器嵌套容器
```C++
#include <iostream>
#include <vector>
using namespace std;

// 这个案例是容器嵌套容器
void test01() {
    vector<vector<int> > v;

    vector<int> v1;
    vector<int> v2;
    vector<int> v3;

    for (int i = 0; i < 5; i++) {
        v1.push_back(i + 1);
        v2.push_back(i + 11);
        v3.push_back(i + 101);
    }

    v.push_back(v1);
    v.push_back(v2);
    v.push_back(v3);

    for (vector<vector<int> >::iterator it = v.begin(); it != v.end(); it++) {
        for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end();
             vit++) {
            cout << *vit << " ";
        }
        cout << endl;
    }
}

int main(void) {
    test01();
    return 0;
}

```


### string容器
没有见过的string构造
写10个a。`string str(10,'a');`

##### 赋值
> string& operator=(const char* s);//char*类型字符串 赋值给当前的字符串
> string& operator=(const string &s);//把字符串s赋给当前的字符串
> string& operator=(char c);//字符赋值给当前的字符串
> string& assign(const char *s);//把字符串s赋给当前的字符串
> string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
> string& assign(const string &s);//把字符串s赋给当前字符串
> string& assign(int n, char c);//用n个字符c赋给当前字符串
> string& assign(const string &s, int start, int n);//将s从start开始n个字符赋值给字符串

##### 获取字符串长度
str.size()

##### 访问方式
str.at(100) 和 str[100] 的效果类似，但是str[100]是没有throw的，就是一个赤裸裸的内存越界。这个时候程序直接挂掉
但是假如是 str.at(100) 会抛出一个系统标准异常类out_of_range。可以通过
```C++
catch(out_of_range& e){
	cout<<"越界！"<<endl;
}
```
来捕获异常: invalid string position！

### 拼接操作


### string重新分配内存，原来的地址被释放了
![[Pasted image 20230305195046.png]]


### vector的swap巧用，收缩内存
```C++
vector<int>(v).swap(v);
```
先是用vector\<int\>通过拷贝构造创建了一个匿名对象。这个拷贝构造的大小是原本的v.size。然后这个匿名对象通过swap成员函数，与v相互交换空间，这个时候v拿到的就是一块小的空间，然后匿名对象，单行执行完后立即释放，原本的大空间就被释放掉了

### vector巧用reserve预留空间
```C++
int main(void){
	vector<int>v;

	int* p = NULL;
	int num = 0;
	for (int i=0; i<100000; i++){
		v.push_back(i);
		if (p != &v[0]){
			p = &v[0];
			num++;
		}
	}
}
```
这段代码的意思就是。尾插10w个数。这个时候就很神奇了，因为这里有10w个数，所以肯定会扩容的，并且vector扩容会直接替换内存空间，所以首地址也会发生改变。首地址发生改变，就会使里面那个if语句成立。所以这个就是用来判断重新分配了几次内存空间的。
上面这段代码的输出是30。就是说开辟10w个数，要重新分配30次内存，运算过程有点浪费。
假如我事先就知道了会重新分配10次内存，那么我可以一开始就先预留10w的空间，然后让他一次插

### vector逆序遍历
逆序遍历的迭代器叫做reverse_iterator
这里就是rbegin和rend
这里还是i++
```C++
for (vector<int>::reverse_iterator it = v.rbegin(); it != v.rend(); i++){
	cout<<*it<<endl;
}
```

### 随机跳跃迭代器
验证随机访问迭代器
```C++
vecotr<int>::iterator itBegin = v.begin();
itBegin = itBegin + 3
```
假如上面那段代码没有发生错误，就是可以进行跳跃的 随机迭代器
- 但是list的迭代器不支持随机访问

假如是能逆序访问的话
```C++
vecotr<int>::iterator itBegin = v.begin();
itBegin = itBegin-- 
```


### vector插入insert

### vector删除erase

### vector尾插push_back

### vector尾删pop_back

### vector清空clear

### deque
- vector是单向开口的，deque是双向开口的。vector的头插效率极低。
底层原理，不是一个真正意义上的连续空间，只是将一段一段的连续空间进行拼接
![[Pasted image 20230305233412.png]]
需要的是中控器




