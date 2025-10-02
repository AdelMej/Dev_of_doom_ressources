# 🐚 Shell Scripting Cheat Sheet
## 🔹 Script Setup
```bash
#!/bin/bash        # always first line (shebang)
set -e             # exit if a command fails (optional)
set -u             # error on undefined variables
```

## 🔹 Variables
```bash
name="Bazzite"
echo "Hello $name"   # use variable
echo "Hello ${name}" # braces safe when appending
```

## 🔹 Input
```bash
read var              # user input
read -p "Enter name: " name
```

## 🔹 Arguments
```bash
./script.sh arg1 arg2
$0    # script name
$1    # first arg
$2    # second arg
$@    # all args
$#    # number of args
```

## 🔹 Conditionals
```bash
if [ "$x" -eq 5 ]; then
  echo "x is 5"
elif [ "$x" -gt 5 ]; then
  echo "x > 5"
else
  echo "x < 5"
fi
```

## 🔹 Shell Operators Table

### 🧮 Numeric Comparison

|Operator|Meaning|Example|
|---|---|---|
|`-eq`|equal|`[ "$a" -eq "$b" ]`|
|`-ne`|not equal|`[ "$a" -ne "$b" ]`|
|`-lt`|less than|`[ "$a" -lt "$b" ]`|
|`-le`|less than or equal|`[ "$a" -le "$b" ]`|
|`-gt`|greater than|`[ "$a" -gt "$b" ]`|
|`-ge`|greater than or equal|`[ "$a" -ge "$b" ]`|

---

### 🔤 String Comparison

|Operator|Meaning|Example|
|---|---|---|
|`=`|equal|`[ "$a" = "$b" ]`|
|`!=`|not equal|`[ "$a" != "$b" ]`|
|`<`|less than (ASCII order)|`[ "$a" \< "$b" ]`|
|`>`|greater than (ASCII order)|`[ "$a" \> "$b" ]`|
|`-z`|string is empty|`[ -z "$a" ]`|
|`-n`|string is not empty|`[ -n "$a" ]`|

_(Notice `<` and `>` need escaping inside single brackets!)_

---

### 📂 File Tests

|Operator|Meaning|Example|
|---|---|---|
|`-e`|file exists|`[ -e file.txt ]`|
|`-f`|is a regular file|`[ -f file.txt ]`|
|`-d`|is a directory|`[ -d mydir ]`|
|`-s`|file exists and not empty|`[ -s file.txt ]`|
|`-r`|file is readable|`[ -r file.txt ]`|
|`-w`|file is writable|`[ -w file.txt ]`|
|`-x`|file is executable|`[ -x script.sh ]`|
|`-L`|is a symbolic link|`[ -L link ]`|
|`file1 -nt file2`|newer than|`[ file1 -nt file2 ]`|
|`file1 -ot file2`|older than|`[ file1 -ot file2 ]`|

---

### 🔀 Logical Operators

|Operator|Meaning|Example|
|---|---|---|
|`!`|NOT|`[ ! -f file.txt ]`|
|`-a`|AND (within `[ ]`)|`[ -f file.txt -a -r file.txt ]`|
|`-o`|OR (within `[ ]`)|`[ -d dir -o -f file.txt ]`|
|`&&`|AND (commands)|`cmd1 && cmd2`|

## 🔹 Loops
### for loop
```bash
for i in 1 2 3; do
  echo "Number $i"
done
```

### while loop
```bash
count=1
while [ $count -le 3 ]; do
  echo "Count $count"
  count=$((count+1))
done
```

### 🔹 Case Switch
```bash
case $var in
  start) echo "Starting";;
  stop)  echo "Stopping";;
  *)     echo "Unknown";;
esac
```

## 🔹 Functions
```bash
greet() {
  echo "Hello $1"
}
greet "World"
```

## 🔹 Arithmetic
```bash
x=$((3+2))      # 5
y=$((x*2))      # 10
```

## 🔹 Arrays
```bash
arr=(apple banana cherry)
echo ${arr[0]}      # apple
echo ${arr[@]}      # all elements
echo ${#arr[@]}     # array length
```

## 🔹 Redirection & Pipes
```bash
command > file      # stdout overwrite
command >> file     # stdout append
command 2> err.log  # stderr
cmd1 | cmd2         # pipe
```

## 🔹 Exit Codes
```bash
echo $?             # last command status
exit 0              # success
exit 1              # error
```