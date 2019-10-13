# CSAPP_2.2.5 Signed VS Unsigned in C

* Most of numbers(including x >= 0) are signed by default
> 0x12345(signed) ---> 0x12345u(declared unsigned)
* %d: as signed decimal  %u:as unsigned deciaml  %x:as hexadecimal
* When handling expressions(e.g a<b a+b a-b)containing signed and unsigned, **signed would implicitly convert into unsigned**
> e.g -1<0U return false.  
> Because the unsigned representation of -1 is 2^32-1