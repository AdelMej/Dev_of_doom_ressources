# 🗂️ Basic Btrfs Setup Tutorial (VM Only)

This guide covers **creating a Btrfs filesystem, subvolumes, and snapshots** inside a single VM disk.

---

## 1. Check if the filesystem is Btrfs

```bash
findmnt -no FSTYPE /home
```

Output should be btrfs if your home is already on Btrfs.

If not, you can convert (optional, advanced) or format a partition as Btrfs.

## 2. Create a Btrfs subvolume
Pick a directory for backup-worthy data. Example:

```bash
sudo btrfs subvolume create /home/me/test-subvol
```

Creates an independent subvolume inside /home/me/

## 3. List Subvolumes
```bash
sudo btrfs subvolume list /home
```

Shows all subvolumes under /home

## 4. Create a Snapshot
```bash
sudo btrfs subvolume snapshot /home/me/test-subvol /home/me/test-subvol-snap
```
Instantly creates a copy of the subvolume

Snapshot is read-write and can be used as backup

## 5. Restore a Snapshot
```bash
sudo btrfs subvolume snapshot /home/me/test-subvol-snap /home/me/test-subvol
```
Restores content from snapshot to the original subvolume (or a new one)

## 6. Delete a Snapshot
```bash
sudo btrfs subvolume delete /home/me/test-subvol-snap
```

Frees space by removing the snapshot

## 7. Check Usage
```bash
sudo btrfs filesystem df /home
```

Shows space used by data, metadata, and snapshots

## 8. Tips
Start small: use one subvolume for testing

Snapshots are instant, so you can experiment without worrying about disk space

Keep consistent naming (test-subvol, test-subvol-snap) for easy restore