---
title: set集合中的泛型
tags: Java 集合 
---

> set集合中泛型的使用，可以将显式的强制类型转换，变成自动转换（手动强制转换易出错）。引入泛型，增强程序稳健性。
<!--more-->

今天在使用set集合添加集合元素时，编译器总报警告：  
![](/assets/img/blog/java/2017-11-18-warning.png)  

# 不使用泛型
```bash
	Set homeSet = new HashSet();
	Iterator it = homeSet.iterator();

	Pet_set Tom = new Pet_set("Tom",12,"cat");
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
set集和迭代器同时引入泛型，区别是：遍历迭代器时，不需要强制类型转换  

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

卒。  
