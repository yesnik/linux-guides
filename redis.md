# Redis

## Commands

- `redis-cli` - run redis console
- `redis-cli --bigkeys` - show biggest keys
- `keys app:claim:2957*` - find keys by pattern
- `debug object <key>` - show the size of the key in bytes - see param `serializedlength` in the output. It is the size an objects would take when save to a RDB file on disk.
- `memory usage <key>` - the number of bytes that a key and its value require to be stored in RAM
- `set app:name MySite` - set the value of the key `app:name`
- `type horizon:123` - get type of the value
- `memory doctor` - reports about different memory-related issues that the Redis server experiences
- `redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del` - delete keys by pattern

### Get key value

- `GET <key>` - for type `string` 
- `HGETALL <key>` - for type `hash`
- `lrange <key> <start> <end>` - for type `lists` 
- `zrange <key> 0 1000` - for type `zset`
- `smembers <key>` - for type `sets` 
- `ZRANGEBYSCORE <key> <min> <max>` - for type sorted `sets`
