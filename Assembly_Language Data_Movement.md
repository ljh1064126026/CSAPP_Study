# Assembly\_Language Data\_Movement

### Pitfalls
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

