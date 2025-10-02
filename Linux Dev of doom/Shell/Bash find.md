# 🔎 `find` Command Cheat Sheet

## 🔹 Basic Usage

```bash
find /path -name "file.txt"       # find exact name
find . -name "*.txt"              # all .txt files in current dir
find . -iname "*.txt"             # case-insensitive
```

---

## 🔹 File Type

```bash
find . -type f                    # regular files
find . -type d                    # directories
find . -type l                    # symbolic links
```

---

## 🔹 Time Filters

```bash
find . -mtime -1                  # modified in last 24 hours
find . -mtime +7                  # modified more than 7 days ago
find . -atime -1                  # accessed in last 24 hours
find . -ctime -2                  # metadata changed last 2 days
```

---

## 🔹 Size Filters

```bash
find . -size +100M                # larger than 100MB
find . -size -10k                 # smaller than 10KB
```


---

## 🔹 Permissions & Ownership

```bash
find . -user alice                # owned by user
find . -group devs                # owned by group 
find . -perm 644                  # exact permissions 
find . -perm -u=x                 # user has execute bit
```

---

## 🔹 Combine Conditions

```bash
find . -type f -name "*.log" -size +1M 
find . \( -name "*.tmp" -o -name "*.bak" \)   # OR condition`
```

---

## 🔹 Execute Commands

```bash
find . -name "*.log" -exec rm {} \;    # delete log files 
find . -type f -exec ls -lh {} \;      # list details of matches 
find . -name "*.sh" -exec chmod +x {} \;
```

✅ `{}` = placeholder for each file, `\;` ends the command.  
✅ Use `+` instead of `\;` to batch files:

```bash
find . -name "*.log" -exec rm {} +
```

---

## 🔹 Depth & Pruning

```bash
find . -maxdepth 1 -type f          # only current directory 
find . -mindepth 2                  # skip top-level 
find . -type d -name ".git" -prune  # skip .git directories
```

---

## 🔹 Output Tricks

```bash
find . -print0 | xargs -0 ls -lh     # safe for filenames with spaces
```

---

🔥 Pro tip: Pair `find` with `xargs`, `grep`, `tar`, or `rsync` to unleash its real power.