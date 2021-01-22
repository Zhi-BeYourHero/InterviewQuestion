<!-- GFM-TOC -->

[1️⃣平时有遇到什么垃圾回收的问题吗？比如说最常见的OOM？如何解决？平时遇到哪些种类的OOM？什么情况会产生OOM？](#1️⃣平时有遇到什么垃圾回收的问题吗？比如说最常见的OOM？如何解决？平时遇到哪些种类的OOM？什么情况会产生OOM？)

[2️⃣什么情况下会产生内存泄漏，如何定位，怎么确定一个对象没有被回收，存在内存泄漏的情况？](#2️⃣什么情况下会产生内存泄漏，如何定位，怎么确定一个对象没有被回收，存在内存泄漏的情况？)

<!-- GFM-TOC -->

### 1️⃣平时有遇到什么垃圾回收的问题吗？比如说最常见的OOM？如何解决？平时遇到哪些种类的OOM？什么情况会产生OOM？



**第2章 Java内存区域与内存溢出异常 ，2.4 实战：OutOfMemoryError异常** ，深入理解JVM第三版P88，四种

![image-20210122121317831](https://zhipic.oss-cn-beijing.aliyuncs.com/20210122121412.png)



![image-20210122121510757](https://zhipic.oss-cn-beijing.aliyuncs.com/20210122121552.png)

![image-20210122121546290](https://zhipic.oss-cn-beijing.aliyuncs.com/20210122121547.png)

![image-20210122121732426](https://zhipic.oss-cn-beijing.aliyuncs.com/20210122121733.png)

![image-20210122121817023](https://zhipic.oss-cn-beijing.aliyuncs.com/20210122121818.png)

### 2️⃣什么情况下会产生内存泄漏，如何定位，怎么确定一个对象没有被回收，存在内存泄漏的情况？

在Java中，内存泄漏就是存在一些被分配的对象，这些对象有下面两个特点：

1. 这些对象是可达的，即在有向图中，存在通路可以与其相连；
2. 这些对象是无用的，即程序以后不会再使用这些对象。

如果对象满足这两个条件，这些对象就可以判定为Java中的内存泄漏，这些对象不会被GC所回收，然而它却占用内存。

可说说ThreadLocal，Thread->ThreadLocalMap->Entry->TheadLocal，ThreadLocal是WeakReference;

参考2.4.1Java堆溢出

先通过jmap -histo初步确定可能产生内存泄漏的对象，如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链，找到泄漏对象是通过怎样的引用路径、与哪些GC Roots相关联，才导致垃圾收集器无法回收它们，根据泄漏对象的类型信息以及它到GC Roots引用链的信息，一般可以比较准确地定位到这些对象创建的位置，进而找出产生内存泄漏的代码的具体位置



