# Supervisor

Supervisor is a great tool to guarantee that your worker process(es) is always running (even if it closes due to failure).

```bash
sudo apt install supervisor
```

Supervisor configuration files typically live in a `/etc/supervisor/conf.d` directory.

Create a new `messenger-worker.conf` file there to make sure that 2 instances of `messenger:consume` are running at all times:

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

Change `user` to the Unix user on your server. Next, tell Supervisor to read your config and start your workers:

```bash
sudo supervisorctl reread

sudo supervisorctl update

# Run all processes inside the group "messenger-consume"
sudo supervisorctl start messenger-consume:*
```
Run this command to tell supervisor to stop these workers:

```bash
sudo supervisorctl stop messenger-consume:*
```
