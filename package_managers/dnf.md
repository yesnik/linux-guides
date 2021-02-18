# DNF

All software on a Fedora system is divided into *RPM packages*, which can be installed, upgraded, or removed.

## Commands

- `dnf install elixir` - install package
- `dnf remove mc` - remove package
- `dnf check-update` - see which installed packages on your system have new versions available.
- `dnf upgrade` - attempt to install any available updates. If a package cannot be updated, 
because of dependency problems for example, it is skipped.
- `dnf upgrade python` - update only *python* package
- `dnf search php` - search "php" word in all RPM package names and summaries
- `dnf list installed` - list installed packages
- `dnf list elixir*` - list installed and available packages with "elixir" word
- `dnf repolist` - list repositories
- `dnf repository-packages fedora list` - list packages from `fedora` repo
- `dnf info elixir.x86_64` - show package's info
- `dnf history` - show the history of changes in packages

## Config

- `dnf config-manager --dump` - show all current config options with values

### Folders

- `/etc/dnf/dnf.conf` - main config files
- `/etc/yum.repos.d` - folder to define individual repositories. Example: `/etc/yum.repos.d/fedora.repo`
