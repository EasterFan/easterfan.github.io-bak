---
title: 多线程通信
tags: Java 线程通信 死锁  
---

> 线程互斥使线程**单独**使用共享对象，线程通信可以让线程**有序**的使用共享对象。  

<!--more-->

# 线程通信的意义
比如在银行存取款过程中，用户需要先存款，才能取款，但线程是通过争抢来获得CPU使用权，很多时候，取款线程争抢到了CPU时间片，就会优先执行。  

线程通信要做的是：在取款线程中加一段判断逻辑，如果余额为0，就阻塞取款线程，让存款线程执行。  

线程通信的意义在于：我们可以用处理逻辑对线程排序。  

# 线程通信的方法

```bash
Boolean flag = false;

public int getN() {
        // 如果没有，get方法等待
        if(!flag){

            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("Customer取出"+ n);
        flag = false;// 取出完毕，容器中没有数据
        return n;
    }
```

# 线程通信的问题--死锁

所有线程都不运行了，都在等待。  

## 死锁出现的原因
线程通信的原理是使用wait()告诉线程，让该线程停止争抢，即通过阻塞线程来停止。  
那么，程序中会出现一种场景，满足一定条件时，所有的线程都被阻塞了，程序终止。  
程序要持久的运行，我们还需要唤醒线程。  

## 唤醒线程
在不确定唤醒哪个线程的时候，用`notifyAll();`唤醒所有线程。  

```bash
Boolean flag = false;

public int getN() {
        // 如果没有，get方法等待
        if(!flag){

            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println("Customer取出"+ n);
        flag = false;// 取出完毕，容器中没有数据
	notifyAll();
        return n;
    }
```

[线程通信--生产消费依次执行](https://github.com/EasterFan/JavaExercise/blob/master/ThreadProj/src/ThreadCommunicate/PublicShareTest.java)  

卒。  


