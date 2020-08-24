# Java ArrayList
## 官方描述
- List接口的可调整大小的数组实现。实现所有可选的列表操作，并允许所有元素，包括null。除了实现List接口之外，此类还提供一些方法来操作内部用于存储列表的数组大小。（此类与Vector大致等效，但它不是同步的）
    > Resizable-array implementation of the List interface. Implements all optional list operations, and permits all elements, including null. In addition to implementing the List interface, this class provides methods to manipulate the size of the array that is used internally to store the list. (This class is roughly equivalent to Vector, except that it is unsynchronized.)

- size、isEmpty、get、set、iterator和listIterator操作在恒定时间内运行。add以固定时间运行，也就是说添加n个元素需要O(n)时间。所有其他操作均以线性时间运行（大致而言）。与LinkedList实现相比，常数因子较低。
    > The size, isEmpty, get, set, iterator, and listIterator operations run in constant time. The add operation runs in amortized constant time, that is, adding n elements requires O(n) time. All of the other operations run in linear time (roughly speaking). The constant factor is low compared to that for the LinkedList implementation.

- 每个ArrayList实例都有一个容量。容量是用于在列表中存储元素的数组的大小。它总是至少与列表大小一样大。将元素添加到ArrayList时，其容量会自动增长。除了添加元素具有固定的推销时间成本外，没有指定增长策略的详细信息。
    > Each ArrayList instance has a capacity. The capacity is the size of the array used to store the elements in the list. It is always at least as large as the list size. As elements are added to an ArrayList, its capacity grows automatically. The details of the growth policy are not specified beyond the fact that adding an element has constant amortized time cost.

- 应用程序可以通过使用sureCapacity操作在添加大量元素之前增加ArrayList实例的容量。这样可以减少增量重新分配的数量。
    > An application can increase the capacity of an ArrayList instance before adding a large number of elements using the ensureCapacity operation. This may reduce the amount of incremental reallocation.



## 字段描述
- DEFAULT_CAPACITY：默认容量大小（默认为10的空列表）
- EMPTY_ELEMENTDATA：空数组对象
- DEFAULTCAPACITY_EMPTY_ELEMENTDATA：默认构造函数使用的空数组对象
- MAX_ARRAY_SIZE：最大容量大小（Integer.MAX_VALUE - 8），数组作为一个对象，需要一定的内存存储对象头信息，对象头信息最大占用内存不可超过8字节。

## 扩容
- 无参构造函数初始化时，默认数组长度为0，第一次添加元素时，分配默认数组长度（10），之后扩容会按照1.5倍进行扩容
```
    /**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
         //>>位运算，右移动一位。 整体相当于newCapacity =oldCapacity + 0.5 * oldCapacity  
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
