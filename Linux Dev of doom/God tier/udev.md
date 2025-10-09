## 🧩 **1️⃣ What is udev?**

> **udev = user-space device manager for the Linux kernel.**

It listens for hardware events from the kernel (via **netlink**) and dynamically:

- Creates or removes `/dev/*` nodes
- Applies rules
- Triggers scripts or services

Basically, it makes `/dev` **come alive** 🧠

---

## ⚙️ **2️⃣ udev architecture**

|Component|Description|
|---|---|
|**Kernel**|Emits events (add/remove/change)|
|**udevd**|Listens and processes rules|
|**Rules**|Match devices and define actions|
|**Systemd**|Can be triggered by udev|
|**/dev**|Where device files appear|

---

## 🧱 **3️⃣ Event Types**

|Event|Meaning|
|---|---|
|`add`|Device plugged in or initialized|
|`remove`|Device unplugged|
|`change`|Attributes changed|
|`bind/unbind`|Device driver attached/detached|

---

## 🧰 **4️⃣ udev rule locations**

|Path|Purpose|
|---|---|
|`/usr/lib/udev/rules.d/`|Default system rules|
|`/etc/udev/rules.d/`|**Your custom rules (preferred)**|
|`/run/udev/rules.d/`|Runtime (temporary)|

Rules are read **in lexical order**, so:

- `10-` runs before `99-`
- Override with a higher number or same name

---

## ✍️ **5️⃣ Basic rule syntax**

Each line = 1 rule  
Format:

```ini
MATCH_KEY=="value", MATCH_KEY2=="value", ACTION_KEY+="command"
```

Example:

```bash
ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="1058", ATTR{idProduct}=="25a2", RUN+="/usr/local/bin/usb_backup.sh"
```

Meaning:

> When a USB device with vendor `1058` and product `25a2` is added, run `usb_backup.sh`.

---

## 🔍 **6️⃣ Common match keys**

|Key|Description|
|---|---|
|`ACTION`|add / remove / change|
|`KERNEL`|kernel device name (e.g., sda, ttyUSB0)|
|`SUBSYSTEM`|“usb”, “block”, “net”, etc.|
|`ATTR{}`|Match a sysfs attribute (e.g., `ATTR{serial}=="XYZ"`)|
|`ENV{}`|Match environment vars|
|`DRIVER`|Match specific driver|
|`TAG`|Add custom tags|
|`RUN`|Run a command/script|
|`IMPORT{program}`|Import env vars from command output|

---

## ⚡ **7️⃣ Example rules**

### 🔹 Auto-mount USB drive

```bash
ACTION=="add", SUBSYSTEM=="block", KERNEL=="sd*", ENV{ID_FS_TYPE}=="vfat", RUN+="/usr/local/bin/mount_usb.sh"
```

### 🔹 Start systemd service when USB is added

```bash
ACTION=="add", SUBSYSTEM=="usb", ENV{ID_VENDOR}=="Kingston", RUN+="/usr/bin/systemctl start usb-backup.service"
```
### 🔹 Create custom symlink for a device

```bash
KERNEL=="ttyUSB0", SYMLINK+="arduino"
```

Now you get `/dev/arduino` 💪

---

## 🧠 **8️⃣ Debugging & Testing**

|Command|Purpose|
|---|---|
|`udevadm info --attribute-walk --name=/dev/sda`|Inspect attributes of a device|
|`udevadm monitor --udev`|Watch live events|
|`udevadm test /sys/class/block/sda`|Simulate a rule|
|`udevadm control --reload`|Reload rules after edit|

---

## 🧩 **9️⃣ Permissions example**

Set ownership and mode automatically:

```bash
KERNEL=="ttyUSB0", MODE="0660", GROUP="dialout", SYMLINK+="arduino"
```

---

## 🧙‍♂️ **🔟 Pro Tips**

- Always put your rules in `/etc/udev/rules.d/`
- Test with `udevadm test` before plugging/unplugging devices
- Scripts run by `RUN+=` **must be short and non-blocking**
    - If you need something complex, trigger a **systemd service** instead
- Use `EnvironmentFile` in that service for configuration
- You can chain `udev → systemd → D-Bus` for clean event pipelines

---

## 🧩 **11️⃣ Chaining example**

**Goal:**  
When you plug in a USB backup drive → systemd runs backup → journald logs it → D-Bus announces it to your app.

### `/etc/udev/rules.d/99-usb-backup.rules`

```bash
ACTION=="add", SUBSYSTEM=="usb", ENV{ID_VENDOR}=="Kingston", RUN+="/usr/bin/systemctl start serverhub-backup.service"
```

### `/etc/systemd/system/serverhub-backup.service`

```ini
[Unit]
Description=USB Backup Service
After=local-fs.target

[Service]
ExecStart=/usr/local/bin/backup_script.sh
Restart=on-failure
```
🎯 Done: plug drive → backup auto-triggers → logs visible via journald → D-Bus can announce it.  
That’s _Linux automation Nirvana_, bro.

---

## ⚡ **12️⃣ TL;DR Summary**

|Action|Command / File|
|---|---|
|View device info|`udevadm info --name=/dev/sda`|
|Watch events live|`udevadm monitor --udev`|
|Reload rules|`udevadm control --reload`|
|Rule location|`/etc/udev/rules.d/*.rules`|
|Trigger systemd service|`RUN+="/usr/bin/systemctl start my.service"`|
|Inspect attributes|`udevadm info --attribute-walk --name=/dev/...`|