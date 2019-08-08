# 进程

一个程序运行至少有一个进程，一个进程默认有一个线程，进程里可创建多个线程，线程依附进程。

**进程是操作系统进行资源分配的基本单位**。

### Process进程类的说明

#### Process\(\[group \[, target \[, name \[, args \[, kwargs\]\]\]\]\]\)：

* group：指定进程组，目前只能使用None
* target：执行目标任务名
* name：进程名字，默认为Process-N，N为从1开始递增的整数
* args：以元组方式给执行任务传参
* kwargs：以字典方式给执行任务传参

#### Process创建的实例对象的常用方法：

* start\(\)：启动子进程实例（创建子进程）
* join\(\)：等待子进程执行结束
* terminate\(\)：不管任务是否完成，立即终止子进程

```python
import multiprocessing
import time
import os

def dance():
    print("dance:", os.getpid())  # 获取当前进程的编号
    print("dance:", multiprocessing.current_process())  # 获取当前进程
    print("dance的父进程编号:", os.getppid())  # 获取父进程的编号
    for i in range(5):
        print("跳舞中...")
        time.sleep(0.2)
        os.kill(os.getpid(), 9)  # 扩展:根据进程编号杀死指定进程

def sing():
    print("sing:", os.getpid())
    print("sing:", multiprocessing.current_process())
    print("sing的父进程编号:", os.getppid())
    for i in range(5):
        print("唱歌中...")
        time.sleep(0.2)


if __name__ == '__main__':
    dance_process = multiprocessing.Process(target=dance)
    sing_process = multiprocessing.Process(target=sing)

    dance_process.start()
    sing_process.start()
```

#### 进程执行带有参数的任务

```python
import multiprocessing
import time

def task(count):
    for i in range(count):
        print("running")
        time.sleep(0.2)
    else:
        print("over")


if __name__ == '__main__':
    # 创建子进程
    # args: 以元组的方式给任务传入参数
    sub_process1 = multiprocessing.Process(target=task, args=(5,))
    sub_process2 = multiprocessing.Process(target=task, kwargs={"count": 3})
    sub_process1.start()
    sub_process2.start()
```

#### 进程之间不共享全局变量 <a id="3-&#x8FDB;&#x7A0B;&#x4E4B;&#x95F4;&#x4E0D;&#x5171;&#x4EAB;&#x5168;&#x5C40;&#x53D8;&#x91CF;&#x7684;&#x5C0F;&#x7ED3;"></a>

创建子进程会对主进程资源进行拷贝，子进程是主进程的一个副本，多个进程操作的不是同一个进程里面的全局变量。

#### 主进程会等待所有的子进程执行结束再结束 <a id="4-&#x4E3B;&#x8FDB;&#x7A0B;&#x4F1A;&#x7B49;&#x5F85;&#x6240;&#x6709;&#x7684;&#x5B50;&#x8FDB;&#x7A0B;&#x6267;&#x884C;&#x7ED3;&#x675F;&#x518D;&#x7ED3;&#x675F;"></a>

* multiprocessing.Process\(daemon=True\)
* sub\_process.terminate\(\)

\*\*\*\*

