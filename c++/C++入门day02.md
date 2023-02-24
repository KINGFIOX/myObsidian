### 设计一个类
- 突然有点明白了，上一章[[C++入门day01#^0f3ea0]]。就是讲结构体的时候，就说到C++中对结构体有增强，结构体中可以包含函数。这个类和结构体很像，有点像是一个自定义的数据类型，但是又不完全是一个数据类型
```C++
class circle
{
public:  // 公共权限

	// 求圆周长
	double calculateZC()
	{
		return 2*PI*m_R;
	}
	void setR(int r)
	{
		m_R = r;
	}
	int getR()
	{
		return m_R;
	}
	// 半径
	int m_R;
};
int main(void)
{
	circle c1;  // 通过类创建一个对象（实例化对象）
	// 给c1半径赋值
	c1.m_R=10;
	ca.setR(10);
	cout<< c1.calculateZC() << endl;
}
```
##### 专业术语：
- 实例化对象：通过类创建一个对象
- 成员函数、成员方法：类中的函数
- 成员变量、成员属性：类中的变量




