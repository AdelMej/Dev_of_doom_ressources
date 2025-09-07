# Vim/Neovim Macros Tutorial

Macros let you record and replay sequences of commands to automate repetitive tasks.

---

## 1. Recording a Macro

- Press `q` followed by a register key (`a` to `z`) to start recording.  
  Example: `qa` starts recording into register `a`.

- Perform the commands you want to record.

- Press `q` again to stop recording.

---

## 2. Playing Back a Macro

- To execute the macro once:  
  Press `@` followed by the register key.  
  Example: `@a` runs the macro stored in register `a`.

- To repeat the last run macro:  
  Press `@@`.

- To run a macro multiple times, prefix with a count:  
  Example: `5@a` runs macro `a` five times.

---

## 3. Viewing Macro Contents

- To see what’s inside a register (including macros):  
  Use `:reg a` to see register `a`.  
  You can replace `a` with any register name.

---

## 4. Practical Tips

- Use macros to fix repetitive formatting or edits in multiple lines.

- Combine macros with counts and motions for powerful automation.

- You can save useful macros by yanking them into your vim config or mapping them to keys.

---

## 5. Example

Suppose you want to add a semicolon at the end of multiple lines:

1. Go to the first line.

2. Start recording macro in register `a`: `qa`.

3. Move to end of line: `$`.

4. Insert semicolon: `a;` then `Esc`.

5. Move to next line: `j`.

6. Stop recording: `q`.

7. Run macro on next 9 lines: `9@a`.

---

## 6. Extra: Mapping Macros

You can map a macro to a key in your `init.vim` or `init.lua` like this:

```vim
" Map F5 to run macro 'a'
nnoremap <F5> @a
