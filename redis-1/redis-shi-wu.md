# Redis事务

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

