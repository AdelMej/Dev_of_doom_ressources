Btrfs: Maximum Cooking 🍳

“Better Filesystem. Because your old ext4 can’t handle save points, snapshots, and ninja backups like this.”

# 1. Intro & Hook

Quick joke: “Btrfs = Better Filesystem. ext4 = fine… but boring.”

Fakémon-style evolution analogy:

Old FS -> ext4
Better FS -> Btrfs
Mega Cluster -> Btrfs + snapshots + rsync + fake NAS

# 2. Key Features of Btrfs

Copy-on-write (COW): saves old versions instead of overwriting

Snapshots: instantly save states for rollback

Subvolumes: lightweight internal partitions

Checksums: detects corruption automatically

Compression: transparent, space-saving

Send/Receive: easy incremental backups

# 3. VM Test Setup

Why test in a VM: “Break stuff without crying on the real server”

Ensures snapshots, backups, and rsync workflows work safely

Fun note: “I destroy my VM daily, server stays happy 😏”

# 4. Fake NAS Setup

It’s not a real NAS—just a random HDD connected to the server

Backups require SSH:

rsync -av -e ssh /mnt/data_snapshot/ user@fake-nas:/backup/location/


Multiple snapshots + rsync = quick and safe backups

# 5. Snapshots & Rsync Workflow

Take a snapshot

btrfs subvolume snapshot /mnt/data /mnt/data_snapshot


Sync with rsync

rsync -av /mnt/data_snapshot/ /backup/location/


Clean up old snapshots

btrfs subvolume delete /mnt/data_snapshot_old

# 6. Rsync Safety Tips

Only copies new or changed files → fast incremental backups

Pro tip: never run rsync recklessly as root on /etc 😏

“Yeah, I backup /etc, but don’t worry… no explosions occurred. This time.”

Use --exclude if needed for sensitive files, even though it’s your personal server

# 7. Systemd Automation

Automate snapshots + rsync with service + timer

Service (backup.service):

[Unit]
Description=Automated Btrfs Backup

[Service]
Type=oneshot
ExecStart=/path/to/backup_script.sh


Timer (backup.timer):

[Unit]
Description=Run Btrfs Backup Daily

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target


systemctl enable --now backup.timer → daily automated backups

Logs go to journalctl for easy monitoring

# 8. Fun Hooks / Humor

Snapshots = “save points in a game”

Rsync = “sending save points to a friend”

Fake NAS = “that friend is a bit lazy, but we love them anyway”

Segfault City callback:

“Last time we crashed memory; this time we crash files safely with snapshots and rsync”

Maximum Cooking line:

“Btrfs + rsync + systemd = maximum cooking, zero crying”

# 9. Closing

Highlight: safe, efficient, reversible backups

Humor + story: “Controlled chaos, personal server style 😏”