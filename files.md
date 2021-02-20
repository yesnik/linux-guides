# Linux Files commands

## File and directory size

- `du -hsc *` - show size of folders
- `du -chxs * | sort -rh` - show size of folders in DESC order
- `df -i` - show free inodes. Filesystems with lots of little files usually run out of inodes way before running out of space. If this is your case you'll need to rebuild the filesystem and use `mkfs -i` to increase the available inodes on it.
- `df -h` - show amount of free space on disks
- `df -h .` - show available free space of the partition where your current working directory is located
- `df -hT .` - ... and additionally display file system's type

**Important:** If you have dot-folders in working folder, execute `shopt -s dotglob` to include them in the count.

## Directories

- `mkdir -p mysite/logs/info` - create folder `info` with all *parent* directories: `mysite` and `logs`

## Files info

- `stat file.txt` - detailed info about file or folder
- `fdisk -l` - list the partition tables for the specified devices
- `lsblk` - list block devices

## Copy files

### scp

- `scp -r username@example.com:/file/to/copy /place/to/save` - copy file from remote to local
- `scp -r /file/to/send username@example.com:/place/to/save` - copy file from local to remote

### rsync

- `rsync -av --progress /opt mysite:/opt` - copy files from `/opt` to folder `/opt` on remote `mysite` host
- `rsync -avu --delete ./dir1/ mysite:/root/dir1` - copy contents of local `dir1` folder to remote `dir1` folder.
- `rsync -avu --delete --exclude='.env' --exclude='.git' --exclude='vendor' ./ sales-stage:/var/www/sales`

Options:
    - `-a` - archive mode; equals `-rlptgoD` (no -H,-A,-X). Do the sync preserving all filesystem attributes.
    - `-r` - recurse into directories
    - `-t` - preserve modification times
    - `-u` - skip files that are newer on the receiver
    - `-v` - info about what files are being transferred and a brief summary at the end - *stats1*.
    - `--exclude='.env'` - exclude local file `.env` from copying

## Useful commands

- `mv temp/hello.txt{,_old}` - rename file `temp/hello.txt` to `temp/hello.txt_old`
- `touch files/file{1..10}` - create 10 empty files in folder `files/`
- `sudo dd if=/dev/sda1 of=/dev/null` - copy disk `sda1` to nowhere: imitation of IO-load
- `iostat -hyx 1 3` - display disk IO load. If param `%util` is high then disk is working hard. 
- `iotop -o` - show programs that use IO now
- `lsof -u mysql` - list open files for user `root`
- `lsof -i` - selects  the  listing of files any of whose Internet address matches the address specified in i.
- `lsof -p 4101` - show files opened by the process with PID

## Search

- `grep -rnw ./ -e 'hello'` - search files with text `hello` in it
- `find -name '*.php'` - find all files with .php extension (in current folder and subfolders)

### Find .log files and delete them

- `find . -type f -name "*.log" -exec rm -f {} \;` - find `.log` files and delete them

### Search string in a file

1. Open file: `less errors.log`
2. To search string 'err' type `/err` and press 'Enter'
3. Press `n` to search next occurence
4. Press `N` to search previous occurence
5. Press `F` to continually read information from file and follow its end. Useful for logs watching.

## Permissions

### chown

- `chown kenny:devops -R docker/` - change owner to `kenny`, group to `devops` of all files in `docker/` folder
- `chown $USER:$USER -R docker/` - change owner, group of all files in `docker/` folder to `$USER`

### chmod

- `chmod u+x main.py` - allow user (owner) to execute file
- `chmod g+x main.py` - allow group to execute file
- `chmod o-x main.py` - disallow others to execute file
- `chmod +x main.py` - allow everyone to execute file 
- `chmod u+rwx,g+rx,o-rwx main.py` - assign these rights to file: `rwxr-x---`
- `chmod g+s myfolder` - new files in this folder will get this folder's group

### File rights

```
rwx r-x r--
user group others
```
We can define rights as digits. Useful hint:
- `r` - 4
- `w` - 2
- `x` - 1

```bash
chmod 751 main.py

ls -lah
# -rwxr-x--x  1 vagrant vagrant    9 May 21 17:59 main.py
```
```
7 = 4 + 2 + 1 => rwx
5 = 4 + 1     => r-x
1 = 1         => --x
```

### Sticky bit

```bash
ls -ld /tmp
# drwxrwxrwt 15 root root ...
```

A Sticky bi**t** is a permission bit that is set on a file or a directory that lets only the owner to delete or rename the file.

- `chmod +t /var/www/dev` - add sticky bit
- `chmod -t /var/www/dev` - remove sticky bit

### SUID bit. Set-user Identification

```bash
ls -ld /usr/bin/password
# -rwsr-xr-x 1 root root ...
```

If we execute a script with SUID bit, its effective UID becomes that of the owner of the file.
 
- `chmod u+s main.py` - add SUID bit
- `chmod u-s main.py` - remove SUID bit

### SGID bit. Set-group Identification

If we execute a script with SGID, it runs as if it were a member of the same group in which the file is a member.

- `chmod g+s main.py` - add SGID bit
- `chmod g-s main.py`

## Archive

### Create archive

- `tar zcvf access.tar.gz access.log` - create `tar.gz` archive of file (`z` - gzip, `c` - create)
- `tar zcvf archive.tar.gz -C /var/www ./` - create archive in current folder with the content of folder `/var/www`
- `tar zcvf archive.tar.gz -C /var/www/. ./` - .. including dot-files: `.env`, `.git`, etc.
- `tar zcvf archive.tar.gz -C /var/www ./ --exclude=./uploads` - ... + exclude contents of `./uploads` folder
- `zip -r node.zip . -x "**/node_modules/*"` - create archive `node.zip` from all files in current dir, exclude `node_modules` folders in all folders
- `zip dump.sql.zip dump.sql` - create archive `dump.sql.zip` from file `dump.sql`
- `gzip file.txt` - creates archive `file.txt.gz` and deletes original file

### Extract archive

- `tar zxvf archive.tar.gz` - extract the content of archive to current folder (`x` - extract). Archive won't be deleted.
- `tar zxvf archive.tar.gz -C /tmp` - extract the content of archive to `/tmp`
- `tar xf file.tar` - extract archive's contents to the current directory. Archive won't be deleted.
- `unzip qm.zip` - extract archive in the current directory
- `gunzip file.gz` - extracts archive and deletes archive file

## File links

A link is a connection between a file name and the actual data on the disk. Two main types of links: 
- *hard* links. Refer to the specific location of physical data.
  - `ln rep1.txt report.txt` - create hard link `report.txt` for file `rep1.txt`. These 2 names will be referring to the same data.
- *soft* (symbolic) links. It's a special file that points to another file or directory (target). It does not contain a copy of the target file's data. If you delete a symbolic link, the target is unaffected. Also, if the target of a symbolic link is deleted, moved, or renamed, the symbolic link is not updated. When this happens, the symbolic link is called "broken", "orphaned".
  - `sudo ln -snf /home/nik/projects/messenger/config/messenger-worker.conf ./` - create symlink `messenger-worker.conf` in the current folder
  - `ln -snf /var/www/builds/10 /var/www/current` - create symlink `current` to folder `10`
  
