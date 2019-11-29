## Queue
多线程的所使用的功能是没有返回值的，需要将运算的结果放入长队列中

> 代码实现功能，将数据列表中的数据传入，使用四个线程处理，将结果保存在Queue中，线程执行完后，从Queue中获取存储的结果

```python
import threading
import time

from queue import Queue

# 定义一个多线程调用函数
def job(L, q):
    """对列表的每个元素进行平方计算，将结果保存在队列中"""
    for i in range(leng(L)):
        L[i] = L[i]**2

    # 不能return
    q.put(L)

def multithreading():
    # 定义队列，保存返回值，代替return
    q = Queue()
    threads = {}

    data = [[1,2,3],[3,4,5],[4,4,4],[5,5,5]]

    # 定义4个线程
    for i in range(4):
        t = threading.Thread(target=job,args=(data[i],q))
        # 开始线程
        t.start()
        # 将该线程添加到多线程的列表中
        threads.append(t)

    # 分别join四个线程到主线程
    for thread in threads:
        thread.join()
    
    # 所有线程运行完成
    results = []

    # 将四个线程运行后保存在队列中的结果返回给空列表results
    for i in range(4):
        # q.get()按顺序取出一个值
        results.append(q.get())

    print(results)

if __name__ == '__main__':
    multithreading()
```