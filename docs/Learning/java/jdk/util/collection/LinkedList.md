# Java LinkedList
## 官方描述
- List和Deque接口的双链接列表实现。实现所有可选的列表操作，并允许所有元素(包括null)
    > Doubly-linked list implementation of the List and Deque interfaces. Implements all optional list operations, and permits all elements (including null).
- 所有操作均按双向链表的预期执行。索引到列表中的操作将从开头或结尾遍历列表，以更接近指定索引的位置为准。
    > All of the operations perform as could be expected for a doubly-linked list. Operations that index into the list will traverse the list from the beginning or the end, whichever is closer to the specified index.

### 特性
- 链表无容量限制，但双向链表本身使用了更多空间，也需要额外的链表指针操作。
- 除了实现List接口外，LinkedList还为在列表的开头及结尾get、remove和insert元素提供了统一的命名方法，这些操作可以将链接列表当作栈，队列和双端队列来使用。
- 底层实现：LinkedList的实现是基于双向链表的，且头结点中不存放数据
- 构造方法：无参构造方法直接建立一个仅包含head节点的空链表；包含Collection的构造方法，先调用无参构造方法建立一个空链表，而后将Collection中的数据加入到链表的尾部后面。

### LinkedList和ArrayList对比分析
相同点
- 接口实现：都实现了List接口，都是线性列表的实现。
- 线程安全：都是线程不安全的，都是基于fail-fast机制

不同点
- 底层：ArrayList内部是数组实现，而LinkedList内实现是双向链表结构
- 接口：ArrayList实现了RandomAccess可以支持随机元素访问，而LinkedList实现了Deque可以当作队列使用
- 性能：新增、删除元素对ArrayList需要使用拷贝原数组，而LinkedList只需移动指针，查找元素ArrayList支持随机元素访问，而LinkedList只能一个节点的去遍历。

### 源码 （LinkedList 结构）

```
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     * Invariant: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     * Invariant: (first == null && last == null) ||
     *            (last.next == null && last.item != null)
     */
    transient Node<E> last;

    /**
     * Constructs an empty list.
     */
    public LinkedList() {
    }



    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }