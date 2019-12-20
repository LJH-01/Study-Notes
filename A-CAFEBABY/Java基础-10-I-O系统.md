# Java基础-10-I/O系统

IO/NIO/AIO

概述和概念补充：关于IO的同步/异步，阻塞/非阻塞

同步：后续任务等待当前调用返回，之后才进行下一步。发起I/O请求后等待。

异步：其他任务不需要等待当前调用返回。发起I/O请求后继续执行，内核I/O完成后通知用户线程。

阻塞：阻塞操作线程不能从事其他任务，当条件就绪才能继续。I/O操作彻底完成后才能返回用户空间。

非阻塞：不管IO操作是否结束，直接返回，相应操作在后台处理。I/O操作被调用后立即返回一个状态值。

传统I/O（BIO）简单直观，但效率和扩展性存在局限。

Java 1.4 引入NIO框架，可以构建多路复用的，同步非阻塞的IO程序。

Java1.7中NIO改进即AIO，引入异步非阻塞IO方式。

一、IO流（同步、阻塞）

按操作数据分为：字节流（Reader，Writer）和字符流（InputStream，OutputStream）

![IO流.png](image/IO流.png)

字符流：只用来处理文本数据

最常见的是文件，操作文件的子类一般是File Reader和FileWriter

字符流读写文件注意事项：

* 写入文件必须要用flush()刷新

* 用完流记得要关闭流

* 使用流对象要抛出IO异常

* 定义文件路径时，可以用"/"或者"\"

* 在创建一个文件时，如果目录下有同名文件将被覆盖

* 在读取文件时，必须保证该文件已存在，否则抛出异常

字符流的缓冲区

* 缓冲区的出现是为了提高流的操作效率而出现的

* 需要被提高效率的流作为参数传递给缓冲区的构造函数

* 在缓冲区中封装了一个数组，存入数据后一次取出

字节流：用来处理媒体数据

字节流读写文件注意事项：

* 字节流和字符流的基本操作是相同的，但是想要操作媒体流就需要用到字节流

* 字节流因为操作的是字节，所以可以用来操作媒体文件（媒体文件也是以字节存储的）

* 输入流（InputStream）、输出流（OutputStream）

* 字节流操作可以不用刷新流操作

* InputStream特有方法：int available()（返回文件中的字节个数）

字节流的缓冲区

字节流缓冲区跟字符流缓冲区一样，也是为了提高效率

Java Scanner类

Java 5添加了java.util.Scanner类，这是一个用于扫描输入文本的新的实用程序

关于nextInt()、next()、nextLine()

nextInt()：只能读取数值，若是格式不对，会抛出java.util.InputMismatchException异常

next()：遇见第一个有效字符（非空格，非换行符）时，开始扫描，当遇见第一个分隔符或结束符（空格或换行符）时，结束扫描，获取扫描到的内容。

nextLine()：可以扫描到一行内容并作为字符串而被捕获到

关于hasNext()、hasNextLine()、hasNextxxx()，就是为了判断输入行中是否还存在xxx的意思

与delimiter()有关的方法，应该是输入内容的分隔符设置

二、NIO（同步、非阻塞）

NIO之所以是同步，是因为它的accept/read/write方法的内核I/O操作都会阻塞当前线程

NIO的三个主要组成部分：Channel（通道）、Buffer（缓冲区）、Selector（选择器）

（1）Channel（通道）

Channel是一个对象，可以通过它读取和写入数据。可以把它看做是IO中的流，不同的是：

* Channel是双向的，既可以读又可以写，而流是单向的

* Channel可以进行异步的读写

* 对Channel的读写必须通过buffer对象

正如上面提到的，所有数据都通过Buffer对象处理，所以，永远不会将字节直接写入到Channel中，相反，是将数据写入到Buffer中；同样，也不会从Channel中读取字节，而是将数据从Channel读入Buffer，再从Buffer获取这个字节。

因为Channel是双向的，所以Channel可以比流更好地反映出底层操作系统的真实情况。特别是在Unix模型中，底层操作系统通常都是双向的。

在Java NIO中的Channel主要有如下几种类型：

* FileChannel：从文件读取数据的

* DatagramChannel：读写UDP网络协议数据

* SocketChannel：读写TCP网络协议数据

* ServerSocketChannel：可以监听TCP连接

（2）Buffer

Buffer是一个对象，它包含一些要写入或者读到Stream对象的内容。应用程序不能直接对 Channel 进行读写操作，而必须通过 Buffer 来进行，即 Channel 是通过 Buffer 来读写数据的。

在NIO中，所有的数据都是用Buffer处理的，它是NIO读写数据的中转池。Buffer实质上是一个数组，通常是一个字节数据，但也可以是其他类型的数组。但一个缓冲区不仅仅是一个数组，重要的是它提供了对数据的结构化访问，而且还可以跟踪系统的读写进程。

使用 Buffer 读写数据一般遵循以下四个步骤：

1.写入数据到 Buffer；

2.调用 flip() 方法；

3.从 Buffer 中读取数据；

4.调用 clear() 方法或者 compact() 方法。

