#一.进程模型
##1.1.nginx使用一个多进程模型来对外提供服务，其中一个master进程，多个worker进程。master进程负责管理nginx本身和其他worker进程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/nginx%E8%BF%9B%E7%A8%8B%E6%A8%A1%E5%9E%8B.png)
##1.2.特点
###1.2.1.对于每个worker进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，同时在编程以及问题查找时，也会方便很多
###1.2.2.采用独立的进程，可以让互相之间不会影响，一个进程退出后，其它进程还在工作，服务不会中断，master进程则很快启动新的worker进程。当然，worker进程的异常退出，肯定是程序有bug了，异常退出，会导致当前worker上的所有请求失败，不过不会影响到所有请求，所以降低了风险



#二.事件模型
##2.1.对于一个基本的web服务器来说，事件通常有三种类型，网络事件、信号、定时器。
###2.1.1.网络事件：异步非阻塞
###2.1.2.信号处理：如果nginx正在等待事件（epoll_wait时），如果程序收到信号，在信号处理函数处理完后，epoll_wait会返回错误，然后程序可再次进入epoll_wait调用
###2.1.3.定时器：nginx里面的定时器事件是放在一颗维护定时器的红黑树里面


#Nginx配置文件的整体结构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Nginx%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%B4%E4%BD%93%E7%BB%93%E6%9E%84.png)

##3.1.全局块
###3.1.1.配置运行Nginx服务器用户（组）
###3.1.2.worker process数
###3.1.3.Nginx进程PID存放路径
###3.1.4.错误日志的存放路径
###3.1.5.配置文件的引入
##3.2.events块
###3.2.1.设置网络连接的序列化
###3.2.2.是否允许同时接收多个网络连接
###3.2.3.事件驱动模型的选择
###3.2.4.最大连接数的配置
##3.3.http块
###3.3.1.定义MIMI-Type（网络资源的媒体类型，也即前端请求的资源类型）
###3.3.2.自定义服务日志
###3.3.3.允许sendfile方式传输文件
###3.3.4.连接超时时间
###3.3.5.单连接请求数上限
##3.4.server块
###3.4.1.配置网络监听
###3.4.2.基于名称的虚拟主机配置
###3.4.3.基于IP的虚拟主机配置
##3.5.location块
###3.5.1.location配置
###3.5.2.请求根目录配置
###3.5.3.更改location的URI
###3.5.4.网站默认首页配置





#三.基础概念
##3.1.connection:在nginx中connection就是对tcp连接的封装，其中包括连接的socket，读事件，写事件
###3.1.1.处理连接过程：1）nginx在启动时，会解析配置文件，得到需要监听的端口与ip地址，然后在nginx的master进程里面，先初始化好这个监控的socket(创建socket，设置addrreuse等选项，绑定到指定的ip地址端口，再listen)；2）fork出多个子进程出来，然后子进程会竞争accept新的连接。此时，客户端就可以向nginx发起连接了。当客户端与服务端通过三次握手建立好一个连接后，nginx的某一个子进程会accept成功，得到这个建立好的连接的socket，然后创建nginx对连接的封装，即ngx_connection_t结构体；3）设置读写事件处理函数并添加读写事件来与客户端进行数据的交换；4）nginx或客户端来主动关掉连接，到此，一个连接就寿终正寝了



##3.2.request:在nginx中我们指的是http请求，具体到nginx中的数据结构是ngx_http_request_t。ngx_http_request_t是对一个http请求的封装。 我们知道，一个http请求，包含请求行、请求头、请求体、响应行、响应头、响应体
###3.2.1.处理请求的过程：
##3.3.keepalive:一般来说，当客户端的一次访问，需要多次访问同一个server时，打开keepalive的优势非常大，比如图片服务器，通常一个网页会包含很多个图片。打开keepalive也会大量减少time-wait的数量。


