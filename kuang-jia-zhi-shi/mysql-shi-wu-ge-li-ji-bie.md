# mysql事务隔离级别

#### MySQL事务隔离级别 <a id="3-mysql&#x4E8B;&#x52A1;&#x9694;&#x79BB;&#x7EA7;&#x522B;"></a>

* 事务隔离级别指的是在处理同一个数据的多个事务中，一个事务修改数据后，其他事务何时能看到修改后的结果。
* MySQL数据库事务隔离级别主要有四种：
  * `Serializable`：串行化，一个事务一个事务的执行。
  * `Repeatable read`：可重复读，无论其他事务是否修改并提交了数据，在这个事务中看到的数据值始终不受其他事务影响。
  * `Read committed`：读取已提交，其他事务提交了对数据的修改后，本事务就能读取到修改后的数据值。
  * `Read uncommitted`：读取未提交，其他事务只要修改了数据，即使未提交，本事务也能看到修改后的数据值。
  * MySQL数据库默认使用可重复读（ Repeatable read）。
* 使用乐观锁的时候，如果一个事务修改了库存并提交了事务，那其他的事务应该可以读取到修改后的数据值，所以不能使用可重复读的隔离级别，应该修改为读取已提交（Read committed）。
* 修改方式：

![](http://lf521.gitee.io/meiduoqiantai/orders/images/03%E4%BF%AE%E6%94%B9%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB1.png)

![](http://lf521.gitee.io/meiduoqiantai/orders/images/03%E4%BF%AE%E6%94%B9%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB2.png)



