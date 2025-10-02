# Btrfs Subvolume & Snapshot Tutorial (RAID Test)

## 1. Mount the RAID filesystem

Assuming you already created the RAID and have a mount point:

```bash
sudo mkdir -p /mnt/raidtest
sudo mount /dev/sdb /mnt/raidtest
```

Btrfs will automatically recognize /dev/sdc as part of the RAID.

## 2. Create subvolumes
Subvolumes let you organize backups efficiently:

```bash
sudo btrfs subvolume create /mnt/raidtest/daily
sudo btrfs subvolume create /mnt/raidtest/weekly
sudo btrfs subvolume create /mnt/raidtest/monthly
sudo btrfs subvolume create /mnt/raidtest/yearly
```

Each subvolume acts like an independent folder that can have snapshots.

You can add more subvolumes later as needed.

## 3. List existing subvolumes
```bash
sudo btrfs subvolume list /mnt/raidtest
```
Example output:

```bash
ID 256 gen 5 top level 5 path daily
ID 257 gen 3 top level 5 path weekly
ID 258 gen 2 top level 5 path monthly
ID 259 gen 1 top level 5 path yearly
```

## 4. Create snapshots
Snapshots are read-only or read/write copies of subvolumes:

```bash
sudo btrfs subvolume snapshot /mnt/raidtest/daily /mnt/raidtest/daily-2025-08-27
```

Creates a snapshot of daily named daily-2025-08-27.

Snapshots are fast and space-efficient — only changes use extra space.

## 5. Restore from a snapshot (destructive)
To roll back a subvolume to a snapshot:

```bash
sudo mv /mnt/raidtest/daily /mnt/raidtest/daily-old
sudo mv /mnt/raidtest/daily-2025-08-27 /mnt/raidtest/daily
```
The old daily is kept as daily-old in case you need it.

The snapshot now becomes the active subvolume.

## 6. Optional: Delete old snapshots
```bash
sudo btrfs subvolume delete /mnt/raidtest/daily-old
```

Cleans up space by removing old snapshots you no longer need.