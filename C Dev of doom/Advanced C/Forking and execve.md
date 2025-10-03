# 🏭 C Process Management Cheat Sheet (`unistd.h`, `sys/types.h`, `sys/wait.h`)

---

## 👶 Process Creation: `fork()`

| Function   | Example              | Meaning                                      |
|------------|----------------------|----------------------------------------------|
| `fork()`   | `pid_t pid = fork();`| Creates a child process (duplicate of parent)|

**Return values:**
- `> 0` → PID of child (in parent process)  
- `0` → In child process  
- `< 0` → Error (process not created)  

---

## 🔄 Executing Programs: `exec*()` Family

| Function       | Example                              | Meaning                                      |
|----------------|--------------------------------------|----------------------------------------------|
| `execl()`      | `execl("/bin/ls", "ls", "-l", NULL);` | Executes program (path + args list)          |
| `execv()`      | `char *args[] = {"ls","-l",NULL}; execv("/bin/ls", args);` | Args as array |
| `execle()`     | `execl("/bin/ls","ls","-l",NULL,envp);` | Like `execl` + custom env |
| `execve()`     | `execve("/bin/ls", args, envp);`     | Most general form (path, argv[], envp[])      |
| `execlp()`     | `execlp("ls","ls","-l",NULL);`       | Uses `PATH` search                           |
| `execvp()`     | `execvp("ls", args);`                | Args as array + `PATH` search                |

**Notes:**
- If successful, `exec*` **replaces the current process image** (never returns).  
- If it returns, it means an error occurred (e.g., file not found).  

---

## ⚰️ Waiting for Child: `wait()`

| Function    | Example                     | Meaning                           |
| ----------- | --------------------------- | --------------------------------- |
| `wait()`    | `wait(&status);`            | Waits for child process to finish |
| `waitpid()` | `waitpid(pid, &status, 0);` | Waits for specific child          |
| Macros      | `WIFEXITED(status)`         | True if child exited normally     |
|             | `WEXITSTATUS(status)`       | Get child’s exit code             |

---

## 🧩 Typical Pattern

```c
pid_t pid = fork();

if (pid == 0) {
    // Child
    char *args[] = {"ls", "-l", NULL};
    execvp("ls", args);
    perror("exec failed"); // Only runs if exec fails
    exit(1);
} else if (pid > 0) {
    // Parent
    int status;
    waitpid(pid, &status, 0);
    if (WIFEXITED(status)) {
        printf("Child exited with %d\n", WEXITSTATUS(status));
    }
} else {
    perror("fork failed");
}
```

---

## ⚠️ Gotchas

- After fork(), both parent and child continue executing the next line.
- Remember to wait() in the parent to avoid zombie processes.
- exec*() doesn’t return on success — code after it is never reached.
- Always check errors (fork() can fail if resources are low).