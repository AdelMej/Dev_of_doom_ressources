# [Valgrind](https://valgrind.org/) Tutorial for C: Memory Debugging Basics 🐛

Valgrind is a powerful tool to detect memory leaks, invalid memory access, and other related bugs in your C programs.

---

## 1. Compile your program with debug symbols

```bash
gcc -g -O0 your_program.c -o your_program
```

The -g flag includes debug info so Valgrind can give detailed reports.

The -O0 disables optimizations for clearer debugging.

## 2. Run your program with Valgrind
```bash
valgrind --leak-check=full ./your_program
```

--leak-check=full performs an in-depth memory leak analysis.

Valgrind runs your program and outputs a report detailing memory errors and leaks.

## 3. Understanding Valgrind output
Invalid read/write: Accessing memory out of bounds or freed memory.

Memory leaks: Allocated memory not freed before program ends.

Each error includes a stack trace pointing to the problem’s origin.

## 4. Useful Valgrind options
```bash
valgrind --leak-check=full --track-origins=yes ./your_program
```

--track-origins=yes: Tracks uninitialized value origins (helps find bugs but slows execution).

--show-leak-kinds=all: Shows all types of leaks (definite, possible, etc.).

--log-file=valgrind.log: Saves output to a file for easier review.

## 5. Example workflow
```bash
gcc -g -O0 myprog.c -o myprog
valgrind --leak-check=full --track-origins=yes ./myprog
```
Review the output carefully, fix the reported issues, and rerun Valgrind until clean.

## 6. Creating a .valgrindrc config file
To avoid typing long commands every time, create a .valgrindrc file in your home directory (~/.valgrindrc) with your preferred options:

```ini
--leak-check=full
--track-origins=yes
--show-leak-kinds=all
--error-limit=no
--log-file=valgrind.log
```

Now, running valgrind ./your_program will automatically use these settings.

## 7. Troubleshooting tips
Valgrind reports no errors but program crashes:
Make sure you compile with -g and without optimizations (-O0). Optimizations can sometimes confuse Valgrind.

False positives or suppressed errors:
Some libraries may cause known harmless errors. You can create suppression files or search for existing ones online.

Program runs very slowly under Valgrind:
Valgrind adds overhead. Be patient or use it only for critical debugging sessions.

## 8. Quick summary of useful Valgrind flags
Flag	Purpose
--leak-check=full	Perform detailed memory leak check
--track-origins=yes	Show origins of uninitialized values
--show-leak-kinds=all	Display all leak types
--log-file=valgrind.log	Save output to a log file
--error-limit=no	Do not limit number of reported errors

💡 Tip: Always fix the first reported errors first — they often cause cascading issues.

Links:
[manuals](https://valgrind.org/docs/manual/manual.html)
[memcheck](https://valgrind.org/docs/manual/mc-manual.html#mc-manual.suppression)
