---
layout: lecture
title: "#10: Debugging and Profiling"
date: 2025-12-08
ready: false
hide: false
---

### Context: Debugging with Print Statements

One of the simplest methods to debug a program is to add print statements.
Whether they are printing values or simply logging that a certain position has
been reached in the code, it is an invaluable way to check assumptions when you
have source code you can modify.

There are a variety of methods you can use in c code to print debug information,
however one of the better options is `fprintf`. By utilising `stderr`, we can
seperate our debugging information and our program output.

```c
#include <stdio.h>

int main(int argc, char *argv[]) {
    fprintf(stderr, "%d argument(s) passed into the program:\n", argc);
    for (int i=0; i<argc; ++i) {
        fprintf(stderr, "\tArg %d: %s\n", i, argv[i]);
    }


    puts("Actual output: ");
    for (int i = 0; i < 10; i++) {
        printf("%d ", i);
    }
    return 0;
}
```

```bash
$ ./test a b c
4 argument(s) passed into the program:
        Arg 0: ./test
        Arg 1: a
        Arg 2: b
        Arg 3: c
Actual output: 
0 1 2 3 4 5 6 7 8 9 

$ ./test a b c >/dev/null
4 argument(s) passed into the program:
        Arg 0: ./test
        Arg 1: a
        Arg 2: b
        Arg 3: c
        
$ ./test a b c 2>/dev/null
Actual output: 
0 1 2 3 4 5 6 7 8 9 
```

There are downsides to this method however!

- We are required to have the source code
- The print statements may change program optimisations, altering any undefined
  behaviour/edge cases

### Enter gdb - the GNU Debugger

`gdb` is a powerful and free debugger that will let you explore and modify
programs during runtime.   The term 'bug' is said to refer to actual moths found
by computing pioneer [Grace Hopper](https://en.wikipedia.org/wiki/Grace_Hopper)
in an early vacuum tube computer in the 1940s!

### Intro and Setup

Debuggers like `gdb` often use symbol tables present in executables to assist
with the debugging process. Symbol tables track, manage, and catalogue data
structures and information in executables, for example:

- Functions
- Variables
- Scopes of data structures

`gdb` can be installed using your operating systems's package manager, E.g: `apt
install gdb`.

There are also a number of useful extensions to gdb such as `peda` and `gef`.

Configuration of`gdb` (such as plugins like the ones above) can be stored in the
`.gdbinit` file in a user's home directory.  For example, `set print
asm-demangle on` will enabled demanging so that c++ functions are displayed
properly.  You can enter this interactively in a `gdb` session or put it into
your `.gdbinit` file.  Another common and useful setting for your `.gdbinit` is
`set disassembly-flavor intel` which will show assembly code in a more common
form.


### Debugging your own program
If you are debugging your *own* programs, it's best to compile with debug
symbols (`-g`) and with optimization disabled (`-O0`).  For example:

`g++ -g -O0 main.cpp -o main`

This is because:
* When you compile your code with debug symbols on, function and variable names
  are left in the program which makes debugging easier.  Note that `-g` might be
the default; `-s` will force symbols to be stripped.  You might actually want
this for production builds as debugging symbols can make executables larger!
* By disabling optimization, you can ensure that your code isn't being
  simplified or altered by the compiler so what is being executed is more likely
to match the code as you have written it.  

### Demos

Below we'll explore debugging some short simple C++ programs, and how gdb can
make things a lot easier for us!

#### Demo 1 - basic debugging

```cpp
#include <iostream>
#include <stdlib.h> //srand
#include <stdio.h>  //printf

using namespace std;

int main () {

    srand (time(NULL));

    int alpha = rand() % 8;
    cout << "Hello world." << endl;
    int beta = 2;

    printf("alpha is set to is %s\n", alpha);
    printf("kiwi is set to is %s\n", beta);

    return 0;
}
```
Here we have a simple program `main.cpp`, which should print "Hello World"
followed by some print statements referencing some integer variables. 

We can compile this code with `g++ main.cpp -g -O0 -o main`, and try running it.
Sadly, we get a segmentation fault :( 

In order to start debugging this program with `gdb` we will need to run `gdb
main`. This will put you into a program with a `(gdb)` prompt (this may be
different if you're using an extension).

This is where you can enter your gdb commands. To run the program, you can type
in `run`, which will run the program until it reached the point where it
segfaults. You will then see some output like the following:

```
Program received signal SIGSEGV, Segmentation fault.
__strlen_avx2 () at ../sysdeps/x86_64/multiarch/strlen-avx2.S:74
../sysdeps/x86_64/multiarch/strlen-avx2.S: No such file or directory.
```

`gdb` already has given us some useful information! We get the signal code and
also the meaning of this signal: in this case we have `SIGSEV`, i.e.
a segmentation fault. The line after that tells us the function and the line of
source code that execution stopped at. This may not be a line you have written
as gdb reports the lowest function name.

In this case, you should use a command like `backtrace` to show the full
function backtrace. You will then get something similar to the following:

```
#0  __strlen_avx2 () at ../sysdeps/x86_64/multiarch/strlen-avx2.S:74
#1  0x00007f59aee0ed31 in __vfprintf_internal (s=0x7f59aefb3780
<_IO_2_1_stdout_>, format=0x55ab53463011 "alpha is set to is %s\n",
    ap=ap@entry=0x7ffe8e60e700, mode_flags=mode_flags@entry=0) at
