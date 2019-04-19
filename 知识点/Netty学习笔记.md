#一.简介
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Netty%E8%AF%A6%E8%A7%A3.png)
##1.1.Netty是一款异步的事件驱动的网络应用程序框架，支持快速的开发可维护的高性能的面向协议的服务器和客户端
##1.2.核心组件：Channel、回调、Future、事件和ChannelHandler
###1.2.1.Channel：代表一个实体（如一个硬件设备、一个文件、一个网络套接字或者一个能够执行一个或多个不同的I/O操作的程序组件）的开放连接，如读/写操作
###1.2.2.回调：一个方法、一个指向已经被提供给另外一个方法的方法的引用
###1.2.3.Future：提供另一种在操作完成是通知应用程序的方法；可以看作一个异步操作的结果的占位符
###1.2.4.事件与ChannelHandler(记录日志；数据转换；流控制；应用程序逻辑)
##1.3.Reactor模型
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Reactor%E6%A8%A1%E5%9E%8B.png)
#二.基本流程
##2.1.服务器编写
    package com.gerry.netty.server;

	import io.netty.bootstrap.ServerBootstrap;
	import io.netty.channel.ChannelFuture;
	import io.netty.channel.ChannelInitializer;
	import io.netty.channel.EventLoopGroup;
	import io.netty.channel.nio.NioEventLoopGroup;
	import io.netty.channel.socket.SocketChannel;
	import io.netty.channel.socket.nio.NioServerSocketChannel;

	public class EchoServer {
    	private final int port;

   	 	public EchoServer(int port) {
        	this.port = port;
    	}

    	public void start() throws Exception {
       	 EventLoopGroup group = new NioEventLoopGroup();
        	try {
           	 ServerBootstrap sb = new ServerBootstrap();
            	sb.group(group) // 绑定线程池
                    .channel(NioServerSocketChannel.class) // 指定使用的channel
                    .localAddress(this.port)// 绑定监听端口
                    .childHandler(new ChannelInitializer<SocketChannel>() { // 绑定客户端连接时候触发操作

                                @Override
                                protected void initChannel(SocketChannel ch) throws Exception {
                                    System.out.println("connected...; Client:" + ch.remoteAddress());
                                    ch.pipeline().addLast(new EchoServerHandler()); // 客户端触发操作
                                }
                            });
            ChannelFuture cf = sb.bind().sync(); // 服务器异步创建绑定
            System.out.println(EchoServer.class + " started and listen on " + cf.channel().localAddress());
            cf.channel().closeFuture().sync(); // 关闭服务器通道
        } finally {
            group.shutdownGracefully().sync(); // 释放线程池资源
        }
    }

    	public static void main(String[] args) throws Exception {
       	 	new EchoServer(65535).start(); // 启动
   	 	}
	}
##2.2.服务器处理程序编写
    package com.gerry.netty.server;

	import io.netty.buffer.Unpooled;
	import io.netty.channel.ChannelFutureListener;
	import io.netty.channel.ChannelHandlerContext;
	import io.netty.channel.ChannelInboundHandlerAdapter;

	public class EchoServerHandler extends ChannelInboundHandlerAdapter {
    	@Override
    	public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
    	    System.out.println("server channelRead...; received:" + msg);
    	    ctx.write(msg);
    	}

    	@Override
    	public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
    	    System.out.println("server channelReadComplete..");
    	    // 第一种方法：写一个空的buf，并刷新写出区域。完成后关闭sock channel连接。
    	    ctx.writeAndFlush(Unpooled.EMPTY_BUFFER).addListener(ChannelFutureListener.CLOSE);
    	    //ctx.flush(); // 第二种方法：在client端关闭channel连接，这样的话，会触发两次channelReadComplete方法。
    	    //ctx.flush().close().sync(); // 第三种：改成这种写法也可以，但是这中写法，没有第一种方法的好。
    	}

    	@Override
    	public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
    	    System.out.println("server occur exception:" + cause.getMessage());
    	    cause.printStackTrace();
    	    ctx.close(); // 关闭发生异常的连接
    	}
	}
