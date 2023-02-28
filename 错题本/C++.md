```Shell
$ gcc main.cpp    # 包括这里使用clang也会报相同的错误
Undefined symbols for architecture arm64:
  "operator new[](unsigned long)", referenced from:
      _main in main-e302b7.o
ld: symbol(s) not found for architecture arm64

# 这个一看就是链接出现了错误，解决方案，用错编译器了
# 可以使用g++或者是clang++来处理
# 蠢了，前几天学了编译的流程，这里我还搞错了
$ clang++ main.cpp
```


