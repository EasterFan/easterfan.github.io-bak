---
title: Java中的代码块
tags: Java 
---

> Java中四种代码块的优先级：静态代码块 > 构造代码块 > 构造方法代码块 > 普通代码块

<!--more-->

```bash
package com.easter.zoo;

public class codeTest {

    public void run(){
        {
            System.out.print("我是普通代码块1\n");
        }

        {
            System.out.print("我是普通代码块2\n");
        }
    }

    public codeTest(){
        System.out.print("我是构造方法代码块\n");
    }

    {
        System.out.print("我是构造代码块\n");
    }

    static {
        System.out.print("我是静态代码块\n");
    }

    public static void main(String args[]){
        codeTest codeBlock = new codeTest();
        codeBlock.run();
    }
}

```
输出结果：  
```bash
我是静态代码块
我是构造代码块
我是构造方法代码块
我是普通代码块1
我是普通代码块2
```
- 关于执行次数
静态代码块在类被创建时加载一次，  
构造代码块和构造方法代码块在类每创建一个实例时，都会加载。  
**构造代码块**是给所有对象进行统一初始化(不同对象的共性内容),  
**构造方法代码块**是给对应的对象初始化(该对象的特性).   
对于一些只需要执行一次的代码，我们可以放在静态代码块中提升效率。  



- 关于在一个方法中定义同名局部变量  

```bash
package com.easter.zoo;


public class codeTest {

    public void run(){
        
         // int test = 5;
        {
            int test = 1;
            System.out.print("我是普通代码块1"+test+"\n");
        }
        
        // int test = 4;

        {
            int test = 2;
            System.out.print("我是普通代码块2"+test);
        }
    }
    

    public static void main(String args[]){
        codeTest codeBlock = new codeTest();
        codeBlock.run();
    }
}

```

输出结果：  
```bash
我是普通代码块11
我是普通代码块22
```

当test1和test2被定义时，运行正常；  

当定义test4时，test1正常，test2报错：重复定义变量;  

当定义test5时，test1和test2同时报错：重复定义变量   
 

> 这是一个局部变量作用域的问题

局部变量的作用域是从变量**定义的位置**到**方法体结束**。  

卒。

