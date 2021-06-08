---
title: python跑满cpu
date: 2020-09-24 10:53:32
tags:
---
## 我的是4核8线程8GB，6个进程能跑满cpu

```python
from multiprocessing import  Process

def loop():
    while True:
        pass

if __name__ == '__main__':
    process_list = []
    for i in range(6):  #开启6个子进程执行loop函数
        p = Process(target=loop) #实例化进程对象
        p.start()
        process_list.append(p)

    for i in process_list:
        p.join()

```
