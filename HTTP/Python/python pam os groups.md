# 🧾 Python PAM + OS Groups Cheat Sheet

## 🔐 PAM (Authentication)
Install:
```bash
sudo apt install python3-pam   # Debian/Ubuntu
# or
sudo pacman -S python-pam      # Arch
```

Basic usage:
```python
from pam import pam

p = pam()
if p.authenticate("username", "password"):
    print("Authenticated successfully!")
else:
    print("Authentication failed!")
```

Optional parameters:
```python
p.authenticate(
    "username",
    "password",
    service="login",  # PAM service name (e.g. 'sudo', 'sshd', 'login')
    env={"REMOTE_ADDR": "127.0.0.1"}  # optional
)
```

---

## 👥 Working with System Users and Groups

Use built-in modules: `os`, `pwd`, and `grp`.

Get current user:
```python
import os
user = os.getlogin()
print(user)
```

Get user info:
```python
import pwd
info = pwd.getpwnam("username")
print(info.pw_uid, info.pw_gid, info.pw_dir, info.pw_shell)
```

Get all groups of a user:
```python
import os, grp

groups = [g.gr_name for g in grp.getgrall() if "username" in g.gr_mem]
print(groups)
```

Get group info:
```python
import grp
g = grp.getgrnam("sudo")
print(g.gr_gid, g.gr_mem)
```

Check if user is in a specific group:
```python
import grp

def in_group(user, group):
    g = grp.getgrnam(group)
    return user in g.gr_mem

print(in_group("username", "sudo"))
```

---

## 🛠 Combining PAM + Group Check
Authenticate a user and ensure they’re in a specific group:
```python
from pam import pam
import grp

def auth_and_check_group(user, password, group):
    p = pam()
    if not p.authenticate(user, password):
        return False, "Authentication failed"
    g = grp.getgrnam(group)
    if user not in g.gr_mem:
        return False, f"User not in group '{group}'"
    return True, "Access granted"

status, msg = auth_and_check_group("alice", "password123", "sudo")
print(msg)
```

---

## ⚙️ Managing Groups (System-Level)
System-level changes using `subprocess`:

```python
import subprocess

# Add user to group
subprocess.run(["sudo", "usermod", "-aG", "wheel", "username"], check=True)

# Remove user from group
subprocess.run(["sudo", "gpasswd", "-d", "username", "wheel"], check=True)
```
