# gevent

## 一：前言

协程又称为微线程，纤程。英文名Coroutine:协程是一种用户态的轻量级线程

协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复之前保存的寄存器上下文和栈。因此：

> 协程能够保留上次一调用时的状态，能够进入上一次离开时所处的逻辑流的位置

 协程的好处

* 无需线程上下文切换的开销
* 无需原子操作\(不会被线程调度机制打断的操作\)锁定以及同步的开销
* 方便切换控制流，简化编程模型
* 高并发+高扩展性+低成文：一个CPU支持上完的协程都不是问题，所以很适合高并发处理

协程的缺点

* 无法利用多核资源：协程的本质是单线程，需要和进程配合才能运行在多CPU上
* 进行阻塞\(Blocking\)操作\(如IO时\)会阻塞掉整个程序

协程的条件

* 必须在只有一个单线程里实现并发
* 修改共享数据不需加锁
* 用户程序里自己保存多个控制流的上下文栈
* 一个协程遇到IO操作自动切换到其它协程

**使用yield实现协程**

```text
def consumer(name):    print("--->starting eating baozi...")    while True:        new_baozi = yield        print("[%s] is eating baozi %s" % (name, new_baozi)) def producer():    next(con)    next(con2)    n = 0    while n < 5:        n += 1        con.send(n)        con2.send(n)        print("\033[32;1m[producer]\033[0m is making baozi %s" % n) if __name__ == '__main__':    con = consumer("c1")    con2 = consumer("c2")    p = producer()
```

## 二：Greenlet

greenlet是一个用C实现的协程模块，相比与python自带的yield，它可以使你在任意函数之间随意切换，而不需把这个函数先声明为generator

**使用greenlet实现协程**

```text
from greenlet import greenletdef f1():    print(12)    gr2.switch()    print(34)    gr2.switch()def f2():    print(56)    gr1.switch()    print(78)if __name__=='__main__':    gr1 = greenlet(f1)    gr2 = greenlet(f2)    gr1.switch()     #手动切换，gevent是对greenlet的封装，实现自动切换 #输出结果 12\n 56\n 34\n 78\n
```

## 三：Gevent

Gevent是一个第三方库\(需要额外自己安装\)，可以轻松通过gevent实现并发同步或异步编程，在gevent中主要用到的模式是Greenlet，它是以C扩展模块形式接入Python的轻量级协程。Greenlet全部运行在主程序操作系统的内部，被协作式调度

**使用gevent库实现协程**

```text
import gevent def func1():    print("func1 running")    gevent.sleep(2)             # 内部函数实现io操作    print("switch func1") def func2():    print("func2 running")    gevent.sleep(1)    print("switch func2") def func3():    print("func3  running")    gevent.sleep(0.5)    print("func3 done..") if __name__=='__main__':    gevent.joinall([gevent.spawn(func1),                    gevent.spawn(func2),                    gevent.spawn(func3),                    ])
```

**同步与异步的性能区别**

```text
import gevent def task(pid):    """    Some non-deterministic task    """    gevent.sleep(0.5)    print('Task %s done' % pid) def synchronous():    for i in range(1, 10):        task(i) def asynchronous():    threads = [gevent.spawn(task, i) for i in range(10)]    gevent.joinall(threads) if __name__ =='__main__':    print('Synchronous:')    synchronous()     print('Asynchronous:')    asynchronous()
```

将task函数封装到Greenlet内部线程的`gevent.spawn`。 初始化的greenlet列表存放在数组`threads`中，此数组被传给`gevent.joinall` 函数，后者阻塞当前流程，并执行所有给定的greenlet。执行流程只会在 所有greenlet执行完后才会继续向下走。　　

**遇到IO阻塞时会自动切换任务**

```text
from gevent import monkey monkey.patch_all()import geventfrom  urllib.request import urlopen def f(url):    print('GET: %s' % url)    resp = urlopen(url)    data = resp.read()    print('%d bytes received from %s.' % (len(data), url)) if __name__=='__main__':    gevent.joinall([        gevent.spawn(f, 'https://www.python.org/'),        gevent.spawn(f, 'https://www.yahoo.com/'),        gevent.spawn(f, 'https://github.com/'),    ])
```

**通过gevent实现单线程下的多socket并发**

**server端**

```text
import sysimport socketimport timeimport gevent from gevent import socket,monkeymonkey.patch_all()  def server(port):    s = socket.socket()    s.bind(('0.0.0.0', port))    s.listen(500)    while True:        cli, addr = s.accept()        gevent.spawn(handle_request, cli)   def handle_request(conn):    try:        while True:            data = conn.recv(1024)            print("recv:", data)            conn.send(data)            if not data:                conn.shutdown(socket.SHUT_WR)     except Exception as  ex:        print(ex)    finally:        conn.close()if __name__ == '__main__':    server(8001)
```

**client 端**

```text
import socket HOST = 'localhost'    # The remote hostPORT = 8001           # The same port as used by the servers = socket.socket(socket.AF_INET, socket.SOCK_STREAM)s.connect((HOST, PORT))while True:    msg = bytes(input(">>:"),encoding="utf8")    s.sendall(msg)    data = s.recv(1024)    #print(data)     print('Received', repr(data))s.close()
```

**并发100socket连接**

```text
import socketimport threading def sock_conn():     client = socket.socket()     client.connect(("localhost",8001))    count = 0    while True:        #msg = input(">>:").strip()        #if len(msg) == 0:continue        client.send( ("hello %s" %count).encode("utf-8"))         data = client.recv(1024)         print("[%s]recv from server:" % threading.get_ident(),data.decode()) #结果        count +=1    client.close()  for i in range(100):    t = threading.Thread(target=sock_conn)    t.start()
```

