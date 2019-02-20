# System boot

## Ubuntu

If you want to edit services started at system startup use following commands.

**Add service to startup**

```
sudo update-rc.d apache2 defaults
```

This command will create the symlink `/etc/rc0.d/K01apache2` to `/etc/init.d/apache2`.

**Remove service from startup**

```
sudo update-rc.d apache2 remove
```

This command will remove symlink from `/etc/rc0.d/K01apache2` to `/etc/init.d/apache2`.
It means that `apache2` will no longer be booted on system startup.
