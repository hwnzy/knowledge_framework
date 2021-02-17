# 邮箱

## 添加邮箱后端逻辑

#### 1. 添加邮箱接口设计和定义 <a id="1-&#x6DFB;&#x52A0;&#x90AE;&#x7BB1;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | PUT |
| **请求地址** | /emails/ |

> **2.请求参数**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **email** | string | 是 | 邮箱 |

> **3.响应结果：JSON**

| 字段 | 说明 |
| :--- | :--- |
| **code** | 状态码 |
| **errmsg** | 错误信息 |

#### 2. 添加邮箱后端逻辑实现 <a id="2-&#x6DFB;&#x52A0;&#x90AE;&#x7BB1;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

```python
class EmailView(LoginRequiredMixin, View):
    """添加邮箱"""

    def put(self, request):
        """实现添加邮箱逻辑"""
        # 接收参数
        json_str = request.body.decode()
        json_dict = json.loads(json_str)
        email = json_dict.get('email')

        # 校验参数
        if not re.match(r'^[a-z0-9][\w\.\-]*@[a-z0-9\-]+(\.[a-z]{2,5}){1,2}$', email):
            return http.HttpResponseForbidden('参数email有误')

        # 赋值email字段
        try:
            request.user.email = email
            request.user.save()
        except Exception as e:
            logger.error(e)
            return http.JsonResponse({'code': RETCODE.DBERR, 'errmsg': '添加邮箱失败'})

        # 响应添加邮箱结果
        return http.JsonResponse({'code': RETCODE.OK, 'errmsg': '添加邮箱成功'})
```

#### 3. 判断用户是否登录并返回JSON <a id="3-&#x5224;&#x65AD;&#x7528;&#x6237;&#x662F;&#x5426;&#x767B;&#x5F55;&#x5E76;&#x8FD4;&#x56DE;json"></a>

> 重要提示：

* 只有用户登录时才能让其绑定邮箱。
* 此时前后端交互的数据类型是JSON，所以需要判断用户是否登录并返回JSON给用户。

> 实现方案：自定义 **`LoginRequiredJSONMixin`** 扩展类

* 新建`meiduo_mall/meiduo_mall/utils/views.py`文件

```python
class LoginRequiredJSONMixin(LoginRequiredMixin):
    """Verify that the current user is authenticated."""

    def handle_no_permission(self):
        return http.JsonResponse({'code': RETCODE.SESSIONERR, 'errmsg': '用户未登录'})
```

```python
handle_no_permission说明：我们只需要改写父类中的处理方式 至于如何判断用户是否登录 在父类中已经判断了
```

> `LoginRequiredJSONMixin`的使用

```python
from meiduo_mall.utils.views import LoginRequiredJSONMixin

class EmailView(LoginRequiredJSONMixin, View):
    """添加邮箱"""

    def put(self, request):
        """实现添加邮箱逻辑"""
        # 判断用户是否登录并返回JSON
        pass
```



## Django发送邮件的配置

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/07%E9%82%AE%E7%AE%B1%E9%AA%8C%E8%AF%81%E9%82%AE%E4%BB%B6.png)

#### 1. Django发送邮件流程分析 <a id="1-django&#x53D1;&#x9001;&#x90AE;&#x4EF6;&#x6D41;&#x7A0B;&#x5206;&#x6790;"></a>

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/05Django%E5%8F%91%E9%80%81%E9%82%AE%E4%BB%B6%E6%B5%81%E7%A8%8B.png)

> `send_mall()`方法介绍
>
> * 位置：
>   * 在`django.core.mail`模块提供了`send_mail()`来发送邮件。
> * 方法参数：
>   * `send_mail(subject, message, from_email, recipient_list, html_message=None)`

```text
subject 邮件标题
message 普通邮件正文，普通字符串
from_email 发件人
recipient_list 收件人列表
html_message 多媒体邮件正文，可以是html字符串
```

#### 2. 准备发邮件服务器 <a id="2-&#x51C6;&#x5907;&#x53D1;&#x90AE;&#x4EF6;&#x670D;&#x52A1;&#x5668;"></a>

> **1.点击进入《设置》界面**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/06%E5%87%86%E5%A4%87%E5%8F%91%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A81.png)

> **2.点击进入《客户端授权密码》界面**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/06%E5%87%86%E5%A4%87%E5%8F%91%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A82.png)

