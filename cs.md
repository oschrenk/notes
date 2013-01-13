# Computer Science #

## System Architecture ##

When you look at your computer memory, it is organized into three segments:

- _Heap_. The heap is used for dynamic memory allocation. Blocks of memory are allocated and freed in this case in an arbitrary order. The pattern of allocation and size of blocks is not known until run time. Heap is usually being used by a program for many different purposes.
- _Stack_. The stack is a LIFO structure, typically located in the higher parts of memory. It usually "grows down" with every register, immediate value or stack frame being added to it. A stack frame consists at minimum of a return address. The stack is a place in the computer memory where all the variables that are declared and initialized before runtime are stored. It is used for procedure calls and returns. Each process has its own stack. Stack size is fixed when the program is loaded into the memory. There is always the risk of a stack overflow at runtime (if too much data are pushed onto the stack). If this is the case, the process is terminated and the OS returns a _stack fault_ message
- _Code_. Its where the executable machine code is stored. The code is placed at the lowest available address followed then by data.

## Multi Threading ##

- _Process_. Are independent from each other. Processes carry considerable state information. They have separate address space. They interact only through system-provided inter-process communication mechanisms
- _Threads_. Exists as subsets of a process. Multiple threads within a process share state as well as memory and other resources. Threads share their address space.
- _Green-Threads_. Are threads that are scheduled by a virtual machine (VM) instead of natively by the underlying operating system. They emulate multithreaded environments without relying on any native OS capabilities, and they are managed in user space instead of kernel space, enabling them to work in environments that do not have native thread support. [Sintes2001][#Sintes:2001]

[#Sintes:2001]: [Four for the ages](http://www.javaworld.com/javaworld/javaqa/2001-04/02-qa-0413-four.html)
