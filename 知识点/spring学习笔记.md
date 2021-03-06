#一.spring的四大原则：

- 使用POJO进行轻量级和最小侵入式开发


- 通过依赖注入和基于接口编程实现松耦合


- 通过AOP和默认习惯进行声明式编程


- 通过AOP和模板减少模式化代码

#二.依赖注入（IoC/DI）：容器负责创建对象和维护对象间的依赖关系，而不是通过对象本身负责自己的创建和解决自己的依赖
> 依赖注入会将所依赖的关系自动交给目标对象，而不是让对象自己去获取

> 对强依赖使用构造器注入，而对可选性的依赖使用属性注入
##2.1.构造函数注入、属性注入、接口注入
##2.2.Java的反射机制
##2.3.Resource类管理资源，加载资源路径
##2.4.BeanFactory
##2.5.ApplicationContext
##2.6.Bean的生命周期
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/bean%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
###2.6.1.BeanFactory中Bean的生命周期
###2.6.2.ApplicationContext中Bean的生命周期
##2.7.Bean的元数据信息（Bean的实现类；Bean的属性信息;Bean的依赖关系;Bean的行为配置）
##2.8.Bean的配置信息：首先定义Bean的实现及依赖关系，Spring容器根据各种形式的Bean配置信息在容器内部建立Bean定义注册表；然后根据注册表加载、实例化Bean,并建立Bean和Bean之间的依赖关系；最后将这些准备就绪的Bean放到Bean缓存池中，并供外层的应用程序进行调用
##2.9.Bean的基本配置
##2.10.IoC容器的初始化过程（在IoC容器中建立BeanDefinition数据映射）
###2.10.1.Resource定位过程
###2.10.2.BeanDefinition的载入（通过一个HashMap维护）
###2.10.3.调用BeanDefinitionRegistry向IoC容器注册这些BeanDefinition的过程
##2.11.先进行Bean的定义，然后注入


##2.12.高级装配技巧
- 利用Spring profile解决跨各种部署环境的通用问题，其是一种运行时条件化创建bean的一种方式；结合@Conditional注解和Spring Condition接口的实现，可以实现条件化创建bean
- 自动装配歧义性解决方法：首选bean以及限定符
- 基于不同的作用域装配
- spring表达式语言，运行时计算注入到bean属性中的值

#三.面向切面编程（AOP）
##3.1.基本概念
###3.1.1.连接点：1.用方法表示的程序执行点；2.用相对位置表示的方位；Spring使用切点对执行点进行定位，而方位则在增强类型中定义
###3.1.2.切点：切点只定位到某个方法
###3.1.3.增强：
###3.1.4.目标对象
###3.1.5.引介：一种特殊的增强，为类添加一些属性和方法
###3.1.6.织入：将增强添加到目标类的具体连接点上的过程
###3.1.7.代理
###3.1.8.切面：由切点和增强组成


##3.2.增强
###3.2.1.增强类型（前置增强；后置增强；环绕增强；异常抛出增强；引介增强）


##3.3.创建切面

##3.4.自动创建代理

##3.5.基于@AspectJ和Schema的AOP

#四.spring的常用配置
##1.@Scope：描述Spring容器如何新建Bean的实例（Singleton/Prototype/Request/Session/GlobalSession）
##2.@Value：使用表达式
##3.Bean的初始化和销毁（@PostConstruct/@PreDestroy）
##4.@Profile:为在不同环境下使用不同的配置提供支持
##5.事件：为Bean与Bean之间的消息通信提供了支持（1.自定义事件ApplicationEvent；2.定义事件监听器实现ApplicationListener;3.使用容器发布事件）
##6.多线程：@EnableAsync开启对异步任务的支持，并通过@Async声明其是一个异步任务
##7.计划任务：@EnableScheduling开启对计划任务的支持，在要执行计划任务的方法上注解@Scheduled声明是一个计划任务
##8.条件注解@Conditional：根据满足某一个特定条件创建特定的Bean
###注：注解和xml都是一种元数据（解释数据的数据，所谓的配置）

