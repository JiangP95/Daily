# Redis

>key-value结构的内存数据库。支持string，list，set，zset(sortedset 有序集合)，hash 等数据类型

**常用的数据结构：**

> 字符串(String)，散列/哈希(hash)，列表(list)，无序集合类型(set)，有序集合类型(sorted set)

**常用命令：**

> 登录 redis-cli -p 5566 -a password
>
> 检查key是否存在 EXISTS key
>
> 搜索某关键字 KSYS *4
>
> 返回一个Key所影响的vsl的类型 TYPE key
>
> - String 类型
>
>   设置一个键的值 SET key value
>
>   获取一个建的值 GET key
>
>   删除键对 DEL key
>
>   同时获取多个 mget key1 key2
>
> - Hash
>
>   设置一个hash HMSET key valueKey value --<key,<valueKey,value>>
>
>   获取hash所有key&value HGETALL key
>
>   获取hash所有key HKEYS key
>
>   获取hash所有keu的vslue HVALS key
>
>   获取hash内键值对的长度 HLEN key
>
>   给一个hash的某个键值对赋值 HSET key valueKey value
>
>   当hash中valueKey不存在时赋值 HSETNX key valueKey value
>
> - List
>
>   给list赋值 LPUSH listName value
>
>   按照索引取值 LINDEX listName 1
>
>   
>
>  incr key  递增
>
>  incrby key increment 递增指定整数
>
>  decr key
>
>  decrby key increment
>
>  zrange 命令会按照元素分数从小到大的顺序返回索引从 start 到 stop 之间的所有元素
>
>  zrange key start stop [ withscores ]
>
> expire key seconds  设置生存时间
>
> ttl  查询生存时间

