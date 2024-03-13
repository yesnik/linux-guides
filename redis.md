# Redis

- `redis-cli` - run redis console
- `redis-cli --bigkeys` - show biggest keys
- `redis-cli --raw keys "app:claim:2957*" | xargs redis-cli del` - delete keys by pattern

## Commands

- `SET age 22` - set key `age` value `22`
- `GET age` - get key `age` value. Returns: `"22"`
- `SET name Kenny EX 10` - set the value of the key `name`, in 10 sec. this key will be deleted
- `KEYS app:claim:2957*` - find keys by pattern
- `DEBUG object <key>` - show the size of the key in bytes - see param `serializedlength` in the output. It is the size an objects would take when save to a RDB file on disk.
- `MEMORY USAGE <key>` - the number of bytes that a key and its value require to be stored in RAM
- `SET app:name MySite` - set the value of the key `app:name`
- `TYPE horizon:123` - get type of the value
- `MEMORY DOCTOR` - reports about different memory-related issues that the Redis server experiences
- `MEMORY PURGE` - attempt to purge dirty pages so these can be reclaimed by the allocator
- `FLUSHDB` - delete all the keys of the currently selected DB

### Get key value

- `GET <key>` - for type `string` 
- `HGETALL <key>` - for type `hash`
- `LRANGE <key> <start> <end>` - for type `lists` 
- `ZRANGE <key> 0 1000` - for type `zset`
- `SMEMBERS <key>` - for type `sets` 
- `ZRANGEBYSCORE <key> <min> <max>` - for type sorted `sets`
