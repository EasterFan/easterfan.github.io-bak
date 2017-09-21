---
title: Android线性布局
tags: Android 
---
> 然而开发中最常用的是相对布局


# 线性布局常用属性

## gravity
```xml
android:gravity:
# 用于设置该控件中内容相对于该控件的对齐方式
android:layout_gravity:
# 用于设置该控件相对于父控件的对齐方式
```

## weight
用于在**线性布局**中指定**父控件剩余空间**的分配比例  
### 1.怎样分配长度相等的子控件
weight是对父控件剩余控件的分配，那就意味着  
> 子控件总长度 = 子控件自身长度 + 父控件分配长度  

所以，当weight设为1：1时，两个控件长度并不相等。weight属性怎样分配出长度相等的两个子控件呢？  

```bash
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#3dff00ff">


	<!--水平线性布局：将layout_width设为0dp-->
	<!--垂直线性布局：将layout_height设为0dp-->
    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="人间四月"
        android:background="#0000ff"/>
    <TextView
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="2"
        android:text="始盛开"
        android:background="#ffff00"/>
</LinearLayout>

```

### 2.实际与weight设置值相反的原因
在layout_width="match_content"的前提下，weight设为1：2，实际长度变成了2：1，为什么？  
设match_content = a；  
> 子控件实际长度 = 子控件长度 + 父控件剩余长度X比例  

X1 = a + (a-2a)*(1/3)  
X2 = a + (a-2a)*(2/3)  
关键在于，父控件的剩余空间变成了负数。  

卒。