##3.4.pipe:对pipeline来说，客户端不必等到第一个请求处理完后，就可以马上发起第二个请求。得到两个响应的时间可能能够达到1*RTT。

##3.5.lingering_close(延迟关闭):


#四.数据结构
##4.1.ngx_str_t（字符串）
    typedef struct {
    	size_t      len;
    	u_char     *data;
	} ngx_str_t;
##4.2.ngx_pool_t（池化技术）：提供一种机制，帮助管理一系列的资源（内存、文件等），使得对这些资源的使用和释放统一进行，免除了使用过程中考虑到对各种各样资源的什么时候释放，是否遗漏了释放的担心
    typedef struct ngx_pool_s        ngx_pool_t;

	struct ngx_pool_s {
   		ngx_pool_data_t       d;
    	size_t                max;
    	ngx_pool_t           *current;
    	ngx_chain_t          *chain;
    	ngx_pool_large_t     *large;
    	ngx_pool_cleanup_t   *cleanup;
    	ngx_log_t            *log;
	};
##4.3.ngx_array_t（数组结构）
	typedef struct ngx_array_s       ngx_array_t;
	struct ngx_array_s {
    	void        *elts;    //指向实际的数据存储区域
    	ngx_uint_t   nelts;    //数组实际元素个数
    	size_t       size;     //数组单个元素的大小，单位是字节
    	ngx_uint_t   nalloc;    //数组的容量。表示该数组在不引发扩容的前提下，可以最多存储的元素的个数
    	ngx_pool_t  *pool;     //该数组用来分配内存的内存池
	};
###注意事项：由于使用ngx_palloc分配内存，数组在扩容时，旧的内存不会被释放，会造成内存的浪费。因此，最好能提前规划好数组的容量，在创建或者初始化的时候一次搞定，避免多次扩容，造成内存浪费
##4.4.ngx_hash_t（hash表）：开链法解决冲突
###4.4.1.ngx_hash_t不像其他的hash表的实现，可以插入删除元素，它只能一次初始化，就构建起整个hash表以后，既不能再删除，也不能在插入元素了
###4.4.2.ngx_hash_t的开链并不是真的开了一个链表，实际上是开了一段连续的存储空间，几乎可以看做是一个数组。这是因为ngx_hash_t在初始化的时候，会经历一次预计算的过程，提前把每个桶里面会有多少元素放进去给计算出来，这样就提前知道每个桶的大小了。那么就不需要使用链表，一段连续的存储空间就足够了。这也从一定程度上节省了内存的使用
    typedef struct {
   		ngx_hash_t       *hash;  //该字段如果为NULL，那么调用完初始化函数后，该字段指向新创建出来的hash表。如果该字段不为NULL，那么在初始的时候，所有的数据被插入了这个字段所指的hash表中
    	ngx_hash_key_pt   key;   //指向从字符串生成hash值的hash函数

    	ngx_uint_t        max_size;  //hash表中的桶的个数
    	ngx_uint_t        bucket_size;  //每个桶的最大限制大小，单位是字节

    	char             *name; //该hash表的名字
    	ngx_pool_t       *pool;  //该hash表分配内存使用的pool
    	ngx_pool_t       *temp_pool; //该hash表使用的临时pool，在初始化完成以后，该pool可以被释放和销毁掉
	} ngx_hash_init_t;

	typedef struct {
    	ngx_str_t         key;
    	ngx_uint_t        key_hash;
    	void             *value;
	} ngx_hash_key_t;
##4.5.ngx_hash_wildcard_t:nginx为了处理带有通配符的域名的匹配问题，实现了ngx_hash_wildcard_t这样的hash表
##4.6.ngx_hash_combined_t:组合类型hash表
    typedef struct {
	    ngx_hash_t            hash;
	    ngx_hash_wildcard_t  *wc_head;
	    ngx_hash_wildcard_t  *wc_tail;
	} ngx_hash_combined_t;
