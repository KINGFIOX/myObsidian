(部分摘自《标准C++输入输出流与本地化》)

1\. 状态标志.

    1). 每个流对象都维护一个状态变量标记流状态(成功或失败),该变量类型是iostate(实际上是ios\_base定义的位域类型),状态变量的不同[二进制](https://so.csdn.net/so/search?q=%E4%BA%8C%E8%BF%9B%E5%88%B6&spm=1001.2101.3001.7020)位用来标记不同状态,共有三个状态标志:

<table><tbody><tr><td><p>状态标志</p></td><td><p>作用</p></td><td><p>所占bit</p></td></tr><tr><td><p>failbit</p></td><td><p>出现可挽回的非致命错误</p></td><td><p>1</p></td></tr><tr><td><p>badbit</p></td><td><p>出现不可挽回的致命错误</p></td><td><p>2</p></td></tr><tr><td><p>eofbit</p></td><td><p>到达文件尾</p></td><td><p>3</p></td></tr><tr><td><p>goodbit</p></td><td><p>没有错误</p></td><td><p>不占bit</p></td></tr></tbody></table>

    2). 函数

<table><tbody><tr><td><p>函数</p></td><td><p>作用</p></td></tr><tr><td><p>bool fail()</p></td><td><p>若failbit位被置为1,返回true</p></td></tr><tr><td><p>bool bad()&nbsp;</p></td><td><p>若badbit位被置为1,返回true &nbsp;</p></td></tr><tr><td><p>bool eof()</p></td><td><p>若eofbit位被置为1,返回true</p></td></tr><tr><td><p>bool good()</p></td><td><p>若failbit,badbit,eofbit位全为0,返回true</p></td></tr><tr><td><p>iostate rdstate() const</p></td><td><p>获取当前流状态变量</p></td></tr><tr><td><p>void clear (iostate state = goodbit)</p></td><td><p>重设流状态为state,并覆盖原有状态,默认为ios:goodbit</p></td></tr><tr><td><p>void setstate (iostate state)</p></td><td><p>在原有状态基础上添加state状态,其实是clear(rdstate()|state)</p></td></tr></tbody></table>

2\. 格式标志.

    1). 除状态变量外,每一个流对象还维护一个fmtflags类型(与iostate类似)的格式变量,用来标记输入输出格式,格式标志有:

<table><tbody><tr><td><p>位组</p></td><td><p>格式标志</p></td><td><p>作用</p></td><td><p>默认值</p></td><td><p>所占bit</p></td></tr><tr><td></td><td><p>skipws</p></td><td><p>使用输入操作符时跳过空白字符</p></td><td><p>设置</p></td><td><p>1</p></td></tr><tr><td></td><td><p>unitbuf</p></td><td><p>每次操作后刷新缓冲区</p></td><td><p>Cerr设置,其他对象不设置</p></td><td><p>2</p></td></tr><tr><td></td><td><p>uppercase</p></td><td><p>字母采用大写</p></td><td><p>不设置</p></td><td><p>3</p></td></tr><tr><td></td><td><p>showbase</p></td><td><p>输出整数时加上进制前缀</p></td><td><p>未设置</p></td><td><p>4</p></td></tr><tr><td></td><td><p>showpoint</p></td><td><p>按精度输出浮点数（不够补0）</p></td><td><p>未设置</p></td><td><p>5</p></td></tr><tr><td></td><td><p>showpos</p></td><td><p>输出非负数时加‘+’</p></td><td><p>未设置</p></td><td><p>6</p></td></tr><tr><td rowspan="3"><p>adjustfield</p></td><td><p>left</p></td><td><p>加入指定字符使输出左对齐</p></td><td rowspan="3"><p>right</p></td><td><p>7</p></td></tr><tr><td><p>right</p></td><td><p>加入指定字符使输出右对齐</p></td><td><p>8</p></td></tr><tr><td><p>Internal</p></td><td><p>在符号和数值中间插入指定字符</p></td><td><p>9</p></td></tr><tr><td rowspan="3"><p>basefield</p></td><td><p>dec</p></td><td><p>10进制输入/输出</p></td><td rowspan="3"><p>dec</p></td><td><p>10</p></td></tr><tr><td><p>oct</p></td><td><p>8进制输入/输出</p></td><td><p>11</p></td></tr><tr><td><p>hex</p></td><td><p>16进制输入/输出</p></td><td><p>12</p></td></tr><tr><td rowspan="2"><p>floatfield</p></td><td><p>scientific</p></td><td><p>浮点数按科学计数法输出</p></td><td rowspan="2"><p>无,由浮点数量级决定</p></td><td><p>13</p></td></tr><tr><td><p>fixed</p></td><td><p>浮点数按小数输出</p></td><td><p>14</p></td></tr><tr><td></td><td><p>boolalpha</p></td><td><p>以字母格式输入和输出布尔值</p></td><td><p>未设置</p></td><td><p>15</p></td></tr></tbody></table>

     (位组:有些标志位相互排斥,相互排斥的标记位属于同一位组,以便于对同一位组的标记位复位)

     2). 函数

