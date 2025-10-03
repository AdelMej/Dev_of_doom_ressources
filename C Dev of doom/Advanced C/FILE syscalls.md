# 🔹 Headers & types

```c
#include <unistd.h>   // read, write, close, pipe, dup, dup2, fork, exec #include <fcntl.h>    // open, O_* flags 
#include <sys/stat.h> // mode constants (S_IRUSR etc.) 
#include <errno.h> 
#include <stdio.h>    // perror 
#include <stdlib.h>`
```

Key types: `int` (file descriptor), `ssize_t` (read/write return), `off_t` (lseek), `mode_t` (permissions).

---

# 🔹 open(2) — open a file (syscall wrapper)

```c
int fd = open("file.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644); 
if (fd == -1) {
	perror("open"); /* handle error */ 
}
```

Flags you’ll often use:

- `O_RDONLY`, `O_WRONLY`, `O_RDWR`
- `O_CREAT` (needs mode arg), `O_TRUNC`, `O_APPEND`
- `O_NONBLOCK`, `O_SYNC`, `O_CLOEXEC` (atomically set close-on-exec)  
	Permissions (mode): e.g. `0644` -> owner rw, group r, other r.

---

# 🔹 read(2) / write(2)

```c
ssize_t r = read(fd, buf, sizeof buf); 
if (r == -1) { if (errno == EINTR) /* retry */; else perror("read"); }  

ssize_t w = write(fd, buf, nbytes); 
if (w == -1) { perror("write"); }`
```

Tips:

- `read`/`write` may return < requested (especially on non-blocking fds or pipes).
- Loop until you’ve read/written all bytes. Handle `EINTR` by retrying.
- `write()` is atomic for writes ≤ `PIPE_BUF` on pipes.

Helper loop for full write:

```c
ssize_t full_write(int fd, const void *buf, size_t count) {
     const char *p = buf;     
     size_t left = count;     
     while (left > 0) {
        ssize_t n = write(fd, p, left);         
        if (n == -1) {             
	        if (errno == EINTR) continue; 
	        return -1;
	    }         
		left -= n;         
		p += n;    
	}     
	return count; 
}
```

---

# 🔹 close(2)

Always `close(fd)` when done. Check errors (e.g., `EINTR` - retry until success or different error). Don’t reuse stale fds.

---

# 🔹 lseek(2)

Move file offset:

```c
off_t pos = lseek(fd, 0, SEEK_END); // get file size 
lseek(fd, 0, SEEK_SET); // rewind
```

---

# 🔹 pipe(2)

Create a one-way pipe: `pipefd[0]` = read end, `pipefd[1]` = write end.

```c
int pipefd[2]; 
if (pipe(pipefd) == -1) { perror("pipe"); }`
```

Typical pattern with `fork()`:

- Parent writes to pipe (close read end)
- Child reads from pipe (close write end)

Example:

```c
int pipefd[2];
pipe(pipefd);
pid_t pid = fork();
if (pid == 0) {     
	// child     
	close(pipefd[1]); // close write end     
	char buf[100];     
	ssize_t n = read(pipefd[0], buf, sizeof buf);     // ...     
	close(pipefd[0]);     
	exit(0); 
} else {     
	// parent     
	close(pipefd[0]); // close read end     
	write(pipefd[1], "hello", 5);     
	close(pipefd[1]); 
}
```

---

# 🔹 dup / dup2

`dup(oldfd)` returns a new fd (lowest unused) that refers to same open file description. `dup2(oldfd, newfd)` duplicates into `newfd` (closing `newfd` first if open).

Common use: redirect stdout/stderr to a file or pipe.

```c
int fd = open("out.log", O_WRONLY | O_CREAT | O_TRUNC, 0644); 
if (fd == -1) perror("open");  

// redirect STDOUT to fd 
if (dup2(fd, STDOUT_FILENO) == -1) perror("dup2"); 
// now writes to stdout go to out.log 
close(fd); // STDOUT still valid`
```

`dup3(oldfd, newfd, O_CLOEXEC)` is Linux-specific (allows flags) — sets `O_CLOEXEC` atomically.

---

# 🔹 close-on-exec

To avoid leaking fds into `exec`ed processes:

- Use `O_CLOEXEC` at `open()` or `pipe2(pipefd, O_CLOEXEC)`.
- Or set `FD_CLOEXEC` via `fcntl(fd, F_SETFD, FD_CLOEXEC)`.

---

# 🔹 Example: run a child that outputs to parent via pipe

Parent reads child's stdout:

```c
int pipefd[2]; pipe(pipefd); 
pid_t pid = fork(); 
if (pid == 0) {     
	// child     
	close(pipefd[0]);            // close read end     
	dup2(pipefd[1], STDOUT_FILENO); // redirect stdout to pipe write end     
	close(pipefd[1]);     
	execlp("ls", "ls", "-l", NULL); // child's output goes to parent     
	_exit(127); 
} else {     
	// parent     
	close(pipefd[1]);            // close write end     
	char buf[4096];     
	ssize_t n;     
	while ((n = read(pipefd[0], buf, sizeof buf)) > 0) {         
		write(STDOUT_FILENO, buf, n); // print child's output     
	}     
	close(pipefd[0]); 
}
```

---

# 🔹 Example: redirect child's stdin from a file (using dup2)

```c
int fd = open("input.txt", O_RDONLY); 
pid_t pid = fork(); 
if (pid == 0) {     
	dup2(fd, STDIN_FILENO);     
	close(fd);     
	execlp("prog", "prog", NULL);     _
	exit(127); 
} else {     
	close(fd); 
}
```

---

# 🔹 Atomicity & race notes

- `open(..., O_CREAT|O_EXCL, ...)` avoids TOCTOU when creating files.
- Use `O_APPEND` for safe concurrent append (kernel guarantees atomic append for each `write()`).
- Use `pipe2(..., O_NONBLOCK|O_CLOEXEC)` if you need non-blocking pipes / auto-CLOEXEC.

---

# 🔹 Error handling checklist

- Check return values for `-1` (`open`, `pipe`, `fork`, `dup2`, `read`, `write`).
- `errno == EINTR` → typically retry `read`/`write`/`open` loops.
- Handle partial writes (loop).
- Close unneeded fds in parent/child to avoid leaks and deadlocks.

---

# 🔹 Small quick-reference examples

Redirect output to a log (shell-like):

```c
int fd = open("log.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644); 
dup2(fd, STDOUT_FILENO); 
dup2(fd, STDERR_FILENO); 
close(fd); 
execlp("myprog", "myprog", NULL);`
```

Spawn child, feed it input from parent:

```c
int p[2]; pipe(p); // p[0]=read, p[1]=write 
if (fork() == 0) {     
	close(p[1]); 
	dup2(p[0], STDIN_FILENO); 
	close(p[0]);     
	execlp("sort", "sort", NULL); 
} 
close(p[0]); 
write(p[1], "z\nb\na\n", 6); 
close(p[1]);`
```

---

# 🔹 Final tips & gotchas

- Prefer `open`/`read`/`write` for low-level performance or when you control buffering; use `fopen`/`fread`/`fwrite` (stdio) for formatted IO and buffered convenience.
- Remember `STDIN_FILENO`, `STDOUT_FILENO`, `STDERR_FILENO` are `0,1,2`.
- Avoid using file descriptors > `FD_SETSIZE` if you plan to use `select()`. Use `poll()`/`epoll()` for large sets.
- Use `fcntl` to set non-blocking or close-on-exec when `open`/`pipe2` lack flags.