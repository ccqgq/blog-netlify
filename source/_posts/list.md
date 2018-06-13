---
title: Vector、ArrayList、LinkedList 有何区别
date: 2018-06-13 21:24:15
tags: 
    - Java常见问题
    - java
    - list
    - 集合
---
Vector、ArrayList、LinkedList是比较常见的List,也就是所谓的有序集合，三者的功能比较相似，但是其中的设计有所区别，导致在性能和线程方面还是有所不一样的。
Verctor 是 Java 早期提供的<b>线程安全的动态数组</b>，现在已经用的很少了，没有线程安全的考虑不是会考虑的，另外CopyOnWriteArrayList实现了线程安全的list，一般会使用CopyOnWriteArrayList。
ArrayList 是应用更加广泛的<b>动态数组实现</b>，它本身不是线程安全的，所以性能要好很多。与Verctor类似，都具有根据需要自动的增加容量的机制，当数组已满时，会创建新的数组，并拷贝原有数组数据。但是这个操作的开销很大，大多数情况应该避免频繁的扩容。
<!-- more -->
LinkedList 顾名思义是 Java 提供的<b>双向链表</b>，所以它不需要像上面两种那样调整容量，它也不是线程安全的。
### 简单分析
1. Vector 和 ArrayList 作为动态数组，有着数组的特性，有较好的随记访问性能，但是在插入和删除元素，除了<b>尾部插入和删除元素</b>，性能会相对较差，比如我们在中间位置插入一个元素，需要移动后续所有元素。
2. LinkedList 底层作为链表，进行节点插入、删除却要高效得多，但是随机访问性能则要比动态数组慢。

ArrayList、LinkedList这些都是线程不安全的， java.util.concurrent的包中提供了线程安全的各种集合。但是在Collections中也提供了一系列的 synchronized 方法。
```Java
List list = Collections.synchronizedList(new ArrayList());
 ```
在Java 8 之中，集合增强了很多，使用stream 或者 parallelStream 的方法实现，可以很方便的实现函数式代码。可以关注更多1.8的新特性   
例如：  
```Java
1.8之前

ArrayList<String>  list = new ArrayList<>();
list.add("123");
list.add("abc");

1.8： 
List<String> list = List.of("Hello","world");
 ```
### 最后一点
这里需要说明的是，作为一个Java的开发者虽然不需要向c语言开发者自己去实现基本的数据结构，但是对基本的数据结构和排序算法还需要有较好的理解，至少需要熟悉基本的交换排序（冒泡、快排）、选择排序、插入排序等。这些东西在有些业务场景也是需要用到的。



