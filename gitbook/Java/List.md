# List

在Collection集合中，List集合是有序的，我们可以对其中的每个元素的位置进行精确的控制。可以通过索引来访问元素，遍历元素。

在List我们常用的是ArrayList和LinkedList。

ArrayList底层是通过数组实现，随着元素的增加而动态的扩容。默认是10，每次增加是按照1.5倍增加。

LinkedList底层是通过链表来实现的，随着元素的增加不断向链表的后端增加节点。

ArrayList是Java集合使用最多的一个类，是一个数组队列，属于线程不安全的集合。

具有以下特点：
   * 有序集合
   * 容量不固定
   * 插入元素可以为null
   * 线程不安全
   
##ArrayList 源码分析

#### 构造方法
ArrayList的构造方法有三个：
```
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
    
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
        //新建一个大小为initialCapacity的数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
        //如果默认大小为零，则新建一个大小为0的数组
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
        
    public ArrayList(Collection<? extends E> c) {
    // 根据指定Collection转换为ArrayList
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

一般情况下，我们用的最多的就是第一个和第二个，这里不便多说
#### 插入 
我们看一下插入，插入主要分两种，一种是插入单个的值，另一种是插入另一个数组。
```
    public boolean add(E e) {
    // 主要判断是否需要扩容 ArrayList的核心方法
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        //在指定位置存放该元素
        elementData[size++] = e;
        return true;
    }
    
    public void add(int index, E element) {
        // 判断指定位置是否合法
        rangeCheckForAdd(index);
        //对数组进行扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        // 对数组进行拷贝
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        //指定位置赋值
        elementData[index] = element;
        size++;
    }
    
     public boolean addAll(Collection<? extends E> c) {
        Object[] a = c.toArray();
        int numNew = a.length;
        //对数组进行扩容
        ensureCapacityInternal(size + numNew);  // Increments modCount
        // 对数组进行拷贝
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        return numNew != 0;
    }
    
    public boolean addAll(int index, Collection<? extends E> c) {
        rangeCheckForAdd(index);

        Object[] a = c.toArray();
        int numNew = a.length;
         //对数组进行扩容
        ensureCapacityInternal(size + numNew);  // Increments modCount

        int numMoved = size - index;
        if (numMoved > 0)
        //移动数组
            System.arraycopy(elementData, index, elementData, index + numNew,
                             numMoved);
        // 把数组c添加到elementData数组内
        System.arraycopy(a, 0, elementData, index, numNew);
        size += numNew;
        return numNew != 0;
    }
```
我们来看一下扩容源码

```
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        // 扩容后的数组大小为原来的1.5倍。
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        //如果扩容后的数组大小小于minCapacity，则minCapacity为数组的大小。
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        //如果newCapacity大于MAX_ARRAY_SIZE 则等于hugeCapacity();
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        //进行扩容，把原来的数组全部复制到新的数组里面
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
    
    // 比较minCapacity和MAX_ARRAY_SIZE的两个值大小，MAX_ARRAY_SIZE等于Integer.MAX_VALUE - 8
    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```
#### 删除
删除有三个方法：
```
    //删除指定的值
     public boolean remove(Object o) {
     // 如果o等于null
        if (o == null) {
            for (int index = 0; index < size; index++)
            //则删除指定的元素
                    fastRemove(index);
                    return true;
                }
        } else {
        // 首先查找到元素，后再删除指定的元素
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }
    // 删除指定位置的元素并返回该元素
    public E remove(int index) {
         rangeCheck(index);
        
         modCount++;
         E oldValue = elementData(index);
        
         int numMoved = size - index - 1;
         if (numMoved > 0)
             System.arraycopy(elementData, index+1, elementData, index,
                              numMoved);
         elementData[--size] = null; // clear to let GC do its work
        
         return oldValue;
    }
    //删除指定元素
    public boolean removeAll(Collection<?> c) {
            Objects.requireNonNull(c);
            return batchRemove(c, false);
        }
    
     public void clear() {
        modCount++;
        // 遍历所有的元素，并设置为null
        // clear to let GC do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }
```

#### 查询
根据指定元素获取
```
    public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }
    
```

####总结

ArrayList总体来说比较简单，所以本篇接受的不太详细，不过ArrayList还有以下一些特点：
* ArrayList自己实现了序列化和反序列化的方法，因为它自己实现了private void writeObject(java.io.ObjectOutputStream s)、private void readObject(java.io.ObjectInputStream s) 方法
* ArrayList基于数组方式实现，无容量的限制（会扩容）
* 添加元素时可能要扩容（所以最好预判一下），删除元素时不会减少容量（若希望减少容量，trimToSize()），删除元素时，将删除掉的位置元素置为null，下次gc就会回收这些元素所占的内存空间。
* 线程不安全，会出现fall-fail。
* add(int index, E element)：添加元素到数组中指定位置的时候，需要将该位置及其后边所有的元素都整块向后复制一位
* get(int index)：获取指定位置上的元素时，可以通过索引直接获取（O(1)）
* remove(Object o)需要遍历数组
* remove(int index)不需要遍历数组，只需判断index是否符合条件即可，效率比remove(Object o)高
* contains(E)需要遍历数组
* 使用iterator遍历可能会引发多线程异常