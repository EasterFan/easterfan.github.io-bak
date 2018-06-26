---
title: 继承中子类对象实例化过程
tags: Java 继承   
---

> 父类静态---子类静态---父类构造代码块---父类构造方法--子类构造代码块---子类构造方法  

父类构造方法不能被子类重写，那么父类构造方法在子类对象实例化过程中起到怎样的作用？  

<!--more-->

![](/assets/img/blog/2017/2017-08-10-seq.png)  

# 测试代码

```bash
public class Catest {
    public static void main(String args[]){

// 父类静态代码块，子类静态代码块已经加载完成
        Cat katty = new Cat();
        System.out.print("以上是子类对象katty实例化过程");
    }
}
```

# 两个阶段
这段代码的执行可以看成两个阶段：  
## 第一阶段：main函数入口到对象实例化之前  
main函数====父类静态属性和静态代码块====子类静态属性和静态代码块  
这就像一个准备阶段，程序在加载父类和子类中的静态属性和方法，对象实例化还没有开始。  

## 第二阶段：对象实例化
程序已经跳到`Cat katty = new Cat();`这一行，按顺序应该跳转到Cat.java中，执行无参构造函数Cat(),但事实却是，程序跳到了Cat.java的父类Animal.java中，准备执行父类的构造方法Animal（），不巧的是，程序检测到Animal.java中有构造代码块（构造代码块优先级高于构造方法代码块），于是，程序先执行父类构造代码块，然后执行父类构造方法代码块。然后，程序跳到Cat.java中，准备执行子类构造方法，同样的，发现子类中也有构造方法代码块，于是，先执行完子类中的构造代码块，然后执行子类构造方法代码块。至此，`Cat katty = new Cat();`这行代码执行完毕，子类对象katty实例化完成，程序执行输出语句`System.out.print("以上是子类对象katty实例化过程");`。  

# 总结
暂时除去静态代码块和构造代码块，继承中子类对象实例化过程可以简化成：  
> 先加载父类构造方法，再加载子类构造方法

Animal是Cat的父类，Cat的实例katty实例化时会先加载Animal的构造方法，更近一步，在加载Animal()这个构造方法之前，程序也加载了object类的构造方法，因为，object是Animal的父类。  

虽然父类的构造方法不能被重写，但他在子类对象实例化过程中起作用。  
子类构造方法中如果没有用`super();`指明父类构造方法，程序默认会加载父类无参构造方法，如果找不到，就要报错了。

[子类对象实例化过程Demo](https://github.com/EasterFan/JavaExercise/tree/master/objectInitInInherihance/src/com/easter)  
卒。  