##2.3.客户端程序编写
	import io.netty.bootstrap.Bootstrap;
	import io.netty.channel.ChannelFuture;
	import io.netty.channel.ChannelInitializer;
	import io.netty.channel.EventLoopGroup;
	import io.netty.channel.nio.NioEventLoopGroup;
	import io.netty.channel.socket.SocketChannel;
	import io.netty.channel.socket.nio.NioSocketChannel;

	import java.net.InetSocketAddress;

	public class EchoClient {
    	private final String host;
    	private final int port;
	
    	public EchoClient() {
    	    this(0);
    	}

    	public EchoClient(int port) {
        	this("localhost", port);
    	}

    	public EchoClient(String host, int port) {
        	this.host = host;
        	this.port = port;
    	}

    	public void start() throws Exception {
        	EventLoopGroup group = new NioEventLoopGroup();
        	try {
        	    Bootstrap b = new Bootstrap();
        	    b.group(group) // 注册线程池
        	            .channel(NioSocketChannel.class) // 使用NioSocketChannel来作为连接用的channel类
        	            .remoteAddress(new InetSocketAddress(this.host, this.port)) // 绑定连接端口和host信息
        	            .handler(new ChannelInitializer<SocketChannel>() { // 绑定连接初始化器
        	                        @Override
        	                        protected void initChannel(SocketChannel ch) throws Exception {
        	                            System.out.println("connected...");
        	                            ch.pipeline().addLast(new EchoClientHandler());
        	                        }
        	                    });
        	    System.out.println("created..");
	
        	    ChannelFuture cf = b.connect().sync(); // 异步连接服务器
        	    System.out.println("connected..."); // 连接完成
	
        	    cf.channel().closeFuture().sync(); // 异步等待关闭连接channel
        	    System.out.println("closed.."); // 关闭完成
        	} finally {
        	    group.shutdownGracefully().sync(); // 释放线程池资源
        	}
    	}

    	public static void main(String[] args) throws Exception {
        	new EchoClient("127.0.0.1", 65535).start(); // 连接127.0.0.1/65535，并启动
    	}
	}
##2.4.客户端处理程序编写
	import java.nio.charset.Charset;
	
	import io.netty.buffer.ByteBuf;
	import io.netty.buffer.ByteBufUtil;
	import io.netty.buffer.Unpooled;
	import io.netty.channel.ChannelHandlerContext;
	import io.netty.channel.SimpleChannelInboundHandler;
	import io.netty.util.CharsetUtil;

	public class EchoClientHandler extends SimpleChannelInboundHandler<ByteBuf> {

    	@Override
    	public void channelActive(ChannelHandlerContext ctx) throws Exception {
    	    System.out.println("client channelActive..");
    	    ctx.writeAndFlush(Unpooled.copiedBuffer("Netty rocks!", CharsetUtil.UTF_8)); // 必须有flush
	
    	    // 必须存在flush
    	    // ctx.write(Unpooled.copiedBuffer("Netty rocks!", CharsetUtil.UTF_8));
    	    // ctx.flush();
    	}
	
    	@Override
    	protected void channelRead0(ChannelHandlerContext ctx, ByteBuf msg) throws Exception {
    	    System.out.println("client channelRead..");
    	    ByteBuf buf = msg.readBytes(msg.readableBytes());
    	    System.out.println("Client received:" + ByteBufUtil.hexDump(buf) + "; The value is:" + buf.toString	(Charset.forName("utf-8")));
    	    //ctx.channel().close().sync();// client关闭channel连接
    	}
	
    	@Override
    	public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
    	    cause.printStackTrace();
    	    ctx.close();
    	}

	}

#三.Netty的组件
##3.1.Channel、EventLoop、ChannelFuture
###3.1.1.Channel----Socket
###3.1.2.EventLoop---控制流、多线程处理、并发
###3.1.3.ChannelFuture---异步处理
- 三者之间的关系
	- 一个 EventLoopGroup 包含一个或者多个 EventLoop
	- 一个 EventLoop 在它的生命周期内只和一个 Thread 绑定
	- 所有由 EventLoop 处理的 I/O 事件都将在它专有的 Thread 上被处理
	- 一个 Channel 在它的生命周期内只注册于一个 EventLoop
	- 一个 EventLoop 可能会被分配给一个或多个 Channel

##3.2.ChannelHandler和ChannelPipeline
###3.2.1.ChannelHandler
###3.2.2.ChannelPipeline:提供了ChannelHandler联的容器，并定义了用于在该链上传播入站和出站事件流的API
##3.3.引导



#四.传输
> 流经网络的数据总是具有相同的类型：字节

- Channel详解
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/channel%E6%8E%A5%E5%8F%A3%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84.png)
##4.1.NIO:选择器：充当一个注册表，请求在Channel的状态发生改变时得到通知
- 新的Channel已被接收并且就绪
- Channel连接已经完成
- Channel有已经就绪的可供读取的数据
- Chnnel可用于写数据

> 零拷贝：只有使用NIO和Epoll传输时才可以使用的特性，可以快速高效的将数据从文件系统移动到网络接口，而不需要将其从内核空间复制到用户空间
##4.2.Epoll
##4.3.OIO
##4.4.用于 JVM 内部通信的 Local 传输
##4.5.Embedded 传输



