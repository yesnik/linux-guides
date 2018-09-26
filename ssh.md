# SSH commands

## Open SSH tunnel to remote server

If you need to forward requests from remote server to your local PC run on your local PC:

```
ssh -R 9009:localhost:9009 user@example.com
```