> **3.开启《授权码》，并完成验证短信**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/06%E5%87%86%E5%A4%87%E5%8F%91%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A83.png)

> **4.填写《授权码》**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/06%E5%87%86%E5%A4%87%E5%8F%91%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A84.png)

> **5.完成《授权码》设置**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/06%E5%87%86%E5%A4%87%E5%8F%91%E9%82%AE%E4%BB%B6%E6%9C%8D%E5%8A%A1%E5%99%A85.png)

> **6.配置邮件服务器**

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend' # 指定邮件后端
EMAIL_HOST = 'smtp.163.com' # 发邮件主机
EMAIL_PORT = 25 # 发邮件端口
EMAIL_HOST_USER = 'hmmeiduo@163.com' # 授权的邮箱
EMAIL_HOST_PASSWORD = 'hmmeiduo123' # 邮箱授权时获得的密码，非注册登录密码
EMAIL_FROM = '美多商城<hmmeiduo@163.com>' # 发件人抬头
```

## 发送邮箱验证邮件 <a id="&#x53D1;&#x9001;&#x90AE;&#x7BB1;&#x9A8C;&#x8BC1;&#x90AE;&#x4EF6;"></a>

> 重要提示：

* 发送邮箱验证邮件是耗时的操作，不能阻塞美多商城的响应，所以需要**异步发送邮件**。
* 我们继续使用Celery实现异步任务。

#### 1. 定义和调用发送邮件异步任务 <a id="1-&#x5B9A;&#x4E49;&#x548C;&#x8C03;&#x7528;&#x53D1;&#x9001;&#x90AE;&#x4EF6;&#x5F02;&#x6B65;&#x4EFB;&#x52A1;"></a>

> **1.定义发送邮件任务**

![](http://lf521.gitee.io/meiduoqiantai/user-center/images/08%E5%87%86%E5%A4%87%E5%8F%91%E9%80%81%E9%82%AE%E4%BB%B6%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1.png)

```text
# bind：保证task对象会作为第一个参数自动传入
# name：异步任务别名
# retry_backoff：异常自动重试的时间间隔 第n次(retry_backoff×2^(n-1))s
# max_retries：异常自动重试次数的上限
@celery_app.task(bind=True, name='send_verify_email', retry_backoff=3)
def send_verify_email(self, to_email, verify_url):
    """
    发送验证邮箱邮件
    :param to_email: 收件人邮箱
    :param verify_url: 验证链接
    :return: None
    """
    subject = "美多商城邮箱验证"
    html_message = '<p>尊敬的用户您好！</p>' \
                   '<p>感谢您使用美多商城。</p>' \
                   '<p>您的邮箱为：%s 。请点击此链接激活您的邮箱：</p>' \
                   '<p><a href="%s">%s<a></p>' % (to_email, verify_url, verify_url)
    try:
        send_mail(subject, "", settings.EMAIL_FROM, [to_email], html_message=html_message)
    except Exception as e:
        logger.error(e)
        # 有异常自动重试三次
        raise self.retry(exc=e, max_retries=3)
```

> **2.注册发邮件的任务：main.py**
>
> * 在发送邮件的异步任务中，我们用到了Django的配置文件。
> * 所以我们需要修改celery的启动文件main.py。
> * 在其中指明celery可以读取的Django配置文件。
> * 最后记得注册新添加的email的任务

```text
# celery启动文件
from celery import Celery


# 为celery使用django配置文件进行设置
import os
if not os.getenv('DJANGO_SETTINGS_MODULE'):
    os.environ['DJANGO_SETTINGS_MODULE'] = 'meiduo_mall.settings.dev'

# 创建celery实例
celery_app = Celery('meiduo')

# 加载celery配置
celery_app.config_from_object('celery_tasks.config')

# 自动注册celery任务
celery_app.autodiscover_tasks(['celery_tasks.sms', 'celery_tasks.email'])
```

> **3.调用发送邮件异步任务**

```text
# 赋值email字段
try:
    request.user.email = email
    request.user.save()
except Exception as e:
    logger.error(e)
    return http.JsonResponse({'code': RETCODE.DBERR, 'errmsg': '添加邮箱失败'})

# 异步发送验证邮件
verify_url = '邮件验证链接'
send_verify_email.delay(email, verify_url)

