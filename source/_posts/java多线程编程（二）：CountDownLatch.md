---
title: java多线程编程（二）：CountDownLatch
comment: true
date: 2017-11-02 10:24:16
tags: [Java, 多线程, CountDownLatch]
---

# 1. CountDownLatch是什么
CountDownLatch，一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待。

**主要方法：**

``` java
public CountDownLatch(int count); // 构造方法,参数指定了计数的次数 
public void countDown(); // 当前线程调用此方法，则计数减一
public void await() throws InterruptedException // 调用此方法会一直阻塞当前线程，直到计时器的值为0
```

# 2. CountDownLatchde的使用场景

1. **实现最大的并行性**：有时我们想同时启动多个线程，实现最大程度的并行性。例如，我们想测试一个单例类。如果我们创建一个初始计数为1的CountDownLatch，并让所有线程都在这个锁上等待，那么我们可以很轻松地完成测试。我们只需调用 一次countDown()方法就可以让所有的等待线程同时恢复执行。
2. **开始执行前等待n个线程完成各自任务**：例如应用程序启动类要确保在处理用户请求前，所有N个外部系统已经启动和运行了。
3. **死锁检测：**一个非常方便的使用场景是，你可以使用n个线程访问共享资源，在每次测试阶段的线程数目是不同的，并尝试产生死锁。

# 3. 实例：实现最大的并行性

> **问题描述：** 我们使用for循环创建50个线程，使用CountDownLatch.await()方法让所有新创建的线程都处于阻塞状态，当所有线程创建完成后使用CountDownLatch.countDown()方法让所有线程同时解除等待状态，恢复执行。

**代码实例：**

``` java
import java.util.Random;
import java.util.concurrent.CountDownLatch;

public class Main {
    public static void main(String[] args) {
        CountDownLatch latch = new CountDownLatch(1);
        int count = 50;
        for (int i = 0; i < count; i++) {
            String threadName = "Thread" + i;
            new Thread(new RunnableDemo(threadName, latch)).start();
            
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

//        latch.countDown();
    }
}

class RunnableDemo implements Runnable {
    private String threadName;
    CountDownLatch latch;

    RunnableDemo(String name, CountDownLatch latch) {
        threadName = name;
        this.latch = latch;
    }

    public void run() {
//        try {
//            this.latch.await();
            System.out.println("Running " + threadName);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
    }
}

```

以下是不使用CountDownLatch的执行结果：

``` 
Running Thread0
Running Thread1
Running Thread2
Running Thread3
Running Thread4
Running Thread5
Running Thread6
Running Thread7
Running Thread8
Running Thread9
...
```

以下是使用CountDownLatch后的执行结果：

（即打开以上代码的注释部分）

```
Running Thread5
Running Thread2
Running Thread11
Running Thread1
Running Thread13
Running Thread14
Running Thread17
Running Thread19
Running Thread21
Running Thread24
...
```

**结论：** 可见使用CountDownLatch可以实现多个线程的最大并行性。

# 参考文献：

- [java 多线程 CountDownLatch用法](http://www.iteye.com/topic/1002652)
- [Java之CountDownLatch使用](http://blog.csdn.net/shihuacai/article/details/8856370)
- [什么时候使用CountDownLatch](http://www.importnew.com/15731.html)