./stdio-common/vfprintf-internal.c:1517
#2  0x00007f59aedf879f in __printf (format=<optimized out>) at
./stdio-common/printf.c:33
#3  0x000055ab534622a4 in main () at main.cpp:15
```

From this, we can see the trace of the function calls from the original
`main.cpp` file, to the function `printf` in `stdio`, and further down after
that. 
#### Demo 2 - changing values

`gdb` allows us to manipulate values in memory during runtime. Combined with
debug symbols and source mapping, you can see the variables in your code and
manipulate them during runtime. Let's consider the following code:


```cpp
#include <iostream>
#include <stdlib.h> //srand
#include <stdio.h>  //printf

using namespace std;

int main () {

    volatile int decider = 1;
    if (decider == 1){
        cout << "you lose" << endl;
    } else {
        cout << "you win" << endl;
    }

    return 0;
}
```

First, let's break on main:

```
(gdb) b main
Breakpoint 1 at 0x1151: file main.cpp, line 9.
```
 
And run:

```
(gdb) r
Breakpoint 1, main () at main.cpp:9
9           volatile int decider = 1;
(gdb) 
```

We're about to run `volatile int decider = 1;`. Let's do that:

```
(gdb) s
10          if (decider == 1){
(gdb)
```
Now we can examine `decider`:

```
(gdb) print decider
$1 = 1
(gdb) 
```

Ah! Panic! If `decider` is 1, we'll lose! Let's change that:

```
(gdb) set decider = 2
(gdb) print decider
$2= 2
(gdb) s
13              cout << "you win" << endl;
(gdb) 
```
Crisis averted! This is great, but what if we only realised that we've lost
after that if statment executed? We can solve this with time travel.

#### Demo 3 - time travel

`gdb` also supports reverse execution, allowing you to go back in time. Often
the root cause of an issue occurs before you halt execution - imagine hitting
a segfault. `gdb` can record execution state and let you travel back in time to
the root cause. Consider this C++ code:

```cpp
#include <iostream>
#include <stdlib.h> //srand
#include <stdio.h>  //printf

using namespace std;

int main () {

    volatile int decider = 1;
    if (decider == 1){
        cout << "you lose" << endl;
    } else {
        cout << "you win" << endl;
    }

    return 0;
}
```

Let's debug this code. First, break on main:

```
(gdb) b main
```

Then, begin execution:

```
(gdb) r

...

Breakpoint 1, main () at main.cpp:9
9           volatile int decider = 1;
```

Great! Now we can tell GDB to start recording execution:

```
(gdb) record
```

From now on, we have access to the `reverse-*` set of commands. For now, lets
keep executing:

```
(gdb) n
10          if (decider == 1){
(gdb) n
11              cout << "you lose" << endl;
(gdb)
```

Oh no! We're about to lose! The `if` check has already run, and failure is
inevitable. Let's go back in time and fix that:

```
(gdb) reverse-step
10          if (decider == 1){
(gdb) reverse-step
10          if (decider == 1){
(gdb) reverse-step
10          if (decider == 1){
(gdb) reverse-step

No more reverse-execution history.
main () at main.cpp:9
9           volatile int decider = 1;
(gdb) s
10          if (decider == 1){
```

Great! We've gone backwards in time. Let's fix our mistake:

```
(gdb) set decider=2
Because GDB is in replay mode, writing to memory will make the execution log
unusable from this point onward.  Write memory at address 0x7fffffffe66c?(y or
n) y
(gdb) 
```

`gdb` helpfully warns us that we're changing the timeline, but that's exactly
what we want. Let's continue in our new future:

```
(gdb) s
13              cout << "you win" << endl;
```

Great! With this you can walk backwards through time to find your root cause and
use all the power of `gdb` to fix it.

#### Demo 4 - Debugging an Operating System

Jacqui is going to demo how you can use `gdb` to debug a running OS.

---

### Commands Reference


To enter the`gdb` debugger, run `gdb` and you should see the prompt: `(gdb)`

gdb has clever autocomplete: for example typing `b` or `br` instead of `break`
or `i r` instead of `info registers`

#### Viewing and opening files

To open an executable `file [FILE NAME]`

`checksec` - checks security measures on the executable

To list decompiled code lines fromthe point of exection use `list .` and repeat
`list` to read further.

`list .` has to be repeated to restart reading code from the current location,
and `list -` and `list +` can be used to list lines before and after the current
location respectively.

`disas main` - Disassemble the main function and print the assembly code.

`run` - Runs the executable.

#### Breakpoints

To stop a program at a specific place to debug it, breakpoints can be used.
Breakpoints stop the program's execution at specific points and allow the
executable to be edited and inspected at certain states.


`break main` sets a breakpoint at the start of the main function.

Executing `run` after this will stop the program just before the first
instruction of the main function.


*Note: If you are using peda, this will also pull up a context menu. this menu
can be reviewed with `context` and displays registers, assembly code, and the
stack, as well the data that pointers are pointing to.*



Breakpoints can be set to lines to code with `break [LINE NUMBER]`

To set breakpoints in specific memory locations, you can use `disas main` to
list the main function, or `disas [FUNCTION NAME]` for another specific function
to find the line at which you'd like to set a breakpoint.

Then, run `break *[FUNCTION NAME] + [LINE]`, for example `break *main + 4` to
break when the 20th memory value of the main function is reached

The `*` operator dereferences references to areas of memory, somewhat similarly
to pointer dereferencing in code, for example *main would point to the area of
memory where the main function starts Gdb will dereference some functions for
you, but it is useful to know.



the contents at a memory address  can also be read with the `x` command with the
following syntax:

`x/[LENGTH][FORMAT] [MEMORY ADDRESS]`

where `[LENGTH]` and `[FORMAT]` are optional and the `/` operator is only used
when either of them are provided.

<center>
<table class="part" data-startline="344" data-endline="346">
<thead>
<tr>
<th style="text-align:center">Character</th>
<th style="text-align:center">x</th>
<th style="text-align:center">i</th>
<th style="text-align:center">s</th>
<th style="text-align:center">a</th>
<th style="text-align:center">d</th>
<th style="text-align:center">u</th>
<th style="text-align:center">c</th>
<th style="text-align:center">f</th>
<th style="text-align:center">t</th>
<th style="text-align:center">o</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">Format</td>
<td style="text-align:center">Hexadecimal</td>
<td style="text-align:center">Instruction</td>
<td style="text-align:center">String</td>
<td style="text-align:center">Address</td>
<td style="text-align:center">Decimal</td>
<td style="text-align:center">Unsigned Decimal</td>
<td style="text-align:center">Char</td>
<td style="text-align:center">Floating Point</td>
<td style="text-align:center">Binary</td>
<td style="text-align:center">Octal</td>
</tr>
</tbody>
</table>
</center>

examples:

`x main` - Reads the first memory value in the main function.

`x/x $rip` - Reads the value of $rip (the next instruction) as hexadecimal.

`x/20 $rsp` - Reads the first 20 memory values in the stack (pointed to by
$rsp).

`x/20i printf` - Reads the first 20 values in the print function as
instructions.



Registers can be set with `set $[REGISTER] = [VALUE]`, but this can easily break
the program, especially when editing rbp/rip/eip.

The info command allows information to be shown about the program in its current
state.

Registers can be read with `info reg`.

Functions in the program can be listed with `info func`.

Current frame data can be read with `info frame`.

Local variables can be read with `info local`.


### Backtrace

The backtrace in a program is what functions are currently active, and what
functions are being called inside those functions. This is a valuable tool in
debugging as it allows the user to work out exactly where bugs are occurring.

`backtrace` can be used to read the call stack order.

`backtrace full` does the same but displays extra information found in symbol
tables, including local variables.


### Plugins
`gdb` is useful in its base form but even better with extensions.

`peda` can be installed using the following:

```
$ git clone https://github.com/longld/peda.git ~/peda
$ echo "source ~/peda/peda.py" >> ~/.gdbinit
$ echo "DONE! debug your program with gdb and enjoy"
```

`gef` can be installed via:
```
$ wget -O ~/.gdbinit-gef.py -q https://gef.blah.cat/py
$ echo source ~/.gdbinit-gef.py >> ~/.gdbinit
```

`pwngdb` can be installed with:

```
git clone https://github.com/pwndbg/pwndbg
cd pwndbg
./setup.sh
```
