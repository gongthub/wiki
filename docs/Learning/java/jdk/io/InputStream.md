# InputStream
InputStream是抽象类，这个抽象类是所有表示字节输入流的父类。继承这个抽象类的类必须提供返回下一个输入字节的方法。

## 字段
-  MAX_SKIP_BUFFER_SIZE：默认为2048；最大跳过字节数组大小。当要跳过当前输入流的一段时，首先定义一个数组，不断地区读取数据，直到读取了传入的n个长度，或者是读到流结束为止。返回跳过的字节个数。
```
 /**
     * Skips over and discards <code>n</code> bytes of data from this input
     * stream. The <code>skip</code> method may, for a variety of reasons, end
     * up skipping over some smaller number of bytes, possibly <code>0</code>.
     * This may result from any of a number of conditions; reaching end of file
     * before <code>n</code> bytes have been skipped is only one possibility.
     * The actual number of bytes skipped is returned. If {@code n} is
     * negative, the {@code skip} method for class {@code InputStream} always
     * returns 0, and no bytes are skipped. Subclasses may handle the negative
     * value differently.
     *
     * <p> The <code>skip</code> method of this class creates a
     * byte array and then repeatedly reads into it until <code>n</code> bytes
     * have been read or the end of the stream has been reached. Subclasses are
     * encouraged to provide a more efficient implementation of this method.
     * For instance, the implementation may depend on the ability to seek.
     *
     * @param n the number of bytes to be skipped.
     * @return the actual number of bytes skipped.
     * @throws IOException if the stream does not support seek,
     *                     or if some other I/O error occurs.
     */
    public long skip(long n) throws IOException {

        long remaining = n;
        int nr;

        if (n <= 0) {
            return 0;
        }

        int size = (int) Math.min(MAX_SKIP_BUFFER_SIZE, remaining);
        byte[] skipBuffer = new byte[size];
        while (remaining > 0) {
            nr = read(skipBuffer, 0, (int) Math.min(size, remaining));
            if (nr < 0) {
                break;
            }
            remaining -= nr;
        }

        return n - remaining;
    }
```

## 方法
| 方法 | 说明
| --- | ---
| public abstract int read() throws IOException; | 从输入流中读取数据的下个字节
| public int read(byte b[]) throws IOException | 从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
| public int read(byte b[], int off, int len) throws IOException | 将输入流中最多len个数据字节读入byte数组
| public long skip(long n) throws IOException | 跳过和丢弃此输入流中数据的n个字节
| public int available() throws IOException | 返回此输入流下一个方法调用可以不受阻塞地从此输入流读取（或跳过）的估计字节数
| public void close() throws IOException | 关闭此输入流并释放与该流关联的所有系统资源
| public synchronized void mark(int readlimit) | 在此输入流中标记当前的位置，对reset方法的后续调用会在最后标记的位置重新定位此流，以便后续重新读取相同的字节
| public synchronized void reset() throws IOException | 将此流重新定位到最后一次对此输入流调用mark方法时的位置
| public boolean markSupported() | 测试此输入流是否支持mark和reset方法

### read抽象方法
read()抽象方法，读取下一个字节，返回的值为0-255之间的数字，如果读到了流的结尾，读不出数据，则返回-1。
```
    /**
     * Reads the next byte of data from the input stream. The value byte is
     * returned as an <code>int</code> in the range <code>0</code> to
     * <code>255</code>. If no byte is available because the end of the stream
     * has been reached, the value <code>-1</code> is returned. This method
     * blocks until input data is available, the end of the stream is detected,
     * or an exception is thrown.
     *
     * <p> A subclass must provide an implementation of this method.
     *
     * @return the next byte of data, or <code>-1</code> if the end of the
     * stream is reached.
     * @throws IOException if an I/O error occurs.
     */
    public abstract int read() throws IOException;
```

### read重载方法
从数组的off位置读入len长度的字节数据到b[]数组中，最后返回读取的字节数。
```
    public int read(byte b[], int off, int len) throws IOException {
        if (b == null) {
            throw new NullPointerException();
        } else if (off < 0 || len < 0 || len > b.length - off) {
            throw new IndexOutOfBoundsException();
        } else if (len == 0) {
            return 0;
        }

        int c = read();
        if (c == -1) {
            return -1;
        }
        b[off] = (byte) c;

        int i = 1;
        try {
            for (; i < len; i++) {
                c = read();
                if (c == -1) {
                    break;
                }
                b[off + i] = (byte) c;
            }
        } catch (IOException ee) {
        }
        return i;
    }
```
