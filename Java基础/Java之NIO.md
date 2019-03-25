## Java NIO

> 参考并发编程网：http://ifeve.com/overview/

#### 目录

- [Java NIO](#java-nio)
  - [一、Java NIO概述](#%E4%B8%80java-nio%E6%A6%82%E8%BF%B0)
    - [Channel和Buffer](#channel%E5%92%8Cbuffer)
    - [Selector](#selector)
  - [二、Channel](#%E4%BA%8Cchannel)
    - [Channel的实现](#channel%E7%9A%84%E5%AE%9E%E7%8E%B0)
    - [基本示例](#%E5%9F%BA%E6%9C%AC%E7%A4%BA%E4%BE%8B)
  - [三、Buffer](#%E4%B8%89buffer)
    - [Buffer的基本用法](#buffer%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
    - [Buffer的capacity、position和limit](#buffer%E7%9A%84capacityposition%E5%92%8Climit)
    - [Buffer的类型](#buffer%E7%9A%84%E7%B1%BB%E5%9E%8B)
    - [Buffer的分配](#buffer%E7%9A%84%E5%88%86%E9%85%8D)
    - [向Buffer中写数据](#%E5%90%91buffer%E4%B8%AD%E5%86%99%E6%95%B0%E6%8D%AE)
    - [从Buffer中读数据](#%E4%BB%8Ebuffer%E4%B8%AD%E8%AF%BB%E6%95%B0%E6%8D%AE)
    - [Buffer中的方法](#buffer%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95)
  - [四、Scatter和Gather](#%E5%9B%9Bscatter%E5%92%8Cgather)
    - [Scattering Reads](#scattering-reads)
    - [Scattering Writes](#scattering-writes)
  - [五、通道间的数据传输](#%E4%BA%94%E9%80%9A%E9%81%93%E9%97%B4%E7%9A%84%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93)
    - [transferFrom()](#transferfrom)
    - [transferTo()](#transferto)
  - [六、Selector](#%E5%85%ADselector)
    - [为什么要使用Selector？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%BD%BF%E7%94%A8selector)
    - [Selector的创建](#selector%E7%9A%84%E5%88%9B%E5%BB%BA)
    - [向Selector注册通道](#%E5%90%91selector%E6%B3%A8%E5%86%8C%E9%80%9A%E9%81%93)
    - [SelectionKey](#selectionkey)
    - [ready集合](#ready%E9%9B%86%E5%90%88)
    - [Channel + Selector](#channel--selector)
    - [附加对象](#%E9%99%84%E5%8A%A0%E5%AF%B9%E8%B1%A1)
    - [通过Selector选择通道](#%E9%80%9A%E8%BF%87selector%E9%80%89%E6%8B%A9%E9%80%9A%E9%81%93)
    - [selectedKeys()](#selectedkeys)
    - [wakeUp()](#wakeup)
    - [close()](#close)
    - [完整示例](#%E5%AE%8C%E6%95%B4%E7%A4%BA%E4%BE%8B)
  - [七、FileChannel](#%E4%B8%83filechannel)
    - [打开FileChannel](#%E6%89%93%E5%BC%80filechannel)
    - [从FileChannel读取数据](#%E4%BB%8Efilechannel%E8%AF%BB%E5%8F%96%E6%95%B0%E6%8D%AE)
    - [向FileChannel写数据](#%E5%90%91filechannel%E5%86%99%E6%95%B0%E6%8D%AE)
    - [关闭FileChannel](#%E5%85%B3%E9%97%ADfilechannel)
    - [FileChannel的position方法](#filechannel%E7%9A%84position%E6%96%B9%E6%B3%95)
    - [FileChannel的size方法](#filechannel%E7%9A%84size%E6%96%B9%E6%B3%95)
    - [FileChannel的truncate方法](#filechannel%E7%9A%84truncate%E6%96%B9%E6%B3%95)
    - [FileChannel的force方法](#filechannel%E7%9A%84force%E6%96%B9%E6%B3%95)
  - [八、SocketChannel](#%E5%85%ABsocketchannel)
    - [打开 SocketChannel](#%E6%89%93%E5%BC%80-socketchannel)
    - [关闭 SocketChannel](#%E5%85%B3%E9%97%AD-socketchannel)
    - [从 SocketChannel 读取数据](#%E4%BB%8E-socketchannel-%E8%AF%BB%E5%8F%96%E6%95%B0%E6%8D%AE)
    - [写入 SocketChannel](#%E5%86%99%E5%85%A5-socketchannel)
    - [非阻塞模式](#%E9%9D%9E%E9%98%BB%E5%A1%9E%E6%A8%A1%E5%BC%8F)
    - [write()](#write)
    - [read()](#read)
    - [非阻塞模式与选择器](#%E9%9D%9E%E9%98%BB%E5%A1%9E%E6%A8%A1%E5%BC%8F%E4%B8%8E%E9%80%89%E6%8B%A9%E5%99%A8)
  - [九、ServerSocketChannel](#%E4%B9%9Dserversocketchannel)
    - [打开 ServerSocketChannel](#%E6%89%93%E5%BC%80-serversocketchannel)
    - [关闭 ServerSocketChannel](#%E5%85%B3%E9%97%AD-serversocketchannel)
    - [监听新进来的连接](#%E7%9B%91%E5%90%AC%E6%96%B0%E8%BF%9B%E6%9D%A5%E7%9A%84%E8%BF%9E%E6%8E%A5)
    - [非阻塞模式](#%E9%9D%9E%E9%98%BB%E5%A1%9E%E6%A8%A1%E5%BC%8F-1)
  - [十、Java NIO DatagramChannel](#%E5%8D%81java-nio-datagramchannel)
    - [打开 DatagramChannel](#%E6%89%93%E5%BC%80-datagramchannel)
    - [接收数据](#%E6%8E%A5%E6%94%B6%E6%95%B0%E6%8D%AE)
    - [发送数据](#%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE)
    - [连接到特定的地址](#%E8%BF%9E%E6%8E%A5%E5%88%B0%E7%89%B9%E5%AE%9A%E7%9A%84%E5%9C%B0%E5%9D%80)
  - [十一、Pipe](#%E5%8D%81%E4%B8%80pipe)
    - [创建管道](#%E5%88%9B%E5%BB%BA%E7%AE%A1%E9%81%93)
    - [向管道写数据](#%E5%90%91%E7%AE%A1%E9%81%93%E5%86%99%E6%95%B0%E6%8D%AE)
    - [从管道读取数据](#%E4%BB%8E%E7%AE%A1%E9%81%93%E8%AF%BB%E5%8F%96%E6%95%B0%E6%8D%AE)
  - [十二、Java NIO与IO](#%E5%8D%81%E4%BA%8Cjava-nio%E4%B8%8Eio)
    - [Java NIO和IO的主要区别](#java-nio%E5%92%8Cio%E7%9A%84%E4%B8%BB%E8%A6%81%E5%8C%BA%E5%88%AB)
    - [面向流与面向缓冲](#%E9%9D%A2%E5%90%91%E6%B5%81%E4%B8%8E%E9%9D%A2%E5%90%91%E7%BC%93%E5%86%B2)
    - [阻塞与非阻塞IO](#%E9%98%BB%E5%A1%9E%E4%B8%8E%E9%9D%9E%E9%98%BB%E5%A1%9Eio)
    - [选择器（Selectors）](#%E9%80%89%E6%8B%A9%E5%99%A8selectors)
    - [NIO和IO如何影响应用程序的设计](#nio%E5%92%8Cio%E5%A6%82%E4%BD%95%E5%BD%B1%E5%93%8D%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E7%9A%84%E8%AE%BE%E8%AE%A1)
      - [1）API调用](#1api%E8%B0%83%E7%94%A8)
      - [2）数据处理](#2%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86)
      - [3）用来处理数据的线程数](#3%E7%94%A8%E6%9D%A5%E5%A4%84%E7%90%86%E6%95%B0%E6%8D%AE%E7%9A%84%E7%BA%BF%E7%A8%8B%E6%95%B0)
  - [十三、Java NIO Path](#%E5%8D%81%E4%B8%89java-nio-path)
    - [创建Path实例](#%E5%88%9B%E5%BB%BApath%E5%AE%9E%E4%BE%8B)
    - [创建绝对路径Path](#%E5%88%9B%E5%BB%BA%E7%BB%9D%E5%AF%B9%E8%B7%AF%E5%BE%84path)
    - [创建相对路径Path](#%E5%88%9B%E5%BB%BA%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84path)
    - [Path.normalize()](#pathnormalize)



### 一、Java NIO概述

NIO又称为新IO或非阻塞IO，由以下几个核心部分组成：

- Channels
- Buffers
- Selectors

#### Channel和Buffer

基本上，所有的 IO 在NIO 中都从一个Channel 开始。Channel 有点象流。 数据可以从Channel读到Buffer中，也可以从Buffer 写到Channel中。

Channel和Buffer有好几种类型。下面是JAVA NIO中的一些主要Channel的实现：

- FileChannel，文件IO
- DatagramChannel，UDP网络IO
- SocketChannel，TCP网络IO
- ServerSocketChannel，TCP网络IO

以下是Java NIO里关键的Buffer实现：

- ByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

这些Buffer覆盖了你能通过IO发送的基本数据类型：byte, short, int, long, float, double 和 char。

#### Selector

Selector允许单线程处理多个Channel。如果你的应用打开了多个连接（Channel），但每个连接的流量都很低，使用Selector就会很方便。例如，在一个聊天服务器中。

**缺图 selector**

要使用Selector，得向Selector注册Channel，然后调用select()方法。这个方法会一直阻塞到某个注册的通道有事件就绪。一旦这个方法返回，线程就可以处理这些事件，事件的例子有如新连接进来，数据接收等。

### 二、Channel 

Java NIO的通道类似流，但又有些不同：

- 既可以从通道中读取数据，也可以写数据到通道。但流的读写通常是单向的。
- 通道可以异步读写。
- 通道中的数据总是先要读到一个Buffer，或者总要从一个Buffer中写入。

#### Channel的实现

这些是Java NIO中最重要的通道实现。

- FileChannel，文件IO
- DatagramChannel，UDP网络IO
- SocketChannel，TCP网络IO
- ServerSocketChannel，TCP网络IO

#### 基本示例

下面是一个使用FileChannel读取数据到Buffer中的示例：

    RandomAccessFile file = new RandomAccessFile("data/a.txt", "rw");
    FileChannel channel = file.getChannel();
    
    ByteBuffer buf = ByteBuffer.allocate(64);
    int reader = channel.read(buf);
    while (reader != -1) {
        // 转换到读模式
        buf.flip();
        while (buf.hasRemaing()) {
            System.out.println(buf.get());        
        }
        // 清空缓冲区，position置为0，limit置为capacity?
        buf.clear();
        reader = channel.read(buf);
    }
    file.close();

### 三、Buffer

Java NIO中的Buffer用于和NIO通道进行交互。如你所知，数据是从通道读入缓冲区，从缓冲区写入到通道中的。

缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成NIO Buffer对象，并提供了一组方法，用来方便的访问该块内存。

#### Buffer的基本用法

使用Buffer读写数据一般遵循以下四个步骤：

1. 写数据到Buffer
2. 调用flip()方法
3. 从Buffer中读取数据
4. 调用clear()方法或compact()方法

当向buffer写入数据时，buffer会记录下写了多少数据。一旦要读取数据，需要通过flip()方法将Buffer从写模式切换到读模式。在读模式下，可以读取之前写入到buffer的所有数据。

一旦读完了所有的数据，就需要清空缓冲区，让它可以再次被写入。有两种方式能清空缓冲区：调用clear()或compact()方法。clear()方法会清空整个缓冲区。compact()方法只会清除已经读过的数据。任何未读的数据都被移到缓冲区的起始处，新写入的数据将放到缓冲区未读数据的后面。

#### Buffer的capacity、position和limit

缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成NIO Buffer对象，并提供了一组方法，用来方便的访问该块内存。

为了理解Buffer的工作原理，需要熟悉它的三个属性：

- capacity
- position
- limit

大小关系：capacity >= limit >= position

position和limit的含义取决于Buffer处在读模式还是写模式。不管Buffer处在什么模式，capacity的含义总是一样的。

这里有一个关于capacity，position和limit在读写模式中的说明，详细的解释在插图后面。

**缺图 Buffer读写模式属性**

**capacity**
作为一个内存块，Buffer有一个固定的大小值，也叫“capacity”.你只能往里写capacity个byte、long，char等类型。一旦Buffer满了，需要将其清空（通过读数据或者清除数据）才能继续写数据往里写数据。

**position**
当你写数据到Buffer中时，position表示当前写入位置。初始的position值为0。当一个byte、long等数据写到Buffer后， position会向前移动到下一个可插入数据的Buffer单元。

**limit**
在写模式下，Buffer的limit表示你最多能往Buffer里写多少数据。 写模式下，limit等于Buffer的capacity。

当切换Buffer到读模式时， limit表示你最多能读到多少数据。因此，当切换Buffer到读模式时，limit会被设置成写模式下的position值。换句话说，你能读到之前写入的所有数据（limit被设置成已写数据的数量，这个值在写模式下就是position）

#### Buffer的类型

Java NIO 有以下Buffer类型：

- ByteBuffer
- MappedByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

如你所见，这些Buffer类型代表了不同的数据类型。换句话说，就是可以通过char，short，int，long，float 或 double类型来操作缓冲区中的字节。

MappedByteBuffer 有些特别，在涉及它的专门章节中再讲。

#### Buffer的分配

要想获得一个Buffer对象首先要进行分配。 每一个Buffer类都有一个allocate方法。下面是一个分配64字节capacity的ByteBuffer的例子。

    ByteBuffer buf = ByteBuffer.allocate(64);

这是分配一个可存储1024个字符的CharBuffer：
    
    CharBuffer buf = CharBuffer.allocate(1024);

#### 向Buffer中写数据

写数据到Buffer有两种方式：

- 从Channel写到Buffer
- 通过Buffer的put()方法写到Buffer

从Channel写到Buffer的例子：

    int reader = channel.read(buf);

通过put方法：

    buf.put(1);

put方法有很多版本，允许你以不同的方式把数据写入到Buffer中。例如， 写到一个指定的位置，或者把一个字节数组写入到Buffer。 更多Buffer实现的细节参考JavaDoc。

#### 从Buffer中读数据

从Buffer中读取数据有两种方式：

- 从Buffer读取数据到Channel。
- 使用get()方法从Buffer中读取数据。

从Buffer读取数据到Channel的例子：

    int writer = channel.writer(buf);

 使用get()方法从Buffer中读取数据的例子：

    byte b = buf.get();

get方法有很多版本，允许你以不同的方式从Buffer中读取数据。例如，从指定position读取，或者从Buffer中读取数据到字节数组。更多Buffer实现的细节参考JavaDoc。

#### Buffer中的方法

**rewind()方法：**

Buffer.rewind()将position设回0，所以你可以重读Buffer中的所有数据。limit保持不变，仍然表示能从Buffer中读取多少个元素（byte、char等）。

**flip()方法：**

flip方法将Buffer从写模式切换到读模式。调用flip()方法会将position设回0，并将limit设置成之前position的值。
换句话说，position现在用于标记读的位置，limit表示之前写进了多少个byte、char等 —— 现在能读取多少个byte、char等。

**clear()和compact()方法：**

一旦读完Buffer中的数据，需要让Buffer准备好再次被写入。可以通过clear()或compact()方法来完成。

如果调用的是clear()方法，position将被设回0，limit被设置成 capacity的值。换句话说，Buffer 被清空了。Buffer中的数据并未清除，只是这些标记告诉我们可以从哪里开始往Buffer里写数据。

如果Buffer中有一些未读的数据，调用clear()方法，数据将“被遗忘”，意味着不再有任何标记会告诉你哪些数据被读过，哪些还没有。

如果Buffer中仍有未读的数据，且后续还需要这些数据，但是此时想要先先写些数据，那么使用compact()方法。

compact()方法将所有未读的数据拷贝到Buffer起始处。然后将position设到最后一个未读元素正后面。limit属性依然像clear()方法一样，设置成capacity。现在Buffer准备好写数据了，但是不会覆盖未读的数据。

**mark()和reset()方法：**

通过调用Buffer.mark()方法，可以标记Buffer中的一个特定position。之后可以通过调用Buffer.reset()方法恢复到这个position。例如：

    buf.mark();
    调用buf.get()多次
    buf.reset();    // 将position设置成mark时候的

**equals()与compareTo()方法**

可以使用equals()和compareTo()方法两个Buffer。

**equals()**
满足下列条件时，表示两个Buffer相等：

1. 相同的类型（byte、char、int等）
2. Buffer中剩余的byte、char等的个数相等
3. Buffer中剩余的byte、char等都相同

如你所见，equals只是比较Buffer的一部分，不是每一个在它里面的元素都比较。实际上，它只比较Buffer中的剩余元素，例如：

    ByteBuffer buf1 = ByteBuffer.allocate(64);
    ByteBuffer buf2 = ByteBuffer.allocate(64);
    buf1.put("cccc".getBytes());
    buf2.put("accc".getBytes());
    System.out.println(buf1.equals(buf2));
    buf1.flip();
    buf2.flip();
    System.out.println(buf1.equals(buf2));

结果：第一个true，第二个false

**compareTo()**

compareTo()方法比较两个Buffer的剩余元素(byte、char等)， 如果满足下列条件，则认为一个Buffer“小于”另一个Buffer：

- 第一个不相等的元素小于另一个Buffer中对应的元素 。
- 所有元素都相等，但第一个Buffer比另一个先耗尽(第一个Buffer的元素个数比另一个少)。

**剩余元素，也就是说position到limit之间的元素**

### 四、Scatter和Gather

Java NIO开始支持scatter/gather，scatter/gather用于描述从Channel（译者注：Channel在中文经常翻译为通道）中读取或者写入到Channel的操作。
分散（scatter）从Channel中读取是指在读操作时将读取的数据写入多个buffer中。因此，Channel将从Channel中读取的数据“分散（scatter）”到多个Buffer中。
聚集（gather）写入Channel是指在写操作时将多个buffer的数据写入同一个Channel，因此，Channel 将多个Buffer中的数据“聚集（gather）”后发送到Channel。

scatter / gather经常用于需要将传输的数据分开处理的场合，例如传输一个由消息头和消息体组成的消息，你可能会将消息体和消息头分散到不同的buffer中，这样你可以方便的处理消息头和消息体。

#### Scattering Reads

Scattering Reads是指数据从一个channel读取到多个buffer中。如下图描述：

缺图：Scattering Reads

代码示例如下：

    ByteBuffer header = ByteBuffer.allocate(1024);
    ByteBuffer body = ByteBuffer.allocate(1024);
    ByteBuffer[] bufArray = { header, body };
    channel.read(bufferArray);

注意buffer首先被插入到数组，然后再将数组作为channel.read() 的输入参数。read()方法按照buffer在数组中的顺序将从channel中读取的数据写入到buffer，当一个buffer被写满后，channel紧接着向另一个buffer中写。

Scattering Reads在移动下一个buffer前，必须填满当前的buffer，这也意味着它不适用于动态消息(译者注：消息大小不固定)。换句话说，如果存在消息头和消息体，消息头必须完成填充（例如 1024byte），Scattering Reads才能正常工作。

#### Scattering Writes

Gathering Writes是指数据从多个buffer写入到同一个channel。如下图描述：

缺图：Gathering Writes

代码示例如下：

    ByteBuffer header = ByteBuffer.allocate(1024);
    ByteBuffer body = ByteBuffer.allocate(1024);
    ByteBuffer[] bufArray = { header, body };
    channel.write(bufferArray);

buffers数组是write()方法的入参，write()方法会按照buffer在数组中的顺序，将数据写入到channel，注意只有position和limit之间的数据才会被写入。因此，如果一个buffer的容量为128byte，但是仅仅包含58byte的数据，那么这58byte的数据将被写入到channel中。因此与Scattering Reads相反，Gathering Writes能较好的处理动态消息。

### 五、通道间的数据传输

在Java NIO中，如果两个通道中有一个是FileChannel，那你可以直接将数据从一个channel（译者注：channel中文常译作通道）传输到另外一个channel。

#### transferFrom()

FileChannel的transferFrom()方法可以将数据从源通道传输到FileChannel中（译者注：这个方法在JDK文档中的解释为将字节从给定的可读取字节通道传输到此通道的文件中）。下面是一个简单的例子：

    FileChannel fromChannel = new RandomAccessFile("f:/a.txt", "rw").getChannel();
    FileChannel toChannel = new RandomAccessFile("f:/b.txt", "rw").getChannel();
    toChannel.transferFrom(fromChannel, 0, fromChannel.size());

方法的输入参数position表示从position处开始向目标文件写入数据，count表示最多传输的字节数。如果源通道的剩余空间小于 count 个字节，则所传输的字节数要小于请求的字节数。
此外要注意，在SoketChannel的实现中，SocketChannel只会传输此刻准备好的数据（可能不足count字节）。因此，SocketChannel可能不会将请求的所有数据(count个字节)全部传输到FileChannel中。

#### transferTo()

transferTo()方法将数据从FileChannel传输到其他的channel中。下面是一个简单的例子：

    FileChannel fromChannel = new RandomAccessFile("f:/a.txt", "rw").getChannel();
    FileChannel toChannel = new RandomAccessFile("f:/b.txt", "rw").getChannel();
    fromChannel.transferTo(0, fromChannel.size(), toChannel);
是不是发现这个例子和前面那个例子特别相似？除了调用方法的FileChannel对象不一样外，其他的都一样。
上面所说的关于SocketChannel的问题在transferTo()方法中同样存在。SocketChannel会一直传输数据直到目标buffer被填满。

### 六、Selector

Selector（选择器）是Java NIO中能够检测一到多个NIO通道，并能够知晓通道是否为诸如读写事件做好准备的组件。这样，一个单独的线程可以管理多个channel，从而管理多个网络连接。

#### 为什么要使用Selector？

仅用单个线程来处理多个Channels的好处是，只需要更少的线程来处理通道。事实上，可以只用一个线程处理所有的通道。对于操作系统来说，线程之间上下文切换的开销很大，而且每个线程都要占用系统的一些资源（如内存）。因此，使用的线程越少越好。

但是，需要记住，现代的操作系统和CPU在多任务方面表现的越来越好，所以多线程的开销随着时间的推移，变得越来越小了。实际上，如果一个CPU有多个内核，不使用多任务可能是在浪费CPU能力。不管怎么说，关于那种设计的讨论应该放在另一篇不同的文章中。在这里，只要知道使用Selector能够处理多个通道就足够了。

#### Selector的创建

通过调用Selector.open()方法创建一个Selector，如下：

    Selector selector = Selector.open();

#### 向Selector注册通道

为了将Channel和Selector配合使用，必须将channel注册到selector上。通过SelectableChannel.register()方法来实现，如下：

    channel.configureBlocking(false);
    SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
与Selector一起使用时，Channel必须处于非阻塞模式下。这意味着不能将FileChannel与Selector一起使用，因为FileChannel不能切换到非阻塞模式。而套接字通道都可以。

注意register()方法的第二个参数。这是一个“interest集合”，意思是在通过Selector监听Channel时对什么事件感兴趣。可以监听四种不同类型的事件：

1. Connect
2. Accept
3. Read
4. Write

通道触发了一个事件意思是该事件已经就绪。所以，某个channel成功连接到另一个服务器称为“连接就绪”。一个server socket channel准备好接收新进入的连接称为“接收就绪”。一个有数据可读的通道可以说是“读就绪”。等待写数据的通道可以说是“写就绪”。

这四种事件用SelectionKey的四个常量来表示：

1. SelectionKey.OP_CONNECT
2. SelectionKey.OP_ACCEPT
3. SelectionKey.OP_READ
4. SelectionKey.OP_WRITE

如果你对不止一种事件感兴趣，那么可以用“位或”操作符将常量连接起来，如下：

    int interestSet = SelectionKey.OP_READ | SelectionKey.OP_WRITE;

#### SelectionKey

在上一小节中，当向Selector注册Channel时，register()方法会返回一个SelectionKey对象。这个对象包含了一些你感兴趣的属性：

- interest集合
- ready集合
- Channel
- Selector
- 附加的对象（可选）

**interest集合**

　　就像向Selector注册通道一节中所描述的，interest集合是你所选择的感兴趣的事件集合。可以通过SelectionKey读写interest集合，像这样：

```java
int interestSet = selectionKey.interestOps();

boolean isInterestedInAccept = (interestSet & SelectionKey.OP_ACCEPT) == SelectionKey.OP_ACCEPT;
boolean isInterestedInConnect = (interestSet & SelectionKey.OP_CONNECT) == SelectionKey.OP_CONNECT;
boolean isInterestedInRead = (interestSet & SelectionKey.OP_READ) == SelectionKey.OP_READ;
boolean isInterestedInWrite = (interestSet & SelectionKey.OP_WRITE) == SelectionKey.OP_WRITE;													
```

可以看到，用“位于”操作interest集合和给定的SelectionKey常量，可以确定某个确定的事件是否在interest集合中。

#### ready集合

ready集合是通道已经准备就绪的操作集合。再一次选择（Selection）之后，你会首先访问这个ready set。Selection将在下一小节进行解释。可以这样访问ready集合：

```java
int readySet = selectionKey.readyOps();
```

可以像检测interest集合那样的方法，来检测channel中什么事件或操作已经准备就绪。但是，也可以使用以下四个方法，它们会返回一个布尔类型：

```
selectionKey.isAcceptable();
selectionKey.isConnectable();
selectionKey.isReadable();
selectionKey.isWritable();
```

#### Channel + Selector

从SelectionKey访问Channel和Selector很简单：

```
Channel channel = selectionKey.channel();
Selector selector = selectionKey.selector();
```

#### 附加对象

可以将一个对象或者更多信息附着到SelectionKey上，这样能方便地识别某个给定的通道。例如，可以附加于通道一起使用的Buffer，或者包含聚集数据的某个对象。使用方法如下：

```
selectionKey.attach(object);
Object obj = selectionKey.attachment();
```

还可以在用register()方法向Selector注册Channel的时候附加对象。如：

```java
SelectionKey key = channel.register(selector, SelectionKey.OP_READ, object);
```

#### 通过Selector选择通道

一旦向Selector注册了一或多个通道，就可以调用几个重载的select()方法。这些方法返回你所感兴趣的事件（如连接、接受、读或写）已经准备就绪的那些通道。换句话说，如果你对“读就绪”的通道感兴趣，select()方法会返回读事件已经就绪的那些通道。

下面是select()方法

- int select()
- int select(long timeout)
- int selectNow()

`select()`阻塞到至少有一个通道在你注册的事件上就绪了。

`select(long timeout)`和select()一样，除了最长会阻塞timeout毫秒(参数)。

`selectNow()`不会阻塞，不管什么通道就绪都立刻返回（*译者注：此方法执行非阻塞的选择操作。如果自从前一次选择操作后，没有通道变成可选择的，则此方法直接返回零。*）。

select()方法返回的int值表示有多少通道已经就绪。亦即，自上次调用select()方法后有多少通道变成就绪状态。如果调用select()方法，因为有一个通道变成就绪状态，返回了1，若再次调用select()方法，如果另一个通道就绪了，它会再次返回1。如果对第一个就绪的channel没有做任何操作，现在就有两个就绪的通道，但在每次select()方法调用之间，只有一个通道就绪了。

#### selectedKeys()

一旦调用了select()方法，并且返回值表明有一个或更多个通道就绪了，然后可以通过调用selector的selectedKeys()方法，访问“已选择键集（selected key set）”中的就绪通道。如下所示：

```
Set selectedKeys = selector.selectedKeys();
```

当像Selector注册Channel时，Channel.register()方法会返回一个SelectionKey 对象。这个对象代表了注册到该Selector的通道。可以通过SelectionKey的selectedKeySet()方法访问这些对象。

可以遍历这个已选择的键集合来访问就绪的通道。如下：

```java
Set selectedKeys = selector.selectedKeys();
Iterator keyIterator = selectedKeys.iterator();
while (keyIterator.hasNext()) {
    SelectionKey key = keyIterator.next();
    if (key.isAcceptable()) {
        // a connection was accepted by a ServerSocketChannel.
    } else if (key.isConnectable()) {
        // a connection was established with a remote server.
    } else if (key.isReadable()) {
        // a channel is ready for reading
    } else if (key.isWritable()) {
        // a channel is ready for writing
    }
	keyIterator.remove();
}
```

这个循环遍历已选择键集中的每个键，并检测各个键所对应的通道的就绪事件。

注意每次迭代末尾的keyIterator.remove()调用。Selector不会自己从已选择键集中移除SelectionKey实例。必须在处理完通道时自己移除。下次该通道变成就绪时，Selector会再次将其放入已选择键集中。

SelectionKey.channel()方法返回的通道需要转型成你要处理的类型，如ServerSocketChannel或SocketChannel等。

#### wakeUp()

某个线程调用select()方法后阻塞了，即使没有通道已经就绪，也有办法让其从select()方法返回。只要让其它线程在第一个线程调用select()方法的那个对象上调用Selector.wakeup()方法即可。阻塞在select()方法上的线程会立马返回。

如果有其它线程调用了wakeup()方法，但当前没有线程阻塞在select()方法上，下个调用select()方法的线程会立即“醒来（wake up）”。

#### close()

用完Selector后调用其close()方法会关闭该Selector，且使注册到该Selector上的所有SelectionKey实例无效。通道本身并不会关闭。

#### 完整示例

这里有一个完整的示例，打开一个Selector，注册一个通道到这个Selector上(通道的初始化过程略去),然后持续监控这个Selector的四种事件（接受，连接，读，写）是否就绪。

```java
Selector selector = Selector.open();
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, SelectionKey.OP_READ);
whilte (true) {
    int readyChannels = selector.select();
    if (readyChannels == 0)
        continue;
    Set selectedKeys = selector.selectedKeys();
    Iterator keyIterator = selectedKeys.iterator();
    while (keyIterator.hasNext()) {
        SelectionKey key = keyIterator.next();
        if (key.isAcceptable()) {
        	// a connection was accepted by a ServerSocketChannel.
        } else if (key.isConnectable()) {
            // a connection was established with a remote server.
        } else if (key.isReadable()) {
            // a channel is ready for reading
        } else if (key.isWritable()) {
            // a channel is ready for writing
        }
        keyIterator.remove();
    }
}
```

### 七、FileChannel

Java NIO中的FileChannel是一个连接到文件的通道。可以通过文件通道读写文件。

FileChannel无法设置为非阻塞模式，它总是运行在阻塞模式下。

#### 打开FileChannel

在使用FileChannel之前，必须先打开它。但是，我们无法直接打开一个FileChannel，需要通过使用一个InputStream、OutputStream或RandomAccessFile来获取一个FileChannel实例。下面是通过RandomAccessFile打开FileChannel的示例：

```
RandomAccessFile file = new RandomAccessFile("a.txt", "rw");
FileChannel inChannel = file.getChannel();
```

#### 从FileChannel读取数据

调用多个read()方法之一从FileChannel中读取数据。如：

```java
ByteBuffer buf = ByteBuffer.allocate(66);
int byteRead = inChannel.read(buf);
```

首先，分配一个Buffer。从FileChannel中读取的数据将被读到Buffer中。

然后，调用FileChannel.read()方法。该方法将数据从FileChannel读取到Buffer中。read()方法返回的int值表示了有多少字节被读到了Buffer中。如果返回-1，表示到了文件末尾。

#### 向FileChannel写数据

使用FileChannel.write()方法向FileChannel写数据，该方法的参数是一个Buffer。如：

```java
String data = "hello world";
ByteBuffer buf = ByteBuffer.allocate(1024);
buf.put(data.getBytes());
buf.flip();
while (buf.hasRemaining()) {
    channel.write(buf);
}
```

注意FileChannel.write()是在while循环中调用的。因为无法保证write()方法一次能向FileChannel写入多少字节，因此需要重复调用write()方法，直到Buffer中已经没有尚未写入通道的字节。

#### 关闭FileChannel

用完FileChannel后必须将其关闭。如：

```
channel.close();
```

#### FileChannel的position方法

有时可能需要在FileChannel的某个特定位置进行数据的读/写操作。可以通过调用position()方法获取FileChannel的当前位置。

也可以通过调用position(long pos)方法设置FileChannel的当前位置。

这里有两个例子:

```
long pos = channel.position();
channel.position(pos + 66);
```

如果将位置设置在文件结束符之后，然后试图从文件通道中读取数据，读方法将返回-1 —— 文件结束标志。

如果将位置设置在文件结束符之后，然后向通道中写数据，文件将撑大到当前位置并写入数据。这可能导致“文件空洞”，磁盘上物理文件中写入的数据间有空隙。

#### FileChannel的size方法

FileChannel实例的size()方法将返回该实例所关联文件的大小。如:

```java
long size = channel.size();
```

#### FileChannel的truncate方法

可以使用FileChannel.truncate()方法截取一个文件。截取文件时，文件将中指定长度后面的部分将被删除。如：

```
channel.truncate(1024);
```

这个例子截取文件的前1024个字节。

#### FileChannel的force方法

FileChannel.force()方法将通道里尚未写入磁盘的数据强制写到磁盘上。出于性能方面的考虑，操作系统会将数据缓存在内存中，所以无法保证写入到FileChannel里的数据一定会即时写到磁盘上。要保证这一点，需要调用force()方法。

force()方法有一个boolean类型的参数，指明是否同时将文件元数据（权限信息等）写到磁盘上。

下面的例子同时将文件数据和元数据强制写到磁盘上：

```
channel.force(true);
```

### 八、SocketChannel

Java NIO中的SocketChannel是一个连接到TCP网络套接字的通道。可以通过以下2种方式创建SocketChannel：

1.  打开一个SocketChannel并连接到互联网上的某台服务器。
2. 一个新连接到达ServerSocketChannel时，会创建一个SocketChannel。

#### 打开 SocketChannel

下面是SocketChannel的打开方式：

```java
SocketChannel socketChannel = SocketChannel.open();
socketChannel.connect(new InetSocketAddress("http://www.baidu.com", 80));
```

#### 关闭 SocketChannel

当用完SocketChannel之后调用SocketChannel.close()关闭SocketChannel：

```
socketChannel.close();
```

#### 从 SocketChannel 读取数据

要从SocketChannel中读取数据，调用一个read()的方法之一。以下是例子：

```
ByteBuffer buf = ByteBuffer.allocate(1024);
int read = socketChannel.read(buf);
```

首先，分配一个Buffer。从SocketChannel读取到的数据将会放到这个Buffer中。

然后，调用SocketChannel.read()。该方法将数据从SocketChannel 读到Buffer中。read()方法返回的int值表示读了多少字节进Buffer里。如果返回的是-1，表示已经读到了流的末尾（连接关闭了）。

#### 写入 SocketChannel

写数据到SocketChannel用的是SocketChannel.write()方法，该方法以一个Buffer作为参数。示例如下：

```
String data = "hello world";
ByteBuffer buf = ByteBuffer.allocate(1024);
buf.clear();
buf.put(data.getBytes());
buf.flip();
while (buf.hasRemaining()) {
    channel.write(buf);
}
```

注意SocketChannel.write()方法的调用是在一个while循环中的。Write()方法无法保证能写多少字节到SocketChannel。所以，我们重复调用write()直到Buffer没有要写的字节为止。

#### 非阻塞模式

可以设置 SocketChannel 为非阻塞模式（non-blocking mode）.设置之后，就可以在异步模式下调用connect(), read() 和write()了。

**connect()**

如果SocketChannel在非阻塞模式下，此时调用connect()，该方法可能在连接建立之前就返回了。为了确定连接是否建立，可以调用finishConnect()的方法。像这样：

```java
socketChannel.configureBlocking(false);
socketChannel.connect(new InetSocketAddress("www.baidu.com", 80));
while (!socketChannel.finishConnect()) {
    // do something
}
```

#### write()

非阻塞模式下，write()方法在尚未写出任何内容时可能就返回了。所以需要在循环中调用write()。前面已经有例子了，这里就不赘述了。

#### read()

非阻塞模式下,read()方法在尚未读取到任何数据时可能就返回了。所以需要关注它的int返回值，它会告诉你读取了多少字节。

#### 非阻塞模式与选择器

非阻塞模式与选择器搭配会工作的更好，通过将一或多个SocketChannel注册到Selector，可以询问选择器哪个通道已经准备好了读取，写入等。Selector与SocketChannel的搭配使用会在后面详讲。

### 九、ServerSocketChannel

Java NIO中的 ServerSocketChannel 是一个可以监听新进来的TCP连接的通道, 就像标准IO中的ServerSocket一样。ServerSocketChannel类在 java.nio.channels包中。

这里有个例子：

```java
ServerSocketChannel channel = ServerSocketChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
while (true) {
    SocketChannel socketChannel = channel.accept();
    // do something
}
```

#### 打开 ServerSocketChannel

通过调用 ServerSocketChannel.open() 方法来打开ServerSocketChannel.如：

```
ServerSocketChannel channel = ServerSocketChannel.open();
```

#### 关闭 ServerSocketChannel

通过调用ServerSocketChannel.close() 方法来关闭ServerSocketChannel. 如：

```
channel.close();
```

#### 监听新进来的连接

通过 ServerSocketChannel.accept() 方法监听新进来的连接。当 accept()方法返回的时候,它返回一个包含新进来的连接的 SocketChannel。因此, accept()方法会一直阻塞到有新连接到达。

通常不会仅仅只监听一个连接,在while循环中调用 accept()方法. 如下面的例子：

```
while (true) {
    SocketChannel socketChannel = channel.accept();
    // do something
}
```

当然,也可以在while循环中使用除了true以外的其它退出准则。

#### 非阻塞模式

ServerSocketChannel可以设置成非阻塞模式。在非阻塞模式下，accept() 方法会立刻返回，如果还没有新进来的连接,返回的将是null。 因此，需要检查返回的SocketChannel是否是null.如：

```
ServerSocketChannel channel = ServerSocketChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
channel.configureBlocking(false);
while (true) {
    SocketChannel socketChannel = channel.accept();
    if (socketChannel != null) {
        // do something
    }
}
```

### 十、Java NIO DatagramChannel

Java NIO中的DatagramChannel是一个能收发UDP包的通道。因为UDP是无连接的网络协议，所以不能像其它通道那样读取和写入。它发送和接收的是数据包。

#### 打开 DatagramChannel

下面是 DatagramChannel 的打开方式：

```java
DatagramChannel channel = DatagramChannel.open();
channel.socket().bind(new InetSocketAddress(9999));
```

这个例子打开的 DatagramChannel可以在UDP端口9999上接收数据包。

#### 接收数据

通过receive()方法从DatagramChannel接收数据，如：

```
ByteBuffer buf = ByteBuffer.allocate(1024);
buf.clear();
channel.receive(buf);
```

receive()方法会将接收到的数据包内容复制到指定的Buffer. 如果Buffer容不下收到的数据，多出的数据将被丢弃。

#### 发送数据

通过send()方法从DatagramChannel发送数据，如:

```java
String data = "hello world";
ByteBuffer buf = ByteBuffer.allocate(1024);
buf.clear();
buf.put(data.getBytes());
buf.flip();
int send = channel.send(buf, new InetSocketAddress("www.baidu.com", 80));
```

这个例子发送一串字符到”www.baidu.com”服务器的UDP端口80。 因为服务端并没有监控这个端口，所以什么也不会发生。也不会通知你发出的数据包是否已收到，因为UDP在数据传送方面没有任何保证。

#### 连接到特定的地址

可以将DatagramChannel“连接”到网络中的特定地址的。由于UDP是无连接的，连接到特定地址并不会像TCP通道那样创建一个真正的连接。而是锁住DatagramChannel ，让其只能从特定地址收发数据。

这里有个例子:

```
channel.connect(new InetSocketAddress("www.baidu.com", 80));
```

当连接后，也可以使用read()和write()方法，就像在用传统的通道一样。只是在数据传送方面没有任何保证。这里有几个例子：

```
int read = channel.read(buf);
int write = channel.write(buf);
```

### 十一、Pipe

Java NIO 管道是2个线程之间的单向数据连接。`Pipe`有一个source通道和一个sink通道。数据会被写到sink通道，从source通道读取。

这里是Pipe原理的图示：

​	缺图：

#### 创建管道

通过`Pipe.open()`方法打开管道。例如：

```java
Pipe pipe = Pipe.open();
```

#### 向管道写数据

要向管道写数据，需要访问sink通道。像这样：

```
Pipe.SinkChanel sinkChannel = pipe.sink();
```

通过调用SinkChannel的`write()`方法，将数据写入`SinkChannel`,像这样：

```java
String data = "hello world";
ByteBuffer buf = ByteBuffer.allocate(1024);
buf.clear();
buf.put(data.getBytes());
buf.flip();

while (buf.hasRemaining()) {
    sinkChannel.write(buf);
}
```

#### 从管道读取数据

从读取管道的数据，需要访问source通道，像这样：

```
Pipe.SourceChannel sourceChannel = pipe.source();
```

调用source通道的`read()`方法来读取数据，像这样：

```
ByteBuffer buf = ByteBuffer.allocate(1024);
int read = sourceChannel.read(buf);
```

`read()`方法返回的int值会告诉我们多少字节被读进了缓冲区。

### 十二、Java NIO与IO

当学习了Java NIO和IO的API后，一个问题马上涌入脑海：

我应该何时使用IO，何时使用NIO呢？在本文中，我会尽量清晰地解析Java NIO和IO的差异、它们的使用场景，以及它们如何影响您的代码设计。

#### Java NIO和IO的主要区别

下表总结了Java NIO和IO之间的主要差别，我会更详细地描述表中每部分的差异。

```
IO                NIO
面向流            面向缓冲
阻塞IO            非阻塞IO
无                选择器
```

#### 面向流与面向缓冲

ava NIO和IO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的。 Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。 Java NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。

#### 阻塞与非阻塞IO

Java IO的各种流是阻塞的。这意味着，当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。 Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取。而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞IO的空闲时间用于在其它通道上执行IO操作，所以一个单独的线程现在可以管理多个输入和输出通道（channel）。

#### 选择器（Selectors）

Java NIO的选择器允许一个单独的线程来监视多个输入通道，你可以注册多个通道使用一个选择器，然后使用一个单独的线程来“选择”通道：这些通道里已经有可以处理的输入，或者选择已准备写入的通道。这种选择机制，使得一个单独的线程很容易来管理多个通道。

#### NIO和IO如何影响应用程序的设计

无论您选择IO或NIO工具箱，可能会影响您应用程序设计的以下几个方面：

1.  对NIO或IO类的API调用。
2. 数据处理。
3. 用来处理数据的线程数。

##### 1）API调用

当然，使用NIO的API调用时看起来与使用IO时有所不同，但这并不意外，因为并不是仅从一个InputStream逐字节读取，而是数据必须先读入缓冲区再处理。

##### 2）数据处理

使用纯粹的NIO设计相较IO设计，数据处理也受到影响。

在IO设计中，我们从InputStream或 Reader逐字节读取数据。假设你正在处理一基于行的文本数据流,该文本行的流可以这样处理：

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(input));

String nameLine = reader.readLine();
String ageLine = reader.readLine();
String emailLine = reader.readLine();
String phoneLine = reader.readLine();
```

请注意处理状态由程序执行多久决定。换句话说，一旦reader.readLine()方法返回，你就知道肯定文本行就已读完， readline()阻塞直到整行读完，这就是原因。你也知道此行包含名称；同样，第二个readline()调用返回的时候，你知道这行包含年龄等。 正如你可以看到，该处理程序仅在有新数据读入时运行，并知道每步的数据是什么。一旦正在运行的线程已处理过读入的某些数据，该线程不会再回退数据（大多如此）。

而一个NIO的实现会有所不同，下面是一个简单的例子：

```java
ByteBuffer buf = ByteBuffer.allocate(1024);
int read = channel.read(buf);
```

注意第二行，从通道读取字节到ByteBuffer。当这个方法调用返回时，你不知道你所需的所有数据是否在缓冲区内。你所知道的是，该缓冲区包含一些字节，这使得处理有点困难。
假设第一次 read(buffer)调用后，读入缓冲区的数据只有半行，例如，“Name:An”，你能处理数据吗？显然不能，需要等待，直到整行数据读入缓存，在此之前，对数据的任何处理毫无意义。

所以，你怎么知道是否该缓冲区包含足够的数据可以处理呢？好了，你不知道。发现的方法只能查看缓冲区中的数据。其结果是，在你知道所有数据都在缓冲区里之前，你必须检查几次缓冲区的数据。这不仅效率低下，而且可以使程序设计方案杂乱不堪。例如：

```java
ByteBuffer buffer = ByteBuffer.allocate(1024);

int bytesRead = inChannel.read(buffer);

while (!bufferFull(bytesRead)) {

	bytesRead = inChannel.read(buffer);

}
```

bufferFull()方法必须跟踪有多少数据读入缓冲区，并返回真或假，这取决于缓冲区是否已满。换句话说，如果缓冲区准备好被处理，那么表示缓冲区满了。

bufferFull()方法扫描缓冲区，但必须保持在bufferFull（）方法被调用之前状态相同。如果没有，下一个读入缓冲区的数据可能无法读到正确的位置。这是不可能的，但却是需要注意的又一问题。

如果缓冲区已满，它可以被处理。如果它不满，并且在你的实际案例中有意义，你或许能处理其中的部分数据。但是许多情况下并非如此。

##### 3）用来处理数据的线程数

NIO可让您只使用一个（或几个）单线程管理多个通道（网络连接或文件），但付出的代价是解析数据可能会比从一个阻塞流中读取数据更复杂。

如果需要管理同时打开的成千上万个连接，这些连接每次只是发送少量的数据，例如聊天服务器，实现NIO的服务器可能是一个优势。同样，如果你需要维持许多打开的连接到其他计算机上，如P2P网络中，使用一个单独的线程来管理你所有出站连接，可能是一个优势。

如果你有少量的连接使用非常高的带宽，一次发送大量的数据，也许典型的IO服务器实现可能非常契合。

### 十三、Java NIO Path

Path接口是java NIO2的一部分。首次在java 7中引入。Path接口在java.nio.file包下，所以全称是java.nio.file.Path。 java中的Path表示文件系统的路径。可以指向文件或文件夹。也有相对路径和绝对路径之分。绝对路径表示从文件系统的根路径到文件或是文件夹的路径。相对路径表示从特定路径下访问指定文件或文件夹的路径。相对路径的概念可能有点迷糊。不用担心，我将在本文的后面详细介绍相关细节。

不要将文件系统的path和操作系统的环境变量path搞混淆。java.nio.file.Path接口和操作系统的path环境变量没有任何关系。

在很多方面，java.nio.file.Path接口和[java.io.File](http://tutorials.jenkov.com/java-io/file.html)有相似性，但也有一些细微的差别。在很多情况下，可以用Path来代替File类。

#### 创建Path实例

为了使用java.nio.file.Path实例，必须首先创建它。可以使用Paths 类的静态方法Paths.get()来产生一个实例。以下是示例：

```java
import java.nio.file.Path;
import java.nio.file.Paths;

class Main {
    public static void main(String[] args) {
        Path path = Paths.get("c:/a.txt");
    }
}
```

请注意例子开头的两个import语句。想要使用Paths类和Path接口，必须首先引入相应包。其次，注意Paths.get("c:/a.txt")的用法。其使用了Paths.get方法创建了Path的实例。它是一个工厂方法。

#### 创建绝对路径Path

调用传入绝对路径当做参数的Paths.get()工厂方法，就可以生成绝对路径Path。示例如下：

```
Path path = Paths.get("c:\\data\\myfile.txt");
```

示例中的绝对路径是c:\data\myfile.txt。有两个\字符的原因是第一个\是转义字符，表示紧跟着它的字符需要被转义。\\表示需要向字符串中写入一个\字符。

上文示例的path是windows下的路径。在Unix系统(Linux,MacOS,FreeBSD等)中，上文中的path是这样的：

```
Path path = Paths.get("/home/jakobjenkov/myfile.txt");
```

/home/jakobjenkov/myfile.txt就称作绝对路径。

如果把以/开头path的格式运行在windows系统中，系统会将其解析为相对路径。例如：

```
/home/jakobjenkov/myfile.txt
```

将会被解析为路径是在C盘。对应的绝对路径是：

C:/home/jakobjenkov/myfile.txt

#### 创建相对路径Path

相对路径指从一个已确定的路径开始到某一文件或文件夹的路径。将确定路径和相对路径拼接起来就是相对路径对应的绝对路径地址。

java NIO Path类也能使用相对路径。可以通过Paths.get(basePath, relativePath)创建一个相对路径Path。示例如下：

```java
Path projects = Paths.get("d:\\data", "projects");
Path file = Paths.get("d:\\data", "projects\\a-project\\myfile.txt");
```

第一个例子创建了一个指向d:\data\projects文件夹的实例。第二个例子创建了一个指向 d:\data\projects\a-project\myfile.txt 文件的实例。

当使用相对路径的时候，可以使用如下两种特别的符号。它们是：**.和..**

.表示当前路径。例如，如果以如下方式创建一个相对路径：

```
Path currentDir = Paths.get(".");
System.out.println(currentDir.toAbsolutePath());
```

创建的Path实例对应的路径就是运行这段代码的项目工程目录。

如果.用在路径中，则其表示的就是当前路径下。示例：

Path currentDir = Paths.get(“d:\\data\\projects\.\a-project”);
对应的就是如下路径
d:\data\projects\a-project

..表示父类目录。示例：

```
Path parentDir = Paths.get(“..”);
```

Path对应的路径是当前运行程序目录的上级目录。

如果在path中使用..，表示上级目录的含义。例如：

```
String path = "d:\\data\\projects\\a-project\\..\\another-project";
Path parentDir2 = Paths.get(path);
```

对应的绝对路径地址为：

d:\data\projects\another-project

在a-project目录后面的..符号，将指向的目录修改为projects目录，因此，最终path指向another-project目录。

.和..都可以在Paths.get()的双形参方法中使用。示例：

```
Path path1 = Paths.get("d:\\data\\projects", ".\\a-project");

Path path2 = Paths.get("d:\\data\\projects\\a-project", "..\\another-project");
```

下面介绍NIO 的Path类有关相对路径的其他使用方法。

#### Path.normalize()

Path 的normalize()方法可以标准化路径。标准化的含义是路径中的.和..都被去掉，指向真正的路径目录地址。下面是Path.normalize()示例：

```
String originalPath = "d:\\data\\projects\\a-project\\..\\another-project";

Path path1 = Paths.get(originalPath);
System.out.println("path1 = " + path1);

Path path2 = path1.normalize();
System.out.println("path2 = " + path2);
```

上文示例，首先创建了一个包含..字符的路径地址。之后输出此路径。

之后，调用normalize方法，返回一个新的path对象。输出新对象的路径。

输出结果如下：

```
path1 = d:\data\projects\a-project\..\another-project
path2 = d:\data\projects\another-project
```

如你所见，标准化后的路径不再包含 a-project\..部分，因为它是多余的。