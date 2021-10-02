# Feature

- Single operation is atomic and multipe operation support transactions

# Data Structure

- String - not more than 512M, similar with Java ArrayList
- List - similar with Java LinkedList
  - Queue: RPUSH & LPOP
  - Stack: RPUSH & RPOP
- Hash - similar with Java HashMap
  - Resize when elements equals length of array, or lower than 10%
- Set - similar with Java HashSet
- Zset - 有序集合
  - Key + Score (sorting reference) + Value
  - HashTable (mapping value to score) + SkipList (mapping score to value)
  - if elements not more than 128 and each element not larger than 64 bytes, will use ziplist (a specially encoded dually linked list)
  - similar performance with Red-Black tree, but support range query and more simple implementation 

## ZSET Illustration

![拜托，面试别再问我跳表了！_有序集合_03](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/skiplist3.png)

![img](https://raw.githubusercontent.com/KOVERcjm/Pictures/master/ced2d46dd093e1999b2c1d58b4015f37.gif)



# Persistency

- RDB (Redis Database) - snapshot

- AOF (Append Only File) - operation log

|            | RDB    | AOF          |
| ---------- | ------ | ------------ |
| 启动优先级 | 低     | 高           |
| 体积       | 低     | 高           |
| 恢复速度   | 快     | 慢           |
| 数据安全性 | 丢数据 | 根据策略决定 |
| 量级       | 重量级 | 轻量级       |

# Prevent big key

- Can cause menory unbalanced in different nodes
- Can cause slow query or blocking Redis
- Can cause network congestion when frequently used
- Can cause blocking when expired

``` shell
redis-cli --bigkeys
```

# Problem with cache

- Cache Avalanche 缓存雪崩
  - Many keys expired in the same time, when expired, all queries go directly to database
  - Add random 1 to 5 miniutes to expire time
- Cache Pentration 缓存穿透
  - Many query for non-exist key in cache, making all these kind of queries go directly to database
  - Using Bloom Filter, or cache null with non-exist key (not recommended)
- Cache Hotspot Invalid 缓存击穿
  - One invalid key is frequently queried after expired, so the queries will go eirectly to database
  - Using SETNX to lock direct database query when cannot find in cache, or set no expire time

