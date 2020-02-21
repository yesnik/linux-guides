# Redis

## Commands

### Run redis console

```bash
redis-cli
```

### Find keys

```bash
KEYS 'app:claim:2957'
```

### Delete keys by pattern

```bash
redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del
```