###4.6.1.该类型实际上包含了三个hash表，一个普通hash表，一个包含前向通配符的hash表和一个包含后向通配符的hash表
##4.7.ngx_hash_keys_arrays_t:
    typedef struct {
    	ngx_uint_t        hsize;   //将要构建的hash表的桶的个数。对于使用这个结构中包含的信息构建的三种类型的hash表都会使用此参数
	
    	ngx_pool_t       *pool;
    	ngx_pool_t       *temp_pool;

    	ngx_array_t       keys;       //存放所有非通配符key的数组
    	ngx_array_t      *keys_hash; //这是个二维数组，第一个维度代表的是bucket的编号，那么keys_hash[i]中存放的是所有的key算出来的hash值对hsize取模以后的值为i的key

    	ngx_array_t       dns_wc_head;  //放前向通配符key被处理完成以后的值。比如：“*.abc.com” 被处理完成以后，变成 “com.abc.” 被存放在此数组中
    	ngx_array_t      *dns_wc_head_hash;

    	ngx_array_t       dns_wc_tail; //存放后向通配符key被处理完成以后的值
    	ngx_array_t      *dns_wc_tail_hash;
	} ngx_hash_keys_arrays_t;
##4.8.ngx_chain_t（链表）
    typedef struct ngx_chain_s       ngx_chain_t;

	struct ngx_chain_s {
    	ngx_buf_t    *buf;
    	ngx_chain_t  *next;
	};
###4.8.1.对ngx_chaint_t类型的释放，并不是真的释放了内存，而仅仅是把这个对象挂在了这个pool对象的一个叫做chain的字段对应的chain上，以供下次从这个pool上分配ngx_chain_t类型对象的时候，快速的从这个pool->chain上取下链首元素就返回了，当然，如果这个链是空的，才会真的在这个pool上使用ngx_palloc函数进行分配
##4.9.ngx_buf_t:是这个ngx_chain_t链表的每个节点的实际数据
    struct ngx_buf_s {
    	u_char          *pos; //当buf所指向的数据在内存里的时候，pos指向的是这段数据开始的位置
    	u_char          *last; //这段数据结束的位置
    	off_t            file_pos; //当buf所指向的数据是在文件里的时候，file_pos指向的是这段数据的开始位置在文件中的偏移量
    	off_t            file_last; //file_last指向的是这段数据的结束位置在文件中的偏移量

	    u_char          *start;         /* start of buffer */
	    u_char          *end;           /* end of buffer */
	    ngx_buf_tag_t    tag; //实际上是一个void*类型的指针，使用者可以关联任意的对象上去，只要对使用者有意义
	    ngx_file_t      *file; //当buf所包含的内容在文件中时，file字段指向对应的文件对象
	    ngx_buf_t       *shadow; //当这个buf完整copy了另外一个buf的所有字段的时候，那么这两个buf指向的实际上是同一块内存，或者是同一个文件的同一部分，此时这两个buf的shadow字段都是指向对方的。那么对于这样的两个buf，在释放的时候，就需要使用者特别小心，具体是由哪里释放，要提前考虑好，如果造成资源的多次释放，可能会造成程序崩溃
	
	
	    /* the buf's content could be changed */
	    unsigned         temporary:1;
	
	    /*
	     * the buf's content is in a memory cache or in a read only memory
	     * and must not be changed
	     */
	    unsigned         memory:1;
	
	    /* the buf's content is mmap()ed and must 	not be changed */
	    unsigned         mmap:1;
	
	    unsigned         recycled:1;
	    unsigned         in_file:1;
	    unsigned         flush:1;
	    unsigned         sync:1;
	    unsigned         last_buf:1;
	    unsigned         last_in_chain:1;
	
	    unsigned         last_shadow:1;
	    unsigned         temp_file:1;
	
	    /* STUB */ int   num;
	};
