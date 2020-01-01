# CSAPP\_2.4.5 Floating\_Point

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

## 关于Float的精度问题
* 当float表示小数时，表示的精度不能通过范围衡量。并不是所有在浮点范围内的小数都可以被float表示。不能表示的需要**approximate**
* 当float表示整数时 进行如下讨论：

#### Float 整数存储 案例讨论
> double 同理

```
float f=1e20;

printf("%f" ,f+1-f);

//Output 0
```

1. 在**一定范围**内的所有整数都可以被float表示
> 此处的**一定范围**不是float表示的最大范围  

2. 超过**一定范围**后float没有越界，但这之后的整数无法被表示
3. 称该**一定范围**为**精度**
4. 考虑`1e20`的二进制表示为：`1010110101111000111010111100010110101100011000100000000000000000000`
5. 观察 从右往左开始的第一个非0数 到最高位 总计超过24位
6. 由于float是32位 故f最多只有23位（加上隐藏的最高位1就是总计24位）虽然*Exponent*最多有127，即最高可以乘2^127，即在二进制表示下，小数点向右移的个数。但因为f最多表示24位 故若*Exponent*>24 二进制下的小数点右移只会在右端补0。
7. 如1e20，二进制表示除了右端的**整段0**之外，还有47位，这47位中只有高位的24位能被float正确存储，剩余的23位都会被存储成*0*
8. 代码中`f+1`实际上是`1e20`的最低位`+1`，根据上述讨论，最低位已经远远大于23位，故变成0。因此float的存储中，`f+1`和`f`是相等的值。

#### 一些实验的证明
> **注意** float的精度不依赖于数字的大小，而依赖于该数字的**二进制表示**下 除去低位的**整段0**后 剩余高位的长度   
> 为什么忽略低位**整段0**的影响？因为小数点移到大于精度的位置 右端补0 不影响数值  

```
float a=10000000000-1024;

printf("%f\n", a);

//Correct output
//Bin(1e10)=1001010100000010111110010000000000
//Bin(1024)=10000000000
//Bin(1e10-1024)=1001010100000010111110000000000000
//The bits of numbers above are all in 24 bits.
```

```
float a=10000002048;//1001010100000010111110110000000000

printf("%f\n", a);//Correct output

a=10000000256;//1001010100000010111110010100000000

printf("%f\n", a);//Incorrect output
//Output:1e10
//最高位数起的24位之后的位都变成0 故最后一个1未被识别
//说明精度不取决于数值大小 而取决于该数字的**二进制表示**下 除去低位的**整段0**后 剩余高位的长度

```

#### Some conclusions derived from discussion above
* 小数的精度不能用范围表示 整数的精度要考察**二进制**表示下特定部分的长度
* The addition and multiplication on the Float is an Abelian group **without** the property of associativity 
> `f+1-f=0` BUT `f-f+1=1`  
* `int -> float`：不会越界，但是会因为精度不足而**rouding**
* `int,float -> double`：不会越界也不会**rounding**
> int的有效位是32位 float的有效位（即f）是23位 double的有效位（即f）是52位  
* `double -> float`：可能越界，也可能精度不足
> 精度的范围 < 可表示的大小范围  
> double的f在23位之后的位表示的数值无法被float表示 因此会发生rounding  
* `float,double -> int`：直接舍去小数点后的部分
> WHY?
