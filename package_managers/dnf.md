# DNF

All software on a Fedora system is divided into *RPM packages*, which can be installed, upgraded, or removed.

## Commands

- `dnf install elixir` - install package
- `dnf -y install /path/to/package.rpm` - install package from RPM file
- `dnf remove mc` - remove package
- `dnf check-update` - see which installed packages on your system have new versions available.
- `dnf upgrade` - attempt to install any available updates. If a package cannot be updated, 
because of dependency problems for example, it is skipped.
- `dnf upgrade python` - update only *python* package
- `dnf search php` - search "php" word in all RPM package names and summaries
- `dnf list installed` - list installed packages
- `dnf list installed systemd --showduplicates` - list installed, available packages for `systemd`
- `dnf list elixir*` - list installed and available packages with "elixir" word
- `dnf repolist` - list repositories that are defined in the folder `/etc/yum.repos.d`
- `dnf repoquery --duplicates` - display packages that have several versions
- `dnf repository-packages fedora list` - list packages from `fedora` repo
- `dnf info elixir.x86_64` - show package's info
- `dnf history` - show the history of changes in packages
- `dnf --releasever=41 --setopt=deltarpm=false --setopt=protected_packages= distro-sync` - 

## Config

- `dnf config-manager --dump` - show all current config options with values
- `dnf config-manager --add-repo http://www.hello.com/example.repo` - add repo from URL
- `dnf config-manager --set-enabled repo_name` - enable repository
- `dnf config-manager --set-disabled repo_name` - disable repository

### Folders

- `/etc/dnf/dnf.conf` - main config files
- `/etc/yum.repos.d` - folder to define individual repositories. Example: `/etc/yum.repos.d/fedora.repo`

## Fedora Upgrade Errors

### Error with protected packages

Problem: The operation would result in removing the following protected packages: NetworkManager, grub2-tools-minimal, selinux-policy-targeted, setup, sudo, systemd, systemd-udev.

Solution:

```bash
sudo dnf system-upgrade download --releasever=41 --allowerasing
# or
dnf --releasever=41 --setopt=deltarpm=false --setopt=protected_packages= distro-sync
```
