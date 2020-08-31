# FileInputStream & FileOutputStream
- FileInputStream是文件输入流，用于从文件系统中的某个文件中获得输入字节。FileInputStream用于读取诸如图像数据之类的原始字节流。要读取字符流，请考虑使用FileReader。
- FileOutputStream是文件输出流，用于将数据写入File或FileDescriptor的输出流。FileOutputStream用于写入诸如图像数据之类的原始字节流。要写入字符流，请考虑使用FileWriter。
- Java语言本身不能对操作系统底层进行访问和操作，但是可以通过JNI接口调用其他语言来实现对底层的访问。
- FileInputStream不支持mark方法与set方法。

## FileInputStream
### 定义 
```
public
class FileInputStream extends InputStream

// FileInputStream的文件描述符
private final FileDescriptor fd;

// 引用文件的路径
private final String path;

// 文件通道
private FileChannel channel = null;

// 关闭锁
private final Object closeLock = new Object();

// 标识流是否关闭
private volatile boolean closed = false;

```

## FileOutputStream
### 定义
```
public
class FileOutputStream extends OutputStream

// 文件描述符
private final FileDescriptor fd;

// 标识添加还是替换文件的内容
private final boolean append;

// 关联的通道 （懒加载）
private FileChannel channel;

// 引用文件的路径
private final String path;

// 关闭锁
private final Object closeLock = new Object();

// 标识流是否关闭
private volatile boolean closed = false;

```