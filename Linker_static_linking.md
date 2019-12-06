# Linker static linking
> Relocatable object files consists of various codes and data sections where each section is **a contiguous sequence of bytes**

* step 1:symbol resolution
1. Each symbol corresponds to **a function** or **a global variable** or **a static variable**
> The local variables are not contained in symbols. They are allocated in stack in run-time

2. Symbol resolution is to associate symbol reference and its symbol definition

* step 2:relocation

* **Object files are merely collections of blocks of bytes**
* A linker concatenated blocks together and decides on run-time locations for blocks and modifies locations within the code and data blocks