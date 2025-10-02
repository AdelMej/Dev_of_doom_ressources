# 🔗 `xargs` Cheat Sheet

## 🔹 Basics

```bash
echo "file1 file2 file3" | xargs rm # → rm file1 file2 file3
```

Takes input (whitespace/newline separated) and runs it as arguments.

---

## 🔹 With `find`

```bash
find . -name "*.log" | xargs rm # delete all .log files
```

---

## 🔹 Safer (handle spaces, weird filenames)

```bash
find . -name "*.log" -print0 | xargs -0 rm
```

- `-print0` → null-terminated output
- `-0` → expect null input

---

## 🔹 Limit Arguments

```bash
echo {1..10} | xargs -n 3 echo 
# → echo 1 2 3 
# → echo 4 5 6 
# → echo 7 8 9 
# → echo 10
```

- `-n N` → max N arguments per command

---

## 🔹 Parallel Execution

```bash
echo {1..100} | xargs -n 10 -P 4 echo
```

- `-P 4` → run 4 commands at once
- `-n 10` → 10 args per command

---

## 🔹 Replace Placeholder

```bash
echo "file1 file2" | xargs -I {} echo "Processing {}" 
# → Processing file1 
# → Processing file2
```

- `-I {}` → replace `{}` with each argument

---

## 🔹 Read from File

```bash
xargs -a list.txt rm
```

- `-a file` → read arguments from file

---

## 🔹 Interactive / Prompt Before Running

`echo "file1 file2" | xargs -p rm # ask "rm file1 file2 ?..."`

---

## 🔹 Dry-Run (show commands before executing)

`echo "file1 file2" | xargs -t rm # prints: rm file1 file2`

---

⚡ **Pro tip:** Use `xargs` with `find`, `grep`, or even custom scripts for insane power. It’s like “batch mode” for the shell.