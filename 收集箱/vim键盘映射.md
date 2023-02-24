## **设置键盘映射**

使用:map命令，可以将键盘上的某个按键与Vim的命令绑定起来。例如使用以下命令，可以通过F5键将单词用花括号括起来：

其中：i{将插入字符{，然后使用Esc退回到命令状态；接着用e移到单词结尾，a}增加字符}，最后退至命令状态。在执行以上命令之后，光标定位在一个单词上（例如amount），按下F5键，这时字符就会变成{amount}的形式。

## **不同模式下的键盘映射**

使用下表中不同形式的map命令，可以针对特定的模式设置键盘映射：

![](https://pic1.zhimg.com/v2-53b70b061cc2f5fa4c496ac15fef89b4_b.png)

Operator-pending模式，是指当你输入操作符（比如d）时，然后继续输入的移动步长和文本对象（dw）的状态。

第一列命令定义的映射，仍然可以被重新映射；第二列命令（包含noremap）定义的映射，是不可以被重新映射的。

## **键盘映射实例**

使用以下命令，可以在Normal Mode和Visual/Select Mode下，利用Tab键和Shift-Tab键来缩进文本：

```
nmap <tab> V>
nmap <s-tab> V<
vmap <tab> >gv
vmap <s-tab> <gv
```

使用以下命令，指定F10键来新建标签页：

```
:map <F10> <Esc>:tabnew<CR>
```

其中：<Esc>代表Escape键；<CR>代表Enter键；而功能键则用<F10>表示。首先进入命令行模式，然后执行新建标签页的:tabnew命令，最后返回常规模式。

同理：对于组合键，可以用<C-Esc>代表Ctrl-Esc；使用<S-F1>表示Shift-F1。对于Mac用户，可以使用<D>代表Command键。

注意：Alt键可以使用<M-key>或<A-key>来表示。

关于键盘符号的详细说明，请使用:h key-notation命令查看帮助信息。

## **查看键盘映射**

![](https://pic4.zhimg.com/v2-2da7f3a2f14bf0898f2bc8905176e84b_b.png)

使用:map命令，可以列出所有键盘映射。其中第一列标明了映射在哪种模式下工作：

标记模式<space>常规模式，可视化模式，运算符模式n常规模式v可视化模式o运算符模式!插入模式，命令行模式i插入模式c命令模式

使用下表中不同形式的map命令（不带任何参数），可以列出针对特定模式设置的键盘映射：

## **取消键盘映射**

如果想要取消一个映射，可以使用以下命令：

注意：必须为:unmap命令指定一个参数。如果未指定任何参数，那么系统将会报错，而不会取消所有的键盘映射。

针对不同模式下的键盘映射，需要使用与其相对应的unmap命令。例如：使用:iunmap命令，取消插入模式下的键盘映射；而取消常规模式下的键盘映射，则需要使用:nunmap命令。

![](https://pic2.zhimg.com/v2-6baf840cdfce6130262a917c164923dd_b.png)

可以使用以下命令，取消所有映射。请注意，这个命令将会移除所有用户定义和系统默认的键盘映射。

Ver: 1.0<[上一篇](http://yyq123.blogspot.com/2010/09/vim-diff.html) | [目录](http://yyq123.github.com/learn-vim/learn-vi-00-List.html) | [下一篇](http://yyq123.blogspot.com/2010/12/vim-abbreviation.html)\>