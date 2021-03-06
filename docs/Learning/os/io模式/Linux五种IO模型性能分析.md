# Linux五种IO模型性能分析

## Linux下的五种I/O模型
1. 阻塞I/O（blocking I/O）
2. 非阻塞I/O（nonblocking I/O）
3. I/O复用（select和poll）（I/O multiplexing）
4. 信号驱动I/O（signal driven I/O（SIGIO））
5. 异步I/O（asynchronous I/O（the POSIX aio_functions））

### 阻塞I/O
    同步阻塞I/O模型是最常用的一个模型，也是最简单的模型。在linux中，默认情况下所有的socket都是blocking。它符合人们最常见的思考逻辑。    
    在这个IO模型中，用户空间的应用程序执行一个系统调用（recvfrom），这会导致应用程序阻塞，什么也不干，直到数据准备好，并且将数据从内核复制到用户进程，最后进程再处理数据，在等待数据到处理数据的两个阶段，整个进程都被阻塞。不能处理别的网络IO。直到kernel返回结果，用户进程才解除block状态，重新运行起来。 
    可以看出来，这两个阶段都被block住了。

![阻塞I/O模型图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/%E9%98%BB%E5%A1%9EIO%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)

### 非阻塞I/O
    在这种模式下，用户进程发出请求后，并不会阻塞，内核会里面返回一个error状态，然后用户进程需要轮询不断的check状态，在轮询期间可以干点别的事，最后直到内核把数据准备好了，然后通知用户进程，把数据从内核空间拷贝到用户所在的进程进行处理。

![非阻塞I/O模型图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/%E9%9D%9E%E9%98%BB%E5%A1%9EIO%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)

### I/O复用
    IO多路复用，指的是由专门的一个进程复制轮询检查IO操作的状态，而不用每个用户进程都得自己复制轮询，这样就大大节省了线程资源。那么这就是所谓的“IO多路复用”。UNIX/Linux下的select、poll、epoll就是干这个的（epoll比poll、select效率高，做的事情是一样的）。它的基本原理就是select、poll、epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。 
    当用户进程调用了socket，那么整个进程会被block，而同时，kernel会“监视”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。多路复用的特点是通过一种机制一个进程能同时等待IO文件描述符，内核监视这些文件描述符（套接字描述符），其中的任意一个进入读就绪状态，select、poll、epoll函数就可以返回

![I/O多路复用模型图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/IO%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8.png)

### 信号驱动I/O
    信号驱动I/O:首先我们允许socket进行信号驱动IO，并安装一个信号处理函数，进程继续运行并不阻塞。当数据准备好时，进程会收到一个SIGIO信号，可以在信号处理函数中调用I/O操作函数处理数据

![信号驱动I/O模型图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/%E4%BF%A1%E5%8F%B7%E9%A9%B1%E5%8A%A8IO%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)

### 异步I/O
    相对于同步IO，异步IO不是顺序执行。用户进程进行aio_read系统调用之后，无论内核数据是否准备好，都会直接返回给用户进程，然后用户态进程可以去做别的事情。等到socket数据准备好了，内核直接复制给进程，然后从内核向进程发送通知。IO两个阶段进程都是非阻塞的。  
    Linux提供了AIO库函数实现异步，但是用的很少。目前很多开源的异步IO库，例如libevent、libev、libuv。异步过程如下图所示：

![异步I/O模型图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/%E5%BC%82%E6%AD%A5IO%E6%A8%A1%E5%9E%8B%E5%9B%BE.png)

### 总结
    各个IO模型的比较图如下：
![各IO模型比较图](https://raw.githubusercontent.com/gongthub/wiki/master/docs/Resources/%E5%90%84IO%E6%A8%A1%E5%9E%8B%E6%AF%94%E8%BE%83%E5%9B%BE.png)
    通过上面的图片，可以发现non-blocking IO和asynchronous IO的区别还是很明显的。在non-blocking IO中，虽然进程大部分时间都不会被block，但是它仍然要求进程去主动check，并且当数据准备完成以后，也需要进程主动的再次调用recvfrom来将数据拷贝到用户内存。而asynchronous IO则完全不同。它就像是用户进程将整个IO操作交给了他人（kernel）完成，然后他人做完后发信号通知。在此期间，用户进程不需要去检查IO操作的状态，也不需要主动的去拷贝数据。