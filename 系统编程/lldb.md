1. 设置断点
```lldb
b main.c:40

b 
```

2. 查看断点
```lldb
br list
```

3. 删除断点
```lldb
br del <num>
```
如果没有写断点序号，那么就是删除所有断点

4. 分布调试
```lldb
next

step
```

5. 打印变量
```lldb
print a
```

6. 查看当前栈中的变量
```lldb
frame variable
```

7. 查看当前调用栈
```lldb
bt
```

8. 进入栈中查看变量
```lldb
fr s <num>
```
-s seletor
这个\<num\>是看上面bt调用标出来的序号

9. 设置监视断点
```lldb
watch set var <变量名>
```
当这个变量的值发生改变的时候就会暂停程序的进行

10. 打印对象
```lldb
po
```


