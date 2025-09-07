# 🚫 .gitignore for C Projects

A good `.gitignore` ensures you don’t accidentally commit build artifacts, logs, temporary files, or editor backups.

Here’s what you should include:

---

## 🛠 Typical C Project .gitignore

Ignore build directories and binaries
/build/
/bin/
/.o
/.out
/*.exe
*.a
*.so
*.dll

Ignore core dumps
core

Ignore dependency files
 \*.d

Ignore OS and editor temp files
\*~
\*.swp
\*.swo
.DS_Store
Thumbs.db

Ignore logs
\*.log

Ignore Unity test binaries (if using test/build directory)
test/build/

Ignore CMake or other build system files
CMakeFiles/
CMakeCache.txt
cmake_install.cmake
Makefile

Ignore project-specific settings (optional)
.vscode/
.idea/

---

## 💡 Tips

✅ **Always commit your `.gitignore` early** — this avoids polluting your repo with unwanted files.

✅ **Customize for your tools** — e.g., if you use `CLion`, add `.idea/`; if you use `VSCode`, add `.vscode/`.

✅ **Global .gitignore** — to ignore OS/editor files *everywhere*, set a global ignore file:
```bash
git config --global core.excludesfile ~/.gitignore_global
```
Then add your common ignores to ~/.gitignore_global.

📌 Useful Resources
[gitignore.io](https://www.toptal.com/developers/gitignore) — generate .gitignore files for multiple languages, IDEs, or frameworks.

[Official Git Documentation](https://git-scm.com/docs/gitignore)

Now your C project is protected from accidentally committing build junk or local configuration! 🚀