# Django-Pagination的使用

 Django自身提供了一些类来实现管理分页，数据被分在不同的页面中，并带有“上一页/下一页”标签。这个类叫做Pagination，其定义位于 django/core/paginator.py 中。

一. Paginator类的解释[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
class Paginator(object):

    def __init__(self, object_list, per_page, orphans=0,
                 allow_empty_first_page=True):
        self.object_list = object_list
        self.per_page = int(per_page)
        self.orphans = int(orphans)
        self.allow_empty_first_page = allow_empty_first_page
        self._num_pages = self._count = None
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

1.根据其定义做出以下解释，上述代码没有将其类属性和方法贴出。

* object\_list:可以是列表，元组，查询集或其他含有 count\(\) 或 \_\_len\_\_\(\)方法的可切片对象。对于连续的分页，查询集应该有序，例如有order\_by\(\)项或默认ordering参数。
* per\_page:每一页中包含条目数目的最大值，不包括独立成页的那页。（见下面 orphans参数解释）。
* orphans=0:当你使用此参数时说明你不希望最后一页只有很少的条目。如果最后一页的条目数少于等于orphans的值，则这些条目会被归并到上一页中（此时的上一页变为最后一页）。例如有23项条目， per\_page=10,orphans=0,则有3页，分别为10，10，3.如果orphans&gt;=3,则为2页，分别为10，13。
* allow\_empty\_first\_page=True： 默认允许第一页为空。

2.类方法：

*     Paginator.page\(number\)：根据参数number返回一个Page对象。（number为1的倍数）

3.类属型：

* Paginator.count：所有页面对象总数，即统计object\_list中item数目。当计算object\_list所含对象的数量时， Paginator会首先尝试调用object\_list.count\(\)。如果object\_list没有 count\(\) 方法，Paginator 接着会回退使用len\(object\_list\)。
* Pagnator.num\_pages:页面总数。
* pagiator.page\_range：页面范围，从1开始，例如\[1,2,3,4\]。

二. Page类的解释

 **通常不用手动创建Page对象，可以从Paginator.page\(\)来获得他们。**

```text
class Page(collections.Sequence):

    def __init__(self, object_list, number, paginator):
        self.object_list = object_list
        self.number = number
        self.paginator = paginator
```

1.类方法

* Page.has\_next\(\)  如果有下一页，则返回True。
* Page.has\_previous\(\) 如果有上一页，返回 True。
* Page.has\_other\_pages\(\) 如果有上一页或下一页，返回True。
* Page.next\_page\_number\(\) 返回下一页的页码。如果下一页不存在，抛出[I](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.InvalidPage)nvlidPage异常。
* Page.previous\_page\_number\(\) 返回上一页的页码。如果上一页不存在，抛出InvalidPage异常。
* Page.start\_index\(\) 返回当前页上的第一个对象，相对于分页列表的所有对象的序号，从1开始。比如，将五个对象的列表分为每页两个对象，第二页的[s](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Page.start_index)tart\_index\(\)会返回3。
* Page.end\_index\(\) 返回当前页上的最后一个对象，相对于分页列表的所有对象的序号，从1开始。 比如，将五个对象的列表分为每页两个对象，第二页的[e](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Page.end_index)nd\_index\(\) 会返回 4。

2.类属型

* Page.object\_list 当前页上所有对象的列表。
* Page.number 当前页的序号，从1开始。
* Page.paginator 相关的[P](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Paginator)aginator对象。

三.非法页面处理

* InvalidPage\(Exception\): 异常的基类，当paginator传入一个无效的页码时抛出。

  [P](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.Paginator.page)aginator.page\(\)放回在所请求的页面无效（比如不是一个整数）时，或者不包含任何对象时抛出异常。通常，捕获InvalidPage异常就够了，但是如果你想更加精细一些，可以捕获以下两个异常之一：

* exception PageNotAnInteger，当向page\(\)提供一个不是整数的值时抛出。
* exception EmptyPage，当向page\(\)提供一个有效值，但是那个页面上没有任何对象时抛出。

       这两个异常都是[I](http://python.usyiyi.cn/documents/django_182/topics/pagination.html#django.core.paginator.InvalidPage)nalidPage的子类，所以可以通过简单的except InvalidPage来处理它们。

四.使用Paginator

官方示例：

views.py:[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger
from django.shortcuts import render

def listing(request):
    contact_list = Contacts.objects.all()  # 获取所有contacts,假设在models.py中已定义了Contacts模型
    paginator = Paginator(contact_list, 25) # 每页25条

    page = request.GET.get('page')
    try:
        contacts = paginator.page(page) # contacts为Page对象！
    except PageNotAnInteger:
        # If page is not an integer, deliver first page.
        contacts = paginator.page(1)
    except EmptyPage:
        # If page is out of range (e.g. 9999), deliver last page of results.
        contacts = paginator.page(paginator.num_pages)

    return render(request, 'list.html', {'contacts': contacts})
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

list.html:[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

```text
{% for contact in contacts %}
    {# Each "contact" is a Contact model object. #}
    {{ contact.full_name|upper }}<br />
    ...
{% endfor %}

<div class="pagination">
    <span class="step-links">
        {% if contacts.has_previous %}
            <a href="?page={{ contacts.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>

        {% if contacts.has_next %}
            <a href="?page={{ contacts.next_page_number }}">next</a>
        {% endif %}
    </span>
</div>
```

[![&#x590D;&#x5236;&#x4EE3;&#x7801;](https://common.cnblogs.com/images/copycode.gif)](javascript:void%280%29;)

final:  根据官方示例代码结果如下，页面共有3页，首页时只有next选项，中间页时可选择previous 或 next，尾页时只有previous选项。

