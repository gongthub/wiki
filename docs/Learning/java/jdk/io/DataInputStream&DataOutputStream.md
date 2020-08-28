# DataInputStream & DataOutoutStream
- DataInputStream提供了一系列从二进制流中读取字节，并根据所有Java基本类型数据进行重构的readXXXX方法。同时还提供根据UTF-8修改版格式的数据重构String的工具，即readUTF方法
- DataOutputStream提供了一系列将数据从任意Java基本类型转换为一系列字节，并将这些字节写入二进制流的writeXXXX方法。同时还提供了一个将String转换成UTF-8修改版格式并写入所得到的系列字节工具，即writeUTF方法。
| 基本数据类型 | byte | short | int | long | float | double | boolean | char
| --- | --- | --- | --- | --- | --- | --- | --- | ---
| 位 | 8 | 16 | 32 | 64 | 32 | 64 | 1 | 16

## DataInputStream
### 定义
```
public
class DataInputStream extends FilterInputStream implements DataInput
```
```
    // readUTF()使用的数组
    /**
     * working arrays initialized on demand by readUTF
     */
    private byte bytearr[] = new byte[80];
    private char chararr[] = new char[80];
```

## DataOutputStream
### 定义
```
public
class DataOutputStream extends FilterOutputStream implements DataOutput 

    // 到目前为止写入到输出流中的字节数 最大为为Integer.MAX_VALUE
    /**
     * The number of bytes written to the data output stream so far.
     * If this counter overflows, it will be wrapped to Integer.MAX_VALUE.
     */
    protected int written;

    // writeUTF方法使用的字节数组
    /**
     * bytearr is initialized on demand by writeUTF
     */
    private byte[] bytearr = null;

```