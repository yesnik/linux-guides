# Timeout

Linux has `timeout` that can kill program after timeout:

```bash
# Stop script in 3 min
timeout 3m bash notify.sh

# Stop script in 10 sec
timeout 10s java MyApp
```
