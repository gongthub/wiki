# ByteArrayInputStream
ByteArrayInputStream属于字节型输入流，包含一个内部缓冲区，该缓冲区包含从流中读取的字节。

## 字段
| 字段 | 说明
| --- | ---
| protected byte buf[]; | 用来创建流的字节数组
| protected int pos; | 从输入流缓冲器中读取的下个字节的索引
| protected int mark = 0; | 流中的当前标记位置
| protected int count; | 字节流的长度

## 构造方法
| 方法 | 说明
| --- | ---
| public ByteArrayInputStream(byte buf[]) | 创建一个ByteArrayInputStream，使用buf作为其缓冲区数组
| public ByteArrayInputStream(byte buf[], int offset, int length) | 创建一个ByteArrayInputStream,

## 方法
| 方法 | 说明
| --- | ---
| public synchronized int read() | 从此输入流中读取下一个数据字节
| public synchronized int read(byte b[], int off, int len) | 将最多len个数据字节从此输入流读入byte数组
| public synchronized long skip(long n) | 从此输入流中跳过n个输入字节
| public synchronized int available() | 返回可从此输入流读取（或跳过）的剩余字节数
| public boolean markSupported() | 测试是否支持mark/reset
| public void mark(int readAheadLimit) | 设置流中的当前标记位置
| public synchronized void reset() | 将缓冲区的位置重置为标记位置
| public void close() throws IOException | 关闭ByteArrayInputStream，此方法无效
