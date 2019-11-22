# SSH commands

## Generate new SSH key pair

On your server execute command:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

This command will create 2 files: private key *id_rsa* and public key *id_rsa.pub*.

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

After this you can connect to remote server from your server:

```
ssh kenny@85.120.50.210
```

## Open SSH tunnel to remote server

If you need to forward requests from remote server to your local PC run on your local PC:

```
ssh -R 9009:localhost:9009 user@example.com
```