当向 Buffer 写入数据时，Buffer 会记录下写了多少数据。一旦要读取数据，需要通过 flip() 方法将 Buffer 从写模式切换到读模式。在读模式下，可以读取之前写入到 Buffer 的所有数据。

一旦读完了所有的数据，就需要清空缓冲区，让它可以再次被写入。有两种方式能清空缓冲区：调用 clear() 或 compact() 方法。clear() 方法会清空整个缓冲区。compact() 方法只会清除已经读过的数据。任何未读的数据都被移到缓冲区的起始处，新写入的数据将放到缓冲区未读数据的后面。

Buffer主要有如下几种：

* ByteBuffer

* CharBuffer

* DoubleBuffer

* FloatBuffer

* IntBuffer

* LongBuffer

* ShortBuffer

copyFile实例（NIO）

CopyFile是一个非常好的读写结合的例子，CopyFile执行三个基本的操作：创建一个Buffer，然后从源文件读取数据到缓冲区，然后再将缓冲区写入目标文件。

public static void copyFileUseNIO(String src,String dst) throws IOException{

    FileInputStream fi=new FileInputStream(new File(src));

    FileOutputStream fo=new FileOutputStream(new File(dst));

    FileChannel inChannel=fi.getChannel();

    FileChannel outChannel=fo.getChannel();

    ByteBuffer buffer=ByteBuffer.allocate(1024);

    while(true){

        int eof =inChannel.read(buffer);

        if(eof==-1){

            break;

        }

        buffer.flip();

        outChannel.write(buffer);

        buffer.clear();

    }

    inChannel.close();

    outChannel.close();

    fi.close();

    fo.close();

}

（三）Selector（选择器对象）

利用一个线程来处理所有的channels。

线程上下文切换开销会在高并发时变得很明显，这是同步阻塞方式的低扩展性劣势。

Selector是一个对象，它可以注册到很多个Channel上，监听各个Channel上发生的事件，并且能够根据事件情况决定Channel读写。有了Selector，我们就可以利用一个线程来处理所有的channels。对系统来说使用的线程越少越好。

……

……

详见链接

……

……

（4）NIO多路复用

主要步骤和元素：

* 首先，通过 Selector.open() 创建一个 Selector，作为类似调度员的角色。

* 然后，创建一个 ServerSocketChannel，并且向 Selector 注册，通过指定 SelectionKey.OP_ACCEPT，告诉调度员，它关注的是新的连接请求。

* 注意，为什么我们要明确配置非阻塞模式呢？这是因为阻塞模式下，注册操作是不允许的，会抛出 IllegalBlockingModeException 异常。

* Selector 阻塞在 select 操作，当有 Channel 发生接入请求，就会被唤醒。

* 在具体的方法中，通过 SocketChannel 和 Buffer 进行数据操作

IO 都是同步阻塞模式，所以需要多线程以实现多任务处理。而 NIO 则是利用了单线程轮询事件的机制，通过高效地定位就绪的 Channel，来决定做什么，仅仅 select 阶段是阻塞的，可以有效避免大量客户端连接时，频繁线程切换带来的问题，应用的扩展能力有了非常大的提高

# 三、NIO2--AIO(异步、非阻塞)

AIO是异步IO的缩写，虽然NIO在网络操作中，提供了非阻塞的方法，但是NIO的IO行为还是同步的。对于NIO来说，我们的业务线程是在IO操作准备好时，得到通知，接着就由这个线程自行进行IO操作，IO操作本身是同步的。

但是对AIO来说，则更加进了一步，它不是在IO准备好时再通知线程，而是在IO操作已经完成后，再给线程发出通知。因此AIO是不会阻塞的，此时我们的业务逻辑将变成一个回调函数，等待IO操作完成后，由系统自动触发。

与NIO不同，当进行读写操作时，只须直接调用API的read或write方法即可。这两种方法均为异步的，对于读操作而言，当有流可读取时，操作系统会将可读的流传入read方法的缓冲区，并通知应用程序；对于写操作而言，当操作系统将write方法传递的流写入完毕时，操作系统主动通知应用程序。 即可以理解为，read/write方法都是异步的，完成后会主动调用回调函数。 在JDK1.7中，这部分内容被称作NIO.2，主要在Java.nio.channels包下增加了下面四个异步通道：

* AsynchronousSocketChannel

* AsynchronousServerSocketChannel

* AsynchronousFileChannel

* AsynchronousDatagramChannel

在AIO socket编程中，服务端通道是AsynchronousServerSocketChannel，这个类提供了一个open()静态工厂，一个bind()方法用于绑定服务端IP地址（还有端口号），另外还提供了accept()用于接收用户连接请求。在客户端使用的通道是AsynchronousSocketChannel,这个通道处理提供open静态工厂方法外，还提供了read和write方法。

在AIO编程中，发出一个事件（accept read write等）之后要指定事件处理类（回调函数），AIO中的事件处理类是CompletionHandler<V,A>，这个接口定义了如下两个方法，分别在异步操作成功和失败时被回调。

void completed(V result, A attachment);

void failed(Throwable exc, A attachment);

---
