# redis实现排行榜

### Redis命名规范

1.建议全部大写

2.key不能太长也不能太短,键名越长越占资源，太短可读性太差

3.key 单词与单词之间以  ： 分开

4.redis使用的时候注意命名空间，一个项目一个命名空间，项目内业务不同命名空间也不同。

```python
_FACEBOOK_ID_KEY_PATTERN = 'ranking:facebook-id:{name}:{season}:{group_id}'
_FACEBOOK_NAME_KEY_PATTERN = 'ranking:facebook-name:{name}:{season}:{group_id}'
_GROUP_INFO_KEY_PATTERN = 'ranking:group-info:{name}:{season}:{user_id}'
_LIST_KEY_PATTERN = 'ranking:list:{name}:{season}:{group_id}'
_JACKPOT_KEY_PATTERN = 'ranking:jackpot:{name}:{season}:{group_id}'
_COLLECTED_KEY_PATTERN = 'ranking:collected:{name}:{season}' 
```

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

### 具体操作简述

```python
group_info_key = GROUP_KEY_PATTERN.format(name='b_system', season=12, user_id=1234)
rds_pipeline = rds.pipeline()
rds_pipline.hgetall(group_info_key)
group_info = rds_pipline.execute()[0]
if not group_info:
    rds_pipline.hmset(group_info_key, {'layout':json.dumps([paid_num, free_num])}
    rds_pipline.execute()
group_id, layout = group_info['id'], json.loads(group_info['lay_out'])
facebook_id_key = FACEBOOK_ID_KEY_PATTERN.format(name='b_system', season=12, group_id=1)
facebook_name_key = FACEBOOK_NAME_KEY_PATTERN.format(name='b_system', season=12, group_id=1)
rds_pipeline.hget(facebook_id_key)
rds_pipeline.hget(facebook_name_key)
facebook_id, facebook_name = rds_pipeline.execute()
rds_pipline.delete(group_info_key)
rds_pipline.hdel(facebook_id_key, user_id)
rds_pipline.hdel(facebook_name_key, user_id)
rds_pipline.zrem(list_key, user_id)
if rds.hlen(facebook_id_key) == 0:
    pass
# 更新排行榜分数
rds.zadd(list_key, {user_id: score})
# 发奖时获取名次
raw_ranking_list = rds.zrevrange(list_key, 0, -1, withscores=True)
prev_score = 0
parallel_placing = 0
for index, (user_id, score) in enumerate(raw_ranking_list):
    if score < prev_score: parallel_placing = index
    prev_score = score
    if int(user_id) == user_id:
        return parallel_placing, score
# 展示时获取信息
rds_pipeline.zrevrank(list_key, user_id)
rds_pipeline.zscore(list_key, user_id)
rds_pipeline.hget(facebook_id_key, user_id)
rds_pipeline.hget(facebook_name_key, user_id)
user_ids = list(map(lambda x: x[0], raw_ranking_list))
# 加jackpot
rds.incrbyfloat(jackpot_key, float_num)
# 判断是否重复收奖
if not rds.sismember(collected_key, user_id):
    rds.sadd(collected_key, user_id)
```

### 一些额外的部分

1. 分房间，分等级，分付费用户

### 出错的地方

1. redis hash键值对，键和值都是字符串
2. 收奖，在多设备登录的时候需要做一下验证，

