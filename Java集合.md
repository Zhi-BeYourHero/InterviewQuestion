<!-- GFM-TOC -->

- [1. CopyOnWriteArrayList 和 ArrayList 区别](#1. CopyOnWriteArrayList 和 ArrayList 区别)
- [2.CopyOnWriteArrayList 怎么保证线程安全](#2.CopyOnWriteArrayList 怎么保证线程安全)
- [3.CopyOnWriteArrayList 的 add 方法拷贝原数组的原因](#3.CopyOnWriteArrayList 的 add 方法拷贝原数组的原因)
- [4.CopyOnWriteArrayList 拷贝性能问题 ](#4.CopyOnWriteArrayList 拷贝性能问题 )
- [5.CopyOnWriteArrayList 可以迭代更改结构的原因](#5.CopyOnWriteArrayList 可以迭代更改结构的原因)
- [6.HashSet的底层实现？HashMap的线程安全？让你实现线程安全的HashSet怎么实现？](#6.HashSet的底层实现？HashMap的线程安全？让你实现线程安全的HashSet怎么实现？)

<!-- GFM-TOC -->

### 1. CopyOnWriteArrayList 和 ArrayList 区别

CopyOnWriteArrayList 和 ArrayList 的底层数据结构相同，都是数组，对外提供的 API 接口也都差不 多。主要的区别就是，ArrayList 不是线程安全的，而 CopyOnWriteArrayList 是线程安全的，在多线 程环境下，无须加锁，直接使用。

### 2.CopyOnWriteArrayList 怎么保证线程安全

1. 数组容器被 volatile 关键字修饰，保证了数组内存地址被任意线程修改后，都会通知到其他线程。
2. 对数组的所有修改操作，都进行了加锁，保证了同一时刻，只能有一个线程对数组进行修改，比如 我在 add 时，就无法 remove。 
3. 修改过程中对原数组进行了复制，是在新数组上进行修改的，修改过程中，不会对原数组产生任何影响

### 3.CopyOnWriteArrayList 的 add 方法拷贝原数组的原因

对数组进行加锁后，能够保证同一时刻，只有一个线程能对数组进行 add，在单核 CPU 下的多线程环境下肯定没有问题，但我们现在的机器都是多核 CPU，如果我们不通过复制拷贝新建数组，修改原数组容器的内存地址的话，是无法触发 volatile 可见性效果的，那么其他 CPU 下的线程就无法感知数组原来已经被修改了，就会引发多核 CPU 下的线程安全问题。
假设我们不复制拷贝，而是在原来数组上直接修改值，数组的内存地址就不会变，而数组被 volatile 修饰时，必须当数组的内存地址变更时，才能及时的通知到其他线程，内存地址不变，仅仅是数组元素值 发生变化时，是无法把数组元素值发生变动的事实，通知到其它线程的。 

### 4.CopyOnWriteArrayList 拷贝性能问题 

在批量操作时，尽量使用 addAll、removeAll 方法，而不要在循环里面使用 add、remove 方法，主要 是因为 for 循环里面使用 add 、remove 的方式，在每次操作时，都会进行一次数组的拷贝(甚至多次)， 非常耗性能，而 addAll、removeAll 方法底层做了优化，整个操作只会进行一次数组拷贝。 

### 5.CopyOnWriteArrayList 可以迭代更改结构的原因

CopyOnWriteArrayList 每次操作时，都会产生新的数组，而迭代时，持有的仍然是原数组的引用，所以 我们说的数组结构变动，是用新数组替换了原数组，原数组的结构并没有发生变化，所以不会抛出异常 了。 

### 6.HashSet的底层实现？HashMap的线程安全？让你实现线程安全的HashSet怎么实现？

通过HashMap的key实现，HashMap不安全，可通过ConcurrentHashMap来实现HashSet的线程安全，组合模式

