# redis实现排行榜

redis的zset可以很方便地用来实现排行榜功能

### 1.获取redis实例

```python
import redis
rds = redis.StrictRedis(host='localhost', port=6379, db=0)
```

### 2. 全量加入排行榜

```python
rds.zadd(name, score, member)  # 向key为name的zset加入一个member,权重为score
```

### 3. 获取某个member的排名

```python
rds.zrank(name, member)  # 获取member的排名，按score从小到大，从0开始
rds.zrevrank(name, member)  # 获取member的排名，按score从大到小，从0开始
```

### 4. 获取某个member的分数

```python
rds.zscore(name, member)
```

### 5. 获取排名在某个区间的元素

```python
rds.zrevrange(name, start, end, withsocres=False)  # 获取index从start到end的所有元素
```