# 响应添加邮箱结果
return http.JsonResponse({'code': RETCODE.OK, 'errmsg': '添加邮箱成功'})
```

> **4.启动Celery**

```text
$ celery -A celery_tasks.main worker -l info
```

#### 2. 生成邮箱验证链接 <a id="2-&#x751F;&#x6210;&#x90AE;&#x7BB1;&#x9A8C;&#x8BC1;&#x94FE;&#x63A5;"></a>

> 邮箱验证链接

* [http://www.meiduo.site:8000/emails/verification/?token=eyJhbGciOiJIUzUxMiIsImlhdCI6MTU1Nzg5NTA2NSwiZXhwIjoxNTU3OTgxNDY1fQ.eyJ1c2VyX2lkIjoxLCJlbWFpbCI6InpoYW5namllc2hhcnBAMTYzLmNvbSJ9.JBRjoAgZMbMrfrTdmhPyJy2gVMpVRe9bAsxmQr5uzADZo3mZhr9d5MjsVrSI9BJagg31UwpvlvuL5iZdPRi4qw](http://www.meiduo.site:8000/emails/verification/?token=eyJhbGciOiJIUzUxMiIsImlhdCI6MTU1Nzg5NTA2NSwiZXhwIjoxNTU3OTgxNDY1fQ.eyJ1c2VyX2lkIjoxLCJlbWFpbCI6InpoYW5namllc2hhcnBAMTYzLmNvbSJ9.JBRjoAgZMbMrfrTdmhPyJy2gVMpVRe9bAsxmQr5uzADZo3mZhr9d5MjsVrSI9BJagg31UwpvlvuL5iZdPRi4qw)

> **1.定义生成邮箱验证链接方法**

```text
def generate_verify_email_url(user):
    """
    生成邮箱验证链接
    :param user: 当前登录用户
    :return: verify_url
    """
    serializer = Serializer(settings.SECRET_KEY, expires_in=constants.VERIFY_EMAIL_TOKEN_EXPIRES)
    data = {'user_id': user.id, 'email': user.email}
    token = serializer.dumps(data).decode()
    verify_url = settings.EMAIL_VERIFY_URL + '?token=' + token
    return verify_url
```

> **2.配置相关参数**

```text
# 邮箱验证链接
EMAIL_VERIFY_URL = 'http://www.meiduo.site:8000/emails/verification/'
```

> **3.使用邮箱验证链接**

```text
verify_url = generate_verify_email_url(request.user)
send_verify_email.delay(email, verify_url)
```

#### 3. 补充celery worker的工作模式 <a id="3-&#x8865;&#x5145;celery-worker&#x7684;&#x5DE5;&#x4F5C;&#x6A21;&#x5F0F;"></a>

* 默认是进程池方式，进程数以当前机器的CPU核数为参考，每个CPU开四个进程。
* 如何自己指定进程数：`celery worker -A proj --concurrency=4`
* 如何改变进程池方式为协程方式：`celery worker -A proj --concurrency=1000 -P eventlet -c 1000`

```text
# 安装eventlet模块
$ pip install eventlet

# 启用 Eventlet 池
$ celery -A celery_tasks.main worker -l info -P eventlet -c 1000
```

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/40eventlet%E7%9A%84%E4%BD%BF%E7%94%A8.png)





## 验证邮箱后端逻辑 <a id="&#x9A8C;&#x8BC1;&#x90AE;&#x7BB1;&#x540E;&#x7AEF;&#x903B;&#x8F91;"></a>

#### 1. 验证邮箱接口设计和定义 <a id="1-&#x9A8C;&#x8BC1;&#x90AE;&#x7BB1;&#x63A5;&#x53E3;&#x8BBE;&#x8BA1;&#x548C;&#x5B9A;&#x4E49;"></a>

> **1.请求方式**

| 选项 | 方案 |
| :--- | :--- |
| **请求方法** | GET |
| **请求地址** | /emails/verification/ |

> **2.请求参数：查询参数**

| 参数名 | 类型 | 是否必传 | 说明 |
| :--- | :--- | :--- | :--- |
| **token** | string | 是 | 邮箱激活链接 |

> **3.响应结果：HTML**

| 字段 | 说明 |
| :--- | :--- |
| **邮箱验证失败** | 响应错误提示 |
| **邮箱验证成功** | 重定向到用户中心 |

#### 2. 验证链接提取用户信息 <a id="2-&#x9A8C;&#x8BC1;&#x94FE;&#x63A5;&#x63D0;&#x53D6;&#x7528;&#x6237;&#x4FE1;&#x606F;"></a>

```text
def check_verify_email_token(token):
    """
    验证token并提取user
    :param token: 用户信息签名后的结果
    :return: user, None
    """
    serializer = Serializer(settings.SECRET_KEY, expires_in=constants.VERIFY_EMAIL_TOKEN_EXPIRES)
    try:
        data = serializer.loads(token)
    except BadData:
        return None
    else:
        user_id = data.get('user_id')
        email = data.get('email')
        try:
            user = User.objects.get(id=user_id, email=email)
        except User.DoesNotExist:
            return None
        else:
            return user
