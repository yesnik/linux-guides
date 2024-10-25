# Ansible commands

- `ansible-doc -l` - list available modules. It has more that 3300 modules.
- `ansible-doc -s setup` - show docs on `setup` module

- `ansible servers -i /root/hosts -m ping` - execute `ping` module against servers in hosts file (see below)
- `ansible-inventory -i /root/hosts --list -y` - show host groups in Yaml format

## hosts file

This file has test and prod environments:

```
[servers]
controlplane
node01

[test_env]
controltest type=client
node01test  type=server

[prod_env]
controlprod type=client
node01prod  type=server

[non-prod:children]
servers
test_env
```
