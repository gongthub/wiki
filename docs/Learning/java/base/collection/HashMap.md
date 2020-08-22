# Java HashMap
## 官方描述
- 基于哈希表的Map接口的实现。此实现提供所有可选的映射操作，并允许空值和空键。（HashMap类与Hashtabl大致等效，不同之处在于它是不同步的，并且允许为null）此类不保证映射的顺序；特别是，它不能保证顺序会随着时间的推移保持恒定。
    > Hash table based implementation of the Map interface. This implementation provides all of the optional map operations, and permits null values and the null key. (The HashMap class is roughly equivalent to Hashtable, except that it is unsynchronized and permits nulls.) This class makes no guarantees as to the order of the map; in particular, it does not guarantee that the order will remain constant over time.

- 假设哈希函数将元素正确分散在存储桶中，则此实现为基本操作（获取和放置）提供恒定时间的性能。集合视图上的迭代所需的时间与HashMap实例“容量”（存储桶数）及其大小（键-值映射数）成正比。因此，如果迭代性能很重要，则不要将初始容量设置得过高（或负载因子过低），这一点非常重要。
    > This implementation provides constant-time performance for the basic operations (get and put), assuming the hash function disperses the elements properly among the buckets. Iteration over collection views requires time proportional to the "capacity" of the HashMap instance (the number of buckets) plus its size (the number of key-value mappings). Thus, it's very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.

- HashMap的实例具有两个影响其性能的参数，初始容量和负载因子。容量是哈希表中存储桶的数量，初始容量只是创建哈希表时的容量。负载因子是散列表的容量自动增加之前允许其填充的完整程度的度量。当哈希表中的条目数超过负载因子和当前容量的乘积时，哈希表将被重新哈希（即，内部数据结构将被重建）因此哈希表的存储桶数约为两倍。
    > An instance of HashMap has two parameters that affect its performance: initial capacity and load factor. The capacity is the number of buckets in the hash table, and the initial capacity is simply the capacity at the time the hash table is created. The load factor is a measure of how full the hash table is allowed to get before its capacity is automatically increased. When the number of entries in the hash table exceeds the product of the load factor and the current capacity, the hash table is rehashed (that is, internal data structures are rebuilt) so that the hash table has approximately twice the number of buckets.

- 通常，默认负载因子（0.75）在时间和空间成本之间提供了很好的折衷。较高的值会减少空间开销，但会增加查找成本（在HashMap类的大多数操作中都得到体现，包括get和put）。设置其初始容量时，应考虑映射中预期的条目数及其负载因子，以最大程度地减少重新哈希操作的次数。如果初始容量大于最大条目数除以负载因子，则将不会进行任务哈希操作。
    > As a general rule, the default load factor (.75) offers a good tradeoff between time and space costs. Higher values decrease the space overhead but increase the lookup cost (reflected in most of the operations of the HashMap class, including get and put). The expected number of entries in the map and its load factor should be taken into account when setting its initial capacity, so as to minimize the number of rehash operations. If the initial capacity is greater than the maximum number of entries divided by the load factor, no rehash operations will ever occur.

- 如果将许多映射存储在HashMap实例中，则创建具有足够大容量的映射将比让其根据需要增长表的自动重新哈希处理更有效地存储映射。请注意，使用许多具有相同hashCode()的键是降低任何哈希表性能的肯定方法。为了改善影响，当键为Comparable时，此类可以使用键之间的比较顺序来帮助打破关系。
    > If many mappings are to be stored in a HashMap instance, creating it with a sufficiently large capacity will allow the mappings to be stored more efficiently than letting it perform automatic rehashing as needed to grow the table. Note that using many keys with the same hashCode() is a sure way to slow down performance of any hash table. To ameliorate impact, when keys are Comparable, this class may use comparison order among keys to help break ties.

### 字段描述
- DEFAULT_INITIAL_CAPACITY：默认初始化大小（16），必须是2的幂次方
- MAXIMUM_CAPACITY：最大桶大小（1 << 30 (1073741824)）
- DEFAULT_LOAD_FACTOR：默认负载因子（0.75f）
- TREEIFY_THRESHOLD：当链表长度大于TREEIFY_THRESHOLD（默认为8），将链表转换为红黑树（jdk 1.8之后）
- UNTREEIFY_THRESHOLD：当链表长度小于UNTREEIFY_THRESHOLD（默认为6），又转为链表以达到性能均衡
- MIN_TREEIFY_CAPACITY：最小树形化容量阈值（默认为64），即 当哈希表中的容量 > 该值时，才允许树形化链表（即 将链表转换成红黑树）

### 1.7和1.8的HashMap的不同点
- JDK1.7用的是头插法，而JDK1.8及之后使用的都是尾插法，因为JDK1.7是用单链表进行的纵向延申，当采用头插法就是能够提高插入效率，但是也会容易出现逆序且环形链表死循环问题。但是在JDK1.8之后是因为加入了红黑树使用尾插法，能够避免出现逆序且链表死循环的问题。
- 扩容后数据存储位置的计算方式也不一样
    1. 在JDK1.7的时候是直接用hash值和需要扩容的二进制数进行&（这里就是为什么扩容的时候一定是2的多少次幂的原因所在，因为如果只有2的n次幂的情况时最后一位二进制数才是1，这样能最大程度减少hash碰撞）（hash值 & length -1）.
    2. 在JDK1.8的时候直接用了JDK1.7的时候计算的规律，也就是扩容前的原始位置+扩容的大小值=JDK1.8的计算方式，而不再是JDK1.7的异或的方法，但是这种方式相当于只需要判断Hash值的新增参与运算的位是0还是1就直接迅速计算出了扩容后的存储方式。
    3. JDK1.7的时候使用的是数组+单链表的数据结构。但是在JDK1.8及之后，使用的是数组+链表+红黑树的数据结构（当链表的深度达到8时，也就是默认阈值，会自动扩容把链表转成红黑树的数据结构来把时间复杂度从O(N) 变成 O（longN）提高了效率）


## 问题
- 为什么要保证HashMap底层数组是2^n ？
    > 原因：减少碰撞次数  
    ```
    eg：假设底层数据长度为15，hashcode 有0,1,2,3,4,5
     则对应i计算为 ： h & (length-1)   ===> h & 14
         length-1      hashcode         index
     即：1110      &    0000     ===>   0000
         1110      &    0001     ===>   0000  (碰撞 1 次)
         1110      &    0010     ===>   0010  
         1110      &    0011     ===>   0010  (碰撞 2 次)
         ...           ....              ....
    由上述例子，可以发现，不管怎样 对应hashcode 最后一位都为0的都都访问不到 也就是说0001,0011,0101,0111,1001,1101,1111,1011 => 1,3,5,7,9,11,13,15为下标的元素都访问不到，浪费存储空间。

    而如果底层数据长度为2^n (eg :16)那么与之对应的index求取公式为 ：
            h & (16-1) = index 即  ： h & 1111 = index
    则对应的数据都能访问得到，减少了不同hash值的碰撞概率，并且能够使数据均匀分布，提高查询效率。
    ```