#五.Spring MVC
##1.@Controller
##2.@RequestMapping：用来映射Web请求（访问路径以及参数）、处理类和方法的。
##3.@ResponseBody：支持将返回值放在response体内，而不是返回一个页面
##4.@RequestBody:允许request的参数在request体中，而不是在直接链接的地址后面
##5.@PathVariable
##6.@RestController

#六.Spring MVC基本配置
##6.1.静态资源映射：在配置中重写addResourceHandlers方法实现
##6.2.拦截器配置：
###6.2.1拦截器：实现对每一个请求处理前后进行相关的业务处理
##6.3.@ControllerAdvice
##6.4.文件上传MultipartResolver
##6.5.MVC(数据模型+视图+控制器)；三层架构（展现层+应用层+数据访问层）


#七.Spring Boot基础


#八.Bean基本配置
##8.1.依赖注入（属性注入；构造函数注入）
##8.2.注入参数详解（字面值；引用其他Bean;内部Bean;null值；级联属性；集合类型属性）
##8.3.方法注入（lookup方法；方法替换）
##8.4.<bean>之间的关系（继承；依赖；引用）
##8.5.Bean的作用域
- singleton作用域：懒加载问题
- prototype作用域：创建一个新的实例
- web应用环境相关（request;session;globalSession）
##8.6.FactoryBean

#九.基于注解的配置
##9.1.@Component,@Repository,@Service,@Controller
##9.2.自动装配Bean(@Autowired进行自动注入；@Qualifier指定注入Bean的名称；@Lazy延迟加载；@Scope作用范围；@PostConstruct和@PreDestroy生命周期)
##9.3.基于Java类的配置
###9.3.1.普通的POJO只要标注@Configuration注解，就可以为Spring容器提供Bean定义的信息，每个标注了@Bean的类方法都相当于提供了一个Bean的定义信息


#十.Spring的事务管理
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/spring%E4%BA%8B%E5%8A%A1%E6%80%BB%E6%B5%81%E7%A8%8B%E5%88%86%E6%9E%90%E6%97%B6%E5%BA%8F%E5%9B%BE.png)
##10.1.ThreadLocal:不是一个线程，而是保存线程本地化对象的容器，以“空间换时间”，访问并行化，对象独享化
###10.2.1.通过声明式事务处理，将事务处理的过程和业务代码分离
###10.2.2.应用在选择数据源是可能会采用不同的方案，Spring在应用和具体的数据源之间，搭建一个中间平台，解耦应用与数据源的绑定
##10.3.TransactionDefinition正好用来定义事务属性
##10.4.编程式事务侵入到了业务代码里面，但是提供了更加详细的事务管理；而声明式事务由于基于AOP，所以既能起到事务管理的作用，又可以不影响业务代码的具体实现


#十一.Spring MVC详解
##11.1.处理请求的整体过程
- 客户端发出一个HTTP请求，Web应用服务器接收到这个请求，如果匹配DispatcherServlet的请求映射路径，则Web容器将该请求转交给DispatcherServlet处理
- DispatcherServlet接收到这个请求后，根据请求的信息及HandlerMapping的配置找到处理请求的处理器。
- 当DispatcherServlet根据HandlerMapping得到对应当前请求的Handler后，通过HandlerAdpter对Handler进行封装，再以统一的适配器接口调用Handler
- 处理器完成业务逻辑的处理后将返回一个ModelAndView给DispatcherServlet,ModelAndView包含了视图逻辑名和模型数据信息
- ModelAndView中包含的是“逻辑视图名”而非真正的视图对象，DispatcherServlet借由ViewResolver完成逻辑视图名到真实视图对象的解析工作
- 得到真实的视图对象View后，DispatcherServlet就使用这个View对象对ModelAndView中的模型数据进行视图渲染
- 最终客户端得到的响应消息可能是一个普通的HTML页面，也可能是一个XML或Json串，甚至一张图片或一个PDF文档等不同的媒体形式