<table><tbody><tr><td><p>fmtflags flags() const</p></td><td><p>获取当前格式变量</p></td></tr><tr><td><p>fmtflags flags (fmtflags fmtfl)</p></td><td><p>将格式变量设为fmtfl,并返回之前的格式变量</p></td></tr><tr><td><p>fmtflags setf (fmtflags fmtfl)</p></td><td><p>在原来格式基础上附加fmtfl,相当于flags(fmtfl|flags()).</p></td></tr><tr><td><p>fmtflags setf (fmtflags fmtfl, fmtflags mask);</p></td><td><p>将位组mask所包含的标记位清零,并置为fmtfl对应标记位的值,相当于flags((fmtfl&amp;mask)|(flags()&amp;~mask))</p></td></tr><tr><td><p>void unsetf (fmtflags mask)</p></td><td><p>清除位组mask的格式标记位</p></td></tr><tr><td><p>streamsize width() const;</p></td><td><p>获取当前域宽度</p></td></tr><tr><td><p>streamsize width (streamsize wide);</p></td><td><p>设置域宽度为wide</p></td></tr><tr><td><p>streamsize precision() const;</p></td><td><p>获取当前精度</p></td></tr><tr><td><p>streamsize precision (streamsize prec);</p></td><td><p>设置精度为prec</p></td></tr><tr><td><p>char fill() const;</p></td><td><p>获取当前的填充字符</p></td></tr><tr><td><p>char fill (char fillch)</p></td><td><p>设置输出宽度不满域宽时的填充字符为fillch,默认为空格</p></td></tr></tbody></table>

     注:在使用setf设置属于某一位组的标记位时,需要提供位组作为第二个参数来将其他互斥的标记位复位,否则可能会出现设置无效的现象.

    3). 使用2)的函数可以设置输入输出的格式,标准库还提供了"操纵符"(类似于函数)使得格式化输入输出更加方便,将操纵符作为<<和>>的右操作数使用可以直接设置输入输出格式,使用这些操纵符需要加头文件<iomanip>操纵符有:

