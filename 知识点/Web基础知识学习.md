#一.TCP三次握手与四次挥手[https://blog.csdn.net/whuslei/article/details/6667471/](https://blog.csdn.net/whuslei/article/details/6667471/)






#一.Json
##1.JavaScript对象表示法，是存储和交换文本信息的语法。
##2.Json语法
###2.1.数据在名称/值对中：名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值
###2.2.数据由逗号分隔
###2.3.花括号保存对象
###2.4.方括号保存数组


#二. MIME 类型
##1.描述消息内容类型因特网标准，主要包含文本、图像、音频、视频以及其他应用程序专用的数据


#三.HTML
##1.用来描述网页的一种语言(超文本标记语言)


#四.XML
##1.可扩展标记语言，被设计用来传输和存储数据

#五.Servlet
##1.Servlet与Servlet容器
##2.驻留在 Web 服务器上，处理新来的请求和输出的响应。它与表示无关
##3.一个 Web 应用对应一个 Context 容器，也就是 Servlet 运行时的 Servlet 容器，添加一个 Web 应用时将会创建一个 StandardContext 容器，并且给这个 Context 容器设置必要的参数，url 和 path 分别代表这个应用在 Tomcat 中的访问路径和这个应用实际的物理路径
##4.Servlet的生命周期：1.Servlet 通过调用 init () 方法进行初始化；2.Servlet 调用 service() 方法来处理客户端的请求；3.Servlet 通过调用 destroy() 方法终止（结束）；4.Servlet 是由 JVM 的垃圾回收器进行垃圾回收的
##5.Servlet方法讨论：
###5.1.init() 方法：init 方法被设计成只调用一次，init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期。
###5.2.service() 方法：service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务。
###5.3.doGet() 方法：GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理。
###5.4.doPost() 方法：POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理。
###5.5.destroy() 方法：destroy() 方法只会被调用一次，在 Servlet 生命周期结束时被调用。destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动。
##6.过滤器
###6.1.过滤器作用位置：当浏览器发送请求给服务器的时候，先执行过滤器，然后才访问Web的资源。服务器响应Response，从Web资源抵达浏览器之前，也会途径过滤器。
##7.监听器
###7.1.定义：监听器就是一个实现特定接口的普通java程序，这个程序专门用于监听另一个java对象的方法调用或属性改变，当被监听对象发生上述事件后，监听器某个方法将立即被执行
###7.2.组件：事件源（拥有事件），事件对象（封装了事件源对象），事件监听器（监听事件源所拥有的事件）












#六.Jsp
##1.Java服务器页面
##2.JSP全称Java Server Pages，是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。标签通常以<%开头以%>结束。
##3.Jsp的生命周期
###3.1.编译阶段：servlet容器编译servlet源文件，生成servlet类
###3.2.初始化阶段：加载与JSP对应的servlet类，创建其实例，并调用它的初始化方法
###3.3.执行阶段：调用与JSP对应的servlet实例的服务方法
###3.4.销毁阶段：调用与JSP对应的servlet实例的销毁方法，然后销毁servlet实例
##4.JSP
###4.1.语法脚本程序：<% 代码片段 %>
###4.2.JSP声明：<%! declaration; [ declaration; ]+ ... %>
###4.3.JSP表达式：<%= 表达式 %>
###4.4.JSP注释：<%-- 这里可以填写 JSP 注释 --%>
###4.5.JSP指令：<%@ directive attribute="value" %>
####4.5.1.<%@ page ... %>：定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等
####4.5.2.<%@ include ... %>：包含其他文件
####4.5.3.<%@ taglib ... %>：引入标签库的定义，可以是自定义标签
###4.6.JSP行为:<jsp:action_name attribute="value" />
###4.7.JSP隐含对象
###4.8.JSP运算符
###4.9.JSP常量
##5.Jsp运行过程
###1.客户端发出请求；2.Jsp文件转换Servlet文件；3.Servlet文件编译.class文件；4..class文件执行Servlet实例；5.Servlet实例返回响应给客户端

##6.JSP和servlet有一点区别就在于：jsp是先部署后编译，而servlet是先编译后部署。
##7.JSP的四大作用域：page,request,session,qpplication
###7.1.page作用域：代表变量只能在当前页面上生效
###7.2.request作用域：代表变量能在一次请求中生效，一次请求可能包含一个页面，也可能包含多个页面，比如页面A请求转发给页面B
###7.3.session:代表变量能在一次会话中生效，基本就是能在Web项目下都有效，session的使用也和cookie有关。一般来说，只要浏览器不关闭，cookie就会一直生效，cookie生效，session的使用就不会受到影响。
###7.4.application:代表变量能在一个应用下（多个会话），在服务器下的多个项目之间都能够使用。

