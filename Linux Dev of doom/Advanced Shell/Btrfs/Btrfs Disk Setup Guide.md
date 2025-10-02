## 1. List all disks

Use lsblk to see all connected drives and partitions:
```bash
lsblk
```

NAME → device name (e.g., sda, sdb)

SIZE → drive size

TYPE → disk or part

MOUNTPOINT → where it’s mounted (if at all)

Example output:

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 931.5G  0 disk 
├─sda1   8:1    0 100.0G  0 part /home
└─sda2   8:2    0 831.5G  0 part /data
sdb      8:16   0 465.8G  0 disk

## 2. Format a disk with Btrfs

⚠️ Warning: This will erase all data on the disk. Make sure you pick the correct device!
```bash
sudo mkfs.btrfs -f /dev/sdX
```


-f → force format (overwrite existing filesystem)

/dev/sdX → replace with your target disk (e.g., /dev/sdb)

Optional: create a label for the filesystem:

sudo mkfs.btrfs -L MyBackup /dev/sdX

## 3. Verify the Btrfs filesystem
sudo btrfs filesystem show /dev/sdX


You should see the disk listed with the label and UUID.