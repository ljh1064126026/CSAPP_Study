# Assembly_Language Tools

### 编译器GCC使用入门
> GNU编译器 gcc 在gcc包中

* `gcc -o object_name source_name.c`:生成可执行文件且不生成中间文件
* `gcc -S source_name.c`:生成编译后的汇编文件，但不执行汇编
* `gcc -g -o object_name object_name.c`:生成可执行文件的同时附带调试信息。必须有调试信息才能执行调试
* `gcc -c source_name.c`:生成目标文件但是不链接，可用于objdump反汇编
* `gcc -fno-common -o t t1.c t2.c`:若多个文件中有相同名字的global会报错
* `gcc -Werror -o t t1.c`:将警告转换成报错

### binutils 工具包
* `rpm -qa|grep binutils`:检查系统中是否安装`binutils`工具包
> `rpm`是`red hat`的`Linux`的**包管理工具**, `-qa`列出所有已安装的包, `grep words`查询标题包含字符串`words`的包

### 汇编器as使用入门
> GNU汇编器 as 在binutils包中

* `as -o object_name.o source_name.s`:将汇编文件汇编成目标文件

### 连接器ld使用入门
> GNU连接器 ld 在binutils包中

* `ld -o object_name source_name.o`:将目标文件链接成可执行文件

### 调试器gdb使用入门

> gdb内部的常用操作查阅帮助手册     
> GNU调试器 gdb 在gdb包中  

* `gdb executable_name`:对可执行文件进行调试
> 执行带有`-g`使可执行文件附带调试信息的操作 会使可执行文件的体积变大  
> `-g`在`as`中也可以使用  

* `gdb -tui name`:开启tui窗口权限
> `la a`=`layout asmble`:打开实时assemble window
> `la r`=`layout regsters`:打开实时regs window

### 反汇编工具objdump使用入门
> GNU反汇编器 objdump 在binutils包中

* `objdump -d object_name.o`:反汇编目标文件

### 简档器gprof使用入门
> GNU简档器gprof 在binutils包中 用于检测程序运行完成后每个函数所消耗的时间

* `gcc -pg -o object_name source_name.c`:生成可执行文件和相关信息帮助gprof生成**调用图标简档文件**
* `./obeject_name`:运行可执行文件后，目录下会自动生成简档文件，为`gmon.out`。该文件不可打开
* `gprof object_name > gprof.txt`:打开简档文件并将该文件重定向到`gprof.txt`文件中
> 注意 不可直接打开`gmon.out`查看简档文件，需要通过可执行文件来打开简档文件

### 查看relocatable object files
> 注意little endian   
* `gcc -c name.c`:得到relocatable object file不连接
* `hexdump -C name.o`:得到二进制内容
* `readefl name.o`:得到relocatable object file的全部信息
* `readelf -h name.o`:得到elf header
* `objdump -d name.o`:反汇编得到.text段
* `objdump -s name.o`:.data
* `objdump -h name.o`:.bss and .data
* `readelf -S name.o`:section table
* `readelf -s name.o`:.symtab
* `nm name.o`:list the symbols defined in the symbol tab of the name.o