##8.JSP的九大对象
###8.1.request对象：代表的是来自客户端的请求，例如我们在FORM表单中填写的信息等。是最常用的对象。客户端的请求信息被封装在request对象中，通过它了解客户的需求，然后做出响应。作用域    Request
###8.2.response对象：对象代表的是客户端的响应，也就是说可以通过response对象来组织发送到客户端的数据。作用域是	page。
###8.3.session对象：客户端与服务器间的一次会话，从客户连到服务器的一个WebApplication开始，直到客户端与服务器端断开连接为止。作用域    Session
###8.4.out对象：out对象是JspWriter类的实例，是向客户端输出内容常用的对象。作用域为	page.
###8.5.page对象：page对象就是指向当前JSP页面本身，有点象类中的this指针。作用域    Page
###8.6.application对象：实现了用户间数据的共享，可以存放全局变量。开始于服务器的启动，直到服务器的关闭，在此期间，对象一直存在。作用域    Application
###8.7.pageContext对象：提供了对JSP页面内所有的对象及名字空间的访问，他可以访问到本页面所有在Session,也可以取本页面所在的application的某一个属性，相当于页面所有功能的集大成者。 作用域    Page
###8.8.config对象：config对象是在一个Servlet初始化时，JSP引擎向它传递信息用的，此信息包括Servlet初始化时所要用到的参数（通过属性名和属性值构成）以及服务器的有关信息（通过传递一个ServletContext对象）。作用域    Page
###8.9.exception对象：是一个例外对象，当一个页面在运行过程中发生了例外，就产生这个对象。如果一个JSP页面要应用此对象，就必须把isErrorPage设为true，否则无法编译。作用域    Page



#七.CS/BS

#八.Cookie与session
##8.1.Cookie通过在客户端记录信息确定用户身份，Session通过在服务器端记录信息确定用户身份。
##8.2.Cookie:给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。
##8.3.Cookie具有不可跨域名性。
##8.4.Cookie的有效期:如果maxAge属性为正数，则表示该Cookie会在maxAge秒之后自动失效。如果maxAge为负数，则表示该Cookie仅在本浏览器窗口以及本窗口打开的子窗口内有效，关闭窗口后该 Cookie即失效。
##8.5.Session是服务器端使用的一种记录客户端状态的机制，使用上比Cookie简单一些，相应的也增加了服务器的存储压力。
##8.6.Session的生命周期:为了获得更高的存取速度，服务器一般把Session放在内存里。每个用户都会有一个独立的Session。Session在用户第一次访问服务器的时候自动创建。Session生成后，只要用户继续访问，服务器就会更新Session的最后访问时间，并维护该Session。
##8.7.Session的有效期：为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。这个时间就是Session的超时时间。

