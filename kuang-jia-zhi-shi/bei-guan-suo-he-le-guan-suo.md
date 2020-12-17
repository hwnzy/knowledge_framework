# 悲观锁和乐观锁

####  并发下单问题演示和解决方案 <a id="1-&#x5E76;&#x53D1;&#x4E0B;&#x5355;&#x95EE;&#x9898;&#x6F14;&#x793A;&#x548C;&#x89E3;&#x51B3;&#x65B9;&#x6848;"></a>

![](http://lf521.gitee.io/meiduoqiantai/orders/images/04%E5%B9%B6%E5%8F%91%E4%B8%8B%E5%8D%95%E9%97%AE%E9%A2%98%E6%BC%94%E7%A4%BA.png)

> **解决办法：**

* 悲观锁
  * 当查询某条记录时，即让数据库为该记录加锁，锁住记录后别人无法操作，使用类似如下语法

    ```text
    select stock from tb_sku where id=1 for update;

    SKU.objects.select_for_update().get(id=1)
    ```

  * 悲观锁类似于我们在多线程资源竞争时添加的互斥锁，容易出现死锁现象，采用不多。
* 乐观锁
  * 乐观锁并不是真实存在的锁，而是在更新的时候判断此时的库存是否是之前查询出的库存，如果相同，表示没人修改，可以更新库存，否则表示别人抢过资源，不再执行库存更新。类似如下操作

    ```text
    update tb_sku set stock=2 where id=1 and stock=7;

    SKU.objects.filter(id=1, stock=7).update(stock=2)
    ```





