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
- `apt-cache show nginx` - search `nginx` package in system repos
- `apt-cache search open-jdk` - search for any package using the keyword related to its name or description
- `sudo dpkg --list | grep -i jdk` - search installed `jdk` packages
- `sudo apt purge openjdk-11-jre-headless` - remove installed package