##11.2.开发步骤
- 配置web.xml，指定业务层对应的Spring配置文件，定义DispatcherServlet
- 编写处理请求的控制器
- 编写视图对象，这里使用JSP作为视图
- 配置Spring MVC的配置文件，使控制器、视图解析器等生效


##11.3.注解驱动的控制器
###11.3.1.@RequestMapping映射请求

###11.3.2.处理方法的数据绑定
##11.4.视图和视图解析器

##11.5.本地化解析

##11.6.文件上传

##11.7.WebSocket支持

#十二.Spring常用注解
##12.1.声明bean的注解
- @Component 组件，没有明确的角色
- @Service 在业务逻辑层使用（service层）
- @Repository 在数据访问层使用（dao层）
-@Controller 在展现层使用，控制器的声明（C）
##12.2.注入bean的注解：都可以注解在set方法和属性上，推荐注解在属性上（一目了然，少写代码）
- @Autowired：由Spring提供
- @Inject：由JSR-330提供
- @Resource：由JSR-250提供
##12.3.java配置类相关注解
- @Configuration 声明当前类为配置类，相当于xml形式的Spring配置（类上）
- @Bean 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上）
- @Configuration 声明当前类为配置类，其中内部组合了@Component注解，表明这个类是一个bean（类上）
- @ComponentScan 用于对Component进行扫描，相当于xml中的（类上）
##12.4.切面（AOP）相关注解
- @Aspect 声明一个切面（类上）
- 使用@After、@Before、@Around定义建言（advice），可直接将拦截规则（切点）作为参数。
##12.5.@Bean的属性支持
- @Scope 设置Spring容器如何新建Bean实例（方法上，得有@Bean）：其设置类型包括：1）Singleton （单例,一个Spring容器中只有一个bean实例，默认模式）；2）Protetype （每次调用新建一个bean）；3）Request （web项目中，给每个http request新建一个bean）；4）Session （web项目中，给每个http session新建一个bean）；5）GlobalSession（给每一个 global http session新建一个Bean实例）
##12.6.@Value注解
- @Value 为属性注入值（属性上）
##12.7.环境切换
- @Profile 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境。（类或方法上）
##12.8.异步相关
- @EnableAsync 配置类中，通过此注解开启对异步任务的支持，叙事性AsyncConfigurer接口（类上）
- @Async 在实际执行的bean方法使用该注解来申明其是一个异步任务（方法上或类上所有的方法都将异步，需要@EnableAsync开启异步任务）
##12.9.定时任务相关
- @EnableScheduling 在配置类上使用，开启计划任务的支持（类上）
- @Scheduled 来申明这是一个任务，包括cron,fixDelay,fixRate等类型（方法上，需先开启计划任务的支持）
##12.10.@Enable*注解说明：这些注解主要用来开启对xxx的支持
- @EnableAspectJAutoProxy 开启对AspectJ自动代理的支持
- @EnableAsync 开启异步方法的支持
- @EnableScheduling 开启计划任务的支持
- @EnableWebMvc 开启Web MVC的配置支持
- @EnableConfigurationProperties 开启对@ConfigurationProperties注解配置Bean的支持
- @EnableJpaRepositories 开启对SpringData JPA Repository的支持
- @EnableTransactionManagement 开启注解式事务的支持
- @EnableCaching 开启注解式的缓存支持
##12.11.测试相关注解
- @RunWith 运行器，Spring中通常用于对JUnit的支持
- @ContextConfiguration 用来加载配置ApplicationContext，其中classes属性用来加载配置类
##12.12.SpringMVC相关注解
- @EnableWebMvc 在配置类中开启Web MVC的配置支持，如一些ViewResolver或者MessageConverter等，若无此句，重写WebMvcConfigurerAdapter方法（用于对SpringMVC的配置）
- @Controller 声明该类为SpringMVC中的Controller
- @RequestMapping 用于映射Web请求，包括访问路径和参数（类或方法上）
- @ResponseBody 支持将返回值放在response内，而不是一个页面，通常用户返回json数据（返回值旁或方法上）
- @RequestBody 允许request的参数在request体中，而不是在直接连接在地址后面。（放在参数前）
- @PathVariable 用于接收路径参数，比如@RequestMapping(“/hello/{name}”)申明的路径，将注解放在参数中前，即可获取该值，通常作为Restful的接口实现方法
- @RestController 该注解为一个组合注解，相当于@Controller和@ResponseBody的组合，注解在类上，意味着，该Controller的所有方法都默认加上了@ResponseBody。
- @ControllerAdvice 通过该注解，我们可以将对于控制器的全局配置放置在同一个位置，注解了@Controller的类的方法可使用@ExceptionHandler、@InitBinder、@ModelAttribute注解到方法上
- @ExceptionHandler 用于全局处理控制器里的异常

