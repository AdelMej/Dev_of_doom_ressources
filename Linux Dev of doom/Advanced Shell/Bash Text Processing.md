# 🛠️ Text Processing Cheat Sheet

## 🔹 awk (field-based processing)
```bash
awk '{print $1}' file        # print first column
awk -F, '{print $2}' file    # delimiter = comma, print 2nd column
awk '{sum+=$2} END{print sum}' file   # sum col 2
awk '$3 > 50 {print $1,$3}' file      # if col3 > 50, print col1 & col3
```

✅ Columns, filters, quick math.

---
## 🔹 sed (stream editor)
```bash
sed 's/foo/bar/' file        # replace first 'foo' per line
sed 's/foo/bar/g' file       # replace all 'foo'
sed -i 's/foo/bar/g' file    # in-place replace
sed '/delete/d' file         # delete lines containing 'delete'
sed -n '5,10p' file          # print lines 5–10
```

✅ Substitution, deletion, line edits.

---

## 🔹 jq (JSON processor)
```bash
jq '.' data.json             # pretty-print JSON
jq '.name' data.json         # extract field
jq '.users[0].id' data.json  # first array element’s id
jq '.users[] | .name' data.json # list all user names
jq '.users[] | select(.age > 30)' data.json # filter by condition
```

✅ JSON querying & reshaping.

---

## 🔹 grep (pattern search)
```bash
grep "text" file             # match text
grep -i "text" file          # case-insensitive
grep -r "text" dir           # recursive search
grep -n "text" file          # show line numbers
grep -E "foo|bar" file       # regex OR
```

✅ Pattern search, regex-friendly.

---

# 🔹 cut (field extraction)
```bash
cut -d',' -f1 file.csv       # first field, comma delimiter
cut -c1-5 file.txt           # characters 1–5
```

✅ Fast column slicing.

---

## 🔹 rev (reverse lines)
```bash
rev file.txt                 # reverse each line
echo "hello" | rev           # -> "olleh"
cut -d',' -f1 file.csv | rev # reverse first field of CSV
```


✅ Reverse characters in each line.

---

## 🔹 tr (translate/delete chars)
```bash
tr 'a-z' 'A-Z' < file        # convert lowercase → uppercase
echo "hello123" | tr -d '0-9' # delete digits → "hello"
echo "foo_bar" | tr '_' '-'   # replace _ with -
tr -s ' ' < file             # squeeze repeated spaces → one space
```

✅ Character translation, deletion, squeezing.