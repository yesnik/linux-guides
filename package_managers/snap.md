# Snap

**Snap** is a software packaging and deployment system developed by Canonical for operating systems that use the Linux kernel. 
The packages, called *snaps*, and the tool for using them, *snapd*, work across a range of Linux distributions
and allow upstream software developers to distribute their applications directly to users. 
Snaps are self-contained applications running in a sandbox with mediated access to the host system.

It is available out-of-the-box on Ubuntu.

```bash
# Show info about package
snap info ruby

# Install ruby package
sudo snap install ruby --classic
```
