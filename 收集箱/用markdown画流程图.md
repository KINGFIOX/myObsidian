**使用工具：** [Typora](https://so.csdn.net/so/search?q=Typora&spm=1001.2101.3001.7020) 轻量级markdown工具

**使用方法：** 添加代码，选择[flow](https://so.csdn.net/so/search?q=flow&spm=1001.2101.3001.7020)语言  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021061015142330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Fkb3JhYmxlXw==,size_16,color_FFFFFF,t_70)  
**基本语法：**

MarkDown中的流程图语法叫flow，该语法只有两个注意事项：定义元素和连接定义好的元素

1.  模块定义：  
    模块名称=>类型: 显示的内容\[:>超链接URL\]  
    注意：=>后不能有空格， :后需要有空格
2.  连接语法：  
    模块名称1->模块连接2  
    模块名称1(箭头方向)->模块连接2  
    模块之间使用->符号连接  
    箭头方向包括：left、right、top、bottom、yes、no (模块射出箭头的方向)
3.  类型：

```
start:开始
end:结束
operation:操作
condition:条件
inputoutput:输入输出
subroutine :子程序
```

**实例（闰年为例）：**

```
开始=>start: 开始
结束=>end: 结束
输入=>inputoutput: 输入年份n
条件1=>condition: n能否被4整除？
条件2=>condition: n能被100整除？
条件3=>condition: n能被400整除？
输出1=>inputoutput: 输出闰年
输出2=>inputoutput: 输出非闰年

开始->输入->条件1(yes)->条件2(yes)->条件3(yes)->输出1->结束
条件1(no)->输出2->结束
条件2(no,left)->输出1
条件3(no)->输出2
```

结果：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210610152022836.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Fkb3JhYmxlXw==,size_16,color_FFFFFF,t_70)