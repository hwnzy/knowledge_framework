# Redis

## Redis事务

**基本事务指令**

Redis提供了一定的事务支持，可以保证一组操作原子执行不被打断，但是如果执行中出现错误，事务不能回滚，Redis未提供回滚支持。

* `multi` 开启事务
* `exec` 执行事务

```text
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set a 100
QUEUED
127.0.0.1:6379> set b 200
QUEUED
127.0.0.1:6379> get a
QUEUED
127.0.0.1:6379> get b
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) "100"
4) "200"
```

使用multi开启事务后，操作的指令并未立即执行，而是被redis记录在队列中，等待一起执行。当执行exec命令后，开始执行事务指令，最终得到每条指令的结果。

```text
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set c 300
QUEUED
127.0.0.1:6379> hgetall a
QUEUED
127.0.0.1:6379> set d 400
QUEUED
127.0.0.1:6379> get d
QUEUED
127.0.0.1:6379> exec
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
3) OK
4) "400"
127.0.0.1:6379>
```

如果事务中出现了错误，事务并不会终止执行，而是只会记录下这条错误的信息，并继续执行后面的指令。所以事务中出错不会影响后续指令的执行。

## Redis持久化

Redis可以将数据写入到磁盘中，在停机或宕机后，再次启动redis时，将磁盘中的备份数据加载到内存中恢复使用。这是redis的持久化。持久化有如下两种机制。

**RDB 快照持久化**

redis可以将内存中的数据写入磁盘进行持久化。在进行持久化时，redis会创建子进程来执行。

**redis默认开启了快照持久化机制。**

进行快照持久化的时机如下

* 定期触发

  redis的配置文件

  ```text
    #   save  
    #
    #   Will save the DB if both the given number of seconds and the given
    #   number of write operations against the DB occurred.
    #
    #   In the example below the behaviour will be to save:
    #   after 900 sec (15 min) if at least 1 key changed
    #   after 300 sec (5 min) if at least 10 keys changed
    #   after 60 sec if at least 10000 keys changed
    #
    #   Note: you can disable saving completely by commenting out all "save" lines.
    #
    #   It is also possible to remove all the previously configured save
    #   points by adding a save directive with a single empty string argument
    #   like in the following example:
    #
    #   save ""

    save 900 1
    save 300 10
    save 60 10000
  ```

* BGSAVE

  执行`BGSAVE`命令，手动触发RDB持久化

* SHUTDOWN

  关闭redis时触发

**AOF 追加文件持久化**

redis可以将执行的所有指令追加记录到文件中持久化存储，这是redis的另一种持久化机制。

**redis默认未开启AOF机制。**

redis可以通过配置如下项开启AOF机制

```text
appendonly yes  # 是否开启AOF
appendfilename "appendonly.aof"  # AOF文件
```

AOF机制记录操作的时机

```text
# appendfsync always  # 每个操作都写到磁盘中
appendfsync everysec  # 每秒写一次磁盘，默认
# appendfsync no  # 由操作系统决定写入磁盘的时机
```

使用AOF机制的缺点是随着时间的流逝，AOF文件会变得很大。但redis可以压缩AOF文件。

**结合使用**

redis允许我们同时使用两种机制，通常情况下我们会设置AOF机制为everysec 每秒写入，则最坏仅会丢失一秒内的数据。