#十三.spring中设计模式
##13.1.设计模式：[https://www.jianshu.com/p/2ae05fe6f318](https://www.jianshu.com/p/2ae05fe6f318 "spring中常用设计模式")

#补充知识点
##1.Java代理
###1.1.静态代理：需要定义接口或者父类，被代理对象与代理对象一起实现相同的接口或者是继承相同的父类
####1.1.1可以做到在不修改目标对象的功能前提下，对目标功能扩展；因为代理对象需要与目标对象实现一样的接口，所以会有很多代理类，类太多。同时，一旦接口增加方法，目标对象和代理对象都要维护
###1.2.动态代理：目标对象一定要实现接口，程序运行时运用反射机制动态创建
##2.Cglib代理：内存中构建一个子类对象从而实现目标对象功能的扩展

##3.BeanFactory与FactoryBean的区别
###3.1.FactoryBean：能生产或者修饰对象生成的工厂Bean,它的实现与设计模式中的工厂模式和修饰器模式类似；根据该Bean的ID从BeanFactory中获取的实际上是FactoryBean的getObject()返回的对象，而不是FactoryBean本身，如果要获取FactoryBean对象，请在id前面加一个&符号来获取。

##Spring引用的7中设计模式
###工厂模式解决的问题：解耦、创建时干预、统一管理
###单例模式解决的问题：可以共享的资源就不要重复创建，特别是创建起来成本很高的资源，比如数据源
###代理模式解决的问题：既不修改基类（无侵入），又可以灵活的扩展它的功能，而且这种扩展是可以复用的，比如AspectJ、CGLIB、JDK动态代理
###观察者模式解决的问题：事件通知，比如zk节点的watch机制，比如tomcat的启动机制（创建IOC、MVC容器）
###模板模式解决的问题：代码冗余，通过模板类+业务类作为参数解决。比如JDBCTemplate，模板方法处理 创建连接、处理异常、释放资源等操作，业务类执行自己的sql
###策略模式解决的问题：同一类型业务的类，有很多公用的流程和方法，只是在核心方法上略有区别，为了降低代码的冗余度，单独把不同的方法抽象成一个接口，各自业务类实现自己的核心方法
###责任链模式解决的问题：把一套流程拆分成不同的Handler，使用的时候根据业务场景拼装，可以非常灵活和低耦合的实现特定的业务流程。

##13.Spring MVC执行流程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Spring%20MVC%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B.png)

##补充知识点
- Bean的作用域
	- 单例
	- 原型：每次注入或者通过Spring应用上下文获取的时候，都会创建一个新的Bean实例
	- 会话：在web应用中，为每个会话创建一个bean实例
	- 请求：在Web应用中，为每个请求创建一个bean实例 




#Spring Boot
##1.1.核心
- 自动配置
	- 使用显示配置进行覆盖
	- 使用属性进行精细化配置 
- 起步依赖
- 命令行界面
- Actuator:提供在运行时检视应用程序内部情况的能力
	- Spring应用程序上下文里配置的Bean
	- Spring Boot的自动配置的决策
	- 应用程序取到的环境变量、系统属性、配置属性和命令行参数
	- 应用程序里线程当前状态
	- 应用程序最近处理过的HTTP请求的追踪情况
	- 各种内存用量、垃圾回收、Web请求以及数据源用量相关指标

