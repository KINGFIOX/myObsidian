[sympy](https://so.csdn.net/so/search?q=sympy&spm=1001.2101.3001.7020)作为相对比较全的数学计算库，其也包含针对线性代数的符号运算部分，本文着重介绍sympy在处理线性代数相关问题时的使用方法，且基本严格结合线性代数教材（同济大学版），便于大家回顾，如果想了解sympy在初等代数或微积分方面的应用，可以看文章[《python之sympy库--数学符号计算与绘图必备》](https://blog.csdn.net/yifengchaoran/article/details/110943305 "《python之sympy库--数学符号计算与绘图必备》")。

## 一、矩阵运算

## 1.1 创建矩阵

创建矩阵是使用sympy处理[线性代数](https://so.csdn.net/so/search?q=%E7%BA%BF%E6%80%A7%E4%BB%A3%E6%95%B0&spm=1001.2101.3001.7020)问题的起点，以下主要介绍通用创建矩阵的方式以及快速创建特殊矩阵的方式，且一部分主要对应于线性代数教材（同济大学版）的第一章和第二章，便于大家结合教材进行快速回顾。

### 1.1.1 创建矩阵语法

```
from sympy import *  A=Matrix([    [1,2,3],    [4,5,6],    [7,8,9]])a=Matrix([    [1],    [4],    [7]])b=Matrix([1,2,3])
```

### 1.1.2 创建特殊矩阵

| 特殊矩阵        | 语法 | 说明/示例 |
| --- | --- | --- |
| 单位矩阵 | eye(n) | 创建n×n单位矩阵，比如eye(3)，即对角线上全为1的对角矩阵 |
| 
0矩阵

 | zeros(shape) | 创建指定形状零矩阵，即矩阵内所有元素全为零，比如zeros(3,4)，即创建3行4列的零矩阵 |
| 1矩阵 | ones(shape) | 创建指定形状的1矩阵，即矩阵内所有元素全为1，比如ones(3,4) |
| 对角矩阵 | diag(1,2,3,4···) | 

创建对角矩阵，且对角线元素为1,2,3,4···

 |

```
from sympy import *eye(5) zeros(3,4)ones(3,4)diag(1,2,3)
```

## 1.2 矩阵运算

### 1.2.1 矩阵常规运算

| 矩阵运算         | 语法 | 说明/示例 |
| --- | --- | --- |
| 矩阵的加减 | A+B  A-B | 求矩阵A和B的和 |
| 矩阵数乘 | k\*A | 求矩阵A的数乘，即k乘A中的每个元素 |
| 矩阵乘 | A\*B    B\*A | 求矩阵A和B的乘，注意乘的顺序，因矩阵不一定满足交换律 |
| 矩阵的幂乘 | A\*\*(n) | 求矩阵的幂乘，此时矩阵需为方阵 |
| 矩阵的逆 | 
A.inv()

A\*\*(-1)

 | 求矩阵的逆，A需为方阵 |
| 矩阵的转置 | A.T | 求矩阵的转置 |
| 矩阵代数余子式 | A.cofactor(i,j) | 求矩阵内指定元素的代数余子式，返回一个数值 |
| 矩阵的伴随矩阵 | A.adjugate() | 求矩阵的伴随矩阵，伴随矩阵为矩阵每个元素的代数余子式组成 |
| 矩阵行列式 | A.det() | 求矩阵的行列式，为一个数值 |
| 矩阵的秩 | A.rank() | 返回矩阵A的秩 |

### 1.2.2 矩阵操作

| 矩阵操作 | 语法 | 说明/示例 |
| --- | --- | --- |
| 取矩阵形状 | A.shape | 返回一个元组，分别为行数和列数 |
| 取矩阵指定行 | 
A.row(i)

A.row(\[i1,i2,i3\])

 | 

取矩阵指定行，行号从0开始

取矩阵指定多行

 |
| 取矩阵指定列 | 

A.col(i)

A.col(\[j1,j2,j3\])

 | 

取矩阵指定列，列号从0开始

取矩阵指定多列

 |
| 插入行 | A.row\_insert(p,M) | 在A的指定位置插入指定列 |
| 插入列 | A.col\_insert(P,M) | 在A的指定位置插入指定行 |
| 删除行 | A.row\_del(i) | 删除A的指定行 |
| 删除列 | A.col\_del(j) | 删除A的指定列 |

### 1.2.3 矩阵初等变换

针对矩阵（不一定非得是方阵），一般线性代数教材都是从求解线性方程组引入，线性方程组与对应的系数矩阵及增广矩阵是一一对应的，则对矩阵的消元法即对其增广矩阵的初等变换，因为在使用消元法简化求解线性方程组时，一般是对行进行变换，故对应有以下几种对矩阵的初等变换

1.  对某行数乘k
2.  交换某两行位置
3.  对某行数乘k并加到另一行

而矩阵的乘运算，其本质即对应用对某矩阵的行或列进行初等变换，如A\*B，即使用A对B进行行变换，或使用B对A进行列变换

![A=\begin{pmatrix} 0 &1 &0 \\ 1&0 &0 \\ 0& 0 &1 \end{pmatrix}](https://latex.codecogs.com/gif.latex?A%3D%5Cbegin%7Bpmatrix%7D%200%20%261%20%260%20%5C%5C%201%260%20%260%20%5C%5C%200%26%200%20%261%20%5Cend%7Bpmatrix%7D)  ，![B=\begin{pmatrix} 3&4 &5 \\ 2& 7& 8\\ 8& 9& 11 \end{pmatrix}](https://latex.codecogs.com/gif.latex?B%3D%5Cbegin%7Bpmatrix%7D%203%264%20%265%20%5C%5C%202%26%207%26%208%5C%5C%208%26%209%26%2011%20%5Cend%7Bpmatrix%7D)

则，A即代表交换第一行与第二行位置，A\*B即交换B的第一行和第二行，且设C可逆，C总可以分解为多个如A的矩阵的乘，C\*B即对B实施一系列的行初等变换，列变换也同理。

以下为另外两种初等矩阵，对应两种初等变换

-   对某行数乘k，比如对第一行数乘k，对应初等矩阵为

![\begin{pmatrix} 3&0 &0 \\ 0& 1 & 0\\ 0&0 & 1 \end{pmatrix}](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bpmatrix%7D%203%260%20%260%20%5C%5C%200%26%201%20%26%200%5C%5C%200%260%20%26%201%20%5Cend%7Bpmatrix%7D)

-   对某行数乘k，并加到另一行，比如对第一行加上第三行的3倍，对应初等矩阵为

![\begin{pmatrix} 1 & 0& 3\\ 0& 1 & 0\\ 0 & 0 & 1 \end{pmatrix}](https://latex.codecogs.com/gif.latex?%5Cbegin%7Bpmatrix%7D%201%20%26%200%26%203%5C%5C%200%26%201%20%26%200%5C%5C%200%20%26%200%20%26%201%20%5Cend%7Bpmatrix%7D)

### 1.2.4 行阶梯、行最简

基于以上对矩阵的初等变换，在没有接触线性代数之前，进行消元法时，最终目标其实是将方程化简为每个方程只含一个未知变量的形式，即转化为简单代数问题，以上使用线性代数的语言，其实即在对增广矩阵进行变换，变为阶梯、最简形，以下即sympy的运算，可直接返回A的以上形式

| 矩阵操作 | 语法 | 说明/示例 |
| --- | --- | --- |
| 行阶梯型 | A.echelon\_form() | 返回矩阵A的行阶梯型，阶梯型即每一行的主元所在列的下方元素均为0 |
| 行最简形 | A.rref() | 返回矩阵A的行最简形，即每一行主元为1 |

![](https://img-blog.csdnimg.cn/f2cb096103ee46509e4f666769516d23.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

## 二、求解线性方程组

以下内容，基本对应于线性代数教材（同济大学版）的第三章，大家可结合教材回顾讨论

## 2.1 线性方程组可解及性质

本章节主要讨论线性方程组是否有解的讨论，主要结合行列式、矩阵的秩的概念。

### 2.1.1 秩

在教材中，一般是基于向量组线性相关性，来引入向量组的的秩，进而引入矩阵的秩，且因为矩阵其实可以看成是列向量组，所以本质上向量组的秩与其对应矩阵的秩本质是一样的。

1.  向量组的秩：即向量组极大线性无关组内向量的个数
2.  矩阵的秩：即最高阶非零子式对应的阶数
3.  因矩阵A与矩阵A的最简形具有相同的秩，且A可看成是由行或列向量构成，结合阶梯型的性质（每行的主元下面的元素全为零），可直观的得出A的向量组的秩和矩阵的秩相等

### 2.1.2 可解性的判断

设线性方程组的系数矩阵为A，增广矩阵为R，形状为(m,n)，则对应线性方程组解的情况，如下

| 解的情况 | 判断条件 | 说明/示例 |
| --- | --- | --- |
| 无解 | r(A)<r(R) | 即如果系数矩阵A的秩小于增广矩阵R的秩，则无解，不管是齐次还是非齐次线性方程 |
| 唯一解 | r(A)=r(R)=n | 
即系数矩阵A与增广矩阵R的秩相等，且等于未知数个数n

如果是其次线性方程组，唯一解为0解

 |
| 无穷多解 | r(A)=r(R)<n | 即系数矩阵A与增广矩阵R的秩相等，且小于未知数个数n |

## 2.2 矩阵的行空间、列空间、零空间

 在此处先引入向量空间的概念，再基于向量空间，讨论线性方程组解的构成

向量空间即对向量加和数乘封闭的空间，以下主要基于齐次线性方程组的系数矩阵A，讨论该矩阵本身独特的三种向量空间

其中，行/列空间基的个数即A的秩r，零空间或解空间基的个数即n-r。

| 矩阵空间 | 对应代码 | 说明/示例 |
| --- | --- | --- |
| 矩阵的行空间 | A.rowspace() | 返回矩阵的行向量组的最大线性无关向量组 |
| 矩阵的列空间 | A.columnspace() | 返回矩阵的列向量组的最大线性无关向量组 |
| 矩阵的零空间/解空间 | A.nullspace() | 返回矩阵A 为系数矩阵的其次线性方程组对应的解空间的基，由这些基向量可构成通解 |

![](https://img-blog.csdnimg.cn/ea95a1107e8f4775bf0258c560a740f7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

## 2.3 线性方程组解的构成

### 2.3.1 齐次线性方程组

此处只讨论非零解的情况，即有无穷解，求基础解系（即解空间的基），解空间的基的线性组合即基础解系

如系数矩阵A对应的其次线性方程组

![](https://img-blog.csdnimg.cn/48f9130075634eefa0c26686dde162de.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

 则，A的基础解系即：

![c1\begin{pmatrix} -1\\ 1\\ 0\\ 0\\0 \end{pmatrix}+c2\begin{pmatrix} -2\\ 0\\ 2\\ 1\\0 \end{pmatrix}+c3\begin{pmatrix} 1\\ 0\\ 1\\ 0\\1 \end{pmatrix}](https://latex.codecogs.com/gif.latex?c1%5Cbegin%7Bpmatrix%7D%20-1%5C%5C%201%5C%5C%200%5C%5C%200%5C%5C0%20%5Cend%7Bpmatrix%7D&plus;c2%5Cbegin%7Bpmatrix%7D%20-2%5C%5C%200%5C%5C%202%5C%5C%201%5C%5C0%20%5Cend%7Bpmatrix%7D&plus;c3%5Cbegin%7Bpmatrix%7D%201%5C%5C%200%5C%5C%201%5C%5C%200%5C%5C1%20%5Cend%7Bpmatrix%7D)

### 2.3.2 非齐次线性方程组

非齐次线性方程组通解，即对应齐次线性方程组的基础解系+特解

齐次线性方程组的基础解析，按照2.3.1直接求出即可，特解可先将增广矩阵变为行最简形，然后设非主元对应的未知数为0，即可直接读出

设A为非齐次线性方程组的系数矩阵，R为对应增广矩阵，则

![](https://img-blog.csdnimg.cn/75ebe1154ac148d2b817a7ab401272e2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

 最终，得出该非齐次线性方程组的通解如下

![c1\begin{pmatrix}-3\\ -1\\ 1\\ 0\end{pmatrix}+c2\begin{pmatrix}0\\ -2\\ 0\\ 1\end{pmatrix}+\begin{pmatrix}0\\ -1\\ 0\\ 0\end{pmatrix}](https://latex.codecogs.com/gif.latex?c1%5Cbegin%7Bpmatrix%7D-3%5C%5C%20-1%5C%5C%201%5C%5C%200%5Cend%7Bpmatrix%7D&plus;c2%5Cbegin%7Bpmatrix%7D0%5C%5C%20-2%5C%5C%200%5C%5C%201%5Cend%7Bpmatrix%7D&plus;%5Cbegin%7Bpmatrix%7D0%5C%5C%20-1%5C%5C%200%5C%5C%200%5Cend%7Bpmatrix%7D)

## 三、矩阵特征向量与特征值

本章节主要对应于线性代数教材（同济大学版）的第四章（相似矩阵及二次型）

## 3.1 向量运算

| 向量运算 | 语法 | 说明/示例 |
| --- | --- | --- |
| 向量点积 | a.dot(b) | 返回向量a和向量b的点击，等价于a.T\*b |
| 向量叉积 | a.cross(b) | 返回向量的叉积，得出的是与A、B均正交的一个向量 |
| 向量的模/长度 | a.norm() | 返回向量的模或长度 |
| 向量单位化 | a.normalized() | 返回向量a的单位向量（即除以其长度） |
| 向量投影 | a.project(b) | 返回向量a在b的投影向量 |

 以下作图简单说明下向量的投影、点积之间的关系：![](https://img-blog.csdnimg.cn/d0efe12031574eaeb6ef3fdd2c6bc696.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

 AB为向量![\alpha](https://latex.codecogs.com/gif.latex?%5Calpha),AC为向量![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)，![e](https://latex.codecogs.com/gif.latex?e)为垂直与AC的线段，![\alpha](https://latex.codecogs.com/gif.latex?%5Calpha)在![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)的投影向量为p,设p=k![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)则

 ![p+e=\alpha](https://latex.codecogs.com/gif.latex?p&plus;e%3D%5Calpha)

e.dot(![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta))=0，即(![\alpha](https://latex.codecogs.com/gif.latex?%5Calpha)\-p).dot(![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta))=0，即(![\alpha](https://latex.codecogs.com/gif.latex?%5Calpha)\-k![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)).dot(![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta))=0，即可求出k，进而求出p，如下：

![\beta ^T*(\alpha -k\beta )=0](https://latex.codecogs.com/gif.latex?%5Cbeta%20%5ET*%28%5Calpha%20-k%5Cbeta%20%29%3D0)，得出：

![k=\frac{\beta ^T\alpha }{\beta ^T\beta }](https://latex.codecogs.com/gif.latex?k%3D%5Cfrac%7B%5Cbeta%20%5ET%5Calpha%20%7D%7B%5Cbeta%20%5ET%5Cbeta%20%7D)

则![\alpha](https://latex.codecogs.com/gif.latex?%5Calpha)在![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)的投影向量p=k![\beta](https://latex.codecogs.com/gif.latex?%5Cbeta)\=![\frac{\beta ^T\alpha }{\beta ^T\beta }\beta](https://latex.codecogs.com/gif.latex?%5Cfrac%7B%5Cbeta%20%5ET%5Calpha%20%7D%7B%5Cbeta%20%5ET%5Cbeta%20%7D%5Cbeta)\=![\frac{[\beta ,\alpha ] }{[\beta ,\beta ] }\beta](https://latex.codecogs.com/gif.latex?%5Cfrac%7B%5B%5Cbeta%20%2C%5Calpha%20%5D%20%7D%7B%5B%5Cbeta%20%2C%5Cbeta%20%5D%20%7D%5Cbeta)

以上即向量的投影公式，之所以单独说明投影，是因为其实在进行向量正交化，理解施密特向量正交化公式的关键。

## 3.2 向量正交化

向量正交化，即将给定的线性无关向量组进行正交化，进而进行单位标准化，因为正交化后，会有相对比较好的性能，比如对由正交的向量组组成的矩阵，![A^T*A=E](https://latex.codecogs.com/gif.latex?A%5ET*A%3DE)

| 向量操作 | 语法 | 说明/示例 |
| --- | --- | --- |
| 向量组正交化 | GramSchmidt(\[A.col(i)\]) | 返回正交化后的向量组，如果第二个参数传入True，则返回的是规范正交化的向量组 |

以上即使用施密特正交化方法进行计算，而施密特正交化，本质则是使用了投影算法，如下

设待正交化的线性无关向量组：![\alpha 1,\alpha 2,\alpha 3,\cdots ,\alpha r](https://latex.codecogs.com/gif.latex?%5Calpha%201%2C%5Calpha%202%2C%5Calpha%203%2C%5Ccdots%20%2C%5Calpha%20r)

![\beta 1=\alpha 1](https://latex.codecogs.com/gif.latex?%5Cbeta%201%3D%5Calpha%201)

![\beta 2=\alpha 2-\frac{[\beta 1,\alpha 2]}{\beta 1,\beta 1}\beta 1](https://latex.codecogs.com/gif.latex?%5Cbeta%202%3D%5Calpha%202-%5Cfrac%7B%5B%5Cbeta%201%2C%5Calpha%202%5D%7D%7B%5Cbeta%201%2C%5Cbeta%201%7D%5Cbeta%201)

![\beta 3=\alpha 3-\frac{[\beta 1,\alpha 3]}{[\beta 1,\beta 1]}\beta 1-\frac{[\beta 2,\alpha 3]}{[\beta 2,\beta 2]}\beta 2](https://latex.codecogs.com/gif.latex?%5Cbeta%203%3D%5Calpha%203-%5Cfrac%7B%5B%5Cbeta%201%2C%5Calpha%203%5D%7D%7B%5B%5Cbeta%201%2C%5Cbeta%201%5D%7D%5Cbeta%201-%5Cfrac%7B%5B%5Cbeta%202%2C%5Calpha%203%5D%7D%7B%5B%5Cbeta%202%2C%5Cbeta%202%5D%7D%5Cbeta%202)

可以发现，针对![\beta 2](https://latex.codecogs.com/gif.latex?%5Cbeta%202)，其即![\alpha 2](https://latex.codecogs.com/gif.latex?%5Calpha%202)减去![\alpha 2](https://latex.codecogs.com/gif.latex?%5Calpha%202)在![\beta 1](https://latex.codecogs.com/gif.latex?%5Cbeta%201)的投影，对应于3.1中的e，而e肯定与![\alpha 1](https://latex.codecogs.com/gif.latex?%5Calpha%201)垂直，同理，![\beta 3](https://latex.codecogs.com/gif.latex?%5Cbeta%203)即![\alpha 3](https://latex.codecogs.com/gif.latex?%5Calpha%203)减去其在![\beta 1](https://latex.codecogs.com/gif.latex?%5Cbeta%201)及![\beta 2](https://latex.codecogs.com/gif.latex?%5Cbeta%202)的投影，即在三维中，垂直于由![\beta 1](https://latex.codecogs.com/gif.latex?%5Cbeta%201)和![\beta 2](https://latex.codecogs.com/gif.latex?%5Cbeta%202)组成的平面的向量，其他的以此类推 。

如上，因为同济大学版本的线性代数教材，并未在此处说的很明白，导致看的云里雾里，其实这个公式如果知道了向量投影的概念，自己即可以推导，无需死记硬背。

## 3.3 特征向量与特征值

方阵的特征值和特征向量，在较多领域有很多用处，比如3.4中进行相似对角化，然后简化矩阵的幂乘运算等。

| 运算         | 语法 | 说明/示例 |
| --- | --- | --- |
| 矩阵的特征值 | A.eigenvals() | 求矩阵A对应的特征值，满足Aa=λa |
| 矩阵特征值和特征向量 | A.eigenvects() | 返回矩阵A的特征值及对应的特征向量，使得Aa=λa |

![](https://img-blog.csdnimg.cn/5b6924b0679b4120b5dfaa4939bb0ffb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

## 3.4 矩阵相似及相似对角化

矩阵相似的定义为：

![P^{-1}AP=B](https://latex.codecogs.com/gif.latex?P%5E%7B-1%7DAP%3DB)

则A与B相似，尤其，当B为对角矩阵时，则此时如下：

![A=P\Lambda P^{-1}](https://latex.codecogs.com/gif.latex?A%3DP%5CLambda%20P%5E%7B-1%7D)

此时，根据特征值和特征向量公式，可知P为A的特征向量组成的矩阵，![\Lambda](https://latex.codecogs.com/gif.latex?%5CLambda)为对应特征向量组成的对角矩阵，以上公式即相似对角化公式。

特别的，当A为实对阵矩阵时，因![A^{T}=A](https://latex.codecogs.com/gif.latex?A%5E%7BT%7D%3DA)，所以 ![P\Lambda P^{-1}=(P^{-1})^{T}\Lambda P^{T}](https://latex.codecogs.com/gif.latex?P%5CLambda%20P%5E%7B-1%7D%3D%28P%5E%7B-1%7D%29%5E%7BT%7D%5CLambda%20P%5E%7BT%7D)，即![P^{T}=P^{-1}](https://latex.codecogs.com/gif.latex?P%5E%7BT%7D%3DP%5E%7B-1%7D)，所以P需为正交矩阵。至于求正交阵P，使得A可相似对角化，可参照教材中的步骤完成即可。

以下为sympy求解语法

 以下，返回的第一个矩阵即P，第二个矩阵即对角矩阵![\Lambda](https://latex.codecogs.com/gif.latex?%5CLambda)

![](https://img-blog.csdnimg.cn/9e9ddbb3ccef48148da412cb411c416a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

## 3.5 矩阵的合同

矩阵的合同是在二次型及其标准型章节引入的，主要是为了对任意二次型化为标准型，可以认为是线性代数在求解或化简二次型的应用

如果存在C，使得![B=C^{T}AC](https://latex.codecogs.com/gif.latex?B%3DC%5E%7BT%7DAC)，则认为B与A合同，特别的，当A为对称矩阵时，由3.4中可知，总存在正交阵P，使得![P^{T}AP=\Lambda](https://latex.codecogs.com/gif.latex?P%5E%7BT%7DAP%3D%5CLambda)，A为二次型的系数对称阵，![\Lambda](https://latex.codecogs.com/gif.latex?%5CLambda)即对应的标准型，此时，![x=Cy](https://latex.codecogs.com/gif.latex?x%3DCy)

## 四、线性代数总结

以下为对本文章介绍的整个现象代数归类为几个公式，或者对矩阵的分解公式

## 4.1 矩阵的LU分解

根据矩阵的初等变换及消元法，因对任意矩阵，均可以用有限次的行初等变换，将A等价（两个矩阵等价即其秩和解相同）变换为行阶梯型，再对该行阶梯型用有限次的列变换，变为对角型。

我们约定

1.  在进行行变换时，总是从第一行开始，第二行用第一行将第一个元素变为0，第三行一次类推，则该组合的初等变换对应于一个下三角矩阵，且可逆，最终得到一个行阶梯型
2.  同样的，在对得到的行阶梯型进行列变换时，也是从第一列开始，依次类推，最终变为对角型，且该组合的初等列变换对应于一个上三角矩阵，且可逆。

故有![A=LU](https://latex.codecogs.com/gif.latex?A%3DLU)或![A=L\Lambda U](https://latex.codecogs.com/gif.latex?A%3DL%5CLambda%20U)

以上即对矩阵A的LU分解的理解，sympy的语法如下：

| 矩阵操作 | 语法 | 说明/示例 |
| --- | --- | --- |
| 矩阵LU分解 | l,u,p = A.LUdecomposition() | 将矩阵A分解为下三角矩阵L和上三角矩阵U |

 ![](https://img-blog.csdnimg.cn/f9ed04de4a824bd7bf595460e90e8533.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiA56eL6Zey6LCI,size_20,color_FFFFFF,t_70,g_se,x_16)

## 4.2 矩阵的相似对角化分解

矩阵的特征值和特征向量，本质都在为矩阵的相似对角化做准备，此时

![A=P\Lambda P^{-1}](https://latex.codecogs.com/gif.latex?A%3DP%5CLambda%20P%5E%7B-1%7D)

将A相似对角化或者相似对角分解后，对求A的高次幂很有帮助，此时

![A^{n}=P\Lambda ^{n}P^{-1}](https://latex.codecogs.com/gif.latex?A%5E%7Bn%7D%3DP%5CLambda%20%5E%7Bn%7DP%5E%7B-1%7D)

## 4.3 矩阵的合同对角化分解

矩阵的合同对角化，本质是基于对矩阵的相似对角化的分解，且A如果为实对称矩阵，得出的分解，性能更加优良

![A=P\Lambda P^{-1}](https://latex.codecogs.com/gif.latex?A%3DP%5CLambda%20P%5E%7B-1%7D)，因P为正交阵，故![A=P\Lambda P^{T}](https://latex.codecogs.com/gif.latex?A%3DP%5CLambda%20P%5E%7BT%7D)，此时无需求矩阵的逆，即可进行矩阵分解

以上，线性代数的基础知识已经讲解的差不多了，但是线性代数的实际应用场景和技巧非常多，如果想更好的掌握线性代数，仍需在实际问题中大量练习和运用，且如果感兴趣，还可以持续关注线性代数领域新的研究成果和进展，当然，对于应付大学考试等，截至目前的知识已经足够了。