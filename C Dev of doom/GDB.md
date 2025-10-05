# GDB Tutorial for C: Automatic Source Code Display Setup

To debug effectively and see your source code, you must compile your C program with debugging symbols. Use the following command to compile:

```bash
gcc -g your_program.c -o your_program
```

The -g flag tells the compiler to include debug symbols so GDB can map the executable back to your source code.


Next, launch GDB by running:

```bash
gdb ./your_program
```

By default, GDB opens in command-line mode without showing your source code. To improve this, use the Text User Interface (TUI) mode by launching GDB with the -tui option:

```bash
gdb -tui ./your_program
```

This opens a split view with your source code on the top pane, where the current line is highlighted, and the GDB prompt at the bottom.

If you want this behavior every time you run GDB, create a .gdbinit file in your home directory or project folder with the following content:

```
set pagination off
set print pretty on
layout src
```

This disables the “press enter” pagination, formats variable printing nicely, and opens the source layout automatically.

If you prefer not to use TUI mode, you can view source code manually by typing the list command in GDB, which displays 10 lines around the current execution point.

 ## Here are some essential GDB commands to get started:
 - break <function_or_line>: sets a breakpoint at a function or specific line.
 - run: starts program execution.
 - next (n): steps over the next line of code without entering functions.
 - step (s): steps into functions.
 - continue (c): continues running until the next breakpoint.
 - print \<variable\> :  prints the value of a variable.
 - list: shows source code lines around the current execution point.
 - info breakpoints: lists all breakpoints.
 - quit: exits GDB.

A typical debugging session workflow would look like this:

Compile your program with debug symbols:

```bash
gcc -g myprog.c -o myprog
```

Start GDB in TUI mode:

```bash
gdb -tui ./myprog
```

Set a breakpoint at the main function:

```gdb
break main
```

Run the program inside GDB:

```gdb
run
```

Step through code line by line using next:

```gdb
next
```

Inspect variable values when needed:

```gdb
print someVariable
```

Continue execution to the next breakpoint or program end:

```gdb
continue
```

Exit GDB:

```gdb
quit
```

Some tips for smoother debugging:

Always compile with -g for full debug info.

Use TUI mode (gdb -tui) for a richer interface.

Use a .gdbinit file to configure your environment and save time.

When your program crashes (e.g., segfaults), running it inside GDB and using backtrace helps find the cause.

Use info breakpoints to keep track of active breakpoints.

Links : 
[Gdb Manual test](https://sourceware.org/gdb/current/onlinedocs/gdb.html/)
Tutorial :
[GDB Tutorial by TutorialsPoint](https://www.tutorialspoint.com/gnu_debugger/index.htm)
[How to Debug C Programs with GDB (by MIT)](https://web.mit.edu/gnu/doc/html/gdb_toc.html)

Cheat sheet:
[GDB Cheat Sheet (by DarkDust)](https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf)
[GDB Cheatsheet on GitHub](https://github.com/cheat/cheatsheets/blob/master/gdb)