##1.2.Actuator端点
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Actuator%E7%AB%AF%E7%82%B9.png)
###1.2.1.查看配置明细
- 获得Bean装配报告
	- bean:Spring应用程序上下文中的Bean名称或ID
	- resource:.class文件的物理位置，通常是一个URL，指向构建出的JAR文件，这会随应用程序的构建和运行方式发生变化
	- dependencies:当前Bean注入的Bean ID列表
	- scope:Bean的作用域
	- type:Bean的Java类型 

- 详解自动配置
- 查看配置属性
	- 应用属性
	- 环境变量
	- JVM系统属性 

- 端点到控制器的映射

###1.2.2.运行时度量
- 查看应用程序的度量值
- 追踪Web请求
- 导出线程活动
- 监控应用程序的健康情况

###1.2.3.关闭应用程序
###1.2.4.获取应用信息

##1.3.定制Actuator
- 重命名端点
- 启动和禁用端点
- 自定义度量信息
- 创建自定义仓库来存储跟踪数据
- 插入自定义的健康指示器
 
##1.4.保护Actuator端点

##2.Spring Boot应用程序的部署
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Spring%20Boot%E9%83%A8%E7%BD%B2%E9%80%89%E9%A1%B9.png)



#Spring Cloud


#补充知识点
##事务机制
> 事务属性：通常由事务的传播行为、事务的隔离级别、事务的超时值、事务只读标志组成

- 事务的隔离级别
	- ISOLATION_DEFAULT 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别.另外四个与JDBC的隔离级别相对应
	- ISOLATION_READ_UNCOMMITTED 这是事务最低的隔离级别，它充许别外一个事务可以看到这个事务未提交的数据。这种隔离级别会产生脏读，不可重复读和幻像读
	- ISOLATION_READ_COMMITTED  保证一个事务修改的数据提交后才能被另外一个事务读取。另外一个事务不能读取该事务未提交的数据。这种事务隔离级别可以避免脏读出现，但是可能会出现不可重复读和幻像读
	- ISOLATION_REPEATABLE_READ  这种事务隔离级别可以防止脏读，不可重复读。但是可能出现幻像读。它除了保证一个事务不能读取另一个事务未提交的数据外，还保证了避免下面的情况产生(不可重复读)。 
	- ISOLATION_SERIALIZABLE 这是花费最高代价但是最可靠的事务隔离级别。事务被处理为顺序执行。除了防止脏读，不可重复读外，还避免了幻像读。

