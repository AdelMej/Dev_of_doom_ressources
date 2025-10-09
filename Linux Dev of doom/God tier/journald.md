## 🧩 **1️⃣ What is `journald`?**

> `journald` is the **central logging service** used by systemd.

It collects logs from:

- system services
- user processes
- kernel messages
- stdout/stderr of daemons

…and stores them in a **binary log** (so it’s indexed and queryable).

---

## ⚙️ **2️⃣ Basic commands**

|Command|Description|
|---|---|
|`journalctl`|Show all logs|
|`journalctl -b`|Logs from **current boot**|
|`journalctl -b -1`|Logs from **previous boot**|
|`journalctl -k`|Kernel-only logs|
|`journalctl -u myapp.service`|Logs for a specific service|
|`journalctl -u myapp.service -f`|Follow logs live (like `tail -f`)|
|`journalctl -xe`|Show last logs + errors (great for debugging)|

---

## 🧱 **3️⃣ Filtering logs**

### 🕐 By time:

```bash
journalctl --since "10 min ago"
journalctl --since "2025-10-09 12:00:00" --until "2025-10-09 13:00:00"
```

### 🧍 By user:

```bash
journalctl _UID=1000
```

### 💻 By process / PID:

```bash
journalctl _PID=2345
```

### ⚙️ By binary:

```bash
journalctl /usr/bin/python3
```

### 🔌 By priority (log level):

|Level|Meaning|
|---|---|
|0|Emergency|
|1|Alert|
|2|Critical|
|3|Error|
|4|Warning|
|5|Notice|
|6|Info|
|7|Debug|

Example:

```bash
journalctl -p 3 -b     # Errors and above
journalctl -p warning  # All warnings
```

---

## 🧠 **4️⃣ Paging & navigation**

`journalctl` uses `less` under the hood.

|Key|Action|
|---|---|
|↑ / ↓|Scroll|
|`G`|Go to bottom|
|`g`|Go to top|
|`/word`|Search|
|`n` / `N`|Next / previous result|
|`q`|Quit|

---

## 🧩 **5️⃣ Following logs (like `tail -f`)**

```bash
journalctl -fu nginx.service
```

or follow all:

```bash
journalctl -f
```

Perfect for watching Flask/systemd logs in real time ⚡

---

## 📦 **6️⃣ Persistent logs**

By default, `journald` may use **volatile storage** (`/run/log/journal` → lost on reboot).

Make logs persistent:

```bash
sudo mkdir -p /var/log/journal
sudo systemctl restart systemd-journald
```

Now your logs survive reboots 💾

---

## 🧰 **7️⃣ Log management**

|Command|Description|
|---|---|
|`journalctl --disk-usage`|Check journal size|
|`sudo journalctl --vacuum-size=500M`|Limit total size to 500MB|
|`sudo journalctl --vacuum-time=7d`|Keep only 7 days of logs|
|`sudo journalctl --rotate`|Force new log file|

---

## 🧙‍♂️ **8️⃣ View service crash reasons**

If a service fails:

```bash
systemctl status myapp.service
```

then dig deeper:

```bash
journalctl -u myapp.service -n 50 -xe
```

This shows the last 50 lines with context + error hints ⚡

---

## 🪵 **9️⃣ Custom logging from your app**

In Python:

```python
import logging
from systemd.journal import JournalHandler

log = logging.getLogger('serverhub')
log.addHandler(JournalHandler())
log.setLevel(logging.INFO)

log.info("Server started!")
log.error("Something exploded!")
```

You’ll see this in `journalctl -u serverhub.service` 😎

---

## ⚡ **10️⃣ Combining filters**

You can combine multiple filters in one command:

```bash
journalctl -u nginx -u postgresql -p err --since today
```
→ shows today’s errors from both services.

---

## 🧠 **11️⃣ Exporting logs**

|Action|Command|
|---|---|
|Save all logs|`journalctl > all_logs.txt`|
|Compress logs|`journalctl|
|JSON output|`journalctl -o json-pretty`|
|Short readable output|`journalctl -o short-monotonic`|

---

## 🧩 **12️⃣ Logs from specific boot**

List all boots:

```bash
journalctl --list-boots
```

Example output:

```yaml
-1 6b73c1c8b17a4e69bb29c4b320e9ad8d Thu 2025-10-09 12:00:00 UTC—Thu 2025-10-09 17:00:00 UTC
 0 9a8df67a7c19452a8fa29bff0dbf842e Thu 2025-10-10 09:00:00 UTC—...
```

Then view logs for that boot:

```bash
journalctl -b -1
```

---

## 🧩 **13️⃣ Pro tips**

- Always pair `-xe` when debugging failures
- Combine `journalctl` + `grep` for fast searching:
	```bash
	journalctl -u serverhub.service | grep "error"
	```   
- Use `journalctl -o cat` for clean no-metadata output
- If you edit log limits → reload:
 ```bash
 sudo systemctl restart systemd-journald
 ```

---

## 🧙‍♂️ **14️⃣ TL;DR Summary**

| Task              | Command                           |
| ----------------- | --------------------------------- |
| Show logs         | `journalctl`                      |
| Service logs      | `journalctl -u myapp.service`     |
| Follow live logs  | `journalctl -fu myapp.service`    |
| Current boot only | `journalctl -b`                   |
| Only errors       | `journalctl -p err`               |
| Limit by time     | `journalctl --since "1 hour ago"` |
| Persistent logs   | `/var/log/journal/`               |
| Limit size        | `--vacuum-size=500M`              |
| Export JSON       | `-o json-pretty`                  |