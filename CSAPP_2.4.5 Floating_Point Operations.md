# CSAPP\_2.4.5 Floating\_Point Operations

* 整数集以及定义在整数集上的加法运算，是一个**Abelian group**
* 当加法运算定义在**浮点数集**上时，是不满足**结合律**的**Abelian group**
```
double a=3.14+1e10-1e10;
double a=3.14+(1e10-1e10);
//前者返回值是3.1399999999
//后者返回值是3.1400000000
```
> 在浮点数集上，对加法使用不同的结合运算（结合不同的数）可能会造成不同的结果  
> 上述约束对于浮点乘法也一样  
> 综上：浮点加法、浮点乘法缺乏**分配律**与**结合律**

* float -> int 后者无法表示的数统统转换成**Tmin**
* float int 参与运算 int会自动转换为float

## 浮点数 易混
* `float a=0x7f7fffff`:会以hexdecimal->signed decimal的方式附加小数点存储这个数。即小数点左边的部分和该值在int状态下相同，小数点右边都是0
> float 类型取到normalized的最大值时，在内存中的表示的确是`0x7f7fffff`，但是不能通过直接assign the value `0x7f7fffff`来获取the normalized maximal in float
* float double在计算机内的存储方式**不能**用作赋值的rvalue
* int unsigned在计算机内的存储方式**可以**用作赋值的rvalue
* float和double的范围见课本p118