- 事务传播行为
	- PROPAGATION_REQUIRED 如果存在一个事务，则支持当前事务。如果没有事务则开启一个新的事务

			Java代码：

			//事务属性 PROPAGATION_REQUIRED
			methodA{
				……
				methodB();
				……
			}

			//事务属性 PROPAGATION_REQUIRED
			methodB{
   				……
			}

			使用spring声明式事务，spring使用AOP来支持声明式事务，会根据事务属性，自动在方法调用之前决定是否开启一个事务，并在方法执行之后决定事务提交或回滚事务。

			单独调用methodB方法：

			Java代码

			main{ 

				metodB(); 

			}  
			相当于

			Java代码

			Main{ 

				Connection con=null; 

				try{ 

					con = getConnection(); 

					con.setAutoCommit(false); 

					//方法调用

					methodB(); 

					//提交事务

					con.commit(); 

				} 

				Catch(RuntimeException ex){ 

            	//回滚事务

					con.rollback();   

				} 

				finally{ 

				//释放资源

					closeCon(); 

				} 

			} 


			Spring保证在methodB方法中所有的调用都获得到一个相同的连接。在调用methodB时，没有一个存在的事务，所以获得一个新的连接，开启了一个新的事务。
	
			单独调用MethodA时，在MethodA内又会调用MethodB.

			执行效果相当于：

			Java代码

         		main{ 

					Connection con = null; 

				try{ 

					con = getConnection(); 

   					methodA(); 

					con.commit(); 

				} 

				catch(RuntimeException ex){ 

					con.rollback(); 

				} 

				finally{ 

   					closeCon(); 

				}  

			} 

			调用MethodA时，环境中没有事务，所以开启一个新的事务.当在MethodA中调用MethodB时，环境中已经有了一个事务，所以methodB就加入当前事务。 
	- PROPAGATION_SUPPORTS 如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行。但是对于事务同步的事务管理器，PROPAGATION_SUPPORTS与不使用事务有少许不同

			Java代码：
             //事务属性 PROPAGATION_REQUIRED
			methodA(){
  				methodB();
			}

			//事务属性 PROPAGATION_SUPPORTS
			methodB(){
 				 ……
			}

			单纯的调用methodB时，methodB方法是非事务的执行的。当调用methdA时,methodB则加入了methodA的事务中,事务地执行。 
	- PROPAGATION_MANDATORY 如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常

			Java代码：

            //事务属性 PROPAGATION_REQUIRED
    		methodA(){
 			 	methodB();
            }

   			 //事务属性 PROPAGATION_MANDATORY
    		methodB(){
   				 ……
    		}

			当单独调用methodB时，因为当前没有一个活动的事务，则会抛出异常throw new IllegalTransactionStateException("Transaction propagation 'mandatory' but no existing transaction found");当调用methodA时，methodB则加入到methodA的事务中，事务地执行。 

	- PROPAGATION_REQUIRES_NEW 总是开启一个新的事务。如果一个事务已经存在，则将这个存在的事务挂起。 

		 	Java代码：

          	//事务属性 PROPAGATION_REQUIRED
		 	methodA(){
   				doSomeThingA();
				methodB();
				doSomeThingB();
			}

			//事务属性 PROPAGATION_REQUIRES_NEW
			methodB(){
  				 ……
			}

			Java代码：

			main(){
 				 methodA();
			}

			相当于

			Java代码：

			main(){
  				TransactionManager tm = null;
			try{
  				//获得一个JTA事务管理器
    			tm = getTransactionManager();
    			tm.begin();//开启一个新的事务
    			Transaction ts1 = tm.getTransaction();
    			doSomeThing();
   				tm.suspend();//挂起当前事务
    			try{
      				tm.begin();//重新开启第二个事务
      				Transaction ts2 = tm.getTransaction();
     				methodB();
     				ts2.commit();//提交第二个事务
   				}
  				Catch(RunTimeException ex){
      				ts2.rollback();//回滚第二个事务
  				}
  				finally{
     			//释放资源
   				}
    			//methodB执行完后，复恢第一个事务
    			tm.resume(ts1);
				doSomeThingB();
    			ts1.commit();//提交第一个事务
			}
			catch(RunTimeException ex){
  				ts1.rollback();//回滚第一个事务
			}
			finally{
   				//释放资源
			}
		}

		在这里，我把ts1称为外层事务，ts2称为内层事务。从上面的代码可以看出，ts2与ts1是两个独立的事务，互不相干。Ts2是否成功并不依赖于 ts1。如果methodA方法在调用methodB方法后的doSomeThingB方法失败了，而methodB方法所做的结果依然被提交。而除了 methodB之外的其它代码导致的结果却被回滚了。使用PROPAGATION_REQUIRES_NEW,需要使用 JtaTransactionManager作为事务管理器。 

	- PROPAGATION_NOT_SUPPORTED  总是非事务地执行，并挂起任何存在的事务。使用PROPAGATION_NOT_SUPPORTED,也需要使用JtaTransactionManager作为事务管理器。
	-  PROPAGATION_NEVER 总是非事务地执行，如果存在一个活动事务，则抛出异常； 
	-  PROPAGATION_NESTED如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行。这是一个嵌套事务,使用JDBC 3.0驱动时,仅仅支持DataSourceTransactionManager作为事务管理器。需要JDBC 驱动的java.sql.Savepoint类。有一些JTA的事务管理器实现可能也提供了同样的功能。使用PROPAGATION_NESTED，还需要把PlatformTransactionManager的nestedTransactionAllowed属性设为true;而 nestedTransactionAllowed属性值默认为false; 
	
			Java代码：

            //事务属性 PROPAGATION_REQUIRED
			methodA(){
   				doSomeThingA();
   				methodB();
   				doSomeThingB();
			}

			//事务属性 PROPAGATION_NESTED
			methodB(){
  				……
			}

			如果单独调用methodB方法，则按REQUIRED属性执行。如果调用methodA方法，相当于下面的效果：

			Java代码：

           	main(){
				Connection con = null;
				Savepoint savepoint = null;
				try{
   					con = getConnection();
   					con.setAutoCommit(false);
   					doSomeThingA();
   					savepoint = con2.setSavepoint();
   					try{
       					methodB();
   					}catch(RuntimeException ex){
      					con.rollback(savepoint);
   					}
   					finally{
     					//释放资源
  					}

   					doSomeThingB();
   					con.commit();
				}
				catch(RuntimeException ex){
  					con.rollback();
				}
				finally{
   					//释放资源
				}
			}

			当methodB方法调用之前，调用setSavepoint方法，保存当前的状态到savepoint。如果methodB方法调用失败，则恢复到之前保存的状态。但是需要注意的是，这时的事务并没有进行提交，如果后续的代码(doSomeThingB()方法)调用失败，则回滚包括methodB方法的所有操作。 

		- 嵌套事务一个非常重要的概念就是内层事务依赖于外层事务。外层事务失败时，会回滚内层事务所做的动作。而内层事务操作失败并不会引起外层事务的回滚。
		- PROPAGATION_NESTED 与PROPAGATION_REQUIRES_NEW的区别:它们非常类似,都像一个嵌套事务，如果不存在一个活动的事务，都会开启一个新的事务。使用 PROPAGATION_REQUIRES_NEW时，内层事务与外层事务就像两个独立的事务一样，一旦内层事务进行了提交后，外层事务不能对其进行回滚。两个事务互不影响。两个事务不是一个真正的嵌套事务。同时它需要JTA事务管理器的支持。 
		- 使用PROPAGATION_NESTED时，外层事务的回滚可以引起内层事务的回滚。而内层事务的异常并不会导致外层事务的回滚，它是一个真正的嵌套事务。DataSourceTransactionManager使用savepoint支持PROPAGATION_NESTED时，需要JDBC 3.0以上驱动及1.4以上的JDK版本支持。其它的JTA TrasactionManager实现可能有不同的支持方式。
		- PROPAGATION_REQUIRES_NEW 启动一个新的, 不依赖于环境的 "内部" 事务. 这个事务将被完全 commited 或 rolled back 而不依赖于外部事务, 它拥有自己的隔离范围, 自己的锁, 等等. 当内部事务开始执行时, 外部事务将被挂起, 内务事务结束时, 外部事务将继续执行。 
		- 另一方面, PROPAGATION_NESTED 开始一个 "嵌套的" 事务,  它是已经存在事务的一个真正的子事务. 潜套事务开始执行时,  它将取得一个 savepoint. 如果这个嵌套事务失败, 我们将回滚到此 savepoint. 潜套事务是外部事务的一部分, 只有外部事务结束后它才会被提交。 
		- 由此可见, PROPAGATION_REQUIRES_NEW 和 PROPAGATION_NESTED 的最大区别在于, PROPAGATION_REQUIRES_NEW 完全是一个新的事务, 而 PROPAGATION_NESTED 则是外部事务的子事务, 如果外部事务 commit, 潜套事务也会被 commit, 这个规则同样适用于 roll back.
		- PROPAGATION_REQUIRED应该是我们首先的事务传播行为。它能够满足我们大多数的事务需求。 