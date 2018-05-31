---
title: Java常见问题(一)-谈谈 final、finally、 finalize 有什么不同
date: 2018-05-31 20:51:25
tags:
    - Java常见问题
    - java
---
  这是一个非常经典的Java基础的题目，也是可以看出掌握Java基础知识的程度。同时也可以涉及到Java的性能、并发、对象的生命周期以及Java的垃圾回收机制。<br>
### 网上的回答
 
1、final可以用来修饰类、方法、变量。final修饰的类不能被继承，修饰的方法不能被重写，修饰的变量不可以修改。  
2、finally异常处理中一定可以执行的代码，try-finally 或者 try-catch-finally 用来关闭io流的关闭等。  
3、finalize是objiect类的一个方法，垃圾收集器将对象从内存中清除出去之前做必要的清理工作。(注：改方法在Java9中已经被标记为过时（deprecated）)  
<!-- more -->
### 简单分析
**final**  
一般来说是推荐使用fianl关键字来修饰我们的代码，这样可以明确代码的逻辑意图是很有用的。  
1、将方法或者class声明为fianl，可以很明确的告诉别人这个是不允许修改的。Java的核心类库或者优秀的开源框架中很多的都是final class，这样可以避免使用者对功能呢的修改，可以保证框架的安全。
2、final修饰变量或者参数可以产生一定程度上的不可变的性质，有效的保护了数据的只读不可修改。在并发编程中，明确了fianl修饰的变量不能二次赋值，减少的同步的开销。
3、使用fianl对性能提升有多少好处，其实对于现在的JVM(HotSpot)并没有什么实质性的提高，现在的JVM改善什么编译能力不需要依赖fianl的提示。  
**finally**  
对于finally其实知道是干什么用的就足够了。对于各种连接的关闭其实可以使用try-with-resources会更好一点。  
**finalize**  
finalize在Java9中被标记为过时的一个方法，总的来说不能去指望可以进行资源的释放问题。finalize()是无法判断什么时候执行。  
目前Java会使用 java.lang.ref.Cleaner去替换finalize的实现，Cleaner利用Java的幻象引用和引用队列去实现资源的回收，会比finalize更轻一点。但是还是需要注意的幻象引用可能导致引用的堆积问题。并不能完全依靠Cleaner进行资源回收。
#### 最后一点
这里提及一下immutable(不可变类)，它与fianl是不相同的。
这里引入一个网上的代码
```Java
 final List<String> strList = new ArrayList<>();
 strList.add("Hello");
 strList.add("world");  
 List<String> unmodifiableStrList = List.of("hello", "world");
 unmodifiableStrList.add("again");
 ```
 上面的代码中fianl修饰的list是明确这个list的引用不能重新被赋值，但是list的行为是可以正常操作的。如果需要创建一个真正本身就是不可变的，上面的List.of（Java9）就是可以创建一个不可变的list。  
 immutable这个其实还是非常有用的，但是Java自身并没有原生的支持，可以去了解搜索一下。。。。。。



