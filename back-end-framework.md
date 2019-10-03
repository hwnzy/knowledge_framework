# back-end framework

### 程序所需库

```python
# 导入B级活动配置
from system_math.b_system_math.xxx.xxx_config import *
# 导入B活动数学类
from system_math.b_system.xxx import XxxMath
# 导入B级活动net_protocol
from net_protocol import CMD_COLLECT_XXX_A, CMD_COLLECT_XXX_B, CMD_XXX_PICK, CMD_GET_XXX_RANKING, CMD_XXX_BUY_PACKAGE, CMD_COLLECT_XXX_FINAL, CMD_XXX_GUIDE
# 导入redis模型类字段
from models.redis_orm.field import IntField, FloatField, ListField, DictField
# 导入协议处理装饰器
from utilities import handler
# 导入有排名功能的B类活动基类
from slots_game.b_system import BSystemRankingGame
# 导入奖励类
from slots_game.rewards import Rewards
# 导入daily_config
from slots_game import daily_config
```



