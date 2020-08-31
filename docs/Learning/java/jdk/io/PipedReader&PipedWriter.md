# PipedReader & PipedWriter
- PipedWriter和PipedOutputStream几乎一模一样。区别在于PipedReader操作的是字符，PipedInputStream操作的是字节。

## PipedReader
### 定义
```
public class PipedReader extends Reader

// 管道输出流是否关闭
boolean closedByWriter = false;

// 管道输入流是否关闭
boolean closedByReader = false;

// 管道输入流是否被连接
boolean connected = false;

// 从管道中读取数据的线程
Thread readSide;

// 向管道中写入数据的线程
Thread writeSide;

// 管道循环输入缓冲区的默认大小
private static final int DEFAULT_PIPE_SIZE = 1024;

// 放置数据的循环缓冲区
char buffer[];

// 缓冲区的位置，当从连接的管道输出流中接收到下一个数据字符时，会将其存储到该位置
int in = -1;

// 缓冲区的位置，此管道输入流将从该位置读取下一个数据字符
int out = 0;

```

## PipedWriter
### 定义
```
public class PipedWriter extends Writer

// 与PipedWriter相连接的管道输入流
private PipedReader sink;

// 标识PipedWriter是否关闭
private boolean closed = false;

```