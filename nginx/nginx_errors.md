# Nginx Errors

### connect() to unix:/var/run/php-fpm/php-fpm.sock failed (11: Resource temporarily unavailable) while connecting to upstream

1. Maximum number of socket connections (mostly default is 128) is too low. View the current limit:
    ```bash
    cat /proc/sys/net/core/somaxconn
    ```
    Increase this limit:
    ```bash
    sudo echo "net.core.somaxconn = 65535" | tee -a /etc/sysctl.conf
    sudo sysctl -p
    ```
2. Perhaps it's lack of number of incoming connections backlog queue (mostly default is 1000). Check the current limit:
    ```bash
    cat /proc/sys/net/core/netdev_max_backlog
    ```
    Increase this limit:
    ```bash
    sudo echo "net.core.netdev_max_backlog = 65535" | tee -a /etc/sysctl.conf
    sudo sysctl -p
    ```
**Important:** If you changed `sysctl` restart `nginx`, `php-fpm`.
