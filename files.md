# Linux Files commands

## File and directory size

- `du -hsc *` - show size of folders
- `du -chxs * | sort -rh` - show size of folders in DESC order
- `df -i` - show free inodes. Filesystems with lots of little files usually run out of inodes way before running out of space. If this is your case you'll need to rebuild the filesystem and use `mkfs -i` to increase the available inodes on it.
- `df -h` - show amount of free space on disks
- `df -h .` - show available free space of the partition where your current working directory is located
- `df -hT .` - ... and additionally display file system's type

**Important:** If you have dot-folders in working folder, execute `shopt -s dotglob` to include them in the count.

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

- `sudo chown kenny:devops -R docker/` - change owner to `kenny`, group to `devops` of all files in `docker/` folder
- `sudo chown $USER:$USER -R docker/` - change owner, group of all files in `docker/` folder to `$USER`

### chmod

- `sudo chmod u+x main.py` - allow user (owner) to execute file
- `sudo chmod g+x main.py` - allow group to execute file
- `sudo chmod o-x main.py` - disallow others to execute file
- `sudo chmod +x main.py` - allow everyone to execute file 

## Archive

### Create archive

- `zip dump.sql.zip dump.sql` - create archive `dump.sql.zip` from file `dump.sql`
- `tar -zcvf archive.tar.gz -C /var/www ./` - create archive in current folder with the content of folder `/var/www`
- `tar -zcvf archive.tar.gz -C /var/www ./ --exclude=./uploads` - ... + exclude contents of `./uploads` folder
- `gzip file.txt` - creates archive `file.txt.gz` and deletes original file

### Extract archive

- `tar -zxvf archive.tar.gz -C /tmp` - extract the content of archive to `/tmp`
- `gunzip file.gz` - extracts archive and deletes archive file
- `tar -xf file.tar` - extracts archive's contents to current directory and doesn't delete archive file
