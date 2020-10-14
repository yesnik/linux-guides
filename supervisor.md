# Supervisor

Supervisor is a great tool to guarantee that your worker process is always running, even if it closes due to failure.

## Installation

```bash
# Ubuntu
sudo apt install supervisor
# Centos
sudo yum install supervisor
```

## Config

Supervisor configuration files typically live in:

- `/etc/supervisor/conf.d` (Ubuntu)
- `/etc/supervisord.d` (Centos)

Create a new `messenger-worker.conf` (Ubuntu) or `messenger-worker.ini` (Centos) file there to make sure that 2 instances of `messenger:consume` are running at all times:

```
;/etc/supervisor/conf.d/messenger-worker.conf
[program:messenger-consume]
command=php /path/to/your/app/bin/console messenger:consume async --time-limit=3600
user=ubuntu
numprocs=2
startsecs=0
autostart=true
autorestart=true
process_name=%(program_name)s_%(process_num)02d
```

Options:

- `[program:messenger-consume]` - name for the worker
- `user` - run command on behalf of this user
- `stdout_logfile=/var/log/worker.log` - output to file
- `autostart=true` - start worker with supervisor
- `autorestart=true` - autorestart worker on failure
- `numprocs=2` - number of instances of this worker

## Console commands

```bash
sudo supervisorctl reread

sudo supervisorctl update

# Add supervisor to autoload on the system boot
systemctl enable supervisord

# Restart supervisor
sudo service supervisor restart

# Run all processes inside the group "messenger-consume"
sudo supervisorctl start messenger-consume:*
```
Run this command to tell supervisor to stop these workers:

```bash
sudo supervisorctl stop messenger-consume:*
```
