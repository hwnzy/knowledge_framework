# 数据库进阶问题

## 数据库视图

### 1. 是什么

视图是虚拟的表，本身不包含数据，数据都存储在原始表中。

### 2. 创建视图

```text
CREATE VIEW myview AS
SELECT C1, Concat(C2, C3)
FROM mytable
WHERE C1 <= 2;
```

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171937.png)

### 3. 有什么用

* 简化复杂的查询，比如复杂的连接查询；
* 只使用实际表的一部分数据；
* 通过只给用户访问视图的权限，保证数据的安全性；
* 更改数据格式和表示。

### 4. 何时可以更新

因为视图不存储数据，所以更新视图需要去更新原始表。如果视图定义只依赖于一个原始表，就很容易进行更新操作。但如果视图定义中有以下操作，那么就不能进行视图的更新：

* 分组查询
* 连接查询
* 子查询
* Union
* 聚集函数
* DISTINCT
* 计算字段

## 乐观锁

### 1. 并发控制

在多线程环境下，为了保证线程安全，需要使用并发控制。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427165230.png)

数据库管理系统中有事务的概念，它是一组操作，并且满足 ACID 特性。一个事务可以看成一组任务，而任务是由线程驱动的，因此事务也可以并发地执行。并发执行多个事务时，为了保证每个事务都具有 ACID 特性，也同样需要使用并发控制。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427164629.png)

主要有两种并发控制方法：悲观并发控制和乐观并发控制，应该注意到它们都是思想，而不是具体的实现。

### 2. 悲观并发控制

悲观并发控制保持着悲观的态度，认为并发执行过程一定会出现问题，也就一定要做点什么去防止出现问题。

传统的锁机制都是悲观并发控制的实现，通过加锁从而保证多线程互斥地修改和访问共享数据，那么多线程就不会竞争使用共享数据，也就不会产生线程安全问题。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427165523.png)

在下面的 Java 示例中，通过使用 synchronized 锁机制为 add\(\) 和 get\(\) 方法加锁，从而保证了多线程互斥地修改和访问 cnt 共享数据。

```text
public class PressimisticLocking {

    private int cnt = 0;

    public synchronized void add() {
        cnt++;
    }

    public synchronized int get() {
        return cnt;
    }
}
```

在数据库管理系统中提供了两种锁：共享锁（S）和排它锁（X），也称为读锁和写锁。

* 一个事务对数据 A 加了 X 锁，就可以对 A 进行读取和更新，加锁期间其它事务不能对 A 加任何锁。
* 一个事务对数据 A 加了 S 锁，可以对 A 进行读取操作，但是不能进行更新操作，加锁期间其它事务能对 A 加 S 锁，但是不能加 X 锁。

MySQL 的 InnoDB 存储引擎可以使用以下方法分别加 S 锁和 X 锁：

```text
SELECT ... LOCK In SHARE MODE;
SELECT ... FOR UPDATE;
```

悲观并发控制能够保证线程安全性，但是无论共享数据是否真的会出现竞争，它都要进行加锁。而加锁操作需要涉及用户态核心态转换、维护锁计数器和检查是否有被阻塞的线程需要唤醒等操作，代价非常高。所以悲观并发控制只适合共享数据经常出现竞争的场景，但是对于共享数据基本不会出现竞争的场景，它花费了很多不必要的锁开销。

### 3. 乐观并发控制

针对悲观并发控制对于共享数据基本不会出现竞争的情况下也需要加锁的问题，出现了乐观并发控制。它保持乐观的态度，认为并发执行过程不会对共享数据出现竞争问题。它只在修改数据之后检测一下修改期间该共享数据有没有出现竞争问题，也就是说有没有其它线程也同样修改了该共享数据，如果没有则修改成功，如果有则需要重做。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171324.png)

#### CAS 实现