<table><tbody><tr><td><p>操纵符</p></td><td><p>影响</p></td><td><p>用途</p></td><td><p>等价表示</p></td></tr><tr><td><p>flush</p></td><td><p>O</p></td><td><p>刷新流缓冲区</p></td><td><p>o.flush()</p></td></tr><tr><td><p>endl</p></td><td><p>O</p></td><td><p>插入换行符并刷新缓冲区</p></td><td><p>o.put(widen(‘\n’));</p><p>o.flush();</p></td></tr><tr><td><p>ends</p></td><td><p>O</p></td><td><p>插入串结束符</p></td><td><p>o.out(o.widen((‘\0’));</p></td></tr><tr><td><p>ws</p></td><td><p>I</p></td><td><p>抽取空白字符</p></td><td></td></tr><tr><td><p>boolalpha</p></td><td><p>I/O</p></td><td><p>设置bool型的输入输出格式</p></td><td><p>io.setf(ios_base::boolalpha)</p></td></tr><tr><td><p>noboolalpha</p></td><td><p>I/O</p></td><td><p>复位上述标志</p></td><td><p>io.unsetf(ios_base::boolalpha)</p></td></tr><tr><td><p>showbase</p></td><td><p>O</p></td><td><p>设置用于整数前缀的标志</p></td><td><p>o.setf(ios_base::showbase)</p></td></tr><tr><td><p>noshowbase</p></td><td><p>O</p></td><td><p>复位上述标志</p></td><td><p>o.unsetf(ios_base::showbase)</p></td></tr><tr><td><p>showpos</p></td><td><p>O</p></td><td><p>为非负整数设置产生’+’的标志</p></td><td><p>o.setf(ios_base::showpos)</p></td></tr><tr><td><p>noshowpos</p></td><td><p>O</p></td><td><p>复位上述标志</p></td><td><p>o.unsetf(ios_base::showpos)</p></td></tr><tr><td><p>showpoint</p></td><td><p>O</p></td><td><p>按精度输出浮点数（不够补0）</p></td><td><p>o.setf(ios_base::showpoint)</p></td></tr><tr><td><p>noshowpoint</p></td><td><p>O</p></td><td><p>复位上述标志</p></td><td><p>o.unsetf(ios_base::showpoint)</p></td></tr><tr><td><p>skipws</p></td><td><p>I</p></td><td><p>设置跳过空白字符的标志</p></td><td><p>i.setf(ios_base::skipws)</p></td></tr><tr><td><p>noskipws</p></td><td><p>I</p></td><td><p>复位上述标志</p></td><td><p>i.unsetf(ios_base::skipws)</p></td></tr><tr><td><p>uppercase</p></td><td><p>O</p></td><td><p>大写输出字母</p></td><td><p>o.setf(ios_base::uppercase)</p></td></tr><tr><td><p>nouppercase</p></td><td><p>O</p></td><td><p>复位上述标志</p></td><td><p>o.unsetf(ios_base::uppercase)</p></td></tr><tr><td><p>unitbuf</p></td><td><p>O</p></td><td><p>设置每次格式化输出后刷新输出的标志</p></td><td><p>o.setf(ios_base::unitbuf)</p></td></tr><tr><td><p>nounitbuf</p></td><td><p>O</p></td><td><p>复位上述标志</p></td><td><p>o.unsetf(ios_base::unitbuf)</p></td></tr><tr><td><p>internal</p></td><td><p>O</p></td><td><p>设置在指定内部节点填充字符的标志</p></td><td><p>o.setf(ios_base::interbal,ios_base::adjustfield)</p></td></tr><tr><td><p>left</p></td><td><p>O</p></td><td><p>设置左对齐标志</p></td><td><p>o.setf(ios_base::left,ios_base::adjustfield)</p></td></tr><tr><td><p>right</p></td><td><p>O</p></td><td><p>设置右对齐标志</p></td><td><p>o.setf(ios_base::right,ios_base::adjustfield)</p></td></tr><tr><td><p>dec</p></td><td><p>I/O</p></td><td><p>按十进制转换整数</p></td><td><p>io.setf(ios_base::dec,ios_base::basefield)</p></td></tr><tr><td><p>hex</p></td><td><p>I/O</p></td><td><p>按十六进制转换整数</p></td><td><p>io.setf(ios_base::hex,ios_base::basefield)</p></td></tr><tr><td><p>oct</p></td><td><p>I/O</p></td><td><p>按八进制转换整数</p></td><td><p>io.setf(ios_base::oct,ios_base::basefield)</p></td></tr><tr><td><p>fixed</p></td><td><p>O</p></td><td><p>设置浮点数按定点表示标志</p></td><td><p>o.setf(ios_base::fixed,ios_base::floatfield)</p></td></tr><tr><td><p>scientific</p></td><td><p>O</p></td><td><p>设置浮点数按科学表示法表示标志</p></td><td><p>o.setf(ios_base::scientific,ios_base::floatfield)</p></td></tr><tr><td><p>setiosflags(ios_base::fmtflags mask)</p></td><td><p>I/O</p></td><td><p>根据mask设置格式标志</p></td><td><p>io.setf(ios_base::fmtflags mask)</p></td></tr><tr><td><p>setiosflags(ios_base::fmtflags mask)</p></td><td><p>I/O</p></td><td><p>根据mask清除格式标志</p></td><td><p>io.unsetf(ios_base::fmtflags mask)</p></td></tr><tr><td><p>setbase(int base)</p></td><td><p>I/O</p></td><td><p>为整数表示设置基数</p></td><td><p>io.setf(base==8?ios_base::oct:base==10?ios_base::dec:base==16?ios_base::hex:ios_base::fmtflags(0),ios_base::basefield)</p></td></tr><tr><td><p>setfill(char c)</p></td><td><p>I/O</p></td><td><p>设置填充字符</p></td><td><p>io.fill(c)</p></td></tr><tr><td><p>setprecision(int n)</p></td><td><p>O</p></td><td><p>设置精度</p></td><td><p>io.precision(n)</p></td></tr><tr><td><p>setw(int n)</p></td><td><p>I/O</p></td><td><p>设置最小字宽</p></td><td><p>io.width(n)</p></td></tr></tbody></table>

     4). 正如以上所展示的,操纵符分为带参数和不带参数的,不带参数的操纵符本质上是具有特定参数和返回值类型的函数指针,带参数的操纵符本质上是重载了移位运算符的对象.

    自定义不带参数的操纵符,可以像这样:

