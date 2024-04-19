# flock

`flock` (file lock) locks the file so only one process can access the file.

- `flock ./file.lock --command "sleep 3"` - wait till the lock is released (the default behavior)
- `flock -n ./file.lock --command "sleep 3"` - stop execution if the file is locked
