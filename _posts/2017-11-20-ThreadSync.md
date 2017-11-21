---
title: 多线程互斥
tags: Java 线程同步 
---
> 多线程依次完整的执行，保证代码执行的完整性。

<!--more-->

# 线程同步的意义
多线程执行的时候，线程之间通过抢占CPU资源获得执行机会，某个线程的执行时间，多个线程执行的顺序是未知的。  

这样，当多个线程共享一个实例对象时，就会出现线程执行到一半，数据未更新进程被终止的情况。  

但我们要在未知中求得已知，虽然我们不知道线程执行的顺序和时间，但我们要保证，同一时刻，一个共享对象，只有一个线程在操作它。  

比如，银行存取款系统中，多个用户同时进行存取款操作，A用户取款，B用户存款，A用户取款的进程被B用户打断终止，就会出现A取款后，账户余额没有变化的情况。  
这是因为A取款后，准备对余额操作时，被B线程打断了，提前终止A线程，数据更新失败。  

线程同步解决这个问题的原理是：  
当A线程对Bank类进行数据操作时，B线程不能对Bank类操作，反之依然。  

线程同步：一个线程在操作共享对象时，**锁定该对象**，必须等到该线程结束后，其他线程才可以操作共享对象。  

# 线程同步的方法--synchronized
synchronized关键字可以用在**成员方法**，**静态方法**，**语句块**中。  

```bash
public synchronized void  moneyIn(){
        // 先获取当前余额
        int balance1 = getBalance();
        System.out.println("存款前的余额"+balance1);

        // 模拟存款进程被打断
//        try {
//            Thread.sleep(1000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }

        // 存入￥10
        balance1 += 10;

        // 修改账户余额
        setBalance(balance1);

        // 输出存款后的余额
        System.out.println("存款后的余额为"+getBalance());
    }
```
使用代码块，传入被锁定的共享对象。

```bash
public void moneyOut() {

        synchronized (this) {
            // 获取余额
            int balance2 = getBalance();
            System.out.println("取款前的余额" + balance2);
            // 取出余额
            balance2 -= 5;

            // 模拟取款进程被打断
//        try {
//            Thread.sleep(1000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }

            // 修改余额
            setBalance(balance2);
            //输出取款后的余额
            System.out.println("取款后的余额为" + getBalance());
        }
    }

```

[线程互斥--银行存取款](https://github.com/EasterFan/JavaExercise/blob/master/ThreadProj/src/_BankTest.java)  
卒。  



