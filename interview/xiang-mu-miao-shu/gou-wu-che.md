# 购物车

## 购物车存储方案 <a id="&#x8D2D;&#x7269;&#x8F66;&#x5B58;&#x50A8;&#x65B9;&#x6848;"></a>

![](http://lf521.gitee.io/meiduoqiantai/carts/images/01%E6%B7%BB%E5%8A%A0%E8%B4%AD%E7%89%A9%E8%BD%A6%E5%87%BA%E5%8F%91%E7%82%B9.png)

![](http://lf521.gitee.io/meiduoqiantai/carts/images/04%E5%B1%95%E7%A4%BA%E8%B4%AD%E7%89%A9%E8%BD%A61.png)

> * 用户登录与未登录状态下，都可以保存购物车数据。
> * 用户对购物车数据的操作包括：**`增`、`删`、`改`、`查`、`全选`**等等
> * **每个用户的购物车数据都要做唯一性的标识。**

#### 1. 登录用户购物车存储方案 <a id="1-&#x767B;&#x5F55;&#x7528;&#x6237;&#x8D2D;&#x7269;&#x8F66;&#x5B58;&#x50A8;&#x65B9;&#x6848;"></a>

> **1.存储数据说明**

* 如何描述一条完整的购物车记录？
  * **`用户itcast，选择了两个 iPhone8 添加到了购物车中，状态为勾选`**
* 一条完整的购物车记录包括：`用户`、`商品`、`数量`、`勾选状态`。
* **存储数据：`user_id`、`sku_id`、`count`、`selected`**

> **2.存储位置说明**

* 购物车数据量小，结构简单，更新频繁，所以我们选择内存型数据库Redis进行存储。
* **存储位置：`Redis数据库 4号库`**

```text
"carts": {
    "BACKEND": "django_redis.cache.RedisCache",
    "LOCATION": "redis://127.0.0.1:6379/4",
    "OPTIONS": {
        "CLIENT_CLASS": "django_redis.client.DefaultClient",
    }
},
```

> **3.存储类型说明**

* 提示：我们很难将**`用户、商品、数量、勾选状态`**存放到一条Redis记录中。所以我们要把购物车数据合理的分开存储。
* **用户、商品、数量：`hash`**
  * `carts_user_id: {sku_id1: count, sku_id3: count, sku_id5: count, ...}`
* **勾选状态：`set`**
  * 只将已勾选商品的sku\_id存储到set中，比如，1号和3号商品是被勾选的。
  * `selected_user_id: [sku_id1, sku_id3, ...]`

![](http://lf521.gitee.io/meiduoqiantai/carts/images/02Redis%E5%AD%98%E5%82%A8%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%95%B0%E6%8D%AE.png)

> **4.存储逻辑说明**

* 当要添加到购物车的商品已存在时，对商品数量进行累加计算。
* 当要添加到购物车的商品不存在时，向hash中新增field和value即可。

#### 2. 未登录用户购物车存储方案 <a id="2-&#x672A;&#x767B;&#x5F55;&#x7528;&#x6237;&#x8D2D;&#x7269;&#x8F66;&#x5B58;&#x50A8;&#x65B9;&#x6848;"></a>

> **1.存储数据说明**

* **存储数据：`user_id`、`sku_id`、`count`、`selected`**

> **2.存储位置说明**

* 由于用户未登录，服务端无法拿到用户的ID，所以服务端在生成购物车记录时很难唯一标识该记录。
* 我们可以将未登录用户的购物车数据缓存到用户浏览器的**`cookie`**中，每个用户自己浏览器的cookie中存储属于自己的购物车数据。
* **存储位置：`用户浏览器的cookie`**

> **3.存储类型说明**

* 提示：浏览器的cookie中存储的数据类型是字符串。
* 思考：如何在字符串中描述一条购物车记录？
* 结论：**JSON字符串**可以描述复杂结构的字符串数据，可以保证一条购物车记录不用分开存储。

```python
{
    "sku_id1":{
        "count":"1",
        "selected":"True"
    },
    "sku_id3":{
        "count":"3",
        "selected":"True"
    },
    "sku_id5":{
        "count":"3",
        "selected":"False"
    }
}
```

![](http://lf521.gitee.io/meiduoqiantai/carts/images/03cookie%E5%AD%98%E5%82%A8%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%95%B0%E6%8D%AE1.png)

> **4.存储逻辑说明**

* 当要添加到购物车的商品已存在时，对商品数量进行累加计算。
* 当要添加到购物车的商品不存在时，向JSON中新增field和value即可。

> **提示：**

* 浏览器cookie中存储的是字符串明文数据。
  * 我们需要对购物车这类隐私数据进行密文存储。
* 解决方案：**`pickle模块`** 和 **`base64模块`**

> **5.pickle模块介绍**

* pickle模块是Python的标准模块，提供了对Python数据的序列化操作，可以将数据转换为bytes类型，且序列化速度快。
* pickle模块使用：
  * `pickle.dumps()`将Python数据序列化为bytes类型数据。
  * `pickle.loads()`将bytes类型数据反序列化为python数据。

```text
>>> import pickle

>>> dict = {'1': {'count': 10, 'selected': True}, '2': {'count': 20, 'selected': False}}
>>> ret = pickle.dumps(dict)
>>> ret
b'\x80\x03}q\x00(X\x01\x00\x00\x001q\x01}q\x02(X\x05\x00\x00\x00countq\x03K\nX\x08\x00\x00\x00selectedq\x04\x88uX\x01\x00\x00\x002q\x05}q\x06(h\x03K\x14h\x04\x89uu.'
>>> pickle.loads(ret)
{'1': {'count': 10, 'selected': True}, '2': {'count': 20, 'selected': False}}
```

> **6.base64模块介绍**

* 提示：pickle模块序列化转换后的数据是bytes类型，浏览器cookie无法存储。
* base64模块是Python的标准模块，可以对bytes类型数据进行编码，并得到bytes类型的密文数据。
* base64模块使用：
  * `base64.b64encode()`将bytes类型数据进行base64编码，返回编码后的bytes类型数据。
  * `base64.b64deocde()`将base64编码后的bytes类型数据进行解码，返回解码后的bytes类型数据。

```text
>>> import base64
>>> ret
b'\x80\x03}q\x00(X\x01\x00\x00\x001q\x01}q\x02(X\x05\x00\x00\x00countq\x03K\nX\x08\x00\x00\x00selectedq\x04\x88uX\x01\x00\x00\x002q\x05}q\x06(h\x03K\x14h\x04\x89uu.'
>>> b = base64.b64encode(ret)
>>> b
b'gAN9cQAoWAEAAAAxcQF9cQIoWAUAAABjb3VudHEDSwpYCAAAAHNlbGVjdGVkcQSIdVgBAAAAMnEFfXEGKGgDSxRoBIl1dS4='
>>> base64.b64decode(b)
b'\x80\x03}q\x00(X\x01\x00\x00\x001q\x01}q\x02(X\x05\x00\x00\x00countq\x03K\nX\x08\x00\x00\x00selectedq\x04\x88uX\x01\x00\x00\x002q\x05}q\x06(h\x03K\x14h\x04\x89uu.'
```

![](http://lf521.gitee.io/meiduoqiantai/carts/images/03cookie%E5%AD%98%E5%82%A8%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%95%B0%E6%8D%AE2.png)







## 添加购物车 <a id="&#x6DFB;&#x52A0;&#x8D2D;&#x7269;&#x8F66;"></a>

**提示：在商品详情页添加购物车使用局部刷新的效果。**

#### 1. 添加购物车接口设计和定义 <a id="1-&#x6DFB;&#x52A0;&#x8D2D;&#x7269;&#x8F66;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | POST |
| **请求地址** | /carts/ |

> **2.请求参数：JSON**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **sku\_id** | int | 是 | 商品SKU编号 |
| **count** | int | 是 | 商品数量 |
| **selected** | bool | 否 | 是否勾选 |

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **code** | 状态码 |
| **errmsg** | 错误信息 |

> **4.后端接口定义**

```python
class CartsView(View):
    """购物车管理"""

    def post(self, request):
        """添加购物车"""
        # 接收和校验参数
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，操作redis购物车
            pass
        else:
            # 用户未登录，操作cookie购物车
            pass
```

#### 2. 添加购物车后端逻辑实现 <a id="2-&#x6DFB;&#x52A0;&#x8D2D;&#x7269;&#x8F66;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.接收和校验参数**

```python
class CartsView(View):
    """购物车管理"""

    def post(self, request):
        """添加购物车"""
        # 接收参数
        json_dict = json.loads(request.body.decode())
        sku_id = json_dict.get('sku_id')
        count = json_dict.get('count')
        selected = json_dict.get('selected', True)

        # 判断参数是否齐全
        if not all([sku_id, count]):
            return http.HttpResponseForbidden('缺少必传参数')
        # 判断sku_id是否存在
        try:
            models.SKU.objects.get(id=sku_id)
        except models.SKU.DoesNotExist:
            return http.HttpResponseForbidden('商品不存在')
        # 判断count是否为数字
        try:
            count = int(count)
        except Exception:
            return http.HttpResponseForbidden('参数count有误')
        # 判断selected是否为bool值
        if selected:
            if not isinstance(selected, bool):
                return http.HttpResponseForbidden('参数selected有误')

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，操作redis购物车
            pass
        else:
            # 用户未登录，操作cookie购物车
            pass
```

> **2.添加购物车到Redis**

```python
class CartsView(View):
    """购物车管理"""

    def post(self, request):
        """添加购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，操作redis购物车
            redis_conn = get_redis_connection('carts')
            pl = redis_conn.pipeline()
            # 新增购物车数据
            pl.hincrby('carts_%s' % user.id, sku_id, count)
            # 新增选中的状态
            if selected:
                pl.sadd('selected_%s' % user.id, sku_id)
            # 执行管道
            pl.execute()
            # 响应结果
            return http.JsonResponse({'code': RETCODE.OK, 'errmsg': '添加购物车成功'})
        else:
            # 用户未登录，操作cookie购物车
            pass
```

> **3.添加购物车到cookie**

```python
class CartsView(View):
    """购物车管理"""

    def post(self, request):
        """添加购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，操作redis购物车
            ......
        else:
            # 用户未登录，操作cookie购物车
            cart_str = request.COOKIES.get('carts')
            # 如果用户操作过cookie购物车
            if cart_str:
                # 将cart_str转成bytes,再将bytes转成base64的bytes,最后将bytes转字典
                cart_dict = pickle.loads(base64.b64decode(cart_str.encode()))
            else:  # 用户从没有操作过cookie购物车
                cart_dict = {}

            # 判断要加入购物车的商品是否已经在购物车中,如有相同商品，累加求和，反之，直接赋值
            if sku_id in cart_dict:
                # 累加求和
                origin_count = cart_dict[sku_id]['count']
                count += origin_count
            cart_dict[sku_id] = {
                'count': count,
                'selected': selected
            }
            # 将字典转成bytes,再将bytes转成base64的bytes,最后将bytes转字符串
            cookie_cart_str = base64.b64encode(pickle.dumps(cart_dict)).decode()

            # 创建响应对象
            response = http.JsonResponse({'code': RETCODE.OK, 'errmsg': '添加购物车成功'})
            # 响应结果并将购物车数据写入到cookie
            response.set_cookie('carts', cookie_cart_str, max_age=constants.CARTS_COOKIE_EXPIRES)
            return response
```



## 展示购物车 <a id="&#x5C55;&#x793A;&#x8D2D;&#x7269;&#x8F66;"></a>

![](http://lf521.gitee.io/meiduoqiantai/carts/images/04%E5%B1%95%E7%A4%BA%E8%B4%AD%E7%89%A9%E8%BD%A62.png)

#### 1. 展示购物车接口设计和定义 <a id="1-&#x5C55;&#x793A;&#x8D2D;&#x7269;&#x8F66;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | GET |
| **请求地址** | /carts/ |

> **2.请求参数：**

```text
无
```

> **3.响应结果：HTML**

```text
cart.html
```

> **4.后端接口定义**

```python
class CartsView(View):
    """购物车管理"""

    def get(self, request):
        """展示购物车"""
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询redis购物车
            pass
        else:
            # 用户未登录，查询cookies购物车
            pass
```

#### 2. 展示购物车后端逻辑实现 <a id="2-&#x5C55;&#x793A;&#x8D2D;&#x7269;&#x8F66;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.查询Redis购物车**

```python
class CartsView(View):
    """购物车管理"""

    def get(self, request):
        """展示购物车"""
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询redis购物车
            redis_conn = get_redis_connection('carts')
            # 获取redis中的购物车数据
            redis_cart = redis_conn.hgetall('carts_%s' % user.id)
            # 获取redis中的选中状态
            cart_selected = redis_conn.smembers('selected_%s' % user.id)

            # 将redis中的数据构造成跟cookie中的格式一致，方便统一查询
            cart_dict = {}
            for sku_id, count in redis_cart.items():
                cart_dict[int(sku_id)] = {
                    'count': int(count),
                    'selected': sku_id in cart_selected
                }
        else:
            # 用户未登录，查询cookies购物车
            pass
```

> **2.查询cookie购物车**

```python
class CartsView(View):
    """购物车管理"""

    def get(self, request):
        """展示购物车"""
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询redis购物车
            ......
        else:
            # 用户未登录，查询cookies购物车
            cart_str = request.COOKIES.get('carts')
            if cart_str:
                # 将cart_str转成bytes,再将bytes转成base64的bytes,最后将bytes转字典
                cart_dict = pickle.loads(base64.b64decode(cart_str.encode()))
            else:
                cart_dict = {}
```

> **3.查询购物车SKU信息**

```python
class CartsView(View):
    """购物车管理"""

    def get(self, request):
        """展示购物车"""
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询redis购物车
            ......
        else:
            # 用户未登录，查询cookies购物车
            ......

        # 构造购物车渲染数据
        sku_ids = cart_dict.keys()
        skus = models.SKU.objects.filter(id__in=sku_ids)
        cart_skus = []
        for sku in skus:
            cart_skus.append({
                'id':sku.id,
                'name':sku.name,
                'count': cart_dict.get(sku.id).get('count'),
                'selected': str(cart_dict.get(sku.id).get('selected')),  # 将True，转'True'，方便json解析
                'default_image_url':sku.default_image.url,
                'price':str(sku.price), # 从Decimal('10.2')中取出'10.2'，方便json解析
                'amount':str(sku.price * cart_dict.get(sku.id).get('count')),
            })

        context = {
            'cart_skus':cart_skus,
        }

        # 渲染购物车页面
        return render(request, 'cart.html', context)
```

> **4.渲染购物车信息**

```text
<div class="total_count">全部商品<em>[[ total_count ]]</em>件</div>
<ul class="cart_list_th clearfix">
    <li class="col01">商品名称</li>
    <li class="col03">商品价格</li>
    <li class="col04">数量</li>
    <li class="col05">小计</li>
    <li class="col06">操作</li>
</ul>
<ul class="cart_list_td clearfix" v-for="(cart_sku,index) in carts" v-cloak>
    <li class="col01"><input type="checkbox" name="" v-model="cart_sku.selected" @change="update_selected(index)"></li>
    <li class="col02"><img :src="cart_sku.default_image_url"></li>
    <li class="col03">[[ cart_sku.name ]]</li>
    <li class="col05">[[ cart_sku.price ]]元</li>
    <li class="col06">
        <div class="num_add">
            <a @click="on_minus(index)" class="minus fl">-</a>
            <input v-model="cart_sku.count" @blur="on_input(index)" type="text" class="num_show fl">
            <a @click="on_add(index)" class="add fl">+</a>
        </div>
    </li>
    <li class="col07">[[ cart_sku.amount ]]元</li>
    <li class="col08"><a @click="on_delete(index)">删除</a></li>
</ul>
<ul class="settlements" v-cloak>
    <li class="col01"><input type="checkbox" name="" @change="on_selected_all" v-model="selected_all"></li>
    <li class="col02">全选</li>
    <li class="col03">合计(不含运费)：<span>¥</span><em>[[ total_selected_amount ]]</em><br>共计<b>[[ total_selected_count ]]</b>件商品</li>
    <li class="col04"><a href="place_order.html">去结算</a></li>
</ul>
```



## 修改购物车 <a id="&#x4FEE;&#x6539;&#x8D2D;&#x7269;&#x8F66;"></a>

**提示：在购物车页面修改购物车使用局部刷新的效果。**

#### 1. 修改购物车接口设计和定义 <a id="1-&#x4FEE;&#x6539;&#x8D2D;&#x7269;&#x8F66;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | PUT |
| **请求地址** | /carts/ |

> **2.请求参数：JSON**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **sku\_id** | int | 是 | 商品SKU编号 |
| **count** | int | 是 | 商品数量 |
| **selected** | bool | 否 | 是否勾选 |

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **sku\_id** | 商品SKU编号 |
| **count** | 商品数量 |
| **selected** | 是否勾选 |

> **4.后端接口定义**

```text
class CartsView(View):
    """购物车管理"""

    def put(self, request):
        """修改购物车"""
        # 接收和校验参数
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，修改redis购物车
            pass
        else:
            # 用户未登录，修改cookie购物车
            pass
```

#### 2. 修改购物车后端逻辑实现 <a id="2-&#x4FEE;&#x6539;&#x8D2D;&#x7269;&#x8F66;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.接收和校验参数**

```text
class CartsView(View):
    """购物车管理"""

    def put(self, request):
        """修改购物车"""
        # 接收参数
        json_dict = json.loads(request.body.decode())
        sku_id = json_dict.get('sku_id')
        count = json_dict.get('count')
        selected = json_dict.get('selected', True)

        # 判断参数是否齐全
        if not all([sku_id, count]):
            return http.HttpResponseForbidden('缺少必传参数')
        # 判断sku_id是否存在
        try:
            sku = models.SKU.objects.get(id=sku_id)
        except models.SKU.DoesNotExist:
            return http.HttpResponseForbidden('商品sku_id不存在')
        # 判断count是否为数字
        try:
            count = int(count)
        except Exception:
            return http.HttpResponseForbidden('参数count有误')
        # 判断selected是否为bool值
        if selected:
            if not isinstance(selected, bool):
                return http.HttpResponseForbidden('参数selected有误')

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，修改redis购物车
            pass
        else:
            # 用户未登录，修改cookie购物车
            pass
```

> **2.修改Redis购物车**

```text
class CartsView(View):
    """购物车管理"""

    def put(self, request):
        """修改购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，修改redis购物车
            redis_conn = get_redis_connection('carts')
            pl = redis_conn.pipeline()
            # 因为接口设计为幂等的，直接覆盖
            pl.hset('carts_%s' % user.id, sku_id, count)
            # 是否选中
            if selected:
                pl.sadd('selected_%s' % user.id, sku_id)
            else:
                pl.srem('selected_%s' % user.id, sku_id)
            pl.execute()

            # 创建响应对象
            cart_sku = {
                'id':sku_id,
                'count':count,
                'selected':selected,
                'name': sku.name,
                'default_image_url': sku.default_image.url,
                'price': sku.price,
                'amount': sku.price * count,
            }
            return http.JsonResponse({'code':RETCODE.OK, 'errmsg':'修改购物车成功', 'cart_sku':cart_sku})
        else:
            # 用户未登录，修改cookie购物车
            pass
```

> **3.修改cookie购物车**

```text
class CartsView(View):
    """购物车管理"""

    def put(self, request):
        """修改购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，修改redis购物车
            ......
        else:
            # 用户未登录，修改cookie购物车
            cart_str = request.COOKIES.get('carts')
            if cart_str:
                # 将cart_str转成bytes,再将bytes转成base64的bytes,最后将bytes转字典
                cart_dict = pickle.loads(base64.b64decode(cart_str.encode()))
            else:
                cart_dict = {}
            # 因为接口设计为幂等的，直接覆盖
            cart_dict[sku_id] = {
                'count': count,
                'selected': selected
            }
            # 将字典转成bytes,再将bytes转成base64的bytes,最后将bytes转字符串
            cookie_cart_str = base64.b64encode(pickle.dumps(cart_dict)).decode()

            # 创建响应对象
            cart_sku = {
                'id': sku_id,
                'count': count,
                'selected': selected,
                'name': sku.name,
                'default_image_url': sku.default_image.url,
                'price': sku.price,
                'amount': sku.price * count,
            }
            response = http.JsonResponse({'code':RETCODE.OK, 'errmsg':'修改购物车成功', 'cart_sku':cart_sku})
            # 响应结果并将购物车数据写入到cookie
            response.set_cookie('carts', cookie_cart_str, max_age=constants.CARTS_COOKIE_EXPIRES)
            return response
```



## 删除购物车 <a id="&#x5220;&#x9664;&#x8D2D;&#x7269;&#x8F66;"></a>

**提示：在购物车页面删除购物车使用局部刷新的效果。**

#### 1. 删除购物车接口设计和定义 <a id="1-&#x5220;&#x9664;&#x8D2D;&#x7269;&#x8F66;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | DELETE |
| **请求地址** | /carts/ |

> **2.请求参数：JSON**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **sku\_id** | int | 是 | 商品SKU编号 |

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **code** | 状态码 |
| **errmsg** | 错误信息 |

> **4.后端接口定义**

```text
class CartsView(View):
    """购物车管理"""

    def delete(self, request):
        """删除购物车"""
        # 接收和校验参数
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，删除redis购物车
            pass
        else:
            # 用户未登录，删除cookie购物车
            pass
```

#### 2. 删除购物车后端逻辑实现 <a id="2-&#x5220;&#x9664;&#x8D2D;&#x7269;&#x8F66;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.接收和校验参数**

```text
class CartsView(View):
    """购物车管理"""

    def delete(self, request):
        """删除购物车"""
        # 接收参数
        json_dict = json.loads(request.body.decode())
        sku_id = json_dict.get('sku_id')

        # 判断sku_id是否存在
        try:
            models.SKU.objects.get(id=sku_id)
        except models.SKU.DoesNotExist:
            return http.HttpResponseForbidden('商品不存在')

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户未登录，删除redis购物车
            pass
        else:
            # 用户未登录，删除cookie购物车
            pass
```

> **2.删除Redis购物车**

```text
class CartsView(View):
    """购物车管理"""

    def delete(self, request):
        """删除购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户未登录，删除redis购物车
            redis_conn = get_redis_connection('carts')
            pl = redis_conn.pipeline()
            # 删除键，就等价于删除了整条记录
            pl.hdel('carts_%s' % user.id, sku_id)
            pl.srem('selected_%s' % user.id, sku_id)
            pl.execute()

            # 删除结束后，没有响应的数据，只需要响应状态码即可
            return http.JsonResponse({'code': RETCODE.OK, 'errmsg': '删除购物车成功'})
        else:
            # 用户未登录，删除cookie购物车
            pass
```

> **3.删除cookie购物车**

```text
class CartsView(View):
    """购物车管理"""

    def delete(self, request):
        """删除购物车"""
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户未登录，删除redis购物车
            ......
        else:
            # 用户未登录，删除cookie购物车
            cart_str = request.COOKIES.get('carts')
            if cart_str:
                # 将cart_str转成bytes,再将bytes转成base64的bytes,最后将bytes转字典
                cart_dict = pickle.loads(base64.b64decode(cart_str.encode()))
            else:
                cart_dict = {}

            # 创建响应对象
            response = http.JsonResponse({'code': RETCODE.OK, 'errmsg': '删除购物车成功'})
            if sku_id in cart_dict:
                del cart_dict[sku_id]
                # 将字典转成bytes,再将bytes转成base64的bytes,最后将bytes转字符串
                cookie_cart_str = base64.b64encode(pickle.dumps(cart_dict)).decode()
                # 响应结果并将购物车数据写入到cookie
                response.set_cookie('carts', cookie_cart_str, max_age=constants.CARTS_COOKIE_EXPIRES)
            return response
```



## 全选购物车 <a id="&#x5168;&#x9009;&#x8D2D;&#x7269;&#x8F66;"></a>

**提示：在购物车页面修改购物车使用局部刷新的效果。**

#### 1. 全选购物车接口设计和定义 <a id="1-&#x5168;&#x9009;&#x8D2D;&#x7269;&#x8F66;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | PUT |
| **请求地址** | /carts/selection/ |

> **2.请求参数：JSON**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **selected** | bool | 是 | 是否全选 |

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **code** | 状态码 |
| **errmsg** | 错误信息 |

> **4.后端接口定义**

```text
class CartsSelectAllView(View):
    """全选购物车"""

    def put(self, request):
        # 接收和校验参数
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，操作redis购物车
            pass
        else:
            # 用户未登录，操作cookie购物车
            pass
```

#### 2. 全选购物车后端逻辑实现 <a id="2-&#x5168;&#x9009;&#x8D2D;&#x7269;&#x8F66;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.接收和校验参数**

```text
class CartsSelectAllView(View):
    """全选购物车"""

    def put(self, request):
        # 接收参数
        json_dict = json.loads(request.body.decode())
        selected = json_dict.get('selected', True)

        # 校验参数
        if selected:
            if not isinstance(selected, bool):
                return http.HttpResponseForbidden('参数selected有误')

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户已登录，操作redis购物车
            pass
        else:
            # 用户已登录，操作cookie购物车
            pass
```

> **2.全选Redis购物车**

```text
class CartsSelectAllView(View):
    """全选购物车"""

    def put(self, request):
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户已登录，操作redis购物车
            redis_conn = get_redis_connection('carts')
            cart = redis_conn.hgetall('carts_%s' % user.id)
            sku_id_list = cart.keys()
            if selected:
                # 全选
                redis_conn.sadd('selected_%s' % user.id, *sku_id_list)
            else:
                # 取消全选
                redis_conn.srem('selected_%s' % user.id, *sku_id_list)
            return http.JsonResponse({'code': RETCODE.OK, 'errmsg': '全选购物车成功'})
        else:
            # 用户已登录，操作cookie购物车
            pass
```

> **3.全选cookie购物车**

```text
class CartsSelectAllView(View):
    """全选购物车"""

    def put(self, request):
        # 接收和校验参数
        ......

        # 判断用户是否登录
        user = request.user
        if user is not None and user.is_authenticated:
            # 用户已登录，操作redis购物车
            ......
        else:
            # 用户已登录，操作cookie购物车
            cart = request.COOKIES.get('carts')
            response = http.JsonResponse({'code': RETCODE.OK, 'errmsg': '全选购物车成功'})
            if cart is not None:
                cart = pickle.loads(base64.b64decode(cart.encode()))
                for sku_id in cart:
                    cart[sku_id]['selected'] = selected
                cookie_cart = base64.b64encode(pickle.dumps(cart)).decode()
                response.set_cookie('carts', cookie_cart, max_age=constants.CARTS_COOKIE_EXPIRES)

            return response
```



## 合并购物车 <a id="&#x5408;&#x5E76;&#x8D2D;&#x7269;&#x8F66;"></a>

**需求：用户登录时，将`cookie`购物车数据`合并`到`Redis`购物车数据中。**

> 提示：
>
> * **`QQ登录`**和**`账号登录`**时都要进行购物车合并操作。

#### 1. 合并购物车逻辑分析 <a id="1-&#x5408;&#x5E76;&#x8D2D;&#x7269;&#x8F66;&#x903B;&#x8F91;&#x5206;&#x6790;"></a>

```text
1.合并方向：cookie购物车数据合并到Redis购物车数据中。
2.合并数据：购物车商品数据和勾选状态。
3.合并方案：
    3.1 Redis数据库中的购物车数据保留。
    3.2 如果cookie中的购物车数据在Redis数据库中已存在，将cookie购物车数据覆盖Redis购物车数据。
    3.3 如果cookie中的购物车数据在Redis数据库中不存在，将cookie购物车数据新增到Redis。
    3.4 最终购物车的勾选状态以cookie购物车勾选状态为准。
```

#### 2. 合并购物车逻辑实现 <a id="2-&#x5408;&#x5E76;&#x8D2D;&#x7269;&#x8F66;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> 新建文件：`carts.utils.py`

```text
def merge_cart_cookie_to_redis(request, user, response):
    """
    登录后合并cookie购物车数据到Redis
    :param request: 本次请求对象，获取cookie中的数据
    :param response: 本次响应对象，清除cookie中的数据
    :param user: 登录用户信息，获取user_id
    :return: response
    """
    # 获取cookie中的购物车数据
    cookie_cart_str = request.COOKIES.get('carts')
    # cookie中没有数据就响应结果
    if not cookie_cart_str:
        return response
    cookie_cart_dict = pickle.loads(base64.b64decode(cookie_cart_str.encode()))

    new_cart_dict = {}
    new_cart_selected_add = []
    new_cart_selected_remove = []
    # 同步cookie中购物车数据
    for sku_id, cookie_dict in cookie_cart_dict.items():
        new_cart_dict[sku_id] = cookie_dict['count']

        if cookie_dict['selected']:
            new_cart_selected_add.append(sku_id)
        else:
            new_cart_selected_remove.append(sku_id)

    # 将new_cart_dict写入到Redis数据库
    redis_conn = get_redis_connection('carts')
    pl = redis_conn.pipeline()
    pl.hmset('carts_%s' % user.id, new_cart_dict)
    # 将勾选状态同步到Redis数据库
    if new_cart_selected_add:
        pl.sadd('selected_%s' % user.id, *new_cart_selected_add)
    if new_cart_selected_remove:
        pl.srem('selected_%s' % user.id, *new_cart_selected_remove)
    pl.execute()

    # 清除cookie
    response.delete_cookie('carts')

    return response
```

#### 3. 账号和QQ登录合并购物车 <a id="3-&#x8D26;&#x53F7;&#x548C;qq&#x767B;&#x5F55;&#x5408;&#x5E76;&#x8D2D;&#x7269;&#x8F66;"></a>

> 在`users.views.py`和`oauth.views.py`文件中调用合并购物车的工具方法

```text
# 合并购物车
response = merge_cart_cookie_to_redis(request=request, user=user, response=response)
```



## 展示商品页面简单购物车 <a id="&#x5C55;&#x793A;&#x5546;&#x54C1;&#x9875;&#x9762;&#x7B80;&#x5355;&#x8D2D;&#x7269;&#x8F66;"></a>

![](http://lf521.gitee.io/meiduoqiantai/carts/images/05%E5%95%86%E5%93%81%E9%A1%B5%E5%8F%B3%E4%B8%8A%E8%A7%92%E8%B4%AD%E7%89%A9%E8%BD%A6.png)

**需求：用户鼠标悬停在商品页面右上角购物车标签上，以下拉框形式展示当前购物车数据。**

#### 1. 简单购物车数据接口设计和定义 <a id="1-&#x7B80;&#x5355;&#x8D2D;&#x7269;&#x8F66;&#x6570;&#x636E;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | GET |
| **请求地址** | /carts/simple/ |

> **2.请求参数：**

```text
无
```

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **code** | 状态码 |
| **errmsg** | 错误信息 |
| **cart\_skus\[ \]** | 简单购物车SKU列表 |
| **id** | 购物车SKU编号 |
| **name** | 购物车SKU名称 |
| **count** | 购物车SKU数量 |
| **default\_image\_url** | 购物车SKU图片 |

```text
{
    "code":"0",
    "errmsg":"OK",
    "cart_skus":[
        {
            "id":1,
            "name":"Apple MacBook Pro 13.3英寸笔记本 银色",
            "count":1,
            "default_image_url":"http://image.meiduo.site:8888/group1/M00/00/02/CtM3BVrPB4GAWkTlAAGuN6wB9fU4220429"
        },
        ......
    ]
}
```

> **4.后端接口定义**

```text
class CartsSimpleView(View):
    """商品页面右上角购物车"""

    def get(self, request):
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询Redis购物车
            pass
        else:
            # 用户未登录，查询cookie购物车
            pass

        # 构造简单购物车JSON数据
        pass
```

#### 2. 简单购物车数据后端逻辑实现 <a id="2-&#x7B80;&#x5355;&#x8D2D;&#x7269;&#x8F66;&#x6570;&#x636E;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> **1.查询Redis购物车**

```text
class CartsSimpleView(View):
    """商品页面右上角购物车"""

    def get(self, request):
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询Redis购物车
            redis_conn = get_redis_connection('carts')
            redis_cart = redis_conn.hgetall('carts_%s' % user.id)
            cart_selected = redis_conn.smembers('selected_%s' % user.id)
            # 将redis中的两个数据统一格式，跟cookie中的格式一致，方便统一查询
            cart_dict = {}
            for sku_id, count in redis_cart.items():
                cart_dict[int(sku_id)] = {
                    'count': int(count),
                    'selected': sku_id in cart_selected
                }
        else:
            # 用户未登录，查询cookie购物车
            pass

        # 构造简单购物车JSON数据
        pass
```

> **2.查询Redis购物车**

```text
class CartsSimpleView(View):
    """商品页面右上角购物车"""

    def get(self, request):
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询Redis购物车
            ......
        else:
            # 用户未登录，查询cookie购物车
            cart_str = request.COOKIES.get('carts')
            if cart_str:
                cart_dict = pickle.loads(base64.b64decode(cart_str.encode()))
            else:
                cart_dict = {}
```

> **3.构造简单购物车JSON数据**

```text
class CartsSimpleView(View):
    """商品页面右上角购物车"""

    def get(self, request):
        # 判断用户是否登录
        user = request.user
        if user.is_authenticated:
            # 用户已登录，查询Redis购物车
            ......
        else:
            # 用户未登录，查询cookie购物车
            ......

        # 构造简单购物车JSON数据
        cart_skus = []
        sku_ids = cart_dict.keys()
        skus = models.SKU.objects.filter(id__in=sku_ids)
        for sku in skus:
            cart_skus.append({
                'id':sku.id,
                'name':sku.name,
                'count':cart_dict.get(sku.id).get('count'),
                'default_image_url': sku.default_image.url
            })

        # 响应json列表数据
        return http.JsonResponse({'code':RETCODE.OK, 'errmsg':'OK', 'cart_skus':cart_skus})
```

#### 3. 展示商品页面简单购物车 <a id="3-&#x5C55;&#x793A;&#x5546;&#x54C1;&#x9875;&#x9762;&#x7B80;&#x5355;&#x8D2D;&#x7269;&#x8F66;"></a>

> **1.商品页面发送Ajax请求**
>
> * `index.js`、`list.js`、`detail.js`

```text
get_carts(){
    let url = '/carts/simple/';
    axios.get(url, {
        responseType: 'json',
    })
        .then(response => {
            this.carts = response.data.cart_skus;
            this.cart_total_count = 0;
            for(let i=0;i<this.carts.length;i++){
                if (this.carts[i].name.length>25){
                    this.carts[i].name = this.carts[i].name.substring(0, 25) + '...';
                }
                this.cart_total_count += this.carts[i].count;
            }
        })
        .catch(error => {
            console.log(error.response);
        })
},
```

> **2.商品页面渲染简单购物车数据**
>
> * `index.html`、`list.html`、`detail.html`

```text
<div @mouseenter="get_carts" class="guest_cart fr" v-cloak>
    <a href="{{ url('carts:info') }}" class="cart_name fl">我的购物车</a>
    <div class="goods_count fl" id="show_count">[[ cart_total_count ]]</div>
    <ul class="cart_goods_show">
        <li v-for="sku in carts">
            <img :src="sku.default_image_url" alt="商品图片">
            <h4>[[ sku.name ]]</h4>
            <div>[[ sku.count ]]</div>
        </li>
    </ul>
</div>
```

