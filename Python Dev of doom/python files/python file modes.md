
# 📄 Python File Modes Cheat Sheet

|Mode|Meaning|Creates file?|Overwrites?|Use case|
|---|---|---|---|---|
|`"r"`|Read (text)|❌ No|❌ No|Default. Read file.|
|`"w"`|Write (text)|✅ Yes|✅ Yes|Start fresh, overwrite file.|
|`"a"`|Append (text)|✅ Yes|❌ No|Add to end of file.|
|`"x"`|Create (text)|✅ Yes|❌ Error if exists|Safe file creation.|
|`"rb"`|Read (binary)|❌ No|❌ No|Open images, videos, etc.|
|`"wb"`|Write (binary)|✅ Yes|✅ Yes|Save images, etc.|
|`"ab"`|Append (binary)|✅ Yes|❌ No|Add to end of binary file.|
|`"rt"`|Read (text)|❌ No|❌ No|Same as `"r"` (default).|
|`"wt"`|Write (text)|✅ Yes|✅ Yes|Same as `"w"`.|
|`"at"`|Append (text)|✅ Yes|❌ No|Same as `"a"`.|
## ⚡ Notes:
- Always combine with encoding="utf-8" in text mode.
- Binary mode = no encoding (raw bytes).
- "+" can be added to allow both reading and writing ("r+", "w+", "a+", "rb+", etc.).