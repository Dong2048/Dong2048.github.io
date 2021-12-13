---
layout: hexo
title: java题库
date: 2021-12-01 16:50:24
tags: java,面试题
cover: https://s3.bmp.ovh/imgs/2021/12/0f977534f0668261.jpg
category: 笔记
top_img: https://s3.bmp.ovh/imgs/2021/12/0f977534f0668261.jpg
---
#### 1、HashMap的put方法
HashMap是数组、链表、红黑树实现的。红黑树又称自平衡二叉查找树。
> HashMap的put的具体流程如下：  
> （1）HashMap通过put方法传进来的key通过哈希算法得到HashCode，HashCode通过与**与操作**去和数组长度-1运算先得出数组下标。
> （2）如果数组下标为止元素为空则将key和value封装为Node（Entry）对象放入该位置。 
> （3）如果数组下标元素不为空，先判断当前位置Node的类型，是红黑树还是链表Node。  
>   如果是是红黑树则将key和value封装成红黑树节点，添加到红黑树中去。 在添加到红黑树的过程中判断红黑树是否存在当前的key，如果存在则更新value。
>   如果Node对象是链表节点的话则将key和value封装成一个链表Node通过尾插法添加到链表最后的位置。（JDK1.7是头插法）尾插法会遍历链表，如果存在当前的key则更新value。
> 插入链表后当前链表元素大于等于8个，会将链表转成红黑树。  
> jdk1.7是插入前判断是否需要扩容，jdk1.8是红黑树Node或者链表Node插入后判断是否需要扩容，需要就扩容，不需要就结束当前的put方法。
#### 2、说一下ThreadLocal
> ThreadLocal是java中线程本地存储机制，可以利用该机制将某个数据、某个对象，某个值缓存在某个线程的内部，
该线程可以在任意时刻、任意方法中获取缓存的数据。
> ThreadLocal是通过ThreadLocalMap实现的，每个Thread对象中都存在一个ThreadLocalMap，Map的Key是Thread对象，
> Map的Value为要缓存的值。  
> 使用场景：  
> 两个线程如果给同一个对象赋不同的值，获取对象值可能为线程1赋得值，也可能为线程2赋得值。会有线程不安全的问题。  
> 使用ThreadLocal会将对象值缓存到该线程中，每个线程拿到的值固定为该线程赋予的。最简单的使用场景，往里存用户信息，用户对象User是全局的，每个线程拿到的用户信息都不一样，就往里存，然后可以根据线程拿到不同用户的信息
