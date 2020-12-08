# game

## 网络连接处理以及定时器

1. 本程序使用 `gevent`，单线程
2. 网络连接处理函数 —— `handle(socket, address)` ，用 `Context` 结构管理连接上下文
3. 对每个`request`，应答一个`response`
4. `request`和`response`的格式都是 `[cmd, cmd_param]`，其中：

   cmd是预定义的字符串（net\_protocol.py 中的 CMD\_xxx）

   cmd\_param是list或者dict，为命令参数

5. 在 slots\_game.py 文件底部：

   `command_table` 记录了每个命令的处理函数

   `process_request(ctx, req)` 是命令处理的入口，该函数在 `process_request(ctx, req)` 中被调用

6. 定时器：`handletimeout()`_，_以及 _“_`handle_timeout`” 开头的函数，各自是一个gevent.Greenlet

## 基本技能体系

1. 语言：python \(pip\)、lua、php、c
2. 框架和库：gevent、django、sqlalchemy
3. Linux：常用命令（ps、top，ipconfig、netstat、tail、screen、kill...） shell工具（grep、awk、shell script） vi, crontab, yum, rpm
4. 工具：git、httpd、mysql、redis、nginx、haproxy
5. python 相关工具：virtualenv、Fabric

### gevent

gevent是第三方库，通过greenlet实现协程，其基本思想是：

当一个greenlet遇到IO操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO。

由于切换是在IO操作时自动完成，所以gevent需要修改Python自带的一些标准库，这一过程在启动时通过monkey patch完成

