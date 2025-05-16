# Redis 

### 基于内存的key-value的结构数据库

读写性能高

适合存储热点数据  

企业应用广 泛

启动Redis命令：redis-server redis.windows.conf  再redis-cli



#### Redis的数据类型：key-value key是字符串类型 value有5中类型

​	string字符串

​	hash哈希 散列 类似（hashmap) 

​	list列表

​	set集合

​	sortedset/zset 有序集合

![image-20250514150253246](C:\Users\Administrator\Desktop\redis.assets\image-20250514150253246.png)

## Redis常用命令

#### 字符串命令：

​	1.set key value 设置指定key的值

​	2.get key 获取指定key的值

​	3.setex key seconds value 设置指定key的值 并将key的过期时间设置为seconds秒 

​	4.setnx key value只有在key不存在的时候设置key的值

#### 哈希操作命令：特别适合存储对象

​	1.HSET key field value 将哈希表key中的字段field的值设为value

​	2.HGET key field 获取存储在哈希表中的指定字段

​	3.HDEL key field 删除存储在哈希表中的指定字段

​	4.HKEYS key 获取哈希表中的所有字段

​	 5.HVALS key 获取哈希表中的所有值

#### 列表操作命令：简单的字符串列表，按照插入顺序排队：

LPUSH key value 1 [vlaue2] 将一个或多个值插入到列表头部         ------------------------------------L意思是LEFT

LRANGE key start stop 获取列表指定范围内的元素 0到-1是所有元素

RPOP key 移除并获取列表最后一个元素-------------------------------------R代表RIGHT

LLEN key 获取列表长度

#### 集合操作命令：是string类型的无序集合，成员唯一，不能重复

SADD key member1 [member2] 向集合中添加一个或者多个成员 ----------------------------S是set  add 

SMEMBERS key 返回集合中的所有成员

SCARD key 获取集合的成员数 

SINTER key1 [key2] 返回给定集合的交集

SUNION key[key2] 返回所有给定集合的并集

SREM key member1 [member2] 删除集合中的一个或者多个成员 ----------------rem是remove

#### 有序集合操作命令：string类型元素的集合，不允许重复，每个元素都会关联一个double类型的分数，用于排序

ZADD  key score1 member1 [sccore2 membner2] 向有序集合中添加一个或者多个成员---------------------Z是zset

ZRANGE  key start stop [WITHSCORES] 通过索引区间返回有序集合中指定区间内的成员 加上WITHSCORES会同时返回分数

ZINCRBY key increment member 有序集合中对指定成员的score 加上增量increment

ZREM key member [member...] 移除有序集合中的一个或者多个成员

#### 通用命令：不区分数据类型

KEYS pattern 查找所有符合给定模式（pattern)的key  比如KEYS * 返回所有的key

EXISTS key 检查给定的key是否存在

TYPE key  返回key所存储的值的类型

DEL key 该命令用于在key存在时删除key



## 在JAVA中操作Redis'

,;.:6*/‘



+
]']/=p0l=-有很多框架 可以在java中操作redis

常用的有S?有很多框架 可以在java中操作redis

常用的有Spring Data Redis  jedis lettuce



引入依赖  

配置数据源  在spring下 和datasource平级 redis : host:还有port: 还有password: 还有database(0-15)

编写配置类 

创建RedisTemplate对象 通过这个对象操作Redis

```JAVA
@Configuration
@Slf4j
public class RedisConfiguration{
    @Bean
    public RedisTemplate redisTemplate(RedisConnecitonFactory redisConnectionFactory){
        log.info("开始创建redis模板");
        RedisTemplate redisTemplate=new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConncetionFactory);
        redisTemplate.setKeySerialinzer(new StringRedisSerializer());
        return redisTemplate;
    }
   
}
```

之后就可以调用它的各种ops方法来操作redis数据库

比如集合的操作如下

![image-20250516154459185](C:\Users\Administrator\Desktop\rediss\redis.assets\image-20250516154459185.png)

# 上面都没用白学

# SpringCache

导入SpringCache依赖

@EnableCaching 开启缓存注解功能 通常用在启动类上

@Cacheable 在方法执行前查询缓存中是否有数据，如果有数据，则直接返回缓存数据；如果没有数据，调用方法并将方法返回值放到缓冲中

@CachePut(cacheNames="cache的名称" )将方法的返回值放到缓冲中    ==一般放在有返回值的方法上==        ==参数cacheNames关乎于key的生成==

@CacheEvict 将一条或多条数据从缓存中删除

![image-20250516161642306](C:\Users\Administrator\Desktop\rediss\redis.assets\image-20250516161642306.png)

![image-20250516162344403](C:\Users\Administrator\Desktop\rediss\redis.assets\image-20250516162344403.png)

![image-20250516162724536](C:\Users\Administrator\Desktop\rediss\redis.assets\image-20250516162724536.png)

只能返回保存为字符串 不能保存为其他的类型 还需要使用ops

