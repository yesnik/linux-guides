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