```

#### 3. 验证邮箱后端逻辑实现 <a id="3-&#x9A8C;&#x8BC1;&#x90AE;&#x7BB1;&#x540E;&#x7AEF;&#x903B;&#x8F91;&#x5B9E;&#x73B0;"></a>

> 验证邮箱的核心：就是将用户的`email_active`字段设置为`True`

```text
class VerifyEmailView(View):
    """验证邮箱"""

    def get(self, request):
        """实现邮箱验证逻辑"""
        # 接收参数
        token = request.GET.get('token')

        # 校验参数：判断token是否为空和过期，提取user
        if not token:
            return http.HttpResponseBadRequest('缺少token')

        user = check_verify_email_token(token)
        if not user:
            return http.HttpResponseForbidden('无效的token')

        # 修改email_active的值为True
        try:
            user.email_active = True
            user.save()
        except Exception as e:
            logger.error(e)
            return http.HttpResponseServerError('激活邮件失败')

        # 返回邮箱验证结果
        return redirect(reverse('users:info'))
```



## 生产者消费者设计模式 <a id="&#x751F;&#x4EA7;&#x8005;&#x6D88;&#x8D39;&#x8005;&#x8BBE;&#x8BA1;&#x6A21;&#x5F0F;"></a>

> 思考：
>
> * 下面两行代码存在什么问题？

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/26celery%E9%97%AE%E9%A2%98%E5%88%86%E6%9E%90.png)

> 问题：
>
> * 我们的代码是自上而下同步执行的。
> * 发送短信是耗时的操作。如果短信被阻塞住，用户响应将会延迟。
> * 响应延迟会造成用户界面的倒计时延迟。

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/35%E5%8F%91%E9%80%81%E7%9F%AD%E4%BF%A1%E4%BB%A3%E7%A0%81%E5%90%8C%E6%AD%A5%E6%89%A7%E8%A1%8C.png)

> 解决：
>
> * 异步发送短信
> * 发送短信和响应分开执行，将**`发送短信`**从主业务中**`解耦`**出来。

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/36%E5%8F%91%E9%80%81%E7%9F%AD%E4%BF%A1%E8%A7%A3%E8%80%A6.png)

> 思考：
>
> * 如何将**`发送短信`**从主业务中**`解耦`**出来。

#### 生产者消费者设计模式介绍 <a id="&#x751F;&#x4EA7;&#x8005;&#x6D88;&#x8D39;&#x8005;&#x8BBE;&#x8BA1;&#x6A21;&#x5F0F;&#x4ECB;&#x7ECD;"></a>

* 为了将**`发送短信`**从主业务中**`解耦`**出来,我们引入**`生产者消费者设计模式`**。
* 它是最常用的解耦方式之一，寻找**中间人\(broker\)**搭桥，**保证两个业务没有直接关联**。

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/34%E7%94%9F%E4%BA%A7%E8%80%85%E6%B6%88%E8%B4%B9%E8%80%85%E6%A8%A1%E5%BC%8F.png)

> 总结：
>
> * 生产者生成消息，缓存到消息队列中，消费者读取消息队列中的消息并执行。
> * 由美多商城生成发送短信消息，缓存到消息队列中，消费者读取消息队列中的发送短信消息并执行。



## Celery介绍和使用 <a id="celery&#x4ECB;&#x7ECD;&#x548C;&#x4F7F;&#x7528;"></a>

> 思考：
>
> * 消费者取到消息之后，要消费掉（执行任务），需要我们去实现。
> * 任务可能出现高并发的情况，需要补充多任务的方式执行。
> * 耗时任务很多种，每种耗时任务编写的生产者和消费者代码有重复。
> * 取到的消息什么时候执行，以什么样的方式执行。
>
> 结论：
>
> * 实际开发中，我们可以借助成熟的工具`Celery`来完成。
> * 有了`Celery`，我们在使用生产者消费者模式时，只需要关注任务本身，极大的简化了程序员的开发流程。

#### 1. Celery介绍 <a id="1-celery&#x4ECB;&#x7ECD;"></a>

* Celery介绍：
  * 一个简单、灵活且可靠、处理大量消息的分布式系统，可以在一台或者多台机器上运行。
  * 单个 Celery 进程每分钟可处理数以百万计的任务。
  * 通过消息进行通信，使用`消息队列（broker）`在`客户端`和`消费者`之间进行协调。
* 安装Celery：

  ```text
  $ pip install -U Celery
  ```

* [Celery官方文档](http://docs.celeryproject.org/en/latest/index.html)

#### 2. 创建Celery实例并加载配置 <a id="2-&#x521B;&#x5EFA;celery&#x5B9E;&#x4F8B;&#x5E76;&#x52A0;&#x8F7D;&#x914D;&#x7F6E;"></a>

> **1.定义Celery包**

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/28%E5%AE%9A%E4%B9%89celery%E5%8C%85.png)

> **2.创建Celery实例**

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/29celery%E5%85%A5%E5%8F%A3%E6%96%87%E4%BB%B6.png)

> celery\_tasks.main.py

```text
# celery启动文件
from celery import Celery


