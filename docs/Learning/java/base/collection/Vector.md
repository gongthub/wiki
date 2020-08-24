# Java Vector
## 官方描述
- Vector类实现可增长的对象数组，像数组一样，它包含可以使用整数索引访问的组件。但是，Vector的大小可以根据需要增大或缩小，以适应创建Vector之后添加和删除项目的需要。
    > The Vector class implements a growable array of objects. Like an array, it contains components that can be accessed using an integer index. However, the size of a Vector can grow or shrink as needed to accommodate adding and removing items after the Vector has been created.

- 每个vector都尝试通过维持一个容量和一个CapacityIncrement来优化存储管理。容量始终至少与vector大小一样大；它通常较大，因为将分量添加到vector中时，vector的存储量会按照容量增加的大小成块增加。应用程序可以在插入大量组件之前增加vector的容量。这减少了增量重新分配的数量。
    > Each vector tries to optimize storage management by maintaining a capacity and a capacityIncrement. The capacity is always at least as large as the vector size; it is usually larger because as components are added to the vector, the vector's storage increases in chunks the size of capacityIncrement. An application can increase the capacity of a vector before inserting a large number of components; this reduces the amount of incremental reallocation.

- 此类的迭代器和listIterator方法返回的迭代器时快速失败的：如果在创建迭代器后的任何时间以任何方式对vector进行结构修改，除非通过迭代器自己的remove和add方法，否则迭代器将抛出ConcurrentModificationException。因此，面对并发修改，迭代器会快速干净地失败，而不会在未来的不确定时间内置任意，不确定的行为的风险。elements方法返回的枚举不是快速失败的。
    > The iterators returned by this class's iterator and listIterator methods are fail-fast: if the vector is structurally modified at any time after the iterator is created, in any way except through the iterator's own remove or add methods, the iterator will throw a ConcurrentModificationException. Thus, in the face of concurrent modification, the iterator fails quickly and cleanly, rather than risking arbitrary, non-deterministic behavior at an undetermined time in the future. The Enumerations returned by the elements method are not fail-fast.

- 从Java 2平台v1.2开始，对该类进行了改进以实现List接口，使其成为Java Collections FrameWork的成员。与新的集合实现不同，Vector是同步的。如果不需要线程安全的实现，建议使用ArrayList代替Vector。
    > As of the Java 2 platform v1.2, this class was retrofitted to implement the List interface, making it a member of the Java Collections Framework. Unlike the new collection implementations, Vector is synchronized. If a thread-safe implementation is not needed, it is recommended to use ArrayList in place of Vector.

## Vector三个成员变量
- elementData：“Object[]类型的数组”，它保存了Vector中的元素，按照Vector的设计elementData为一个动态数组，可以随着元素的增加而动态的增长。如果在初始化Vector时没有指定容器大小，则使用默认大小10；
- elementCount：Vector对象中有效的组件数
- capacityIncrement：Vector的大小小于其容量时，容量自动增加的量。如果在创建Vector时，指定了capacityIncrement的大小；则，每次当Vector中动态数组容量增加时，增加的大小都是capacityIncrement。如果容量的增量小于等于零，则每次需要增大容量时，Vector的容量将增大一倍。

## 与ArrayList的差异
- 构造函数：ArrayList比Vector稍有深度，Vector默认数组长度是10，创建时设置。
- 扩容方法grow()：ArrayList通过位运算进行扩容，而Vector则通过增长系数（创建时设置，如果为空，则增长一倍）
- Vector方法是线程安全的
- 成员变量有所不同

## 扩容
```
 /**
     * The maximum size of array to allocate.
     * Some VMs reserve some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds VM limit
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }