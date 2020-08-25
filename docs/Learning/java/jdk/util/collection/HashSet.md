# Java HashSet
## HashSet 简介
    HashSet是Set接口的典型实现，实现了Set接口中的所有方法，并没有添加额外的方法，大多数时候使用Set集合时就是使用这个实现类。HashSet按照Hash算法来存储集合中的元素。因此具有很好的存取和查找性能。

## HashSet 特点
- 不能保证元素的排列顺序，顺序可能与添加顺序不同，顺序也有可能发送变化
- HashSet不是同步的，如果多个线程同时访问一个HashSet，则必须通过代码来保证其同步
- 集合元素值可以是null  
    除此之外，HashSet判断两个元素是否相等的标准也是一大特点，HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等。

## HashSet官方描述
[链接](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)
- 此类实现Set接口，该接口由哈希表(实际上是HashMap实例)支持。它不保证集合的迭代顺序。特别是，它不能保证顺序会随着时间的推移保持恒定。此类允许使用null元素。  
    > This class implements the Set interface, backed by a hash table (actually a HashMap instance). It makes no guarantees as to the iteration order of the set; in particular, it does not guarantee that the order will remain constant over time. This class permits the null element.

- 该类为基本操作(添加、删除、包含和大小)提供恒定的时间性能，假设哈希函数将元素正确地分散在存储桶中。遍历此集合需要的时间与HashSet实例大小（元素的数量）加上后备HashMap实例的“容量”（存储桶的数量）之和成比例。因此，如果迭代性能很重要，则不要将初始化容量设置得过高（或负载因子过低），这一点非常重要。  
    > This class offers constant time performance for the basic operations (add, remove, contains and size), assuming the hash function disperses the elements properly among the buckets. Iterating over this set requires time proportional to the sum of the HashSet instance's size (the number of elements) plus the "capacity" of the backing HashMap instance (the number of buckets). Thus, it's very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.

- 请注意，此实现未同步。如果多个线程同时访问哈希集，并且至少有一个线程修改了哈希集，则必须在外部对其进行同步。通常，通过在自然封装了该集合的某个对象上进行同步来实现。如果不存在这样的对象，则应使用Collections.synchronizedSet方法将其"包装"。最好在创建时完成此操作，以防止意外地异步访问集合。
Set s = Collections.synchronizedSet(new HashSet(……));
    > Note that this implementation is not synchronized. If multiple threads access a hash set concurrently, and at least one of the threads modifies the set, it must be synchronized externally. This is typically accomplished by synchronizing on some object that naturally encapsulates the set. If no such object exists, the set should be "wrapped" using the Collections.synchronizedSet method. This is best done at creation time, to prevent accidental unsynchronized access to the set:  
     > Set s = Collections.synchronizedSet(new HashSet(...));

- 此类的迭代器方法返回的迭代器是快速失败的：如果在创建迭代器后的任何时候以任何方式修改集合（除了通过迭代器自己的remove方法之外），迭代器都会抛出ConcurrentModificationException。因此，面对并发修改，迭代器将快速而干净地失败，而不是冒着在未来不确定的时间冒任意不确定行为的风险。
    > The iterators returned by this class's iterator method are fail-fast: if the set is modified at any time after the iterator is created, in any way except through the iterator's own remove method, the Iterator throws a ConcurrentModificationException. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future.

- 请注意，迭代器的快速失败行为无法得到保证，因为通常来说，在存在不同步的并发修改的情况下，不可能做出任何严格的保证。快速失败的迭代器会尽最大努力抛出ConcurrentModificationException。因此，编写依赖于此异常的程序的正确性是错误的：迭代器的快速失败行为应仅用于检测错误。
    > Note that the fail-fast behavior of an iterator cannot be guaranteed as it is, generally speaking, impossible to make any hard guarantees in the presence of unsynchronized concurrent modification. Fail-fast iterators throw ConcurrentModificationException on a best-effort basis. Therefore, it would be wrong to write a program that depended on this exception for its correctness: the fail-fast behavior of iterators should be used only to detect bugs.

## HashSet  默认初始容量、加载因子、扩容增量等
### 初始容量
    > 默认初始容量为 16

### 加载因子
- 默认加载因子
    > 默认加载因子为 0.75

### 扩容增量
    > 

### 去重原理
    > 当HashSet add一个元素A的时候，首先获取这个元素的散列码(hashCode方法),假设散列码为400，然后将散列码对散列表元的数量取模，400%16=0；0表示第一个元素，然后将元素A与散列码列表中的第一个列表中（取模为0，所以这里是第一个链表）的每个元素进行比较（通过equals进行比较）如果该链表中没有找到与元素A相同的元素，则将元素A添加到该链表，如果找到某个元素与元素A相同，则表示Set中已经存在了该元素，不添加元素A。


