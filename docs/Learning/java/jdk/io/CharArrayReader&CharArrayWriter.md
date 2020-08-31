# CharArrayReader & CharArrayWriter
- CharArrayReader实现一个可用作字符输入流的字符缓冲区。支持mark/set。
- CharArrayWriter实现一个可用作字符输出流的字符缓冲区。缓冲区会随向流中写入数据而自动增长。可使用toCharArray()和toString()获取数据。
- CharArrayReader在作所有操作前，都要确认流处于open状态。判断流处于open状态的依据是buf不为null。close方法中会将buf置为null。
- CharArrayReader支持mark()操作
- charArrayWriter中的close()方法无效

## CharArrayReader
### 定义
```
public class CharArrayReader extends Reader

// 字符缓冲区
protected char buf[];

// 缓冲区中下一个被获取的字符的索引
protected int pos;

// mark标记的索引
protected int markedPos = 0;

// 字符缓冲区大小
protected int count;

    /**
     * Creates a CharArrayReader from the specified array of chars.
     * @param buf       Input buffer (not copied)
     */
    public CharArrayReader(char buf[]) {
        this.buf = buf;
        this.pos = 0;
        this.count = buf.length;
    }

    /**
     * Creates a CharArrayReader from the specified array of chars.
     *
     * <p> The resulting reader will start reading at the given
     * <tt>offset</tt>.  The total number of <tt>char</tt> values that can be
     * read from this reader will be either <tt>length</tt> or
     * <tt>buf.length-offset</tt>, whichever is smaller.
     *
     * @throws IllegalArgumentException
     *         If <tt>offset</tt> is negative or greater than
     *         <tt>buf.length</tt>, or if <tt>length</tt> is negative, or if
     *         the sum of these two values is negative.
     *
     * @param buf       Input buffer (not copied)
     * @param offset    Offset of the first char to read
     * @param length    Number of chars to read
     */
    public CharArrayReader(char buf[], int offset, int length) {
        if ((offset < 0) || (offset > buf.length) || (length < 0) ||
            ((offset + length) < 0)) {
            throw new IllegalArgumentException();
        }
        this.buf = buf;
        this.pos = offset;
        this.count = Math.min(offset + length, buf.length);
        this.markedPos = offset;
    }

```

## CharArrayWriter
### 定义
```
public
class CharArrayWriter extends Writer

// 存储数据的字符缓冲区
protected char buf[];

// 缓冲区中的字符个数
protected int count;

    /**
     * Creates a new CharArrayWriter.
     */
    public CharArrayWriter() {
        this(32);
    }

    /**
     * Creates a new CharArrayWriter with the specified initial size.
     *
     * @param initialSize  an int specifying the initial buffer size.
     * @exception IllegalArgumentException if initialSize is negative
     */
    public CharArrayWriter(int initialSize) {
        if (initialSize < 0) {
            throw new IllegalArgumentException("Negative initial size: "
                                               + initialSize);
        }
        buf = new char[initialSize];
    }

```