#九.Jdbc
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/JDBC%E6%B5%81%E7%A8%8B.png)
##9.1.JDBC代表Java数据库连接
##9.2.JDBC体系结构:1.JDBC API：提供应用程序到JDBC管理器连接;2.JDBC驱动程序API：支持JDBC管理器到驱动程序连接。
##9.3.JDBC API常见接口与类：
###9.3.1.DriverManager: 这个类管理数据库驱动程序的列表
###9.3.2.Driver: 此接口处理与数据库服务器通信。很少直接直接使用驱动程序（Driver）对象，一般使用DriverManager中的对象，它用于管理此类型的对象。
###9.3.3.Connection : 此接口与接触数据库的所有方法。连接对象表示通信上下文，即，与数据库中的所有的通信是通过此唯一的连接对象。
###9.3.4.Statement : 可以使用这个接口创建的对象的SQL语句提交到数据库。一些派生的接口接受除执行存储过程的参数。
###9.3.5.ResultSet: 这些对象保存从数据库后，执行使用Statement对象的SQL查询中检索数据。它作为一个迭代器，可以通过移动它来检索下一个数据。
###9.3.6.SQLException: 这个类用于处理发生在数据库应用程序中的任何错误
##9.4.创建JDBC应用程序的步骤
###9.4.1.导入包：需要包含包含数据库编程所需的JDBC类的包。 大多数情况下，使用import java.sql.*就足够了。
###9.4.2.注册JDBC驱动程序：需要初始化驱动程序，以便可以打开与数据库的通信通道。
###9.4.3.打开一个连接：需要使用DriverManager.get                                                                                                                                                                                                                                                                                                                       Connection()方法创建一个Connection对象，它表示与数据库的物理连接。
###9.4.4.执行查询：需要使用类型为Statement的对象来构建和提交SQL语句到数据库。
###9.4.5.从结果集中提取数据：需要使用相应的ResultSet.getXXX()方法从结果集中检索数据。
###9.4.6.清理环境：需要明确地关闭所有数据库资源，而不依赖于JVM的垃圾收集。
##9.5.与数据库的交互：
###9.5.1.Statement:用于对数据库进行通用访问，在运行时使用静态SQL语句时很有用。 Statement接口不能接受参数。
###9.5.2.PreparedStatement:当计划要多次使用SQL语句时使用。PreparedStatement接口在运行时接受输入参数。
###9.5.3.CallableStatement:当想要访问数据库存储过程时使用。CallableStatement接口也可以接受运行时输入参数。
##9.6.JDBC的事务以及保存点
##9.7.JDBC的批处理
##9.7.0.方式一：使用Statement对象进行批处理
###9.7.1.使用createStatement()方法创建Statement对象。
###9.7.2.使用setAutoCommit()将自动提交设置为false。
###9.7.3.使用addBatch()方法在创建的Statement对象上添加SQL语句到批处理中。
###9.7.4.在创建的Statement对象上使用executeBatch()方法执行所有SQL语句。
###9.7.5.最后，使用commit()方法提交所有更改。
##9.7.0.方式二：使用PrepareStatement对象进行批处理
###9.7.1.使用占位符创建SQL语句。
###9.7.2.使用prepareStatement()方法创建PrepareStatement对象。
###9.7.3.使用setAutoCommit()将自动提交设置为false。
###9.7.4.使用addBatch()方法在创建的Statement对象上添加SQL语句到批处理中。
###9.7.5.在创建的Statement对象上使用executeBatch()方法执行所有SQL语句。
###9.7.6.最后，使用commit()方法提交所有更改
##9.8.应用与数据库间的timeout层级
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Timeout%20for%20Each%20Levels..png)
###9.8.1.transaction timeout:“statement Timeout * N（需要执行的statement数量） + @（垃圾回收等其他时间）”。transaction timeout用来限制执行statement的总时长。
###9.8.2.Statement Timeout:用来限制statement的执行时长，timeout的值通过调用JDBC的java.sql.Statement.setQueryTimeout(int timeout) API进行设置。不过现在开发者已经很少直接在代码中设置，而多是通过框架来进行设置。
###9.8.3.为了避免dead connections，socket必须要有超时配置。socket timeout可以通过JDBC设置，socket timeout能够避免应用在发生网络错误时产生无休止等待的情况，缩短服务失效的时间。
####9.8.3.1.socket连接时的timeout：通过Socket.connect(SocketAddress endpoint, int timeout)设置
####9.8.3.2.socket读写时的timeout：通过Socket.setSoTimeout(int timeout)设置

##9.9.超时设置常见问题
###9.9.1.我已经使用Statement.setQueryTimeout()方法设置了查询超时，但在网络出错时并没有产生作用。
###9.9.1.答：查询超时仅在socket timeout生效的前提下才有效，它并不能用来解决外部的网络错误，要解决这种问题，必须设置JDBC的socket timeout
###9.9.2. transaction timeout，statement timeout和socket timeout和DBCP的配置有什么关系
###9.9.2.答：当通过DBCP获取数据库连接时，除了DBCP获取连接时的waitTimeout配置以外，其他配置对JDBC没有什么影响
###9.9.3.如果设置了JDBC的socket timeout，那DBCP连接池中处于IDLE状态的连接是否也会在达到超时时间后被关闭
###9.9.3.答： 不会。socket的设置只会在产生数据读写时生效，而不会对DBCP中的IDLE连接产生影响。当DBCP中发生新连接创建，老的IDLE连接被移除，或是连接有效性校验的时候，socket设置会对其产生一定的影响，但除非发生网络问题，否则影响很小。
###9.9.4.socket timeout应该设置为多少？
###9.9.4.答：socket timeout必须高于statement timeout，但并没有什么推荐值。在发生网络错误的时候，socket timeout将会生效，但是再小心的配置也无法避免网络错误的发生，只是在网络错误发生后缩短服务失效的时间（如果网络恢复正常的话）

##9.10.Java连接数据库有那几步
###9.10.1.注册驱动类
###9.10.2.新建数据库连接
###9.10.3.新建语句（statement）
###9.10.4.查询
###9.10.5.关闭连接

#十.基础知识
##10.1.粘包与拆包
###10.1.1.应用程序write写入的字节大小大于套接口缓冲区的大小
###10.1.2.进行MSS大小的TCP分段
###10.1.3.以太网帧的payload大于MTU进行IP分片
##10.2.解决方案
###10.2.1.消息定长，例如每个报文固定200字节，如果不够，空位补空格
###10.2.2.在包尾增加回车换行符进行分割，如FTP协议
###10.2.3.将消息分为消息头和消息体，消息头中包含消息的长度，字段等信息
###10.2.4.更复杂的应用层协议