CAS 指令是硬件支持的操作：比较并交换（Compare-and-Swap），它需要有 3 个操作数，分别是内存地址 V、旧值 A 和新值 B。CAS 用于冲突检测，内存地址 V 中的值如果不等于旧值 A，则表示冲突，否则才将内存地址 V 的值更新为新值 B。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/otherae4136a8-83b2-4d02-bc14-dc6d4afc4a96.gif)

Java 中的 AtomicInteger 使用 CAS 来实现，以下使用 AtomicInteger 声明了一个共享变量 cnt，该共享变量在多线程环境下不会出现线程安全问题。

```text
public class AtomicExample {

    private AtomicInteger cnt = new AtomicInteger();

    public void add() {
        cnt.incrementAndGet();
    }

    public int get() {
        return cnt.get();
    }
}
```

AtomicInteger 的核心代码是 getAndAddInt\(\)，其中 var1 是内存地址 V，getIntVolatile\(var1, var2\) 得到旧值 A，而 var5 + var4 是新值 B。可以看到在检测到冲突时，AtomicInteger 采用不断重试的方式，直到不再发生冲突为止。

```text
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
```

如果一个变量初次读取的时候是 A 值，它的值被改成了 B，后来又被改回为 A，那 CAS 操作就会误认为它从来没有被改变过，这就是 ABA 问题。J.U.C 包提供了一个带有标记的原子引用类 AtomicStampedReference 来解决这个问题，它可以通过控制变量值的版本来保证 CAS 的正确性。

#### 使用版本号实现

在数据库管理系统中，可以为表增加一个 version 列，那么每个记录就可以维护一个版本号，每次修改时版本号加 1。在对记录进行修改之前先读出 version，并在修改后判断 version 是否发生改变。如果没有发生改变就表示没有发生冲突，执行提交，否则回滚。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427171209.png)

```text
start transaction;
select version from t_goods where id=#{id};
update t_goods set status=2,version=version+1 where id=#{id} and version=version;
commit;
```

## 死锁

### 1. 死锁为什么存在

为了保证多进程（或多线程）的安全性，一个进程在使用临界资源（必须互斥访问的资源）时需要对临界资源加锁，从而防止和其它进程同时使用该临界资源。如果有其它进程已经在使用该临界资源，那么就无法完成加锁操作，会处于一直等待的状态。在访问临界资源之后，需要对临界资源进行解锁，从而把临界资源让出来给其它进程使用。

但是当满足某些条件时，导致无法对资源完成解锁操作，那么其它想要访问这些资源的进程就无法完成加锁操作，而处于一直等待的状态。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427152711.png)

### 2. 死锁产生条件

* 互斥：每个资源要么已经分配给了一个进程，要么就是可用的，也就是说资源必须是临界资源。
* 占有和等待：已经得到了某个资源的进程可以再请求新的资源。
* 不可抢占：已经分配给一个进程的资源不能强制性地被抢占，它只能被占有它的进程显式地释放。
* 环路等待：有两个或者两个以上的进程组成一条环路，该环路中的每个进程都在等待下一个进程所占有的资源。

在下图中，有两个进程 P1 和 P2，以及两个资源 R1 和 R2。进程指向资源表示进程请求使用资源，资源指向进程表示资源已经分配给进程。下图出现了环路等待，所有进程都不能继续执行下去，而是处于一直等待状态。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427152838.png)

### 3. 死锁示例代码

以下示例代码模拟银行转账操作。Transfer 是转账类，实现了 Runnable 接口，可以被线程执行。在 run\(\) 方法中，为了保证 from 账户的扣钱操作和 to 账户的加钱操作具有原子性，需要使用 synchronized 同时锁住 from 和 to 对象。

```java
public class Transfer implements Runnable {

    private final Account from;
    private final Account to;
    private final int amount;

    public Transfer(Account from, Account to, int amount) {
        this.from = from;
        this.to = to;
        this.amount = amount;
    }

    @Override
    public void run() {
        synchronized (from) {
            synchronized (to) {
                from.amount = from.amount - amount;
                to.amount = to.amount + amount;
                System.out.println("success");
            }
        }

    }
}

public class Account {
    int amount;
}
```

