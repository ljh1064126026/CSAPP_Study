# CSAPP_2 INTRO

* Big endian and little endian 只考虑 **立即数** 的存放

## 内存对齐机制
```
#include <stdio.h>

typedef struct
{
    int i;
    short i2;
    char i1;
}test;

int main(int argc, char const *argv[])
{
    test a;
    printf("%lu\n", sizeof(a)); 
    return 0;
}//output:8
```

* 计算机中访问常规访问内存的步长是**4个字节**，故为了保证变量内存不会被二次访问而截断，会采用内存对齐机制

1. 当前**4个字节**中填放变量，若当前**4个字节**中剩余的字节不足以存放整个变量temp,就将temp后移到下一个**4个字节**中
2. 保证最后结构体的sizeof是4的整数倍（扩充容量）

## 一些重要的ASCII码
* 0~9:48~57
* A~Z:65~90
* a~z:97~122
