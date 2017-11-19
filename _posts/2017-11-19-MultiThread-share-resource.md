---
title: Runnable实现类的多线程资源共享
tags: Java 线程 资源共享 
---

> 实现Runnable接口创建两个线程，这两个线程共享同一个对象（Runnable实现类）的**成员变量**。  

<!--more-->

# 不共享的状态

```bash
class PrintRunnable implements Runnable{
    @Override
    public void run() {
        int i = 0;
        while (i<5) {
            // 只能调用Thread类的静态方法
            System.out.println(Thread.currentThread().getName() + "正在运行"+(i++));
            // i++与++i的区别在这里显现
        }
    }
}

public class RunnableTest {
    public static void main(String[] args) {
        // 启动线程三步
        // 1.新建自定义线程类对象
        PrintRunnable printRunnable1 = new PrintRunnable();
        PrintRunnable printRunnable2 = new PrintRunnable();

        // 2.将自定义线程类对象作为参数，新建Tread线程对象
        Thread thread1 = new Thread(printRunnable1);
        Thread thread2 = new Thread(printRunnable2);

        // 3.通过Thread类的start方法启动线程
        thread1.start();
        thread2.start();
    }
}

```
两个子线程各自执行5次  
![](/assets/img/blog/java/2017-11-19-noshare.png)  

# 共享的状态

## 共享的条件

1. 自定义的线程类通过实现Runnable接口创建  
2. 两个子线程来自同一个自定义线程类对象  
3. 被共享的是成员变量  

当i是成员变量时，才会被共享，共享后，两个子线程一共执行了5次。  
当i是局部变量时，不共享，两个线程各自执行5次。  

## 代码

```bash

class ShareRunnable implements Runnable{

    // 被共享的成员变量
    int i = 0;
    
    @Override
    public void run() {

        while (i<5) {
            System.out.println(Thread.currentThread().getName() + "正在运行"+(i++));
            // i++与++i的区别在这里显现
        }
    }
}


public class MultiThreadShareTest {
    public static void main(String[] args) {
        ShareRunnable sr = new ShareRunnable();

        Thread t1 = new Thread(sr);
        Thread t2 = new Thread(sr);

        t1.start();
        t2.start();

    }
}


```

![](/assets/img/blog/java/2017-11-19-share.png)
## 共享的意义
线程间共享对象和成员变量，可以实现交互处理和数据交换。  

卒。  




