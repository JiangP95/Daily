# Redis 
	是一款先进的key/value存储系统，他与Memcached类似区别如下：
	1. redis不仅支持简单的key/value类型的数据，还提供list，set，hash等数据接口的存储
	2. 支持数据的备份，即master，slave模式的数据备份
	3. 支持数据的持久化，可以将内存的数据保存在磁盘中使用，重启时再次加载使用。
-------
	在redis中，并不是所有数据都一直存储在内存中。redis只会缓存所有key信息，如果redis发现内存的使用量超过了某个阈值，将会触发交换操作。redis  swappabillity计算出哪些key对应的value需要交换到磁盘，然后再将这些key对应的value持久到磁盘中，同时在内存中清除。
    
    
# Redis 与 Memcached 比较
	1. Redis和Memcache都是将数据存放在内存中，都是内存数据库。不过memcache还可用于缓存其他东西，如图片，视频
	2. redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，hash等数据结构的存储
	3. 虚拟内存--redis当物理内存用完时，可以将一些很久没用到的value交换到磁盘。
	4. 过期策略--memcache在set时就指定，redis可以通过expire指定，例如，expire name 10
	5. 存储数据安全，memcache挂掉后，数据不可恢复，redis数据可以定期保存在磁盘中，重启的时候可以再次加载进行使用。
	6. 灾难恢复，memcache挂掉后，数据不可恢复，redis数据丢失后，可以通过aof恢复
	7. redis支持数据备份，即master-slave模式的备份