##4.10.ngx_list_t:ngx_list_t的节点实际上是一个固定大小的数组
    typedef struct {
    	ngx_list_part_t  *last; //指向该链表的最后一个节点
    	ngx_list_part_t   part; //该链表的首个存放具体元素的节点
    	size_t            size; //链表中存放的具体元素所需内存大小
    	ngx_uint_t        nalloc;  //每个节点所含的固定大小的数组的容量
    	ngx_pool_t       *pool;   //该list使用的分配内存的pool
	} ngx_list_t; 
###每个节点的定义
    typedef struct ngx_list_part_s  ngx_list_part_t;
	struct ngx_list_part_s {
    	void             *elts;  //节点中存放具体元素的内存的开始地址
    	ngx_uint_t        nelts; //节点中已有元素个数
    	ngx_list_part_t  *next;   //指向下一个节点
	};
##4.11.ngx_queue_t:双向链表
    typedef struct ngx_queue_s ngx_queue_t;

	struct ngx_queue_s {
    	ngx_queue_t  *prev;
    	ngx_queue_t  *next;
	};

#五.配置系统
##5.1.配置指令
##5.2.指令参数
##5.3.指令上下文

#六.请求处理
##6.1.请求的简单处理流程
###6.1.1.操作系统提供的机制（例如epoll, kqueue等）产生相关的事件。
###6.1.2.接收和处理这些事件，如是接受到数据，则产生更高层的request对象。
###6.1.3.处理request的header和body。
###6.1.4.产生响应，并发送回客户端。
###6.1.5.完成request的处理。
###6.1.6.重新初始化定时器及其他事件


#七.模块管理
##7.1.event module:	搭建了独立于操作系统的事件处理机制的框架，及提供了各具体事件的处理。包括ngx_events_module， ngx_event_core_module和ngx_epoll_module等。nginx具体使用何种事件处理模块，这依赖于具体的操作系统和编译选项。
##7.2.phase handler:	此类型的模块也被直接称为handler模块。主要负责处理客户端请求并产生待响应内容，比如ngx_http_static_module模块，负责客户端的静态页面请求处理并将对应的磁盘文件准备为响应内容输出。
##7.3.output filter:	也称为filter模块，主要是负责对输出的内容进行处理，可以对输出进行修改。例如，可以实现对输出的所有html页面增加预定义的footbar一类的工作，或者对输出的图片的URL进行替换之类的工作。
##7.4.upstream:	upstream模块实现反向代理的功能，将真正的请求转发到后端服务器上，并从后端服务器上读取响应，发回客户端。upstream模块是一种特殊的handler，只不过响应内容不是真正由自己产生的，而是从后端服务器上读取的。
##7.5.load-balancer:	负载均衡模块，实现特定的算法，在众多的后端服务器中，选择一个服务器出来作为某个请求的转发服务器。


#八.handler模块
##8.1.Handler模块就是接受来自客户端的请求并产生输出的模块
##8.2.配置信息的作用域：main, server, 以及location。
##8.3.模块的基本结构
###8.3.1.模块的配置结构：main, server, 以及location。
###8.3.2.模块配置指令
####ngx_command_t的定义
    struct ngx_command_s {
    	ngx_str_t             name; //配置指令的名称
    	ngx_uint_t            type; //该配置的类型，其实更准确一点说，是该配置指令属性的集合
    	char               *(*set)(ngx_conf_t *cf, 	ngx_command_t *cmd, void *conf);  //这是一个函数指针，当nginx在解析配置的时候，如果遇到这个配置指令，将会把读取到的值传递给这个函数进行分解处理
    	ngx_uint_t            conf;   //该字段被NGX_HTTP_MODULE类型模块所用 (我们编写的基本上都是NGX_HTTP_MOUDLE，只有一些nginx核心模块是非NGX_HTTP_MODULE)，该字段指定当前配置项存储的内存位置。实际上是使用哪个内存池的问题。因为http模块对所有http模块所要保存的配置信息，划分了main, server和location三个地方进行存储，每个地方都有一个内存池用来分配存储这些信息的内存。
    	ngx_uint_t            offset;  //指定该配置项值的精确存放位置，一般指定为某一个结构体变量的字段偏移
    	void                 *post;       //该字段存储一个指针。可以指向任何一个在读取配置过程中需要的数据，以便于进行配置读取的处理
	};
