# Linker symbol table
> Symbol table is generated by **compiler**  
> 有纸质笔记 p7

### Global symbols
* nonstatic C functions + nonstatic global
* externals: nonstatic C functions + nonstatic global defined in other modules

### Local symbols
* static C functions + static
> The LOCAL in `.symtab` are not the local variables

### In .symtab
* value:the offset from the beginning of the section where the object is defined
> For excutable object files, the value is an absolute run-time address

* bind:indicate whether the symbol is local or global
* ndx:the index of a certain section in that the entry is located

#### pseudosections
> These sections dont have entries in the section header table, they dont exist in executable object files, only in relocatable files

* ABS:symbols that should not be allocated
* UND:undefined symbols(defined in outside of this object file)
* COM:uninitialzed global variable(nonstatic) **are not yet allocated**

#### The differences between COMMON and .bss
1. COM:**uninitialized nontated global**
2. .bss:**uninitialized static and initialized to zero global and stated local**
3. COM and .bss are not yet allocated before run time. The later one is just a **placeholder**
4. .bss would be allocated and initialized to zero during run time.
5. COM is just located in no more than **relocatable object file**, which means it would not exist in **executable object file**
6. If there is one strong symbol(which must be either in .bss or in .data), linker would choose the strong symbol other than the weak symbols in **COM**. Then the **COM** would not be allocated in excutable object file
7. If all global symbols with the same name are weak symbols in **COM**, linker would randomly choose one of them to be the original global vabiable.

> 可见初始化非静态全局变量（global symbol + .data)在连接运行之前（编译期）就已经在内存给全局变量赋值内存了 又因为内存区的大小会大于运行时stack 故开大数组时应当开在全局变量区（局部变量内存在run-time stack赋予内存）

![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2019-12-06下午12.27.05.png)
![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2019-12-06下午12.28.57.png)
![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2019-12-06下午12.29.03.png)
![](/Users/administrator/Documents/CSAPP_Study/photo/截屏2019-12-06下午12.29.12.png)
