---
title: 学生-专业小案例
tags: Java 参数传递   
---

> 对以前做的一个小案例的总结：model类的规范写法，参数传递，注解

<!--more-->

# 关于案例
![](/assets/img/blog/2017/2017-08-07-target.png)  
同时，计算出这个专业有多少学生报名

# 知识点总结
## model类的规范写法
> private声明属性----生成getters和setters----无参构造方法----有参构造方法（用set方法赋值）

## 参数传递
将专业名称和学年与学生信息关联起来的时候，需要将两个类中的属性联系起来，就要用到参数传递  
1.直接传参数  

```bash
/**
     * 方法一：传入两个参数，输出学生所有信息
     * @param SubjectName
     * @param SubjectYear
     * @return
     */
    public String stuInfo(String SubjectName,int SubjectYear){
        String str = "学生信息如下：\n姓名："+this.getSname()+"\n学号："+this.getSno()+"\n性别："+this.getSex()
                +"\n年龄："+this.getAge()+"\n专业名称："+SubjectName+"\n专业学年："+SubjectYear;
        return str;
    }
```
2.将对象作为参数传递

```bash
/**
     * 方法二：将对象作为参数，输出学生所有信息
     * @param subject
     * @return
     */
    public String stuInfo(Subject subject){
        String str = "学生信息如下：\n姓名："+this.getSname()+"\n学号："+this.getSno()+"\n性别："+this.getSex()+"\n年龄："
                +this.getAge()+"\n专业名称："+subject.getSubname()+"\n专业学年："+subject.getSchoolyear();
        return str;
    }
```
将专业这个对象作为参数传递到学生类的方法中，通过mis.getSubname，使在学生类中访问专业类的属性。

3.将对象作为成员属性

```bash
/**
     * 方法三：将对象作为成员变量，输出学生所有信息
     * @return
     */
    public String stuInfo(){
        String str = "学生信息如下：\n姓名："+this.getSname()+"\n学号："+this.getSno()+"\n性别："+this.getSex()
                +"\n年龄："+this.getAge()+"\n专业名称："+this.getSubject().getSubname()+"\n专业学年："
                +this.getSubject().getSchoolyear();
        return str;
    }
```

方法三相对方法二，关联性更强。  
另外，将对象作为成员属性，生成getter和setter方法，在getter方法取值前要加一段处理逻辑，判断对象有没有被初始化，如果没有初始化会抛出空指针异常。  

```bash
/**
     * 方法三中获取的专业对象，获取前先判断有没有实例化，先实例化再返回
     * @return
     */
    public Subject getSubject() {
        if(this.subject == null)
            this.subject = new Subject();
        return subject;
    }
```

## 注解
> 声明在包，类，属性，方法，局部变量，方法参数前

按执行分类：源码注解（编译成.class后不存在）---编译注解（在源码和.class中都存在）---运行注解（甚至会影响运行逻辑）  
按来源分类：JDK自带（@overide）---第三方框架（spring@autoride）---自定义  


[学生专业地址](https://github.com/EasterFan/JavaExercise)  
卒。
