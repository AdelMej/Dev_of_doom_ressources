# 🐚 Shell Cheat Sheet (Bash)
## 🔹 Basics
### Run a command
```bash
ls                # list files
pwd               # print working directory
cd /path          # change directory
clear             # clear terminal
```

## 🔹 Files & Directories
```bash
touch file.txt         # create empty file
mkdir dir              # create directory
cp file.txt new.txt    # copy file
mv old.txt new.txt     # rename/move
rm file.txt            # remove file
rmdir dir              # remove empty dir
rm -r dir              # remove dir + contents
 ```
 
## 🔹 Viewing & Editing
```bash
cat file.txt           # print file
less file.txt          # scroll file
head -n 10 file.txt    # first 10 lines
tail -n 20 file.txt    # last 20 lines
nano file.txt          # edit (or vim/emacs)
```

## 🔹 Permissions
```bash
ls -l                  # show permissions
chmod +x script.sh     # make executable
chmod 644 file.txt     # rw-r--r--
chown user:group file  # change owner
 ```
 
## 🔹 Processes
```bash
ps aux                 # list processes
top / htop             # live process view
kill PID               # kill by PID
killall name           # kill by name
jobs                   # background jobs
fg %1                  # bring job 1 to foreground
bg %1                  # resume job 1 in background
```

## 🔹 Redirection & Pipes
```bash
command > file         # stdout → file (overwrite)
command >> file        # stdout → file (append)
command 2> error.log   # stderr → file
command < input.txt    # use file as input
cmd1 | cmd2            # pipe output to another cmd
```

## 🔹 Searching
```bash
grep "text" file       # search in file
grep -r "text" dir     # search recursively
find /path -name "*.txt"   # find files
```

## 🔹 Archiving & Compression
```bash
tar -cvf archive.tar dir     # create tar
tar -xvf archive.tar         # extract tar
tar -czvf archive.tar.gz dir # create gzipped tar
tar -xzvf archive.tar.gz     # extract gzipped tar
```

## 🔹 Networking
```bash
ping google.com       # test connectivity
curl example.com      # fetch URL
wget example.com      # download file
ip a                  # show IP addresses
```

## 🔹 Package Management

### Ubuntu/Debian
```bash
sudo apt update && sudo apt upgrade
sudo apt install pkg
```

### Fedora
```bash
sudo dnf upgrade
sudo dnf install pkg
```

### Arch
```bash
sudo pacman -Syu
sudo pacman -S pkg
```

## 🔹 Shortcuts
```bash
Ctrl + C    # stop process
Ctrl + Z    # suspend process
Ctrl + D    # exit shell / EOF
Ctrl + R    # search command history
!!          # repeat last command
!n          # repeat command n from history
```