# 创建celery实例
celery_app = Celery('meiduo')
```

> **3.加载Celery配置**

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/30celery%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6.png)

> celery\_tasks.config.py

```text
# 指定消息队列的位置
broker_url = "redis://127.0.0.1/10"
```

> celery\_tasks.main.py

```text
# celery启动文件
from celery import Celery


# 创建celery实例
celery_app = Celery('meiduo')
# 加载celery配置
celery_app.config_from_object('celery_tasks.config')
```

#### 3. 定义发送短信任务 <a id="3-&#x5B9A;&#x4E49;&#x53D1;&#x9001;&#x77ED;&#x4FE1;&#x4EFB;&#x52A1;"></a>

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/31%E5%AE%9A%E4%B9%89%E5%8F%91%E9%80%81%E7%9F%AD%E4%BF%A1%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1.png)

> **1.定义任务：celery\_tasks.sms.tasks.py**

```text
@celery_app.task(name='ccp_send_sms_code')
def ccp_send_sms_code(mobile, sms_code):
    """
    发送短信异步任务
    :param mobile: 手机号
    :param sms_code: 短信验证码
    :return: 成功0 或 失败-1
    """
    send_ret = CCP().send_template_sms(mobile, [sms_code, constants.SMS_CODE_REDIS_EXPIRES // 60], constants.SEND_SMS_TEMPLATE_ID)
    return send_ret
```

> **2.注册任务：celery\_tasks.main.py**

```text
# celery启动文件
from celery import Celery


# 创建celery实例
celery_app = Celery('meiduo')
# 加载celery配置
celery_app.config_from_object('celery_tasks.config')
# 自动注册celery任务
celery_app.autodiscover_tasks(['celery_tasks.sms'])
```

#### 4. 启动Celery服务 <a id="4-&#x542F;&#x52A8;celery&#x670D;&#x52A1;"></a>

```text
$ cd ~/projects/meiduo_project/meiduo_mall
$ celery -A celery_tasks.main worker -l info
```

> * `-A`指对应的应用程序, 其参数是项目中 Celery实例的位置。
> * `worker`指这里要启动的worker。
> * `-l`指日志等级，比如`info`等级。

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/32%E5%90%AF%E5%8A%A8celery%E6%95%88%E6%9E%9C.png)

#### 5. 调用发送短信任务 <a id="5-&#x8C03;&#x7528;&#x53D1;&#x9001;&#x77ED;&#x4FE1;&#x4EFB;&#x52A1;"></a>

```text
# 发送短信验证码
# CCP().send_template_sms(mobile,[sms_code, constants.SMS_CODE_REDIS_EXPIRES // 60], constants.SEND_SMS_TEMPLATE_ID)
# Celery异步发送短信验证码
ccp_send_sms_code.delay(mobile, sms_code)
```

![](http://lf521.gitee.io/meiduoqiantai/user-verification-code/images/33celery%E6%89%A7%E8%A1%8C%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1%E6%95%88%E6%9E%9C.png)

