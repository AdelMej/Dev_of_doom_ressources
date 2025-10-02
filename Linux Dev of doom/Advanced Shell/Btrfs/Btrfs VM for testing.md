# 🖥️ Fedora Btrfs Testing VM Setup

## 1. Download Fedora ISO
- Go to [Fedora Workstation](https://fedoraproject.org/en/workstation/download)
- Download the **x86_64 Live ISO** (e.g., `Fedora-Workstation-Live-42-1.1.x86_64.iso`)

---

## 2. Create QEMU script

Create a file named `btrfs.sh` with the following content:

```bash
#!/bin/bash
VM_NAME="fedora-btrfs"
ISO="Fedora-Workstation-Live-42-1.1.x86_64.iso"
DISK_MAIN="${VM_NAME}-disk.qcow2"
DISK1="${VM_NAME}-extra1.qcow2"
DISK2="${VM_NAME}-extra2.qcow2"

create_disks() {
	[ ! -f "$DISK_MAIN" ] && qemu-img create -f qcow2 "$DISK_MAIN" 30G
	[ ! -f "$DISK1" ] && qemu-img create -f qcow2 "$DISK1" 10G
	[ ! -f "$DISK2" ] && qemu-img create -f qcow2 "$DISK2" 10G
}

install_vm() {
	create_disks
	qemu-system-x86_64 -enable-kvm -m 4096 -smp 4 -name "$VM_NAME" \
		-boot d -cdrom "$ISO" \
		-drive file="$DISK_MAIN",format=qcow2 \
		-drive file="$DISK1",format=qcow2 \
		-drive file="$DISK2",format=qcow2 \
		-net nic -net user,hostfwd=tcp::2222-:22 -vga std
}

run_vm() {
	create_disks
	qemu-system-x86_64 -enable-kvm -m 4096 -smp 4 -name "$VM_NAME" \
		-drive file="$DISK_MAIN",format=qcow2 \
		-drive file="$DISK1",format=qcow2 \
		-drive file="$DISK2",format=qcow2 \
		-net nic -net user,hostfwd=tcp::2222-:22 \
		-vga virtio \
		-device usb-ehci,id=usb -device usb-tablet \
		-device usb-tablet
}

case "${1:-}" in
install) install_vm ;;
run) run_vm ;;
*) echo "Usage: $0 {install|run}" ;;
esac
```

Notes:
DISK1 & DISK2 = extra disks for Btrfs/RAID testing

-vga virtio + usb-tablet ensures mouse works

Port 2222 is forwarded for SSH

## 3. Install Fedora
Run the VM:

```bash
./btrfs.sh install
```
Complete installation on DISK_MAIN

Shutdown VM

 ## 4. Enable SSH

Run the VM:
```bash
./btrfs.sh run
```

Inside the VM:
```bash
sudo dnf install -y openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
```

Connect from host:
```bash
ssh -p 2222 yourusername@localhost
```

⚠️ If you see REMOTE HOST IDENTIFICATION HAS CHANGED:
```bash
ssh-keygen -R "[localhost]:2222"
```

## 5. Extra Disks for Btrfs Experiments
DISK1 & DISK2 are attached to VM

Create Btrfs pool:
```bash
sudo mkfs.btrfs -f /dev/vdb /dev/vdc
sudo mount /dev/vdb /mnt/test
```
Use these for subvolumes, snapshots, RAID tests, etc.

## 6. Tips
Use terminal-only workflow for realistic server testing
Keep backup-worthy folders in subvolumes
Test snapshot/restore commands safely before automating