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
#### 3、说一下JVM中哪些是共享区域
> 首先jvm分为三个主要部分：运行时数据区域（内存区域）、类装载系统、字节码执行引擎。
> 运行时数据区域分为：方法区、栈、堆、程序计数器、本地方法栈；其中栈又叫做线程栈，每一个线程开始运行，JVM虚拟机就会给此线程分配一个线程栈，程序计数器同理栈。本地方法栈同理栈，只是运行方法为java虚拟机缺省的方法。由此可知栈、程序计数器、本地方法栈为非共享区域，
> 方法区（元空间）存储的是类信息和常量、静态变量、类信息，这些数据每个线程都可以去调用是共享的。堆存放的是对象同理元空间，也是共享的每个线程都可以去访问。
#### 4、说一下gc root
> java是有垃圾回收机制的，jvm会主动进行垃圾回收，区分哪些为"垃圾"，那些为"非垃圾"就是通过对象是否被引用判断，gc root作为是否被引用进行查找的根，gc root特征是只会引用，而不会被引用。举例有：栈中的本地变量、方法区中的静态变量、本地方法栈中的变量，正在运行的线程等可以叫gc root。
#### 5、说一下Minor GC、full gc 和 Major gc的区别
> Minor gc 从年轻代空间（包括 Eden 和 Survivor 区域）回收内存被称为 Minor gc。  
> Major gc 是清理老年代。会stop the world停止应用线程或者叫用户线程。
> full gc是对青年代、老年代、元空间的全局垃圾回收，触发full gc的时候会stop the world停止应用线程或者叫用户线程。
> Minor gc使用复制算法不会STW，Major gc和full gc使用标记整理算法会STW。
#### 6、正在运行的项目如何排查jvm问题
> 可以使用jmap命令查看jvm各个区域的使用情况。     
> 可以使用jstack命令查看jvm各个线程的情况。 
> 可以使用jstat命令查看垃圾回收情况。 
> 一般使用的jvm监控程序也是调用这些命令。  
> 一般jvm问题就是减少full gc。 
> 系统发生OOM的时候会生成dump文件，通过工具分析dump文件查看异常的对象、线程定位到具体代码。  
> jvm 调优需要分析，定位问题，不同场景使用不同的解决方法，目的是解决cpu占用高、频繁full gc、内存溢出等。
#### 7、如何查看线程死锁
> 通过工具或者直接用jstack命令
> mysql线程死锁可用通过sql或查看表
#### 8、线程之间是如何进行通讯的
> 一个进程内两个线程进行通讯和两个进程（两台机器）的线程如和进行通讯。  
> 第一种可以通过共享内存的方式进行通讯，第二种通过网络的方式进行通讯。  
#### 9、介绍一下Spring
> Spring 是一个快速开发框架，Spring可以管理对象。
> Spring 设计模式应用、并发安全的实现、面向接口的设计都是很优秀的。
> （1）启动Spring的时候就是创建一个Spring容器，就是创建一个SpringApplicationContext的对象。  
> （2）首先Spring会扫面，得到所有的BeanDefinition对象，存在一个map里。
> （3）筛选非懒加载的单例BeanDefinition对象进行创建bean，对于多例的bean不需要在启动的过程中去创建。对于多例的bean会在每次获取的时候通过BeanDefinition去创建。
> （4）利用BeanDefinition创建bean就是bean的创建生命周期，创建bean期间包括合并BeanDefinition、推断构造方法、实例化、属性填充、初始化前、初始化后等步骤。其中AOP就是初始化后这一步骤中。
> （5）单例Bean创建完成后，Spring会发布一个容器启动事件。  
> （6）String启动结束。  
> （7）在源码中会更加复杂，比如1源码中会提供一些模板方法，让子类来实现。比如源码中还涉及到了一些BeanFactoryPostProcessor和BeanPostProcessor的注册，Spring的扫描就是通过BeanFactoryPostProcessor来实现的，依赖注入就是通过BeanPostProcessor来实现的。  
> （8）在Spring启动中还会处理@import等注解。
#### 10、说一下Spring的事务机制
> (1)Spring事务底层是基于数据库事务和APO机制的。  
> (2)