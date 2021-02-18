# DNF

All software on a Fedora system is divided into *RPM packages*, which can be installed, upgraded, or removed.

## Commands

- `dnf check-update` - see which installed packages on your system have new versions available.
- `dnf upgrade` - attempt to install any available updates. If a package cannot be updated, 
because of dependency problems for example, it is skipped.
- `dnf upgrade python` - update only *python* package
