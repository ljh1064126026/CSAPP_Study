# Assembly_Language Accessing Info

* 变址访问内存方式：`offset(base ,index ,scale_factor) == offset+base+index*scale_factor`
* When performing a cast that involves both a **size change** and a **change of 'signedness'** in C, the operation should **change the size first**
> unsigned and signed(two's complement)本质上只是计算机表达数字的方式不同 在二进制存储上没有区别 因此在进行两者转换时不应当改变高位（符号位等）  
> 本质上包括**size change** and a **change of 'signedness'** 其实只有**size change**(extend) 后者其实由系统自动表达完成

## The size of data
1. `b`:1 byte 8 bits
2. `w`:2 bytes 16 bits
3. `l`:4 bytes 32 bits
4. `q`:8 bytes 64 bits

## Two conventions when generate values into registers
1. Those that generate 1 or 2 bytes leave the remaining bytes unchanged
2. ..generate 4 bytes leave the upper 4 bytes of the register to **zero**

## mov, lea, and 变址访问
> movl a ,%eax # movl the **value** of *a* to the register %eax  
> leal a ,%eax # movl the address of *a* to the register %eax

* `movl offset(base ,index ,scale_factor) ,b`:将offset+base+index*scale_factor表示的地址**指向**的内存赋值给b
* `leal offset(base ,index ,scale_factor) ,b`:将offset+base+index*scale_factor表示的地址**直接**赋值给b
> 由于leal是直接赋值，因此可使用*rd(ra ,rb ,c)=rd+ra+rb\*c*的方式进行对b的赋值，该表达式是arithmetic的运算  
> 只有在`leal`指令下`offset(base ,index ,scale_factor)`不是访问该语句表示的地址指向的值  
> Base and index must be registers

```
.section .data
value:
	.int 10
.section .text
.globl _start
_start:
	nop
	movl $1 ,%eax
	leal value(%eax ,%eax ,4) ,%ebx
	leal 10(%eax ,%eax ,4) ,%ecx
	movl $0 ,%ebx
	movl $1 ,%eax
	int $0x80
```

* 上述代码中 %ebx中的值是6万+  %ecx中的值是15
* value是一个标签 lea本质上`load effective address`，故会取value的地址作为操作数

* 为了便于理解 内存变址访问 `offset(base ,index ,scale_factor)`也可以理解为`base(offset ,index ,scale_factor)`
* `offset(base ,index ,scale_factor)=*(offset+base+index*scale_factor)`
> `lea`is an exception  
> `lea`的括号前面不能用寄存器 只能用立即数或变量（变量被识别为它的内存地址）

## The 'x' of movx
* If the destination is a register, it must match the 'x' of movx(such as 'b' for byte ,'l' of 32-bit)
* If the destination is a block of memory, like (%rsi), the x depends on the source. We cannot acknowledge the infomation of size of (%rsi)
* 一个立即数的具体大小（在计算机中被表示的值）取决于'x'
> 若'x'='b' 则`2^32-1`就无法正确表示 若'x'='l'则可以正确表示

## movl, movq and movabsq
* `movl` can set the **high-order 4 bytes** to `0x00 00 00 00`
* The source of `movq` is at most the **32-bit number**, which is to be adopted in the convention.
* `movabsq source ,destination`: Move 64-bit number to destination

## Expanding of Unsigned and Signed
* `movzbq source ,destination`:无符号数从1字节（byte b)扩展到8字节（quad q) **高位补0**
* `movswl s ,d`:有符号数从2字节(w)扩展到4字节(l) **高位补符号位**
> 特殊情况：`movzlq`不存在 用`movl`代替

## 关于PC寄存器
* 在本条语句执行过程及执行完毕时候 PC寄存器指向的都是本条语句之后的下一条语句 指向的语句尚未被执行
![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2020-01-01下午3.54.17.png)
![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2020-01-01下午4.02.01.png)

> RIP寄存器不能被直接访问 例如：RIP寄存器的值不能被直接赋值给其它寄存器
