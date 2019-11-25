# 线程安全集合
在Java当中，我们知道HashMap和ArrayList不是线程安全的，那么有哪些是线程的集合那？今天我们就简单介绍一下

Vector出现在jdk的早期版本，和ArrayList一样都是长度可变的数组，不同的是Vector是线程安全的，它集合给所有的public方法都加上了synchronized，在某一时刻只有一个线程能访问，实现同步的代价很好，性能低下，所以现在已经弃用。

HashTable和HashMap类似，不同的是除了HashTable不允许key和value为null外，还有HashTable是线程安全的，它和Vector一样，几乎给所有的public方法都加上了synchronized，同样效率低下，不建议使用。

#Collections
那有没有一种方法可以是List和Map线程安全那？
Jdk提供了一个Collections工具类，这个工具类提供了几个方法可以使其线程安全，代码如下：
```
List<E> list = Collections.synchronizedList(new ArrayList<E>());

Set set = Collections.synchronizedSet(new HashSet<>());

Map map = Collections.synchronizedMap(new HashMap<>());

...
```

Collections 工具类对每种集合都声明了一个线程安全的包装类，集合中的每个方法都通过锁对象来实现同步，但这种方式效率不高，不推荐使用。

那有没有其他效率高的方式那？
#java.util.concurrent
该包下提供了创建并发应用程序的工具类。像Executor，CountDownLatch，Lock，ConcurrentMap等，本篇主要介绍集合，其他类不展开说明。

ConcurrentHashMap和HashTable都是线程安全的集合，但是他们主要不同是锁的粒度，HashTable锁住的是整个对象，它对每一个方法都加上了锁，所以效率低。
而ConcurrentHashMap的锁粒度更细，在jdk1.8之前是通过锁Segment,每个Segment含有整个table的一部分，这样不同分段之间的并发互不影响。而1.8对此做了更改，它取消了Segment，直接在table上加锁，实现对每一行加锁，进一步减少了并发的冲突。

CopyOnWriteArrayList和CopyOnWriteArraySet它们是加了写锁的ArrayList和ArraySet，锁住的是整个对象，但读操作可以并发执行
除此之外还有ConcurrentLinkedDeque，ConcurrentSkipListMap，ConcurrentSkipListSet，ConcurrentLinkedQueue等。

ps: 后续计划分析ConcurrentHashMap源码。