####注意:就是在ngx_http_hello_commands这个数组定义的最后，都要加一个ngx_null_command作为结尾。
###8.3.3.模块上下文结构：这是一个ngx_http_module_t类型的静态变量。这个变量实际上是提供一组回调函数指针，这些函数有在创建存储配置信息的对象的函数，也有在创建前和创建后会调用的函数。这些函数都将被nginx在合适的时间进行调用
    typedef struct {
    	ngx_int_t   (*preconfiguration)(ngx_conf_t 	*cf);        //创建和读取该模块的配置信息之前被调用
	    ngx_int_t   (*postconfiguration)(ngx_conf_t 	*cf);   //在创建和读取该模块的配置信息之后被调用

	    void       *(*create_main_conf)(ngx_conf_t 	*cf);       //调用该函数创建本模块位于http block的配置信息存储结构。该函数成功的时候，返回创建的配置对象。失败的话，返回NULL。
	    char       *(*init_main_conf)(ngx_conf_t 	*cf, void *conf); //调用该函数初始化本模块位于http block的配置信息存储结构。该函数成功的时候，返回NGX_CONF_OK。失败的话，返回NGX_CONF_ERROR或错误字符串。

	    void       *(*create_srv_conf)(ngx_conf_t 	*cf);     //调用该函数创建本模块位于http server block的配置信息存储结构，每个server block会创建一个。该函数成功的时候，返回创建的配置对象。失败的话，返回NULL。
	    char       *(*merge_srv_conf)(ngx_conf_t 	*cf, void *prev, void *conf);

	    void       *(*create_loc_conf)(ngx_conf_t 	*cf);       //调用该函数创建本模块位于location block的配置信息存储结构。每个在配置中指明的location创建一个。该函数执行成功，返回创建的配置对象。失败的话，返回NULL。
	    char       *(*merge_loc_conf)(ngx_conf_t 	*cf, void *prev, void *conf);
	} ngx_http_module_t;

##8.3.4.模块的定义
    typedef struct ngx_module_s      ngx_module_t;
	struct ngx_module_s {
    	ngx_uint_t            ctx_index;
    	ngx_uint_t            index;
    	ngx_uint_t            spare0;
    	ngx_uint_t            spare1;
    	ngx_uint_t            abi_compatibility;
    	ngx_uint_t            major_version;
    	ngx_uint_t            minor_version;
    	void                 *ctx;
    	ngx_command_t        *commands;
    	ngx_uint_t            type;
    	ngx_int_t           (*init_master)(ngx_log_t *log);
    	ngx_int_t           (*init_module)	(ngx_cycle_t *cycle);
    	ngx_int_t           (*init_process)	(ngx_cycle_t *cycle);
    	ngx_int_t           (*init_thread)	(ngx_cycle_t *cycle);
    	void                (*exit_thread)	(ngx_cycle_t *cycle);
    	void                (*exit_process)	(ngx_cycle_t *cycle);
    	void                (*exit_master)	(ngx_cycle_t *cycle);
    	uintptr_t             spare_hook0;
    	uintptr_t             spare_hook1;
    	uintptr_t             spare_hook2;
    	uintptr_t             spare_hook3;
    	uintptr_t             spare_hook4;
    	uintptr_t             spare_hook5;
    	uintptr_t             spare_hook6;
    	uintptr_t             spare_hook7;
	};
