---
title: 动手实现redis数据锁
comment: true
date: 2017-10-18 10:18:20
tags: [redis, Jedis, Java, 事务, 字段锁]
---

# 1.问题：

1. redis安装使用工作原理，
2. 操纵方式（使用jedis的 特定语言java下的数据操作方式）
3. jedis数据锁实现方式
4. 为什么要使用redis（[为什么要使用redis？](http://blog.csdn.net/yujin2010good/article/details/54729939)）

# 2.redis安装使用：

可以从菜鸟[redis教程](http://www.runoob.com/redis/redis-install.html)（主要看下安装配置）快速顺下来 ，然后看最后的java使用。
# 3.jedis使用

使用Jedis需要的第三方jar包：

> jedis-2.9.0.jar
>
> commons-pool2-2.4.2.jar (Jedis包依赖的包 - 抽象的池的管理工具)

简单jedis包类图：（Redis客户端：Jedis）
Jedis包类图：
![Jedis类图](http://ortur5wom.bkt.clouddn.com/redis.png)

> Jedis类是整个客户端的入口，通过Jedis可以建立与Redis Server的连接并发送命令。Jedis继承自BinaryJedis，同时Jedis和BinaryJedis都实现了很多接口，通过一张类图来描述：每一个接口都代表了一类Redis命令，例如JedisCommands中包含了SET GET等命令，MultiKeyCommands中包含了针对多个Key的MSET MGET等命令。一部分命令有两个版本的接口，如JedisCommands和BinaryJedisCommands。JedisCommands是字符串参数版本命令，会在Jedis内部将参数转换成UTF-8编码的字节数组。BinaryJedisCommands提供的是字节参数版本，允许用户自己决定编码等细节。ClusterCommands和SentinelCommands与集群、高可用等相关的命令只有一个版本。
>

# 4.动手实现redis数据锁

**重要：** ***（以下所有讨论涉及的代码下载链接，包含封装良好的加锁代码）***

## 4.1redis并发问题 

**问题：**

有个键，假设名称为`myNum`，里面保存的是阿拉伯数字，假设现在值为1，存在多个连接对`myNum`进行操作的情况，这个时候就会有并发的问题。假设有两个连接`linkA`和`linkB`，这两个连接都执行下面的操作，取出`myNum`的值`+1`，然后再存回去，看看下面的交互：

```
linkA get myNum => 1
linkB get myNum => 1
linkA set muNum => 2
linkB set myNum => 2
```



执行完操作之后，结果可能是2，这和我们预期的3不一致。

## 4.2尝试使用redis事务解决问题：



redis中也是有事务的，不过这个事务没有mysql中的完善，只保证了一致性和隔离性，不满足原子性和持久性。

redis事务使用multi、exec命令 - [菜鸟教程：redis事务的基本操作](http://www.runoob.com/redis/redis-transactions.html)

> 原子性，redis会将事务中的所有命令执行一遍，哪怕是中间有执行失败也不会回滚。kill信号、宿主机宕机等导致事务执行失败，redis也不会进行重试或者回滚。
>
> 持久性，redis事务的持久性依赖于redis所使用的持久化模式，遗憾的是各种持久化模式也都不是持久化的。
>
> 隔离性，redis是单进程，开启事务之后，会执行完当前连接的所有命令直到遇到exec命令，才处理其他连接的命令
>

经过代码实验，redis事务解决不了上面的问题。

当然了redis还有一个watch命令，这个命令可以解决这个问题，看下面的例子，对一个键执行watch，然后执行事务，由于watch的存在，他会监测键a，当a被修该之后，后面的事务就会**执行失败**，这就确保了多个连接同时来了，都监测着a，

**注意：**

 使用watch也只是保证了几个操作不能同时进行，但最终结果是几个并行操作中只有第一个操作执行成功，其他的都执行失败并不再执行。

即解决了能同时修改的问题，但是却只执行了单个任务，冲突的任务都没有执行。所以说使用redis内置事务解决不了问题，接下来尝试了解redis锁相关内容解决问题。

## 4.3为redis中的数据加锁

上面给出了源码链接，源码中已经将加锁操作进行了封装，可用在生产环境中没有问题。

**核心函数：**

redis中的setnx函数：

> 官方：Set `key` to hold string `value` if `key` does not exist.

使用`SETNX mykey "Hello"` ，有两种情况：

1. mykey不存在，新建`mykey`并设为`Hello`，返回值`1`
2. mykey已存在，不做操作，返回值`0`

``` java
// 为“aa”加字段锁
RedisLock lock = new RedisLock("aa",redisPool);
lock.lock(10000);
```

``` java
    /**
     * @param timeout 超时时间
     * @param expire  锁的超时时间（秒），过期删除
     * @return 成功或失败标志
     */
    public boolean lock(long timeout, int expire) {
        long nano = System.nanoTime();
        timeout *= MILLI_NANO_CONVERSION;
        try {
            while ((System.nanoTime() - nano) < timeout) {
              // 多个并行用户同时请求为字段"aa"加锁，只有一个能够加锁成功
              // 只允许加锁成功的用户对字段"aa"进行操作，即实现了字段锁的效果 
                if (this.jedis.setnx(this.key, LOCKED) == 1) {
                    this.jedis.expire(this.key, expire);
                    this.locked = true;
                    return this.locked;
                }
                // 短暂休眠，避免出现活锁
                Thread.sleep(3, RANDOM.nextInt(500));
            }
        } catch (Exception e) {
            throw new RuntimeException("Locking error", e);
        }
        return false;
    }
```

结束。




















