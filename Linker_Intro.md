# Linker Intro

#### A relocatable object file = A relocatable object module

* `gcc -O1 -o prog main.c sum.c`:编译连接两个文件形成一个excutable object file
> `-O1 -o` is the compile options
* `cpp main.c mian.1`:preprocess
* `cc1 main.i -Og -o main.s`:compile
* `as -o main.o main.s`:assemble
* `ld -o prog main.o sum.o`:link
* **.c -> preprocessor -> .i -> compiler -> .s -> assembler -> .o -> linker -> excutable object file**
> `.o` is the relocatable object file