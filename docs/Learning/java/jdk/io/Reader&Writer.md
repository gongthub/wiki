# Reader & Writer
- Reader是字符输入流的抽象类。子类必须实现的方法只有read(char[],inti,int)和close()。但是，多数子类将重写此处定义的一些方法，以提供更高的效率和/或其他功能。如重写read方法提供更高的效率；重写mark/set方法提供标记功能。
- Writer是字符输出流的抽象类。子类必须实现的方法仅有 write(char[],int,int)、flush()和close()。但是，多数子类将重写此处定义的一些方法吗，以提供更高的效率和/或其他功能。
- Reader与InputStream的区别
    - 操作对象的不同。字节流操作字节，字符流操作字符。
    - 实现的接口不同。Reader比InputStream多实现了一个Readable接口，用于提供一个可以将字符写入到指定缓存数组的方法。
    - close方法不同。Reader的close方法是抽象的、子类必须重写。InputStream的close方法不是抽象的。

- Writer与OutputStream区别
    - 操作的对象不同，字节流操作字节，字符流操作字符
    - 实现的接口不同。Writer比OutputStream多实现了一个Appendable接口，用于提供几个向此流中追加字符的方法。
    - close、flush方法不同。Writer的close、flush方法都是抽象的，而OutputStream则不是。

## Reader
### 定义
```
public abstract class Reader implements Readable, Closeable

// 用于同步针对此流的操作的对象
protected Object lock;

    /**
     * Creates a new character-stream reader whose critical sections will
     * synchronize on the reader itself.
     */
    protected Reader() {
        this.lock = this;
    }

    /**
     * Creates a new character-stream reader whose critical sections will
     * synchronize on the given object.
     *
     * @param lock  The Object to synchronize on.
     */
    protected Reader(Object lock) {
        if (lock == null) {
            throw new NullPointerException();
        }
        this.lock = lock;
    }
```

## Writer
### 定义
```
public abstract class Writer implements Appendable, Closeable, Flushable

// 字符缓存数组 用于临时存放要写入字符输出流中的字符
private char[] writeBuffer;

// 字符缓存数组的默认大小
private static final int WRITE_BUFFER_SIZE = 1024;

// 用于同步针对此流的操作的对象
protected Object lock;

    /**
     * Creates a new character-stream writer whose critical sections will
     * synchronize on the writer itself.
     */
    protected Writer() {
        this.lock = this;
    }

    /**
     * Creates a new character-stream writer whose critical sections will
     * synchronize on the given object.
     *
     * @param  lock
     *         Object to synchronize on
     */
    protected Writer(Object lock) {
        if (lock == null) {
            throw new NullPointerException();
        }
        this.lock = lock;
    }

```

