# Enabling Core Dumps on Linux (for Automatic GDB Debugging)

Core dumps are files your system creates when a program crashes, saving its memory state so you can inspect what went wrong using tools like GDB.

---

## Step 1: Check Current Core Dump Limits

Open your terminal and run:

```bash
ulimit -c
```

If it returns 0, core dumps are disabled.

If it returns unlimited or a positive number, core dumps are enabled.

## Step 2: Enable Core Dumps Temporarily

To enable core dumps for the current terminal session:
```bash
ulimit -c unlimited
```

## Step 3: Enable Core Dumps Permanently
Edit /etc/security/limits.conf with root privileges:

```bash
sudo vim /etc/security/limits.conf
```

Add these lines at the end (replace * with your username for per-user config):

```markdown
*               soft    core            unlimited
*               hard    core            unlimited
```

Optionally, configure systemd core dumps in /etc/systemd/coredump.conf:

```ini
[Coredump]
Storage=external
Compress=yes
MaxUse=10G
```

## Step 4: Customize Core Dump File Location (Optional)

Check current core dump pattern:
```bash
cat /proc/sys/kernel/core_pattern
```

Set custom core dump file path and name pattern (example saves to /tmp):
```bash
echo "/tmp/core-%e-%p-%t" | sudo tee /proc/sys/kernel/core_pattern
```

Pattern variables:

%e — executable name

%p — PID

%t — UNIX timestamp

Step 5: Test Core Dumps
Compile a program that crashes (e.g., null pointer dereference).

Run it and check for core dump files in the configured directory.

Step 6: Debug with GDB
Load core dump file:

```bash
gdb ./your_program core-<executable>-<pid>-<timestamp>
```

Or use systemd’s coredumpctl:

```bash
coredumpctl gdb your_program
```

Bonus: Auto-Launch GDB on Crash
Use utilities like catchsegv or write a wrapper script to launch GDB automatically when your program crashes.

