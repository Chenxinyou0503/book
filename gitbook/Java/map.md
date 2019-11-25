# 集合简介

###Java 集合和数组的区别
   1.  Java数组存放的数据是基本数据类型和对象，而集合存放的是对象的引用。
   2.  Java数组不支持动态扩容，而集合却很好的支持了这一特点
   3.  Java数据判断长度的length并不能判断数组存放内容的大小，只会告诉数组的长度，而集合的size()可以确切的知道有多少元素。
 
###Java集合的分类

集合可以分为两大类，一类是Collection，一类Map。 
* Collection可分为List，Set，Queue，他们都是接口，又有各自的实现类。
    * List实现类有ArrayList，LinkedList，Vector。
    * Set实现类有HashSet，LinkedHashSet ,Set的子接口SortedSet的实现类是TreeSet。
* Map提供了一组键值的映射，存储每个对象都有一个对象的key，key决定了对象的存储位置，并且key不能重复。其实现主要包括以下几个类：
    * HashMap
    * TreeMap
    * LinkedHashMap
    * Hashtable
    
* ArrayList：数组实现，查询快，增删慢，轻量级，线程不安全。

    * 时间复杂度为:
    
        get() 复杂度O(1) ,add(E) 复杂度O(1),add(index,E) 复杂度为O(n) remove() 复杂度为O(n)
* LinkedList: 双向链表实现，增删快，查询慢，线程不安全。
    * 时间复杂度为：
    get()复杂度为O(n),add(E)复杂度为O(1),add(index,E)复杂度为O(n),remove()复杂度为O(1)
* Vector：数组实现，重量级, 线程安全、使用少    
   
* HashSet: 直接实现了Set，其底层是包装了一个HashMap实现，根据HashCode算法来存取元素。具有比较好的存取和查找项目
    * 不保证元素插入的顺序
    * HashSet是线程分安全的
    * HashSet元素值可以为Null
* LinkedHashSet 是HashSet的一个子类，它不仅根据HashCode来决定元素的存储位置，还维护了一个链表来保证插入是的顺序。
* TreeSet 是依靠TreeMap来实现的，TreeSet是一种有序集合，它支持两种排序方式，一种是自然排序，一种是定制排序。自然排序需要对象实现Comparable接口，否则会报异常，定制排序需要关联一个Comparator对象，由Comparator提供排序逻辑。
* HashMap：键值对，key不能重复，但是value可以重复，key的实现就是HashSet；value对应着放；允许null的键或值。
* Hashtable：线程安全的，不允许null的键或值。
* Properties:key和value都是String类型，用来读配置文件。
* TreeMap：对key排好序的Map; key 就是TreeSet, value对应每个key; key要实现Comparable接口或TreeMap有自己的构造器。
* LinkedHashMap： 此实现与 HashMap 的不同之处在于，后者维护着一个运行于所有条目的双重链接列表。存储的数据是有序的。


###HashMap和Hashtable的区别

HashMap和Hashtable都是java的集合类，都可以用来存放java对象，这是他们的相同点

以下是他们的区别：

* 历史原因：

Hashtable是基于陈旧的Dictionary类的，HashMap是java 1.2引进的Map接口的一个现实。

* 同步性：

Hashtable是同步的，这个类中的一些方法保证了Hashtable中的对象是线程安全的，而HashMap则是异步的，因此HashMap中的对象并不是线程安全的，因为同步的要求会影响执行的效率，所以如果你不需要线程安全的结合那么使用HashMap是一个很好的选择，这样可以避免由于同步带来的不必要的性能开销，从而提高效率，我们一般所编写的程序都是异步的，但如果是服务器端的代码除外。

* 值：

HashMap可以让你将空值作为一个表的条目的key或value，
Hashtable是不能放入空值（null）的

###ArrayList和Vector的区别：

ArrayList与Vector都是java的集合类，都是用来存放java对象，这是他们的相同点，

区别：

* 同步性：

Vector是同步的，这个类的一些方法保证了Vector中的对象的线程安全的，而ArrayList则是异步的，因此ArrayList中的对象并不 是线程安全的，因为同步要求会影响执行的效率，所以你不需要线程安全的集合那么使用ArrayList是一个很好的选择，这样可以避免由于同步带来的不必 要的性能开销。

* 数据增长：

从内部实现的机制来讲，ArrayList和Vector都是使用数组（Array）来控制集合中的对象，当你向两种类型中增加元素的时候，如果元素的数目超过了内部数组目前的长度他们都需要扩展内部数组的长度，Vector缺省情况下自动增长原来一倍的数组长度，ArrayList是原来的50%，所以最后你获得的这个集合所占的空间总是比你实际需要的要大，所以如果你要在集合中保存大量的数据，那么使用Vector有一些优势，因为你可以通过设置集合的初始大小来避免不必要的资源开销。

* 总结：

1) 如果要求线程安全，使用Vector，Hashtable

2) 如果不要求线程安全，使用ArrayList，LinkedList，HashMap

3) 如果要求键值对，则使用HashMap，Hashtable

4) 如果数据量很大，又要求线程安全考虑Vector

###Arraylist和Linkedlist联系与区别
1. ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。
2. 对于随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。
3. 对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。 这一点要看实际情况的。若只对单条数据插入或删除，ArrayList的速度反而优于LinkedList。但若是批量随机的插入删除数据，LinkedList的速度大大优于ArrayList. 因为ArrayList每插入一条数据，要移动插入点及之后的所有数据。

 

###HashMap与TreeMap联系与区别
1. HashMap通过hashcode对其内容进行快速查找，而TreeMap中所有的元素都保持着某种固定的顺序，如果你需要得到一个有序的结果你就应该使用TreeMap（HashMap中元素的排列顺序是不固定的）。
2. 在Map 中插入、删除和定位元素，HashMap是最好的选择。但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。使用HashMap要求添加的键类明确定义了hashCode()和 equals()的实现。
两个map中的元素一样，但顺序不一样，导致hashCode()不一样。

同样做测试：

在HashMap中，同样的值的map,顺序不同，equals时，false;
而在treeMap中，同样的值的map,顺序不同,equals时，true，说明，treeMap在equals()时是整理了顺序了的。