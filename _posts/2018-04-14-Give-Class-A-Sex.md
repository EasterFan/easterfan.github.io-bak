---
title: 当我看到一个类时, 我看到的是什么
tags: java 类与对象
---
> 怎样描述一个类

<!--more-->

## 第一印象
> 属性, 方法, 描述, 肤浅的表面

属性: 公共属性和特有属性

方法: 共性方法, 特性方法, getter和setter方法, 以及,构造方法
getter和setter方法, 在操作属性时,可能需要加的过滤条件
构造方法中, 哪些属性是这个类与生俱来的, 哪些是可以从父类中 super 来的.

描述: toString() 转为字符串描述

比如,一个学生类:
```java
class Student{
    private String name;
    private int age;

    Student(String name,int age){
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

@Override
    public String toString() {
        return name + ":" + age;
    }
  }
```
## 第二印象
>  比较性, 唯一性   横向扩展

当一个类可能生成很多对象时，就要考虑这些对象，将来可能被存入到容器中（有可能是HashSet集合或TreeSet集合）。  

该考虑是否为该类添加**比较性**和**唯一性**  

如果该类存入TreeSet（二叉树数据结构）中需要具备**比较性**：  
```java
class Student implements Comparable<Student>{
    private String name;
    private int age;

    Student(String name,int age){
        this.name = name;
        this.age = age;
    }

/*
  @Override
    public int compareTo(Object obj) {
        if(!(obj instanceof Student))
            throw new ClassCastException("类型不匹配");
        Student s = (Student)obj;

        int num = this.name.compareTo(s.name);
        if(num == 0)
            return this.age - s.age;
        return num;
    }
*/

// Comparable 使用泛型, 不用判断类型转换异常
@Override
    public int compareTo(Student s) {
        int num = this.name.compareTo(s.name);
        if(num == 0)
            return this.age - s.age;
        return num;
    }
}
```

如果该类存入HashSet（哈希表数据结构）集合中，需具备**唯一性**：  
根据需求，重写hashcode()和equals()方法。HashSet会先比较两个对象的哈希值是否相同(hashCode)，哈希值相同时会再比较对象类型（equals）是否相同，如果哈希值和对象类型都相同，存储失败。
```java
class Student implements Comparable{
    private String name;
    private int age;

    Student(String name,int age){
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if(!(obj instanceof Student))
            throw new ClassCastException("类型不匹配");
        Student s = (Student)obj;
        return this.name.equals(s.name) && this.age == s.age;
    }

    @Override
    public int hashCode() {
        return name.hashCode() + age * 34;
    }
}
```

## 第三印象

> `(父类 || 子类 ) ? 父类方法 : 子类实现` 纵向扩展

比如 Set 集合, 向上看, 它拥有的方法和 Collection 接口的方法完全一致, 没有特有方法,

向下看, 它的实现类有`HashSet`和`TreeSet`, HashSet 的底层实现是哈希表, TreesSet 的底层实现是二叉树,

比如List集合, 向上看，与父类 Collection 相比，它具有自己的特有方法：ListIterator 迭代方法， ListIterator 是 Iterator 的子类，拥有更多的功能，比如在迭代的同时操作 ArrayList 数组。  

向下看， List 的实现类有 `ArrayList`, `LinkedList`， `Vector`,  
`ArrayList`底层是数组，`LinkedList`底层是链表，前者的优势在于可通过角标进行频繁快速的查找，后者的优势在于进行频繁快速的增删操作，`Vector`是一个逐渐被ArrayList完全替代的老集合（jdk1.0），底层实现也是数组，通过枚举取出对象（方法名冗长），与ArrayList的异步相比，Vector 是同步的，也意味着安全而慢速

向上看一个类，关注这个类能否再向上提取共性方法，能否进一步成为抽象类。
向下看一个类，对比子类而演变过程，关注这个类衍生的子类之间各自适用的场景，关注被逐渐淘汰的子类和新生的子类，看到这个体系的发展历史

## 其它幻想
> 属性的多面性，以及，**类的性别**

从上面三个角度，就足以完全刻画描绘一个类了吗？  

自然界中二氧化碳加压下会变成干冰，再加压，会变成石墨，更高压下就变成金刚石。  
人活着的时候是人的样子。人死着的时候是鬼的样子或仙的样子。

实体的属性是依附于环境而存在的，属性并不是一成不变的。它具有多面性，且随环境这个自变量的变化而变化。
那么定义属性的时候是否可以这样写：  
```java
class CO2{
    private String name("common pressure");
    private String name("medium pressure");
    private String name("high pressure");
    private String name("very high pressure");

    Student(String name(Environment e)){
        this.name = name;
    }
}
```

继承作为类的三大特性，它现在所做的事情就是**复制**和**添加**（重写）。子类重写父类方法的依据在哪里呢？换句话说，子类是怎样判断自己该怎样重写父类的方法呢？  

生物进化的过程是染色体分离和重组的过程，java 继承的局限性，在于其单继承简单粗暴的复制和重写，即使在接口处实现了多继承，也没有解决**组合替代复制**的问题，毕竟接口方法都没有方法体。  

这样看来，目前的所有面向对象的语言都是有局限性的，因为它们在定义类的属性时，包括在继承时，都没有定义**类的性别**，从继承命名上就可以看出来，只知父类而不知母类（当然有可能是翻译的问题），主要原因是继承时，子类重写哪些方法，怎样重写，都是由程序员根据当前需求手动选择重写的，它的局限性就在于继承来的子类在当前特定环境是固定的，有效的。如果属性的环境参数稍微变化，这个子类就失效了，如果能用统筹兼顾的方法，在继承时考虑到子类属性参数的环境因素（子类属性由父类和母类属性参数组合而成），是否可以让子类更加具有适应性？

继承中怎样实现组合？

双继承且不同性别的类可以实现组合。即在进行对象实例化的时候，就把性别作为初始化参数传入到对象中，并且在继承时引入类的性别参数，子类通过对父类和母类属性的环境参数综合运算，由随机数随机生成子类的性别。同时，子类的方法来自父类和母类的组合，同名方法的参数微调（根据父类和母类方法的环境参数得出），即对父类和母类的同名方法参数进行组合，使得子类的方法更适应当前的环境。  

给类定义性别的意义在哪？  
为了基因的重组，为了让子类的方法更加多元而适应当前的环境，更夸张的说，是为了实现子类的自适应继承。  
你看，就算是雌雄同体的蜗牛，也是通过异体受精完成继承，性别之分在自然界实体中是普遍存在的属性，我们在用语言去描述实体时，是否也可以考虑纳入其中？就像这样：  
```java
class Snail<XY>{
    private String name("land");

    void eat(Environment e1,Environment e2){
        System.out.println(e1 + e2);
    }
}

class Snail<XX>{
    private String name("sea");
    void eat(Environment e3,Environment e4){
        System.out.println(e3 + e4);
    }
}

class SnailSon<XY> extends Snail<XY>,Snail<XX>{
    private String name("land");
    void eat(Environment e1,Environment e4){
        System.out.println(e1 + e4);
    }

    void eat(Environment e1,Environment e2,Environment e4){
        System.out.println(e1 + e2 + e4);
    }
}

class SnailDau<XX> extends Snail<XY>,Snail<XX>{
    private String name("land");
    void eat(Environment e1,Environment e4){
        System.out.println(e1 + e4);
    }

    void eat(Environment e1,Environment e2,Environment e4){
        System.out.println(e1 + e2 + e4);
    }
}
```
[痴人猜想,请勿当真]卒.
