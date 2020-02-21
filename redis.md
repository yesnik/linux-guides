# Redis

## Commands

- `redis-cli` - run redis console
- `KEYS 'app:claim:2957*'` - find keys by pattern
- `GET 'app:key:123'` - get key value
- `redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del` - delete keys by pattern
