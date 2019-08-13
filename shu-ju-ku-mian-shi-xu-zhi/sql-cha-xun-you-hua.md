---
description: '8'
---

# SQL查询优化



* **避免全表扫描**，应考虑**在 where 及 order by 涉及的列上建立索引**；
* 查询时使用select**明确指明所要查询的字段**，避免使用`select *`的操作；
* **SQL语句尽量大写**，如：

  ```sql
    SELECT name FROM t WHERE id=1
  ```

  对于小写的sql语句，通常数据库在解析sql语句时，通常会先转换成大写再执行。

* **尽量避免在 where 子句中使用!=或&lt;&gt;操作符**， MySQL只有对以下操作符**才使用索引**：&lt;，&lt;=，=，&gt;，&gt;=，BETWEEN，IN，以及某些时候的LIKE；

  ```sql
    SELECT id FROM t WHERE name LIKE ‘abc%’
  ```

* 对于**模糊查询**，如：

  ```sql
    SELECT id FROM t WHERE name LIKE ‘%abc%’
  ```

  或者

  ```sql
    SELECT id FROM t WHERE name LIKE ‘%abc’
  ```

  **将导致全表扫描**，应避免使用，若要提高效率，可以考虑全文检索；

* **遵循最左原则**，在where子句中写查询条件时把**索引字段放在前面**，如：

  ```sql
    -- mobile为索引字段，name为非索引字段
    -- 推荐
    SELECT ... FROM t WHERE mobile='13911111111' AND name='python'
    -- 不推荐
    SELECT ... FROM t WHERE name='python' AND mobile='13911111111' 

    -- 建立了复合索引 key(a, b, c)
    -- 推荐
    SELECT ... FROM t WHERE a=... AND b=... AND c= ...
    SELECT ... FROM t WHERE a=... AND b=...
    SELECT ... FROM t WHERE a=...
    -- 不推荐 (字段出现顺序不符合索引建立的顺序)
    SELECT ... FROM t WHERE b=... AND c=...
    SELECT ... FROM t WHERE b=... AND a=... AND c=...
    ...
  ```

* **能使用关联查询解决的尽量不要使用子查询**，如

  ```sql
    -- 子查询
    SELECT article_id, title FROM t_article WHERE user_id IN (SELECT user_id FROM t_user  WHERE user_name IN ('itcast', 'itheima', 'python'))

    -- 关联查询(推荐)
    SELECT b.article_id, b.title From t_user AS a INNER JOIN t_article AS b ON a.user_id=b.user_id WHERE a.user_name IN ('itcast', 'itheima', 'python');
  ```

  能不使用关联查询的尽量不要使用关联查询；

* **不需要获取全表数据的时候**，不要查询全表数据，**使用LIMIT来限制数据**。

