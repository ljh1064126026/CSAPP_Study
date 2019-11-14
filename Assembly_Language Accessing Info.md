# Assembly_Language Accessing Info

* 变址访问内存方式：`offset(base ,index ,scale_factor) == offset+base+index*scale_factor`

## mov and lea
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

## The 'x' of movx
* If the destination is a register, it must match the 'x' of movx(such as 'b' for byte ,'l' of 32-bit)
* If the destination is a block of memory, like (%rsi), the x depends on the source. We cannot acknowledge the infomation of size of (%rsi)

## movl, movq and movabsq
* `movl` can set the **high-order 4 bytes** to `0x00 00 00 00`
* The source of `movq` is at most the **32-bit number**, which is to be adopted in the convention.
* `movabsq source ,destination`: Move 64-bit number to destination

## Expanding of Unsigned and Signed
* `movzbq source ,destination`:无符号数从1字节（byte b)扩展到8字节（quad q) **高位补0**
* `movswl s ,d`:有符号数从2字节(w)扩展到4字节(l) **高位补符号位**
> 特殊情况：`movzlq`不存在 用`movl`代替
