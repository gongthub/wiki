# ByteArrayOutputStream
ByteArrayOutputStream属于字节输出流，该输出流中的数据被写入到一个byte数组里。byte数组会随着被写入其中的数据的增长而增长。

## 字段
| 字段 | 说明
| --- | ---
| protected byte buf[]; | 存储数据的缓冲区
| protected int count; | 缓冲区中的有效字节数
| MAX_ARRAY_SIZE | 分配给数组的最大值 默认（Integer.MAX_VALUE - 8）

## 构造方法
| 方法 | 说明
| --- | ---
| public ByteArrayOutputStream() | 创建新的ByteArrayOutputStream，缓冲区容量初始化为32
| public ByteArrayOutputStream(int size) | 创建新的ByteArrayOutputStream，缓冲区容量初始化为size

## 扩容

write() 操作方法先调用ensureCapacity()方法判断当前长度已经超过缓冲区长度时，调用 grow()方法，默认扩容到原有的2倍（int newCapacity = oldCapacity << 1;）

```
   public synchronized void write(int b) {
        ensureCapacity(count + 1);
        buf[count] = (byte) b;
        count += 1;
    }
    private void ensureCapacity(int minCapacity) {
        // overflow-conscious code
        if (minCapacity - buf.length > 0)
            grow(minCapacity);
    }

    /**
     * The maximum size of array to allocate.
     * Some VMs reserve some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds VM limit
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = buf.length;
        int newCapacity = oldCapacity << 1;
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        buf = Arrays.copyOf(buf, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
                Integer.MAX_VALUE :
                MAX_ARRAY_SIZE;
    }
```