![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) ![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
ios_base& boolalpha(ios_base& strm){
    strm.setf(ios_base::boolaplha);
    return strm;
}
//ios_base是所有io流类的基类
```

View Code

    自定义带参数的操纵符,可以像这样:

![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) ![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
class width{
public:
    explicit width(unsigned int i):i_(0){}
private:
    unsigned int i_;
    template<class charT,class Traits>
    friend base_ostream<char T,Traits>&   operator<<(basic_ostream<charT,Traits>&ib,const width&w){
        ib.width(w.i_);
        return ib;
}
```

View Code

    关于自定义操纵符的具体细节,详见《标准C++输入输出流与本地化》3.2

3\. 文件打开方式标志

    1). 使用文件流类型时,可以在构造函数或open函数中指定打开方式,参数是ios\_base::openmode类型(和iostate和fmtflags类似),以下是用于指定文件打开方式的标记位:

<table><tbody><tr><td><p>名称</p></td><td><p>作用</p></td><td><p>所占bit</p></td></tr><tr><td><p>app</p></td><td><p>向文件尾增加数据</p></td><td><p>1</p></td></tr><tr><td><p>ate</p></td><td><p>从文件尾部开始</p></td><td><p>2</p></td></tr><tr><td><p>binary</p></td><td><p>以二进制方式读写</p></td><td><p>3</p></td></tr><tr><td><p>in</p></td><td><p>以读方式打开文件</p></td><td><p>4</p></td></tr><tr><td><p>out</p></td><td><p>以写方式打开文件</p></td><td><p>5</p></td></tr><tr><td><p>trunc</p></td><td><p>清除文件内容（默认标志）</p></td><td><p>6</p></td></tr></tbody></table>

    2). 可以使用|同时按多个模式打开文件,但并不是所有的打开方式都可以组合,下表是合法的组合:

<table><tbody><tr><td><p>打开方式</p></td><td><p>作用</p></td><td><p>+ate标志</p></td><td><p>+binary标志</p></td></tr><tr><td><p>in</p><p>in|trunc</p></td><td><p>只读方式打开文件</p><p>只读方式打开文件设文件长度为0</p></td><td><p>初始文件位置指针在文件尾部</p></td><td><p>抑制转换</p></td></tr><tr><td><p>out</p><p>out|trunc</p></td><td><p>只写方式打开文件</p><p>只写方式打开文件并设文件长度为0</p></td><td><p>初始文件位置指针在文件尾部</p></td><td><p>抑制转换</p></td></tr><tr><td><p>out|app</p></td><td><p>追加,尽在文件尾写入</p></td><td><p>初始文件位置指针在文件尾部</p></td><td><p>抑制转换</p></td></tr><tr><td><p>in|out</p></td><td><p>读写均可</p></td><td><p>初始文件位置指针在文件尾部</p></td><td><p>抑制转换</p></td></tr><tr><td><p>int|out|trunc</p></td><td><p>读写均可并设文件长度为0</p></td><td><p>初始文件位置指针在文件尾部</p></td><td><p>抑制转换</p></td></tr></tbody></table>

    3). 其他注意事项

        1''. 输出文件流默认打开方式为ios\_base::out|ios\_base::trunc,输入文件流默认打开方式为ios\_base::in|ios\_base::trunc,也就是说,对于单项文件流trunc标志位是默认设置的,但双向文件流默认打开方式为ios\_base:in|ios\_base::out,默认不设置trunc标志位

        2''. ate(ate-end)和app,都是将文件指针定位在文件尾,不同之处在于,ate模式允许修改文件指针位置,app模式强制将指针定位到文件尾,即使修改指针位置也无效.

转载于:https://www.cnblogs.com/reasno/p/4875656.html