#五.ByteBuf（Netty的数据容器）
- 可以被用户自定义的缓冲区类型扩展
- 通过内置的复合缓冲区类型实现透明的零拷贝
- 容量可以按需增长
- 在读和写两种模式之间切换不需要调用ByteBuffer的flip()方法
- 读和写使用不同的索引
- 支持方法的链式调用
- 支持引用计数
- 支持池化
##5.1.网络数据的基本单位总是字节
##5.2.工作模式：ByteBuf 维护了两个不同的索引：一个用于读取，一个用于写入。
##5.3.使用模式
- 堆缓冲区：存储在JVM的堆空间，没有使用池化的情况下提供快速的分配和释放；适合有遗留数据需要处理的情况
- 直接缓冲区：避免在每次调用本地IO操作前后将缓冲区的内容复制到一个中间缓冲区；相对于基于堆的缓冲区，分配和释放较为昂贵；因为数据不是在堆上，不得不进行一次复制
- 复合缓冲区
##5.4.字节级操作
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/ByteBuf%E5%86%85%E9%83%A8%E5%88%86%E6%AE%B5.png)
###5.4.1.随机访问索引
###5.4.2.顺序访问索引
###5.4.3.可丢弃字节
###5.4.4.可读字节
###5.4.5.可写字节
###5.4.6.索引管理
###5.4.7.查找操作
###5.4.8.派生缓冲区
###5.4.9.读/写操作
##5.5.ByteBufHolder:缓冲区池化
##5.6.ByteBuf分配
- 按需分配：ByteBufAllocator接口
- Unpooled缓冲区
- ByteBufUtil类

#六.ChannelPipeline、ChannelHandler、ChannelHandlerContext
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Channel%E3%80%81ChannelPipeline%E3%80%81ChannelHandler%E4%BB%A5%E5%8F%8AChannelHandlerContext%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB.png)


#七.EventLoop和线程模型
- EventLoop执行逻辑
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/EventLoop%E6%89%A7%E8%A1%8C%E9%80%BB%E8%BE%91.png)
> 永远不要将一个长时间运行的任务放到执行队列中，因为它将阻塞需要在同一个线程上执行的任何其他任务，如果必须要进行阻塞调用或者执行长时间运行的任务，建议使用一个专门的EventExecutor

- EventLoop分配方式

![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/EventLoop%E5%88%86%E9%85%8D%E6%96%B9%E5%BC%8F.png)



##7.1.池化和重用线程相对于简单地为每个任务都创建和销毁线程是一种进步，但是它并不能消除由上下文切换所带来的开销，其将随着线程数量的增加很快变得明显
##7.2.EventLoop：将由一个永远都不会改变的 Thread 驱动， 同时任务（Runnable 或者 Callable）可以直接提交给 EventLoop 实现，以立即执行或者调度执行；单个EventLoop 可能会被指派用于服务多个 Channel
##7.3.线程模型Reactor




#八.引导
> 编写Netty应用程序的一个一般原则：仅可能的重用EventLoop,以减少线程创建带来的开销


#九.编解码器
##9.1.将字节解码为消息——ByteToMessageDecoder 和 ReplayingDecoder；
##9.2.将一种消息类型解码为另一种——MessageToMessageDecoder
##9.3.Netty需要在字节可以编码之前在内存中缓冲它们，因此，不能让解码器缓冲大量的数据以至于耗尽可用的内存。

#补充知识点
##1.Netty多线程编程问题
###1.1.创建两个NioEventLoopGroup,用于逻辑隔离NIO Acceptor和NIO I/O线程
###1.2.尽量不要在ChannelHandler中启动用户线程(解码后用于将POJO消息派发到后端业务线程的除外)
###1.3.解码要放在NIO线程调用的解码Handler中进行,不要切换到用户线程完成消息的解码
###1.4.如果业务逻辑操作非常简单(纯内存操作),没有复杂的业务逻辑计算,也可能会导致线程被阻塞的磁盘操作,数据库操作,网络操作等,可以直接在NIO线程上完成业务逻辑编排,不需要切换到用户线程.
###1.5.如果业务逻辑复杂,不要在NIO线程上完成,建议将解码后的POJO消息封装成任务,派发到业务线程池中由业务线程执行,以保证NIO线程尽快释放,处理其它I/O操作.
###1.6.可能导致阻塞的操作,数据库操作,第三方服务调用,中间件服务调用,同步获取锁,Sleep等
###1.7.Sharable注解的ChannelHandler要慎用
###1.8.避免将ChannelHandler加入到不同的ChannelPipeline中,会出现并发问题.


#2.ByteBuf的零拷贝[https://www.cnblogs.com/xys1228/p/6088805.html](https://www.cnblogs.com/xys1228/p/6088805.html)