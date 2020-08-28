# FilterInputStream & FilterOutputStream
- FilterInputStream、FilterOutputStream是字节输入输出流。它们的主要用途在于封装其他的输入输出流，为它们提供一些额外的功能
- FilterInputStream、FilterOutputStream并没有提供什么装饰功能。FilterInputStream、FilterOutputStream的子类可进一步重写这些方法中的一些方法，来提供装饰功能。
- FilterInputStream装饰功能的实现的关键在于类中有一个InputStream字段，依赖这个字段它才可以对InputStream的子类提供装饰功能。FilterOutputStream也是如此。

## FilterInputStream
```
protected volatile InputStream in; 
```

## FilterOutputStream
```
protected OutputStream out;
```