##8.3.5.模块的挂载：handler模块真正的处理函数通过两种方式挂载到处理过程中，一种方式就是按处理阶段挂载;另外一种挂载方式就是按需挂载
##8.4.实现一个handler的步骤
###8.4.1.编写模块基本结构。包括模块的定义，模块上下文结构，模块的配置结构等
###8.4.2.实现handler的挂载函数。根据模块的需求选择正确的挂载方式
###8.4.3.编写handler处理函数。模块的功能主要通过这个函数来完成

#九.过滤模块
##9.1.过滤（filter）模块是过滤响应头和内容的模块，可以对回复的头和内容进行处理。它的处理时间在获取回复内容之后，向用户发送响应之前。它的处理过程分为两个阶段，过滤HTTP回复的头部和主体，在这两个阶段可以分别对头部和主体进行修改


#十.upstream模块
##10.1.数据转发功能，为nginx提供了跨越单机的横向处理能力，使nginx摆脱只能为终端节点提供单一功能的限制，而使它具备了网路应用级别的拆分、封装和整合的战略功能
##10.2.upstream属于handler，只是他不产生自己的内容，而是通过请求后端服务器得到内容；请求并取得响应内容的整个过程已经被封装到nginx内部，所以upstream模块只需要开发若干回调函数，完成构造请求和解析响应等具体的工作。
##10.3.模块接口
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/upstream%E7%9A%84%E6%A8%A1%E5%9D%97%E6%8E%A5%E5%8F%A3.png)
##10.4.操作流程
###10.4.1.创建upstream数据结构
###10.4.2.设置模块的tag和schema。schema现在只会用于日志，tag会用于buf_chain管理
###10.4.3.设置upstream的后端服务器列表数据结构
###10.4.4.设置upstream回调函数。在这里列出的代码稍稍调整了代码顺序
###10.4.5.创建并设置upstream环境数据结构
###10.4.6.完成upstream初始化并进行收尾工作
##10.5.负载均衡模块：nginx先使用负载均衡模块找到一台主机，再使用upstream模块实现与这台主机的交互。
###10.5.1.ip hash的负载均衡算法
    upstream test {
   		ip_hash;

    	server 192.168.0.1;
    	server 192.168.0.2;
	}
###10.5.2.指令：配置决定指令系统，现在就来看ip_hash的指令定义
    static ngx_command_t  	ngx_http_upstream_ip_hash_commands[] = {

    	{ ngx_string("ip_hash"),
      	  NGX_HTTP_UPS_CONF|NGX_CONF_NOARGS,
       	  ngx_http_upstream_ip_hash,
          0,
          0,
      	  NULL },

         ngx_null_command
	};
###10.5.3.钩子：
    static char *
	ngx_http_upstream_ip_hash(ngx_conf_t *cf, 	ngx_command_t *cmd, void *conf)
	{
    	ngx_http_upstream_srv_conf_t  *uscf;

    	uscf = ngx_http_conf_get_module_srv_conf	(cf, ngx_http_upstream_module);

    	uscf->peer.init_upstream = 	ngx_http_upstream_init_ip_hash;

    	uscf->flags = NGX_HTTP_UPSTREAM_CREATE
    	            |NGX_HTTP_UPSTREAM_MAX_FAILS
    	            |NGX_HTTP_UPSTREAM_FAIL_TIMEOUT
    	            |NGX_HTTP_UPSTREAM_DOWN;

    	return NGX_CONF_OK;
	}
###10.5.4.初始化配置
###10.5.5.初始化请求

#十一.注意
##11.1.反向代理：以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86%E4%B8%8E%E6%AD%A3%E5%90%91%E4%BB%A3%E7%90%86.png)


#十二.Nginx的组成
##12.1.Nginx二进制可执行文件：每个模块源码编译出的一个文件
##12.2.Nginx.conf配置文件：控制Nginx的行为
##12.3.access.log访问日志：记录每一条http请求信息
##12.4.error.log错误日志：定位问题

#十三.Nginx的请求处理流程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Nginx%E8%AF%B7%E6%B1%82%E5%A4%84%E7%90%86%E6%B5%81%E7%A8%8B.png)