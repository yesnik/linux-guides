# Apt

Apt stores a list of repositories or software channels in the file

`/etc/apt/sources.list`

and in any file with the suffix `.list` under the directory

`/etc/apt/sources.list.d/`

## Commands

- `apt list --installed` - show installed packages
- `sudo apt remove mongodb-org` - remove installed package by name
- `apt search lmysqlclient` - find packages that contain phrase *lmysqlclient*
- `sudo apt update` - update versions of packages
