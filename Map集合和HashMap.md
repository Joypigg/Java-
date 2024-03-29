---
typora-root-url: ./
---

# Map集合

map集合中常用的类：

![Map集合](/Map集合.png)

##### HashMap：

​       它根据<u>键的hashCode值存储数据</u>，大多数情况下可以直接定位到它的值，因而<u>具有很快的访问速度</u>，但遍历顺序却是不确定的。

​       HashMap最多<u>只允许一条记录的键为null</u>，允许多条记录的值为null。HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap

##### Hashtable:

​      Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换

##### LinkedHashMap:

​     <u>LinkedHashMap是HashMap的一个子类</u>，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序

##### TreeMap:

​      TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap。在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常



对于上述四种Map类型的类，要求映射中的key是不可变对象。不可变对象是该对象在创建后它的哈希值不会被改变。如果对象的哈希值发生变化，Map对象很可能就定位不到映射的位置了



# HashMap

> static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16    //初始容量因子
>
> static final int MAXIMUM_CAPACITY = 1 << 30;
>
> static final float DEFAULT_LOAD_FACTOR = 0.75f;

