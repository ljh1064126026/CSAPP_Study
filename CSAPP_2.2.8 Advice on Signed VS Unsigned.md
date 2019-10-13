# CSAPP_2.2.8 Advice on Signed VS Unsigned
```
unsigned a=0;
int test=0;

for(int i=0 ;i<=a-1 ;++i)
	b+=i;

```

* 上面的代码会产生越界溢出
* 由于`unsigned` and `signed`同时运算时 `signed`会隐式强制转化为`unsigned`
* 故`a-1=0-1=-1`转化为`unsigned`后就等于`-1+2^32=2^32-1`  
* 故循环不会退出
> 上面代码中由于`a`是`unsigned`类型 `i and 1`都会强制转化为`unsigned`


