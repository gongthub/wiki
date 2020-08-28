# BufferedInputStream & BufferedOutputStream
- BufferedInputStream是缓冲输入流，作用是为另一个输入流添加一些功能，如果缓冲输入功能以及支持mark和reset方法的能力
- BufferedOutputStream是缓冲输出流，通过设置这种输出流，应用程序就可以将各个字节写入底层输出流中，而不必针对每次字节写入调用底层系统

# BufferedInputStream
| 字段 | 说明
| --- | ---
| private static int DEFAULT_BUFFER_SIZE = 8192; | 默认缓冲区大小 8192
| private static int MAX_BUFFER_SIZE = Integer.MAX_VALUE - 8; | 最大缓冲区大小
| protected volatile byte buf[]; | 存放数据的内部缓冲数组
| protected int count; | 缓冲区中的字节数
| protected int pos; | 缓冲区当前位置的索引
| protected int markpos = -1; | 最后一次调用mark方法时pos字段的值
| protected int marklimit; | 调用mark方法后，在后续调用reset方法失败之前允许的最大提前读取量


# BufferedOutputStream
| 字段 | 说明 
| --- | --- 
| protected byte buf[]; | 存储数据的内部缓冲区
| protected int count; | 缓冲区中的有效字节数

默认缓冲区大小为 8192
```
   public BufferedOutputStream(OutputStream out) {
        this(out, 8192);
    }
```
