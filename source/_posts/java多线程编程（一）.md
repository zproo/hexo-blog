---
title: java多线程编程（一）
comment: true
date: 2017-10-30 15:39:10
tags: [Java, 多线程]
---

# 1. 线程与进程

1. 线程：进程中负责程序执行的执行单元。线程本身依靠程序进行运行，线程是程序中的顺序控制流，只能使用分配给程序的资源和环境
2. 进程：执行中的程序，一个进程至少包含一个线程
3. 单线程：程序中只存在一个线程，实际上主方法就是一个主线程
4. 多线程：在一个程序中运行多个任务，目的是更好地使用CPU资源

# 2. 一个线程的生命周期

![线程的生命周期图](http://www.runoob.com/wp-content/uploads/2014/01/java-thread.jpg)

- 创建（new）状态: 准备好了一个多线程的对象
- 就绪（runnable）状态: 调用了`start()`方法, 等待CPU进行调度
- 运行（running）状态: 如果就绪状态的线程获取 CPU 资源，就可以执行`run()`方法
- 阻塞（blocked）状态: 暂时停止执行, 可能将资源交给其它线程使用。如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。
- 终止（dead）状态: 线程销毁

# 3. 线程的实现

创建线程有两种主要的方式：

- 通过实现 Runnable 接口；（推荐）
- 通过继承 Thread 类本身；

## 3.1 通过实现Runnable接口来创建线程

实例：

``` java
public class Main {
    public static void main(String args[]) {
        RunnableDemo R1 = new RunnableDemo( "Thread-1");
        Thread t1 = new Thread(R1);
        t1.start();

        RunnableDemo R2 = new RunnableDemo( "Thread-2");
        Thread t2 = new Thread(R2);
        t2.start();
    }
}
class RunnableDemo implements Runnable {
    private String threadName;

    RunnableDemo( String name) {
        threadName = name;
        System.out.println("Creating " +  threadName );
    }

    public void run() {
        System.out.println("Running " +  threadName );
        try {
            for(int i = 4; i > 0; i--) {
                System.out.println("Thread: " + threadName + ", " + i);
                // 让线程睡眠一会
                Thread.sleep(50);
            }
        }catch (InterruptedException e) {
            System.out.println("Thread " +  threadName + " interrupted.");
        }
        System.out.println("Thread " +  threadName + " exiting.");
    }
}
```

运行输出：

``` 
Creating Thread-1
Creating Thread-2
Running Thread-2
Thread: Thread-2, 4
Running Thread-1
Thread: Thread-1, 4
Thread: Thread-1, 3
Thread: Thread-2, 3
Thread: Thread-2, 2
Thread: Thread-1, 2
Thread: Thread-1, 1
Thread: Thread-2, 1
Thread Thread-2 exiting.
Thread Thread-1 exiting.
```

## 3.2 通过继承Thread来创建线程

实例：

``` java
public class Main {
    public static void main(String args[]) {
        ThreadDemo T1 = new ThreadDemo( "Thread-1");
        T1.start();

        ThreadDemo T2 = new ThreadDemo( "Thread-2");
        T2.start();
    }
}
class ThreadDemo extends Thread {
    private String threadName;

    ThreadDemo( String name) {
        threadName = name;
        System.out.println("Creating " +  threadName );
    }

    public void run() {
        System.out.println("Running " +  threadName );
        try {
            for(int i = 4; i > 0; i--) {
                System.out.println("Thread: " + threadName + ", " + i);
                // 让线程睡醒一会
                Thread.sleep(50);
            }
        }catch (InterruptedException e) {
            System.out.println("Thread " +  threadName + " interrupted.");
        }
        System.out.println("Thread " +  threadName + " exiting.");
    }
}

```

输出结果：

```
Creating Thread-1
Creating Thread-2
Running Thread-1
Thread: Thread-1, 4
Running Thread-2
Thread: Thread-2, 4
Thread: Thread-1, 3
Thread: Thread-2, 3
Thread: Thread-2, 2
Thread: Thread-1, 2
Thread: Thread-2, 1
Thread: Thread-1, 1
Thread Thread-2 exiting.
Thread Thread-1 exiting.
```

## 3.3 为什么推荐使用Runnable接口的方式

Runnable接口和Thread之间的联系：

`public class Thread extends Object implements Runnable`

我们发现Thread类其实也是Runnable接口的子类。

在程序开发中只要是多线程都以实现Runnable接口为主，因为实现Runnable接口相比继承Thread类有如下好处：

- 避免点继承的局限，一个类可以继承多个接口。
- 适合于资源的共享

# 参考文献：

- [Java 多线程与并发编程专题](https://www.ibm.com/developerworks/cn/java/j-concurrent/)
- [Java多线程干货系列（1）：Java多线程基础](http://www.importnew.com/21136.html)
- [菜鸟教程：Java多线程编程](http://www.runoob.com/java/java-multithreading.html)
- [Java中Runnable和Thread的区别](http://developer.51cto.com/art/201203/321042.htm)


