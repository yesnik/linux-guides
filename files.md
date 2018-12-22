# Linux Files commands

## File and directory size

- `du -hsc *` - show size of folders
- `du -hsx * | sort -rh` - show size of folders in DESC order
- `df -i` - show free inodes. Filesystems with lots of little files usually run out of inodes way before running out of space. If this is your case you'll need to rebuild the filesystem and use `mkfs -i` to increase the available inodes on it.
- `df -h` - show amount of free space on disks

## Search

- `grep -rnw ./ -e 'hello'` - search files with text `hello` in it
- `find -name '*.php'` - find all files with .php extension (in current folder and subfolders)

## Archive

- `tar -zcvf archive.tar.gz -C /var/www ./` - create archive in current folder with the content of folder `/var/www`
- `tar -zxvf archive.tar.gz -C /tmp` - extract the content of archive to `/tmp`