启动了两个线程 t1 和 t2，并且新建两个账户 a1 和 a2，两个线程的转账对象不同。如果 t1 锁住 a1、t2 锁住了 a2， t1 想要锁住 a2、t2 想要锁住 a1，那么就出现环路等待。

```java
public class DeadLock {
    public static void main(String[] args) {
        Account a1 = new Account();
        Account a2 = new Account();
        Thread t1 = new Thread(new Transfer(a1, a2, 10));
        Thread t2 = new Thread(new Transfer(a2, a1, 10));
        t1.start();
        t2.start();
    }
}
```

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427153149.png)

为了更容易看到死锁效果，可以两个加锁操作之间为线程加一个停顿时间。

```java
@Override
public void run() {
    synchronized (from) {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        synchronized (to) {
            from.amount = from.amount - amount;
            to.amount = to.amount + amount;
            System.out.println("success");
        }
    }
}
```

### 4. 死锁处理

#### 4.1 死锁预防

死锁预防是在程序运行之前通过某些手段从而保证不发生死锁。

#### 4.1.1 破坏互斥条件 <a id="section411"></a>

临界资源需要互斥访问，所以基本不能破坏互斥条件。但也可以使用一些手段来使得多个进程可以用共享的方式去使用临界资源。例如假脱机打印机技术允许若干个进程同时输出，唯一真正请求物理打印机的进程是打印机守护进程。

#### 4.1.2 破坏占有和等待条件 <a id="section412"></a>

规定所有进程在开始执行前一次性获取全部资源。

但这种方式会增加开销，并使得并发程度变低。

对于上面的死锁示例代码，可以将锁的粒度变为 Account 类，那么获取该锁就相当于获取了所有 Account 对象锁。

```java
@Override
public void run() {
    synchronized (Account.class) {
        from.amount = from.amount - amount;
        to.amount = to.amount + amount;
        System.out.println("success");
    }
}
```

#### 4.1.3 破坏不可抢占条件 <a id="section413"></a>

规定在进程在请求资源失败时，释放它获得的所有资源。

#### 4.1.4 破坏环路等待 <a id="section414"></a>

给资源统一编号，进程只能按编号顺序来请求资源。

但有些资源本身并不具有编号属性，如果加上编号的话，那么会让程序逻辑变得复杂。

#### 4.2 死锁检测与恢复

不试图阻止死锁，而是当检测到死锁发生时，采取措施进行恢复。

主要有两种死锁检测方法：超时机制和检测是否存在环路。

在检测到发生死锁之后，可以使用进程回退或者事务回滚等机制，释放获取的资源，之后再重新执行。

InnoDB 存储引擎使用检测是否存在环路的方式，并且选择将回滚操作代价小的事务进行回滚。

### 5. 活锁与饥饿

活锁是指进程互相谦让，都释放资源给别的进程，导致资源在进程之间跳动但是进程却一直不执行。

饥饿是指将低优先级的进程长时间请求不到所需要的资源。但是饥饿中进程最后可以请求到资源，只要不再有高优先级的进程使用资源，这和死锁有所不同。

## DELETE、TRUNCATE 和 DROP

### 1. 作用

DELETE 删除表中 WHERE 语句指定的数据。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172519.png)

TRUNCATE 清空表，相当于删除表中的所有数据。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172344.png)

DROP 删除表结构。

![](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/other20190427172441.png)

### 2. 事务

* DELETE 会被放到日志中以便进行回滚；
* TRUNCATE 和 DROP 立即生效，不会放到日志中，也就不支持回滚。

### 3. 删除空间

* DELETE 不会减少表和索引占用的空间；
* TRUNCATE 会将表和索引占用的空间恢复到初始值；
* DROP 会将表和索引占用的空间释放。

### 4. 耗时

通常来说，DELETE &lt; TRUNCATE &lt; DROP。

