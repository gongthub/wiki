# Java 集合框架
### 集合框架设计满足一下几个目标
- 该框架必须是高性能的。基本集合（动态数组，链表，树，哈希表）的实现也是必须是高效的。
- 该框架允许不同类型的集合，以类似的方式工作，具有高度的互操作性。
- 对一个集合的扩展和适应必须是简单的。
## Java集合框架图
![Java集合框架图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/collection.png)
从上面的集合框架图可以看到，Java集合框架主要包含两种类型的容器，一种是集合（Collection）,存储一个元素集合，另一种是图（Map），存储健/值对映射，Collection接口又有3种子类型，List、Set和Queue，再下面是一些抽象类，最后是具体实现类，常用的有ArrayList、LinkedList、HashSet、LinkedHashSet、HashMap、LinkedHashMap等等；
集合框架是一个用来代表和操作集合的统一架构，所有的集合框架都包含如下内容：
- 接口：是代表集合的抽象数据类型，例如 Collection、List、Set、Map等。之所以定义多个接口，是为了以不同的方式操作集合对象
- 实现（类）：是集合接口的具体实现。从本质上讲，他们是可重复使用的数据结构，例如：ArrayList、LinkedList、HashSet、HashMap
- 算法：是实现集合接口的对象里的方法执行的一些有用的计算，例如：搜索和排序。这些算法被称为多态，那是因为相同的方法可以再相似的接口上有着不同的实现

## 集合接口
- Collection接口  
    > Collection是最基本的集合接口，一个Collection代表一组Object，即Collection的元素，Java不提供直接继承自Collection的类,只提供继承于的子接口(如List和Set),Collection接口存储一组不唯一，无序的对象。

- List接口
    > List接口是一个有序的Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引(元素在List种的位置，类似于数组的下标)来访问List的元素，第一个元素的索引为0，而且允许有相同的元素。  
    > List接口存储一组不唯一，有序（插入顺序）的对象。

- Set接口
    > Set具有与Collection完全一样的接口，只是在行为上不同，Set不保存重复的元素。  
    > Set接口存储一组唯一，无序的对象。

- SortedSet接口
    > 继承于Set保存有序的集合。

- Map接口
    > Map接口存储一组键值对象，提供Key（健）到Value（值）的映射。

- Map.Entry
    > 描述在一个Map中的一个元素（健/值对），是一个Map的内部类

- SortedMap 接口
    > 继承于Map，使Key保持在升序排列。

- Enumeration接口
    > 这是一个传统的接口和定义的方法，通过它可以枚举(一次获得一个)对象集合中的元素，这个传统接口已被迭代器取代。

### Set和List的区别
- Set接口实例存储的是无序的，不重复的数据。List接口实例存储的是有序的，可以重复的元素。
- Set检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变<实现类有HashSet，TreeSet>
- List和数组类似，可以动态增长，根据实际存储的数据长度自动增长List的长度。查找元素效率高，插入和删除效率低，因为会引起其它元素位置改变<实现类有 ArrayList,LinkedList,Vector>.

## 集合实现类(集合类)
- AbstractCollection
    > 实现了大部分的集合接口

- AbstractList
    > 继承于AbstractCollection并且实现了大部分List接口

- AbstractSequentialList
    > 继承于AbstractList，提供了对数据元素的链式访问而不是随机访问

- LinkedList
    > 实现了List接口，允许有null元素，主要用于创建链表数据结构，该类没有同步方法，如果多线程同时访问List，则必须自己实现访问同步，解决方法就是在创建的List的时候构造一个同步的List,例：  
    List list=new Collections.synchronizedList(new LinkedList());

- ArrayList
    > 该类也实现了List的接口，实现了可变大小的数组，随机访问和遍历元素时，提供更好的性能。该类也是非同步的。ArrayList增长当前长度的50%，插入删除效率低。

- AbstractSet
    > 继承于AbstractCollection并且实现了大部分Set接口

- HashSet
    > 该类实现了Set接口，不允许出现重复元素，不保证集合中元素顺序，允许包含值为null元素，但最多只能一个。

- LinkedHashSet
    > 具有可预知迭代顺序的Set，接口的哈希表和链接列表实现

- TreeSet
    > 该类实现了Set接口，可以实现排序等功能

- AbstractMap
    > 实现了大部分Map接口

- HashMap
    > HashMap是一个散列表，它存储的内容是键值对映射。该类实现了Map接口，根据健的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的健为null，不支持线程同步。

- TreeMap
    > 继承了AbstractMap，并且使用一棵树

- WeakHashMap
    > 继承AbstractMap类，使用弱密钥的哈希表

- LinkedHashMap
    > 继承于HashMap，使用元素的自然顺序对元素进行排序

- IdentityHashMap
    > 继承AbstractMap类，比较文档时使用引用相等。

- Vector
    > 该类和ArrayList非常相似，但是该类是同步的，可以在多线程的情况，该类允许设置默认的增长长度，默认扩容方式为原来的2倍。

- Stack
    > 栈是Vector的一个子类，它实现了一个标准的后进先出的栈。

- Dictionary
    > Dictionary类是一个抽象类，用来存储健/值对，作用和Map类相似

- Hashtable
    > Hashtable是Dictionary的子类

- Properties
    > Properties继承于Hashtable，表示一个持久的属性集，属性列表中每个健及其对应值都是一个字符串

- BitSet
    > 一个BitSet类创建以中国特殊类型的数组来保持位值，BitSet中数组大小会随需要怎加。