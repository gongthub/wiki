# PipedInputStream & PipedOutputStream
PipedInputStream与PipedOutputStream分别为管道输入流和管道输出流。管道输入流通过连接到管道输出流实现了类似管道的功能，用于线程间通信。

通常，由某个线程向管道输出流中写入数据。根据管道的特性，这些数据会自动发送到与管道输出流对应的管道输入流中。这时其他线程就可以从管道输入流中读取数据，这样就实现了线程之间的通信。

不建议对这两个流对象尝试使用单个线程，因为这样可能死锁线程。

## PipedInputStream

### 定义
```
public class PipedInputStream extends InputStream
```

### 字段
| 字段 | 说明
| --- | ---
| boolean closedByWriter = false; | 管道输出流是否关闭
| volatile boolean closedByReader = false; | 管道输入流是否关闭
| boolean connected = false; | 管道输入流是否被链接
| Thread readSide; | 从管道中读取数据的线程
| Thread writeSide; | 向管道中写入数据的线程
| private static final int DEFAULT_PIPE_SIZE = 1024; | 管道循环输入缓冲区的默认大小
| protected static final int PIPE_SIZE = DEFAULT_PIPE_SIZE; | 管道循环输入缓冲区的默认大小
| protected byte buffer[]; | 放置数据的循环缓冲区
| protected int in = -1; | 缓冲区的位置，当从链接的管道输入流接收到下一个数据字节时，会将其存储到该位置
| protected int out = 0; | 缓冲区的位置，此管道输入流将对该位置读取下一个数据字节

## PipedOutputStream
### 定义
```
public
class PipedOutputStream extends OutputStream
```
### 字段
| 字段 | 说明
| --- | ---
| private PipedInputStream sink; | 与PipedOutputStream连接的管道输入流


## 总结
- PipedInputStream与PipedOutputStream分别为管道输入流和管道输出流。管道输入流通过连接到管道输出流实现了类似管道的功能，用于线程间通信。
- 通常，由某个线程向管道输出流中写入数据，根据管道的特性，这些数据会自动发送到与管道输出流对应的管道输入流中。这时其他线程就可以从管道输入流中读取数据，这样就实现了线程之间的通信。
- 不建议对这两个流对象尝试使用单个线程，因为这样可能会死锁线程
- PipedOutputStream是数据的发送者，PipedInputStream是数据的接收者
- PipedInputStream缓冲区大小默认是1024，写入数据时写入到这个缓冲区的，读取数据也是从这个缓冲区读取
- PipedInputStream通过read方法读取数据。PipedOutputStream通过write方法写入数据，write方法其实调用PipedInputStream中的receive方法来实现将数据写入到缓冲区的，因为缓冲区是在PipedInputStream中
- PipedOutputStream和PipedInputStream之间通过connect()方法连接
- 使用后要关闭输入输出流