#十一.HTTP
##11.1.Http协议的变化
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/HTTP%E5%8D%8F%E8%AE%AE.png)
###11.1.1.HTTP/1.x 有连接无法复用、队头阻塞、协议开销大和安全因素等多个缺陷
###11.1.2.HTTP/2 通过多路复用、二进制流、Header 压缩等等技术，极大地提高了性能，但是还是存在着问题的
###11.1.3.QUIC 基于 UDP 实现，是 HTTP/3 中的底层支撑协议，该协议基于 UDP，又取了 TCP 中的精华，实现了即快又可靠的协议
##11.2.HTTP2.0[https://blog.csdn.net/zhuyiquan/article/details/69257126](https://blog.csdn.net/zhuyiquan/article/details/69257126)





#十二.WebService:Http+XML（[https://segmentfault.com/a/1190000013353808](https://segmentfault.com/a/1190000013353808)）
##12.1.XML：用于传输格式化的数据，是Web服务的基础。
##12.2.WSDL:Web服务描述语言
###12.2.1.通过XML形式说明服务在什么地方－地址。
###12.2.2.通过XML形式说明服务提供什么样的方法 – 如何调用。
##12.3.SOAP:简单对象访问协议
###12.3.1.SOAP作为一个基于XML语言的协议用于有网上传输数据。
###12.3.2.SOAP = 在HTTP的基础上+XML数据。
###12.3.3.SOAP的组成如下：
- Envelope – 必须的部分。以XML的根元素出现。
- Headers – 可选的。
- Body – 必须的。在body部分，包含要执行的服务器的方法。和发送到服务器的数据。

#十三.Tomcat(一个运行JAVA的网络服务器)
##13.1.目录文件
###13.1.1. bin：启动和关闭tomcat的bat文件
###13.1.2.conf:配置文件
- server.xml该文件用于配置server相关的信息，比如tomcat启动的端口号，配置主机(Host)
- web.xml文件配置与web应用（web应用相当于一个web站点）
- tomcat-user.xml配置用户名密码和相关权限.
###13.1.3.lib：该目录放置运行tomcat运行需要的jar包
###13.1.4.logs：存放日志，当我们需要查看日志的时候，可以查询信息
###13.1.5.webapps：放置我们的web应用
###13.1.6.work工作目录：该目录用于存放jsp被访问后生成对应的server文件和.class文件
##13.2.web站点的目录
![](https://github.com/HelloWucq/working-knowledge-point/blob/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Web%E7%AB%99%E7%82%B9%E7%9B%AE%E5%BD%95.png)


#十四.web之I/O模型
##14.1.系统调用：进程想获取磁盘中的数据，而能和硬件答交道的只能是内核，进程通知内核说要磁盘中的数据，此过程成为系统调用
##14.2.I/O
1. 磁盘把数据装载到内核的内存空间
2. 内核的内存空间的数据copy到用户的内存空间中(此过程是I/O发生的地方)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E8%BF%9B%E7%A8%8B%E8%8E%B7%E5%8F%96%E6%95%B0%E6%8D%AE%E7%9A%84%E8%AF%A6%E7%BB%86%E5%9B%BE%E8%A7%A3%E8%BF%87%E7%A8%8B.png)

###14.2.1.阻塞IO图解
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E9%98%BB%E5%A1%9EIO%E8%BF%87%E7%A8%8B.gif)
###14.2.2.非阻塞IO图解
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E9%9D%9E%E9%98%BB%E5%A1%9EIO.gif)
###14.2.3.IO复用图解
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/IO%E5%A4%8D%E7%94%A8.gif)
###14.2.4.事件驱动IO图解
- 水平触发的事件驱动机制；内核通知进程来读取数据，进程没来读取数据，内核需要一次一次的通知进程
- 边缘触发的事件驱动机制；内核只通知一次让进程来读取数据，进程可以在超时时间之内随时来读取数据
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E4%BA%8B%E4%BB%B6%E9%A9%B1%E5%8A%A8IO.gif)

###14.2.5.异步IO图解
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E5%BC%82%E6%AD%A5IO.gif)
#十五.web工作模式
##15.1.Prefork工作原理
- 主进程生成多个工作进程，由工作进程一对一的去响应客户端的请求
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/prefork.gif)
##15.2.Worker工作原理
- 主进程生成多个工作进程，每个工作进程生成一个多个线程，每个线程去响应客户端的请求

![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Worker%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.gif)
##15.3.Event工作原理
- 主进程生成多个工作进程，每个工程进程响应多个客户端的请求，当接收到客户端的I/O操作请求后，把I/O操作交给内核执行，进程去响应其他客户端的请求，此进程最后接到内核的通知，然后通过此进程回复客户端的请求结果，通过事件回调函数
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Event%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F.gif)

#十六.select/poll/epoll



#十七.服务器设计模型[https://blog.csdn.net/u013074465/article/details/46276967](https://blog.csdn.net/u013074465/article/details/46276967)
##17.1.Reactor
##17.2.Proactor


#十.补充知识
##10.1.select/epoll模型
##10.2.长连接：


