---
title: set集合中的泛型
tags: Java 集合
---

> 为什么要在set集合中使用泛型？

<!--more-->

今天在使用set集合添加集合元素时，编译器总报警告：  
![](/assets/img/blog/2017/2017-11-18-warning.png)  

# 不使用泛型
```bash
	Set homeSet = new HashSet();
	Iterator it = homeSet.iterator();

	Pet_set Tom = new Pet_set("Tom",12,"cat");
	Pet_set tomm = null;
        homeSet.add(Tom);

  	while (it.hasNext()){
            tomm = (Pet_set) it.next();
            if(tomm.getName().equals("Tom")){
                System.out.println("find tom");
                break;
            }
        }
```

# 引入泛型
set集和迭代器同时引入泛型，区别：遍历迭代器时，不需要强制类型转换  
泛型的作用：限制了set集合元素只能是这个类型。手动强转成其他类型会在编译时报错。  

```bash
	Set<Pet_set> delSet = new HashSet<Pet_set>();
        Iterator<Pet_set> itdel = homeSet.iterator();

	Pet_set Tom = new Pet_set("Tom",12,"cat");
        homeSet.add(Tom);

  	 while (itdel.hasNext()){
            Pet_set ppdel = itdel.next();
            if(ppdel.getAge()<13){
                delSet.add(ppdel);
            }
        }

	homeSet.removeAll(delSet);
	System.out.println("del tom");
```

警告消失了，泛型是在java1.5以后引入的，1.5以后会有警告。  

# 不使用泛型的危险

`it.next()`返回的是object对象，我们需要将object类手动转为Pet_set类，这是在我们知道自己要转换的前提下。  
如果在强制转化时手动转化成了别的类，比如这样转：  

```bash
while (it.hasNext()){
            String tomm = (String) it.next();
            if(tomm.getName().equals("Tom")){
                System.out.println("find tom");
                break;
            }
        }
```

编译器不会报错，运行报错：  

```bash
java.lang.ClassCastException: Pet_set cannot be cast to java.lang.String
```

set集合中泛型的使用，可以将显式的强制类型转换，变成自动转换（手动强制转换易出错）。  

不使用泛型---> 手动强制类型转换 --if--> 正确转换----> success  
                             ---else--> 转换错误----> 编译通过，运行报错  

卒。  
