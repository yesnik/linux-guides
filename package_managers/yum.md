# Yum

- `yum update` - update packages' versions
- `yum repolist` - list the enabled repositories
- `yum repolist all` - list all repositories
- `yum-config-manager --add-repo https://example.com/docker-ce.repo` - add repository
- `yum search php-mbstring` - search repos which name contains substring "php-mbstring"
- `yum search netstat` - search repos that provide command "netstat"
- `yum install php-mbstring.x86_64` - install package by name
- `yum install /path/to/package.rpm` - install package from RPM-file
- `yum remove mc` - uninstall package
- `yum list installed | grep php` - show installed php packages
- `yum -y remove php*` - remove all php packages
