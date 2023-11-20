# SSH commands

## Generate a new SSH key pair

```bash
ssh-keygen -t ed25519 -C "your-name@mail.com"
```

This command will create 2 files: private key `id_ed25519` and public key `id_ed25519.pub`.

```bash
# On legacy system that doesn't support the Ed25519 algorithm, use:
ssh-keygen -t rsa -b 4096 -C "your-name@mail.com"
```

This command will create 2 files: private key `id_rsa` and public key `id_rsa.pub`.

## Allow SSH connections to server

Add your public key to remote server. 
Copy the content of your local `.ssh/id_rsa.pub` to `.ssh/authorized_keys` of remote server:

```
ssh-copy-id kenny@85.120.50.210
```

**Important:** Ensure that files have proper rights on remote server:

```
sudo chown kenny:kenny -R /kenny/.ssh/
sudo chmod 700 /kenny/.ssh/
sudo chmod 600 /kenny/.ssh/authorized_keys
```

## Connect to remote server

```
# If SSH on port 22
ssh kenny@85.120.50.210

# If SSH on port 1022
ssh kenny@85.120.50.210 -p 1022
```

### `config` example

File `~/.ssh/config`:

```
Host mysite
HostName 73.31.69.200
User root
```

This will allow you to use `ssh mysite` command to connect via SSH to remote server.

## Open SSH tunnel to remote server

If you need to forward requests from remote server to your local PC run on your local PC:

```
ssh -R 9009:localhost:9009 user@example.com
```

## Execute commands on remote host

```bash
ssh sales-stage 'cd /var/www/esia-connector; pwd; date'
```
