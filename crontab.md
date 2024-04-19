# Crontab

## Commands

- `crontab -l` - list current user's crontab
- `crontab -e` - edit current user's crontab
- `crontab -u nginx -e` - edit crontab of *nginx* user

## Show crontabs for all users

```bash
for user in $(cut -f1 -d: /etc/passwd); do echo "Crontab for $user:"; sudo crontab -u $user -l; done
```

## Examples

#### Write STDIN, STDERR to separate files

```bash
*/10 * * * * /usr/bin/php /var/www/site/current/yii upload-data/index > /var/www/shared/site/logs/upload-data.log 2> /var/www/shared/site/logs/upload-data.error.log
```

#### Prevent more than one execution at once

Attempt to execute command will be ignored if it is running already:

```bash
* * * * * /usr/bin/flock -xn /tmp/clientdataconsumer.lock -c 'php /var/www/script.php > /dev/null'
```
