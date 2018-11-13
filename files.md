# Linux Files commands

- `du -hsc *` - show size of folders
- `du -hsx * | sort -rh` - show size of folders in DESC order
- `df -i` - show free inodes. Filesystems with lots of little files usually run out of inodes way before running out of space. If this is your case you'll need to rebuild the filesystem and use `mkfs -i` to increase the available inodes on it.
