---
title: 对象常量池
tags: Java 常量池
---

对象常量池是JVM中一块特殊的内存地址，用来保存用过的对象。以提高程序的效率，具体应用有String类型和包装类。  

<!--more-->

# 对象池的作用
减少程序的内存开销，提高程序效率。  
java中对象的生命周期有三个阶段：对象的创建（T1），对象的使用（T2），对象的清除（T3）。对象池通过缓存和共享对象，省去了对象重复创建和销毁的时间T1和T2。  

# 对象池是什么

对象池类就像是一个对象管理员，对象池在类被初始化的时候就被初始化了，实例就被创建出来，对象池以Static列表的形式存存储多个对象，每一个对象还要加一个标记，标记该对象是否被占用。然后，用户可以向这个类索取对象，如果池中所有的对象都已经被占用了，那么抛出异常。用户用完以后，还要把对象“还”回来，即释放占用。

对象池技术基本原理的核心有两点：**缓存**和**共享**，即对于那些被频繁使用的对象，在使用完后，不立即将它们释放，而是将它们缓存起来，以供后续的应用程序重复使用，从而减少创建对象和释放对象的次数，进而改善应用程序的性能。


# 对象池的应用--String类

```bash
	String a = "blast";
        String b = "rain";
        b = b + "sky";
```

在执行`b = b + "sky";`后，池中变量值有三个：blast，rain，rain sky，但是变量b的引用不再指向rain了，而是指向rain sky，rain在池中，等待回收。
当编译期遇见String 字符串时，对象池检查该池内是否已经存在相同的String 字符串，如果找到，就把新变量的引用指向现有的字符串对象，不创建任何新的String 常量对象，没找到再创建新的。所以对一个字符串对象的任何修改，都会产生一个新的字符串对象，原来的依然存在，等待垃圾回收。  


# 对象池的应用--包装类

```bash
public class ObjectPool_Wrap {
    public static void main(String args[]){

        // 1. 等号两边比较的是对象---new关键字创建两个不同的内存空间，内存地址不相等
        Integer one = new Integer(100);
        Integer two = new Integer(100);
        System.out.println("one == two的结果："+(one == two)); // 1
	System.out.println("one == two的结果11："+(one.equals(two))); // 11

        // 2. 等号两边比较的是int数值---由于自动装箱和拆箱
        Integer three = 100; // 自动装箱
        System.out.println("three == 100 的结果："+(three == 100)); // 2 自动拆箱

        // 3. 等号两边比较的是对象--- three和four指向对象池中同一块内存空间
        Integer four = 100;
//        Integer four = Integer.valueOf(100);
        System.out.println("three == four 的结果："+(three == four)); // 3

        // 4. 等号两边比较的是int数值---由于自动装箱和拆箱
        Integer five = 200;
        System.out.println("five == 200 的结果："+(five == 200)); // 4

        // 5. 等号两边比较的是对象--- 超出对象池范围，new新的对象
        Integer six = 200;
        System.out.println("six == five 的结果："+(five == six)); // 5

        // 6. 等号两边比较的是对象--- double和float没有对象常量池的概念
        Double seven = 100.0;
        Double eight = 100.0;
//        Double seven = Double.valueOf(100);
        System.out.println("seven == eight 的结果："+(seven == eight)); // 6
    }
}

```

1. 用new String（）创建的字符串不是常量，不能在编译期就确定，所以new String（）创建的字符串不放入常量池中，他们有自己的地址空间。  

2. 包装类对象常量池的取值范围是【-128 ～ 127】，此范围之外将重新new一个对象  

3. 浮点型Double和Float不能池化，即使在【-128 ～ 127】的范围内也不可以。  

4. 两个对象的比较，equals比较内容，== 比较内存地址，按需使用。  

5. `Integer four = 100;`自动装箱的过程，编译器执行的是`Integer four = Integer.valueOf(100);`,返回Integer对象，而`Integer.parseInt(100)`返回的是int整型。  

# 参考资料：
对象池Demo  http://www.cnblogs.com/javawebsoa/p/3243964.html  

卒。  
