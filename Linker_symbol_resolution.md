# Linker_symbol_resolution
> 把所有变量名 函数名理解为统一的symbol 这样函数和变量就可以在同一个范围内讨论 他们是两种不同type的symbols

#### Symbol Resolution:associate each reference with exactly one symbol definition from the symbol tables

* When the **compiler** encounter a symbol that is not defined in the current module, it assumes that it is defined in other modules and generates a **linker symbol table entry** and leaves it for the **linker** to handle

* `undefine reference to '_func'`:这是Linker的报错——compiler会当作在其他文件定义的symbol 

## Multiple global symbols in different object modules with the same name
* For the symbols of which the type are `local`, they are **visible only to the module that defines them**. 
* **So we only consider the `global symbols`**
> nonstatic global variables and C functions are all **visible in other modules**  
> (对于nonstatic)若一个函数f在文件a.c中定义 则在b.c中声明（e.g include声明原型的头文件）（不带extern)即可使用  
> (对于nonstatic)若一个全局变量在a.c中定义 则在b.c中 **带extern** 声明即可使用  
> 上述定义和使用分开都需要 **link**  

* 关于multiple symbols with the same name的报错一般都是linker报错 因为compiler只会决定把symbols放在symbol table的什么section

### Strong and Weak
> **Compiler** exports each global symbol to the assembler as either **strong** or **weak**, and **Assembler** encode this info in the symbol table of the relocatable object file. Then **linker** choose the correct symbols that have the **same name**

* **strong**:Defined functions and initialized global variables
* **weak**:Undefined functions and uninitialized global variables

1. Multiple strong **with the same name** is not allowed
2. Strong and Weak **with the same name** Linker choose **strong**
3. All Weak **with the same name** Linker randomly choose a weak

## Linking with Static Libraries
* 在没有static library的前提下 调用外部函数（standard functions）的几种方式：

1. Compiler recognize the calls to the **standard functions** and to **generate** the codes **directly**
> 对于定义了大量standard func的语言来说（如 C）耗时较长 significant complexity to the **compiler**

2. 将所有standard func放置在一个 **relocatable object module**
> 假设所有standard func放置在libc.o中 任意一个executable object file只要包含任意一个standard func symbol **都需要将整个libc.o粘贴到exe中** 从而去resolve the standard func symbol 这样会增加内存占用  
> 除此之外 任意一个standard func需要被修改 都要把libc.o **重新编译一次** 会消耗大量的开发时间并且给维护libc.o带来巨大的困难

3. 将每个st func 都编译成一个分离的 **relocatable object module** (如 printf.o scanf.o) 当需要相应函数时在`gcc`命令行下引用相应module即可
> 若需要调用的函数很多 就要在命令行中 **type** 所有需要用到的module 会很消耗时间

* 采用 **static library** 解决上述问题（见纸质版笔记 p7）
