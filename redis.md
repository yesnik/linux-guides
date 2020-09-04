# Redis

## Commands

- `redis-cli` - run redis console
- `redis-cli --bigkeys` - show biggest keys
- `keys app:claim:2957*` - find keys by pattern
- `debug object <key>` - show the size of the key in bytes - see param `serializedlength` in the output
- `set app:name MySite` - set the value of the key `app:name`
- `type horizon:123` - get type of the value
- `redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del` - delete keys by pattern

### Get key value

- `GET <key>` - for type `string` 
- `HGETALL <key>` - for type `hash`
- `lrange <key> <start> <end>` - for type `lists` 
- `zrange <key> 0 1000` - for type `zset`
- `smembers <key>` - for type `sets` 
- `ZRANGEBYSCORE <key> <min> <max>` - for type sorted `sets`
