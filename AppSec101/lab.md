---
permalink: /appsec101/lab/
title: Lab environment
parent: Application Security 101
nav_order: 4
---

[<< Memory](https://beaujeant.github.io/appsec101/memory/){: .btn .btn-outline }
[Assembly >>](https://beaujeant.github.io/appsec101/assembly/){: .btn .btn-outline }

# Lab environment

In this course, we will do a little bit of _reversing_, _debugging_ and _compiling_.

__Reversing__, short for __reverse engineering__, is the process of reading machine language instructions and making sense out of it. The process could result in the translation of instructions into a higher-level language (usually pseudo-code). To help us in this task, we will use the built-in command-line debugger __GDB__ (see below). Many disassembler are much better than GDB, such as [IDA](https://www.hex-rays.com/products/ida/support/download_freeware.shtml), however, for what we need from it, GDB will be sufficient.

In the context of this course, __Debugging__ means analyzing the binary application while it is running thanks to a _debugger_. A _debugger_ allows you to set _breakpoints_ in the debugged running application. A _breakpoint_ can be set on one or several instructions. Once the instruction with the breakpoint is reached and is about to be processed by the CPU, the application will pause the program. While being paused, the analyst can read and edit instructions, the memory and _registers_ (see more about registers in chapter [CPU](https://beaujeant.github.io/AppSec101/cpu/)). In this course, we will use __GDB__ (see below).

__Compiling__ is the process of transforming computer code written in one programming language (the source language) into another programming language (the target language). In the context of this course, this means transforming C code in a binary application format using machine language instructions. The compiler used in this course is __GCC__.


## Operating System

For this course, we decided to use the standard __Ubuntu 32-bit Desktop__ distribution. We could have found a much lighter Operating System (OS) which would have been sufficient for what we need, however, we thought this would be easier to install configure and maintain. Furthermore, Ubuntu is free and widely used, so there is a high chance you already encountered and used it, so you should feel already comfortable with it.

As mentioned in the [introduction](https://beaujeant.github.io/AppSec101/introduction/), this course cover 32-bit only. Although it is possible to compile, run and debug x386 application on 64-bit operating systems, using a 32-bit OS will reduce the dependency and environment complexity.

### Download

Ubuntu is not anymore available in 32-bit version since 18.04, so we need to use the version [16.04](http://releases.ubuntu.com/16.04/).

Download [Ubuntu 16.04.5 Desktop 32-bit](http://releases.ubuntu.com/16.04/ubuntu-16.04.5-desktop-i386.iso) (ISO)

### VM deployment

 You can either run Linux natively on you hardware or in a virtual environment. If you have Linux already installed on your computer, you can skip this part.

In order to keep this course free, we recommend to use [VirtualBox](https://www.virtualbox.org) as virtual environment. Once the ISO downloaded and VirtualBox installed, you can follow this [tutorial](https://medium.com/@tushar0618/install-ubuntu-16-04-lts-on-virtual-box-desktop-version-30dc6f1958d0). The given specs are sufficient, i.e.:

* 1024 Mb RAM
* 10 Gb Hard disk (VDI - Dynamically allocated)

> Note: during the Ubuntu installation, make sure you select the right keyboard (use "Detect keyboard layout"). For the rest, you can choose whatever name, username, password and language.

Once the VM deployed, open your terminal and update VM then install the VirtualBox Guest Addition:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install virtualbox-guest-additions-iso
```

## Tools

For this course, we only need 2 tools: __GDB__ and __GCC__. Both tools are already pre-installed in Ubuntu Desktop.

### GCC

Let's compile our first C code for this course. Use you favorite text editor and create the following `mul.c` file:

```C
#include <stdio.h>


int mul(int, int);


int main()
{
    int a,b,c;

    a = 42;
    b = 2;

    c = mul(a, b);

    printf("%d x %d = %d\n", a, b, c);

    return 0;
}


int mul(int x, int y)
{
    int res;

    res = x * y;

    return res;
}
```

Learning C is beyond the purpose of this course, so we assume you understand what this code does (i.e. it multiplies 42 by 2 and print the result).

Now, in order to compile this code, you simply need to execute the following command:

```
gcc mul.c -a mul
```

This will compile `mul.c` and create the binary application `mul`. Now, in order to run application, you simply need to execute the following command:

```
./mul
```

### GDB

As mentioned earlier, GDB is a debugger. The main feature of a debugger is to set breakpoint and read memory and registers. Let's debug our first application:

```
gdb mul -q
```

> Note: The option `-q` is avoid printing introducery and copyright message

Now, we want to disassemble the `main()` function so we know where to set a breakpoint.

```
(gdb) disassemble main
Dump of assembler code for function main:
   0x0804840b <+0>:	lea    0x4(%esp),%ecx
   0x0804840f <+4>:	and    $0xfffffff0,%esp
   0x08048412 <+7>:	pushl  -0x4(%ecx)
   0x08048415 <+10>:	push   %ebp
   0x08048416 <+11>:	mov    %esp,%ebp
   0x08048418 <+13>:	push   %ecx
   0x08048419 <+14>:	sub    $0x14,%esp
   0x0804841c <+17>:	movl   $0x2a,-0x14(%ebp)
   0x08048423 <+24>:	movl   $0x2,-0x10(%ebp)
   0x0804842a <+31>:	sub    $0x8,%esp
   0x0804842d <+34>:	pushl  -0x10(%ebp)
   0x08048430 <+37>:	pushl  -0x14(%ebp)
   0x08048433 <+40>:	call   0x8048461 <mul>
   0x08048438 <+45>:	add    $0x10,%esp
   0x0804843b <+48>:	mov    %eax,-0xc(%ebp)
   0x0804843e <+51>:	pushl  -0xc(%ebp)
   0x08048441 <+54>:	pushl  -0x10(%ebp)
   0x08048444 <+57>:	pushl  -0x14(%ebp)
   0x08048447 <+60>:	push   $0x8048500
   0x0804844c <+65>:	call   0x80482e0 <printf@plt>
   0x08048451 <+70>:	add    $0x10,%esp
   0x08048454 <+73>:	mov    $0x0,%eax
   0x08048459 <+78>:	mov    -0x4(%ebp),%ecx
   0x0804845c <+81>:	leave  
   0x0804845d <+82>:	lea    -0x4(%ecx),%esp
   0x08048460 <+85>:	ret    
End of assembler dump.
```

If your resolution is not big enough, it is possible that GDB print the following message:

```
---Type <return> to continue, or q <return> to quit---
```

In this case, you simply need to type `ENTER` to see the rest of the disassembled code.

As explained in the [introduction](https://beaujeant.github.io/appsec101/introduction/), machine code instructions are basically a group of binary values that signify a specific instruction for the CPU. E.g. `b8 00 00 00 00` means moving `0x00000000` in the register `EAX`. However, it is usually easier for human to read pseudo-english rather than hexadecimal value, therefore disassembler translate the binary values (opcodes) in human readable code. Here in this case, `b8 00 00 00 00` is translated as `mov $0x0, %eax` (see instruction at the address `0x08048454`). This representation is the AT&T syntax. However, it exists a different way(s) to represent opcodes, the most known one being "Intel". In Intel syntax, `b8 00 00 00 00` is translated as `mov eax, 0x0`. We think the Intel syntax is easier to read than AT&T, therefore this course will be using Intel syntax. To change the syntax in GDB, you can run the following command:

```
(gdb) disassemble main
Dump of assembler code for function main:
   0x0804840b <+0>:	lea    ecx,[esp+0x4]
   0x0804840f <+4>:	and    esp,0xfffffff0
   0x08048412 <+7>:	push   DWORD PTR [ecx-0x4]
   0x08048415 <+10>:	push   ebp
   0x08048416 <+11>:	mov    ebp,esp
   0x08048418 <+13>:	push   ecx
   0x08048419 <+14>:	sub    esp,0x14
   0x0804841c <+17>:	mov    DWORD PTR [ebp-0x14],0x2a
   0x08048423 <+24>:	mov    DWORD PTR [ebp-0x10],0x2
   0x0804842a <+31>:	sub    esp,0x8
   0x0804842d <+34>:	push   DWORD PTR [ebp-0x10]
   0x08048430 <+37>:	push   DWORD PTR [ebp-0x14]
   0x08048433 <+40>:	call   0x8048461 <mul>
   0x08048438 <+45>:	add    esp,0x10
   0x0804843b <+48>:	mov    DWORD PTR [ebp-0xc],eax
   0x0804843e <+51>:	push   DWORD PTR [ebp-0xc]
   0x08048441 <+54>:	push   DWORD PTR [ebp-0x10]
   0x08048444 <+57>:	push   DWORD PTR [ebp-0x14]
   0x08048447 <+60>:	push   0x8048500
   0x0804844c <+65>:	call   0x80482e0 <printf@plt>
   0x08048451 <+70>:	add    esp,0x10
   0x08048454 <+73>:	mov    eax,0x0
   0x08048459 <+78>:	mov    ecx,DWORD PTR [ebp-0x4]
   0x0804845c <+81>:	leave  
   0x0804845d <+82>:	lea    esp,[ecx-0x4]
   0x08048460 <+85>:	ret    
End of assembler dump.
```

Now, let say you want to set a breakpoint at the `mov eax, 0x0` (located at the address `0x08048454`), you simply need to enter the following command:

```
(gdb) break *0x08048454
Breakpoint 1 at 0x08048454
```

In order to reach this instruction, you need to run the application:

```
(gdb) run
Starting program: /home/lab/mul
42 x 2 = 84

Breakpoint 1, 0x08048454 in main ()
```

As you can see, at this stage, the application already did the multiplication and printed the result. Once the breakpoint reached, the application pauses. At this point, you can read (and write) memory, instructions and registers. To view the registers, you can run the command:

```
(gdb) info registers
eax            0xc	12
ecx            0x7ffffff4	2147483636
edx            0xb7fbc870	-1208235920
ebx            0x0	0
esp            0xbfffef20	0xbfffef20
ebp            0xbfffef38	0xbfffef38
esi            0xb7fbb000	-1208242176
edi            0xb7fbb000	-1208242176
eip            0x8048454	0x8048454 <main+73>
eflags         0x282	[ SF IF ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
```

> We will learn more about registers in the chapter [CPU](#), but for now, you have to see registers as a collection of 32 bits variable available to the CPU, where read/write access is much faster than access to "normal" memory.

To read memory locations, including instructions (since instruction are located in memory), you can use the command `x` (for eXamine). The command `x` has the following format:

```
x/nfu addr
```

* __n__ _(optional)_ is the repeat count: the repeat count is a decimal integer; the default is 1. It specifies how much memory (counting by units u) to display. If a negative number is specified, memory is examined backward from addr.
* __f__ _(optional)_ is the display format: the display format is one of the formats used by print ('_x_', '_d_', '_u_', '_o_', '_t_', '_a_', '_c_', '_f_', '_s_'), and in addition '_i_' (for machine instructions). The default is '_x_' (hexadecimal) initially. The default changes each time you use either x or print.
* __u__ _(optional)_ is the unit size:
  * _b_: Bytes
  * _h_: Halfwords (two bytes)
  * _w_: Words (four bytes) (default)
  * _g_: Giant words (eight bytes)
* _addr_ is the starting display address: _addr_ is the address where you want GDB to begin displaying memory. The expression need not have a pointer value

Source and more info [here](https://sourceware.org/gdb/onlinedocs/gdb/Memory.html).

We now have reached the breakpoint located at `0x8048454`. So if we use the command `x/i 0x8048454`, we should see the instruction `mov eax, 0x0`:

```
(gdb) x/i 0x8048454
=> 0x8048454 <main+73>:	mov    eax,0x0
```

Now, if we want to see the opcode instead of the instruction, we have to change the format from _i_ (machine instruction) to _x_ (hexadecimal representation). Furthermore, it would be more readable to print the opcode byte by byte, instead of a full word (4 bytes), which is the default unit size, so we will use the unit size _b_. Lastly, the instruction `mov eax, 0x0` is 5 bytes long, therefore, the repeat counter should be set to _5_:

```
(gdb) x/5xb 0x8048454
0x8048454 <main+73>:	0xb8	0x00	0x00	0x00	0x00
```

> Note: Unlike ARM, x386 instruction length is variable, so we first need to read the first byte to know how long will be the instruction in opcode. For instance, `0xb8` means move the next 4 bytes (word) in EAX. So the entire instruction is 1 byte (`0xb8`) + 4 bytes (value to copy in EAX) = 5 bytes. Whilst `0x89` means moving the value of a register into another register. The following byte will indicate which is the source register and the destination register. For instance `0xc8` would means source is ECX and destination is EAX, which means `89 c8` means `mov eax, ecx`. So the entire instruction is 1 byte (`0x89`) + 1 byte (source/destination register) = 2 bytes.

Now that we know how to read memory, let's navigate through the instructions. The four main commands to remember are `run`, `continue`, `nexti` and `stepi`:

* `run` will start the application. It is recommended to set breakpoint before executing the command `run` otherwise, it will just execute the application and you might not have the time to pause it to investigate the memory, registers and instructions. You can add arguments after the command. For instance, if our application `mul` was expecting a number as argument, we could have executed the command `run 123`.
* `continue` resume the program execution. So once you reached a breakpoint and you want to resume the execution until the next breakpoint or the end of the program, you can use the command `continue`.
* `nexti`, short for _next instruction_, execute one machine instruction, but if it is a function call, proceed until the function returns.
* `stepi`, short for _step instruction_, execute one machine instruction, then stop and return to the debugger.

The difference between `nexti` and `stepi` might not be clear, so here is an example. Let's first set two additional breakpoint: one at the `call 0x8048461 <mul>` and one at the `call 0x80482e0 <printf@plt>` then restart the app:

```
(gdb) break *0x08048433
Breakpoint 2 at 0x8048433
(gdb) break *0x0804844c
Breakpoint 3 at 0x804844c
(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /home/lab/mul

Breakpoint 2, 0x08048433 in main ()
```

> Note: Since the application was already running, you should have a warning asking you whether you really want to start from the beginning. You simply need to type `y` to confirm.

Now we reached the breakpoint at `call 0x8048461 <mul>`. If you want to, you can execute the command `disassemble main` again to see where we are. The `call` instruction will redirect the execution flow to the function `mul`. If you want the function to execute and go straight to the next instruction (i.e. `add esp,0x10`) you can use the command `nexti`. But if you want to follow the execution flow and jump in the function `mul`, you can use the command `stepi`. Let's use the later:

```
(gdb) stepi
0x08048461 in mul ()
```

As you can see, we now are in the function `mul` located at `0x8048461`. The debugger executed one single instruction, then automatically paused the program without the need to set another breakpoint.

Now let's have a look at the next 4 instructions:

```
(gdb) x/4i 0x08048461
=> 0x8048461 <mul>:	push   ebp
   0x8048462 <mul+1>:	mov    ebp,esp
   0x8048464 <mul+3>:	sub    esp,0x10
   0x8048467 <mul+6>:	mov    eax,DWORD PTR [ebp+0x8]
```

We can now use either `stepi` or `nexti` to execute the current instruction and move to the next one:

```
(gdb) nexti
0x08048462 in mul ()
(gdb) nexti
0x08048464 in mul ()
(gdb) nexti
0x08048467 in mul ()
```

Now, let's go to the next breakpoint. For this, we can use the command `continue`, so that the debugger will continue the program and until it reaches a breakpoint or the application close.

```
(gdb) continue
Continuing.

Breakpoint 3, 0x0804844c in main ()
```

Now we have reached the instruction `call 0x80482e0 <printf@plt>`. The function `printf` is a native fonction from stdio, so we don't want to debug/reverse that function, we know already what it does. So we just want to move the instruction after the call. For this, we simply need to execute the command `nexti`:

```
(gdb) nexti
42 x 2 = 84
0x08048451 in main ()
```

As you can see, `printf` has been executed and printed in the terminal `42 x 2 = 84`.

Now, let's continue the execution until the last breakpoint at the instruction `mov eax,0x0`, print the registers, execute the instruction and print again the registers so that we can see the modification of EAX:

```
(gdb) continue
Continuing.

Breakpoint 1, 0x08048454 in main ()
(gdb) info registers
eax            0xc	12
ecx            0x7ffffff4	2147483636
edx            0xb7fbc870	-1208235920
ebx            0x0	0
esp            0xbfffef20	0xbfffef20
ebp            0xbfffef38	0xbfffef38
esi            0xb7fbb000	-1208242176
edi            0xb7fbb000	-1208242176
eip            0x8048454	0x8048454 <main+73>
eflags         0x282	[ SF IF ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
(gdb) nexti
0x08048459 in main ()
(gdb) info registers
eax            0x0	0
ecx            0x7ffffff4	2147483636
edx            0xb7fbc870	-1208235920
ebx            0x0	0
esp            0xbfffef20	0xbfffef20
ebp            0xbfffef38	0xbfffef38
esi            0xb7fbb000	-1208242176
edi            0xb7fbb000	-1208242176
eip            0x8048459	0x8048459 <main+78>
eflags         0x282	[ SF IF ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
```

> Note: You can print a single register by using `print $reg`, for instance `print $eax`.

Finally, let's have a look at the breakpoints we have set with the command `info breakpoints`:

```
(gdb) info breakpoints
Num     Type           Disp Enb Address    What
1       breakpoint     keep y   0x08048454 <main+73>
	breakpoint already hit 1 time
2       breakpoint     keep y   0x08048433 <main+40>
	breakpoint already hit 1 time
3       breakpoint     keep y   0x0804844c <main+65>
	breakpoint already hit 1 time
```

Let say you want to delete the breakpoint at the instruction `mov eax,0x0`. You simply need to execute the command `delete` together with the breakpoint index number (`Num`). In this case, the index is `1` (the instruction `mov eax,0x0` is located at the address `0x08048454`):

```
(gdb) delete 1
(gdb) info breakpoints
Num     Type           Disp Enb Address    What
2       breakpoint     keep y   0x08048433 <main+40>
	breakpoint already hit 1 time
3       breakpoint     keep y   0x0804844c <main+65>
	breakpoint already hit 1 time
```

That's it for now. In the next exercises, we will go a bit deeper with the usage of gdb.

[<< Memory](https://beaujeant.github.io/appsec101/memory/){: .btn .btn-outline }
[Programming >>](https://beaujeant.github.io/appsec101/programming/){: .btn .btn-outline }
