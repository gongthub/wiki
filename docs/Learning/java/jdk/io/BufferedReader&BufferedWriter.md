# BufferedReader & BufferedWriter
- BufferedReader，字符缓冲输入流，作用是为其他输入流提供缓冲功能。BufferedReader从其他字符输入流中读取文本，缓冲各个字符，从而实现字符、数组和行的高效读取。
- BufferedReader支持标记功能。
- BufferedWriter，字符缓冲输出流，作用是为其他输出流提供缓冲功能。BufferedWriter将文本写入其他字符输出流，缓冲各个字符，从而提供单个字符、数组和字符串的高效写入。
- BufferedReader与BufferedWriter使用装饰模式

## BufferedReader
### 定义
```
public class BufferedReader extends Reader

// 底层字符输入流
private Reader in;

// 字符缓冲区
private char cb[];

// nChars是cb字符缓冲区中字符的总个数
// nextChar是下一个要读取的字符在cb缓冲区中的位置
private int nChars, nextChar;

// 表示标记无效，设置了标记，但是被标记位置由于某种原因导致标记无效
private static final int INVALIDATED = -2;

// 表示没有标记
private static final int UNMARKED = -1;

// 标记位置初始化为UNMARKED
private int markedChar = UNMARKED;

// 在仍保留该标记的情况下，对可读取字符数量的限制
// 在读取达到或超过此限制的字符后，尝试重置流可能会失败
// 限制值大于输入缓冲区的大小将导致分配一个新缓冲区，其大小不小于该限制值，因此应该小心使用较大的值
private int readAheadLimit = 0;

// 表示是否跳过换行符
private boolean skipLF = false;

// 表示当做了标记时，是否忽略换行符
private boolean markedSkipLF = false;

// 字符缓冲区默认大小
private static int defaultCharBufferSize = 8192;

// 每行默认的字符个数
private static int defaultExpectedLineLength = 80;

```

## BufferedWriter
### 定义
```
public class BufferedWriter extends Writer

// 底层字符输出流
private Writer out;

// 缓冲区
private char cb[];

// nChars 缓冲区大小
// nextChar 缓冲区当前位置的下个字符
private int nChars, nextChar;

// 默认字符缓冲区大小
private static int defaultCharBufferSize = 8192;

// 行分隔符
private String lineSeparator;

```