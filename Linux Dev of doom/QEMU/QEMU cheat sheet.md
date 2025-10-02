# ⚙️ QEMU Cheat Sheet (Starter)
## 🔹 Create a Virtual Disk
```bash
qemu-img create -f qcow2 disk.qcow2 20G
```

- -f qcow2 → format (supports snapshots, thin-provisioning)
- 20G → size

---

## 🔹 Basic Boot
```bash
qemu-system-x86_64 -hda disk.qcow2 -boot d -cdrom installer.iso -m 2G
```

- -hda → disk
- -boot d → boot from CD-ROM
- -cdrom → ISO file
- -m 2G → memory

---

## 🔹 UEFI Firmware
```bash
qemu-system-x86_64 -bios /usr/share/OVMF/OVMF_CODE.fd -hda disk.qcow2
```

---
## 🔹 CPU & Acceleration
```bash
qemu-system-x86_64 -enable-kvm -cpu host -smp 4 -m 4G
```

- -enable-kvm → use hardware acceleration
- -cpu host → passthrough host CPU
- -smp 4 → 4 cores

---
## 🔹 Networking
```bash
qemu-system-x86_64 -net nic -net user,hostfwd=tcp::2222-:22
```

- forwards host port 2222 → guest port 22 (SSH into VM via ssh -p 2222 user@localhost)

---
## 🔹 Snapshots (qcow2 only)
```bash
qemu-img snapshot -c snap1 disk.qcow2   # create snapshot
qemu-img snapshot -l disk.qcow2         # list snapshots
qemu-img snapshot -a snap1 disk.qcow2   # apply snapshot
```

---
## 🔹 Monitor Console

Inside QEMU, press:
```
Ctrl + Alt + 2   → enter monitor
Ctrl + Alt + 1   → back to VM
```
---

This is the barebones kit, but you can expand it with:
- GPU passthrough
- Multiple NICs / TAP bridges
- Cloud-init for automation