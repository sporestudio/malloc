# Simple Memory Allocator
This project is about writing a simple memory allocator in [C](https://www.c-language.org/). I will implement `malloc()`, `calloc()`, `realloc()` and `free()`.

## Requeriments
- Unix/Linux System
- GCC compiler

## Memory layout

Before we get into the build of the memory allocator, we need to be familiar with the memory layout of a program. The virtual address space typically compromises of 5 sections:

- **Text Section**: The part that contains the binary instructions to be executed by the processor.
- **Data section**: Contains non-zero initialized static data.
- **BSS (Block Started by Symbol)**: Contains zero-initialized static data. Static data in program is initialized 0 and goes here.
- **Heap**: Contains the dynamically allocated data.
- **Stack**: Contains your automatic variables, functions arguments, copy of base pointer, etc...


Assuming we run Linux (or a Unix-like system), we can make use of `sbrk()` system call that lets us manipulate the program break.<br>
Calling `sbrk(0)` gives the current address of program break.<br>
Calling `srbk(x)` with a positive value increments brk by x bytes, as a result allocatung memory.<br>
Calling `srbk(-x)` with a negative vlue decrements brk by x bytes, as a result releasing memory.<br>
On failure, `srbk()` returns `void(*) -1`.

`Sbrk()` is not our best buddy in 2025. There are better alternatives like `mmap()` available today. `Sbrk()` is not really thread safe. It can can only grow or shrink in LIFO order.

> The brk and sbrk fucntions are historically curiosities left over from earlier days before the advent of virtual memory management.

However, the glibc implementation of mnalloc still uses `srbk()` for allocating memory that's not too big in size. So, we will go ahead with `srbk()` for our simple memory allocator.

