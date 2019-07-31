# 线程

线程是进程中执行代码的分支，每个执行分支要执行代码需cpu调度 ，线程是**cpu调度的基本单位**，每个进程至少都有一个线程，而这个线程就是我们通常说的主线程。

### 线程类Thread参数说明

#### Thread\(\[group \[, target \[, name \[, args \[, kwargs\]\]\]\]\]\)：

* group：线程组，目前只能使用None
* target：执行的目标任务名
* args：以元组的方式给执行任务传参
* kwargs：以字典方式给执行任务传参
* name：线程名，一般不用设置

#### Thread创建的实例对象的常用方法： <a id="3-&#x542F;&#x52A8;&#x7EBF;&#x7A0B;"></a>

* start\(\)：启动子线程实例（创建子线程）

```python
import threading
import time

def sing():
    print("sing当前执行的线程为：", threading.current_thread())
    for i in range(3):
        print("正在唱歌...%d" % i)
        time.sleep(1)

def dance():
    for i in range(3):
        print("正在跳舞...%d" % i)
        time.sleep(1)


if __name__ == '__main__':
    sing_thread = threading.Thread(target=sing)
    dance_thread = threading.Thread(target=dance)
    
    sing_thread.start()
    dance_thread.start()
```

#### 线程执行带有参数的任务

```python
import threading
import time

def task(count):
    for i in range(count):
        print("任务执行中..")
        time.sleep(0.2)
    else:
        print("任务执行完成")


if __name__ == '__main__':
    sub_thread1 = threading.Thread(target=task, args=(5,))
    sub_thread2 = threading.Thread(target=task, kwargs={"count": 3})
    sub_thread1.start()
    sub_thread2.start()
```

#### 线程之间共享全局变量 <a id="3-&#x7EBF;&#x7A0B;&#x4E4B;&#x95F4;&#x5171;&#x4EAB;&#x5168;&#x5C40;&#x53D8;&#x91CF;"></a>

1. 线程等待\(join\)
2. 互斥锁

#### 线程之间执行是无序的 <a id="2-&#x7EBF;&#x7A0B;&#x4E4B;&#x95F4;&#x6267;&#x884C;&#x662F;&#x65E0;&#x5E8F;&#x7684;"></a>

* 线程执行由cpu调度决定
* 进程执行由操作系统决定

#### 主线程会等待所有的子线程执行结束再结束 <a id="3-&#x4E3B;&#x7EBF;&#x7A0B;&#x4F1A;&#x7B49;&#x5F85;&#x6240;&#x6709;&#x7684;&#x5B50;&#x7EBF;&#x7A0B;&#x6267;&#x884C;&#x7ED3;&#x675F;&#x518D;&#x7ED3;&#x675F;"></a>

1. threading.Thread\(daemon=True\)
2. sub\_thread.setDaemon\(True\)



