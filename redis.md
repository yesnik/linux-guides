# Redis

## Commands

- `redis-cli` - run redis console
- `KEYS 'app:claim:2957*'` - find keys by pattern
- `set 'horizon:5' hello`
- `type 'horizon:123'` - get type of the value
- `redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del` - delete keys by pattern

### Get key value

- `GET <key>` - for type `string` 
- `HGETALL <key>` - for type `hash`
- `lrange <key> <start> <end>` - for type `lists` 
- `smembers <key>` - for type `sets` 
- `ZRANGEBYSCORE <key> <min> <max>` - for type sorted `sets`
