### **一、NIO简介** {#一nio简介}

Java NIO，通常称为 New IO 或 No Block IO， 是从Java 1.4版本开始引入的一个新的IO API，可以替代标准的Java IO API。NIO与原来的IO有同样的作用和目的，但是使用的方式完全不同，NIO支持面向缓冲区的、基于通道的IO操作。

注：JDK1.4的版本为 NIO 1.0， 而JDK7的版本为 NIO 2.0

### **二、NIO与传统IO的区别** {#二nio与传统io的区别}

1.传统IO是面向流的，NIO是面向缓冲区的。 单线程只能有一个客户端，虽然用线程池可以有多个客户端连接，但非常消耗性能。

Java 传统IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。若网速较慢，那么程序就一直等着，直到传输完成为止。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。

2.传统IO的各种流是阻塞的。而NIO是非阻塞的。

Java传统IO阻塞意味着当一个线程调用read\(\) 或 write\(\)时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。 Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞IO的空闲时间用于在其它通道上执行IO操作，所以一个单独的线程现在可以管理多个输入和输出通道。

### **三、NIO的组成** {#三nio的组成}

**1.Channel**

NIO中的channel和IO中的Stream是类似的。只不过Stream是单向的，如：InputStream, OutputStream.而Channel是双向的，既可以用来进行读操作，又可以用来进行写操作。

**2.Buffer**

NIO中的Buffer主要是当作缓冲区的作用。

**3.Selectors**

Java NIO引入了多路复用器的概念，它用于监听多个通道的事件。

### **四、NIO的特点** {#四nio的特点}

1.事件驱动模型

2.避免多线程

3.单线程处理多任务

4.非阻塞I/O，I/O读写不再阻塞，而是返回0

5.基于block的传输，通常比基于流的传输更高效

6.更高级的IO函数，zero-copy

7.IO多路复用大大提高了Java网络应用的可伸缩性和实用性

### **五、线程模型** {#五线程模型}

**1.Reactor线程模型**

Reactor模式是基于同步I/O的，在Reactor模式中，事件分发器等待某个事件或者可应用或个操作的状态发生（比如文件描述符可读写，或者是socket可读写），事件分发器就把这个事件传给事先注册的事件处理函数或者回调函数，由后者来做实际的读写操作。

例：在Reactor中实现读

* 注册读就绪事件和相应的事件处理器。
* 事件分发器等待事件。
* 事件到来，激活分发器，分发器调用事件对应的处理器。
* 事件处理器完成实际的读操作，处理读到的数据，注册新的事件，然后返还控制权。

**2.Proactor线程模型**

Proactor模式是基于异步I/O。在Proactor模式中，事件处理者（或者代由事件分发器发起）直接发起一个异步读写操作（相当于请求），而实际的工作是由操作系统来完成的。发起时，需要提供的参数包括用于存放读到数据的缓存区、读的数据大小或用于存放外发数据的缓存区，以及这个请求完后的回调函数等信息。事件分发器得知了这个请求，它默默等待这个请求的完成，然后转发完成事件给相应的事件处理者或者回调。

例：在Proactor中实现读：

* 处理器发起异步读操作（注意：操作系统必须支持异步IO）。在这种情况下，处理器无视IO就绪事件，它关注的是完成事件。
* 事件分发器等待操作完成事件。
* 在分发器等待过程中，操作系统利用并行的内核线程执行实际的读操作，并将结果数据存入用户自定义缓冲区，最后通知事件分发器读操作完成。
* 事件分发器呼唤处理器。
* 事件处理器处理用户自定义缓冲区中的数据，然后启动一个新的异步操作，并将控制权返回事件分发器。

  
[http://blog.csdn.net/baiye\_xing/article/details/73127595](http://blog.csdn.net/baiye_xing/article/details/73127595)
