# Btrfs `.config` Backup & Restore Tutorial

This guide shows how to **backup your `.config` directory** using Btrfs subvolumes and snapshots, and how to **restore it safely** if something breaks.

---

## 1. Create a backup subvolume

Create a dedicated subvolume to store `.config` backups:
```bash
sudo btrfs subvolume create /home/me/config-backup
```

Copy your .config into it (first-time setup):
```bash
rsync -a /home/me/.config/ /home/me/config-backup/
```

## 2. Take a snapshot of the backup

Create a snapshot with a timestamp for easy rollback:
```bash
sudo btrfs subvolume snapshot /home/me/config-backup /home/me/config-backup-YYYY-MM-DD
```

Example:
```bash
sudo btrfs subvolume snapshot /home/me/config-backup /home/me/config-backup-2025-08-27
```

Snapshots are immutable copies of the subvolume at a point in time.

You can take snapshots daily, weekly, or on important changes.

### 3. List existing snapshots
Check which snapshots exist:

```bash
sudo btrfs subvolume list /home/me
```
Example output:

```bash
ID 256 gen 100 top level 5 path config-backup
ID 257 gen 101 top level 5 path config-backup-2025-08-25
ID 258 gen 102 top level 5 path config-backup-2025-08-26
```
## 4. Restore from a snapshot

If .config breaks:
Rename the broken backup (optional):

```bash
sudo mv /home/me/config-backup /home/me/config-backup-broken
```

Restore the snapshot:
```bash
sudo mv /home/me/config-backup-2025-08-26 /home/me/config-backup
```

Sync the snapshot back to live .config:
```bash
rsync -a /home/me/config-backup/ /home/me/.config/
```

This safely restores deleted or corrupted files.

You can now continue working with a fully restored .config.

## 5. Cleanup old backups

Remove broken or unnecessary backup subvolumes to reclaim space:
```bash
sudo btrfs subvolume delete /home/me/config-backup-broken
```

## 6. Automate backups (optional)
Create a shell script that runs:

rsync live .config → backup subvolume

Take a Btrfs snapshot

Rotate old snapshots according to your retention policy

Schedule it with cron for automatic backups.
