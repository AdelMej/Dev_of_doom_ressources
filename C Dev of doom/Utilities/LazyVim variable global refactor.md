# Vim One-liner for Multi-file Rename (Batch Refactoring)

You can use Vim’s `argdo` command from the shell to rename a variable or function **across multiple files** quickly.

---

## Basic usage (current directory only)

```bash
vim +"argdo %s/\<old_name\>/new_name/g | update" *.c *.h
```

Opens Vim

Runs substitution on all .c and .h files in the current directory

Substitutes all whole-word occurrences of old_name with new_name (without confirmation)

Saves files automatically if changed

Recursive usage (all files in subfolders)
```bash
vim +"argdo %s/\<old_name\>/new_name/g | update" $(find . -type f \( -name '*.c' -o -name '*.h' \))
```

Uses find to gather all .c and .h files recursively

Runs the same substitution and saves changes

Notes
The \< and \> ensure you only match whole words (avoiding partial matches).

Removing the g flag will only replace the first occurrence on each line.

This runs non-interactively — no confirmation prompts. Be sure you want to replace all matches!

Always back up or use version control before bulk renaming.

Example
To rename get_print_function to get_handler_function in all .c and .h files in the current directory:

```bash
vim +"argdo %s/\<get_print_function\>/get_handler_function/g | update" *.c *.h
```