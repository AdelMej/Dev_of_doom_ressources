
## 1. Mount a Btrfs filesystem

Basic mount of a Btrfs device:

```bash
sudo mkdir -p /mnt/raidtest
sudo mount /dev/sdb /mnt/raidtest
```

/dev/sdb → replace with your Btrfs device
/mnt/raidtest → mount point directory

Btrfs automatically detects other devices in the same RAID array.

## 2. Mount a specific subvolume
To mount a particular subvolume instead of the whole filesystem:

```bash
sudo mkdir -p /mnt/raidtest-daily
sudo mount -o subvol=daily /dev/sdb /mnt/raidtest-daily
```
-o subvol=daily → mounts the daily subvolume

You can mount any subvolume this way to work on it independently.

## 3. List mounted filesystems
Check what is currently mounted:

```bash
mount | grep btrfs
```

Or:

```bash
sudo btrfs filesystem show
```
Shows devices, labels, and mount points.

## 4. Unmount a filesystem or subvolume
To safely unmount:

```bash
sudo umount /mnt/raidtest
```
Replace /mnt/raidtest with the mount point of the subvolume or filesystem.

Make sure no process is using the filesystem (you can check with lsof +D /mnt/raidtest).

## 5. Optional: Remount with different options
For example, mounting a subvolume as read-only:

```bash
sudo mount -o subvol=daily,ro /dev/sdb /mnt/raidtest-daily
```
ro → read-only, good for working on snapshots without risking changes.

