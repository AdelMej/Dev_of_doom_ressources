## 🧩 **Basics**

|Command|Description|
|---|---|
|`getenforce`|Shows current mode (Enforcing / Permissive / Disabled).|
|`setenforce 1`|Switch to **Enforcing** mode.|
|`setenforce 0`|Switch to **Permissive** mode.|
|`sestatus`|Full SELinux status overview.|
|`cat /etc/selinux/config`|View boot-time configuration.|

---

## ⚙️ **Modes**

- **Enforcing** → SELinux **blocks** unauthorized actions.
- **Permissive** → Logs denials but doesn’t block.
- **Disabled** → SELinux not active (don’t do this unless testing).

---

## 📂 **File Contexts**

|Command|Description|
|---|---|
|`ls -Z`|Show SELinux context (user, role, type, level).|
|`chcon -t httpd_sys_content_t /var/www/html/index.html`|Temporarily change context.|
|`restorecon -Rv /var/www/html`|Restore default SELinux contexts.|
|`semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"`|Make context **persistent** across reboots.|
|`restorecon -R /srv/web`|Apply persistent context.|

---

## 🔒 **Ports & Networking**

|Command|Description|
|---|---|
|`semanage port -l|grep http`|
|`semanage port -a -t http_port_t -p tcp 8080`|Allow httpd to use TCP port 8080.|
|`semanage port -d -t http_port_t -p tcp 8080`|Remove rule.|

---

## 🧠 **Booleans (Toggle Behaviors)**

|Command|Description|
|---|---|
|`getsebool -a`|List all SELinux booleans.|
|`getsebool httpd_can_network_connect`|Check specific boolean.|
|`setsebool -P httpd_can_network_connect on`|Permanently allow httpd to connect to network.|
|`setsebool -P ssh_sysadm_login on`|Allow sysadm to log in via SSH.|

---

## 🪵 **Logs & Debugging**

| Command                                | Description                                       |
| -------------------------------------- | ------------------------------------------------- |
| `sudo cat /var/log/audit/audit.log     | grep denied`                                      |
| `ausearch -m avc -ts recent`           | Search for recent access violations.              |
| `sealert -a /var/log/audit/audit.log`  | Analyze audit logs with suggestions.              |
| `audit2why < /var/log/audit/audit.log` | Explain why it was denied.                        |
| `audit2allow -a`                       | Generate custom policy modules (⚠️ advanced use). |

---

## 🧱 **Policy Modules**

|Command|Description|
|---|---|
|`audit2allow -M mymodule < /var/log/audit/audit.log`|Build a custom policy module from denials.|
|`semodule -i mymodule.pp`|Install custom module.|
|`semodule -l`|List loaded modules.|
|`semodule -r mymodule`|Remove a module.|

---

## 🧰 **Pro Tips**

- 🔸 Always test with **Permissive** mode first when debugging services.
- 🔸 Use **audit2allow** only for known safe actions.
- 🔸 Use **`system_u:system_r:service_t:s0`** contexts for daemons.
- 🔸 Keep custom services in `/usr/lib/systemd/system/` and label them correctly.
- 🔸 Never disable SELinux permanently — it’s part of becoming a true Linux god 😎