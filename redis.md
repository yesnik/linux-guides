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

### ZADD

This command is used to add one or more members to a sorted set, or to update the score for members that already exist in the sorted set.

```bash
ZADD scores 59 user1 90 user2 22 user3
# (integer) 3
```

See `ZRANGE` command to get results.

### ZRANGE

This command returns the specified range of elements in the sorted set stored at the specified key.

```bash
ZRANGE scores 0 -1
# 1) "user3"
# 2) "user1"
# 3) "user2"

ZRANGE scores 0 -1 REV
# 1) "user2"
# 2) "user1"
# 3) "user3"
```

We can use the `WITHSCORES` argument to include the scores in the result:

```bash
ZRANGE scores 0 -1 WITHSCORES
# 1) "user3"
# 2) "22"
# 3) "user1"
# 4) "59"
# 5) "user2"
# 6) "90"
```

### Get key value

- `GET <key>` - for type `string` 
- `HGETALL <key>` - for type `hash`
- `LRANGE <key> <start> <end>` - for type `lists` 
- `ZRANGE <key> 0 1000` - for type `zset`
- `SMEMBERS <key>` - for type `sets` 
- `ZRANGEBYSCORE <key> <min> <max>` - for type sorted `sets`
