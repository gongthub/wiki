# OutputStream
OutputStream是抽象类。这个抽象类是所有表示字节输出流的类的父类。输出流接受输出字节并将这些字节发送到某个“池”。继承这个抽象类的类必须提供至少一种可写入一个输出字节的方法。

OutputStream实现了Flushable接口。Flushable是可刷新数据的目标池，其中声明了flush方法。flush方法通过将所有已缓冲输出写入底层流来刷新此流。

## 方法
| 方法 | 说明
| --- | ---
| public abstract void write(int b) throws IOException; | 将指定的字节写入此输出流
| public void write(byte b[]) throws IOException | 将b.length个字节从指定的byte数组写入此输出流
| public void write(byte b[], int off, int len) throws IOException | 将指定byte数组中从偏移量off开始的len个字节写入此输出流
| public void flush() throws IOException | 刷新此输出流并强制写出所有缓冲的输出字节
| public void close() throws IOException | 关闭此输出流并释放与此流有关的所有系统资源
