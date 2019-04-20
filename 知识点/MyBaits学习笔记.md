#一.简介
##1.1.ORM（对象关系映射）:数据库的表和简单的Java对象的映射关系模型，解决数据库数据和POJO对象的相互映射
##1.2.Hibernate的缺点
###1.2.1.全表映射带来的不便，比如更新时需要发送所有的字段
###1.2.2.无法根据不同的条件组装不同的SQL
###1.2.3.对多表关联和复杂SQL查询支持较差，需要自己写SQL，返回后，需要自己将数据组装成POJO
###1.2.4.不能有效支持存储过程
###1.2.5.虽有HQL,但性能较差。
##1.3.半自动映射框架（SQL,映射规则，POJO）

#MyBatis整体结构
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBatis%E6%95%B4%E4%BD%93%E6%9E%B6%E6%9E%84.png)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBatis%E7%BB%93%E6%9E%84%E4%BF%A1%E6%81%AF.png)

#二.基础概念
##2.1.核心概念
###2.1.1.SqlSession : 代表和数据库的一次会话，向用户提供了操作数据库的方法。
###2.1.2.MappedStatement: 代表要发往数据库执行的指令，可以理解为是Sql的抽象表示。
###2.1.3.Executor: 具体用来和数据库交互的执行器，接受MappedStatement作为参数。
###2.1.4.映射接口: 在接口中会要执行的Sql用一个方法来表示，具体的Sql写在映射文件中。
###2.1.5.映射文件: 可以理解为是Mybatis编写Sql的地方，通常来说每一张单表都会对应着一个映射文件，在该文件中会定义Sql语句入参和出参的形式。

#二.核心组件
###2.1.1.SqlSessionFactoryBuilder(构造器)：根据配置信息或者代码来生成SqlSessionFactory(工厂接口)
###2.1.2.SqlSessionFactory:依靠工厂生成SqlSession(会话)
###2.1.3.SqlSession：是一个既可以发送SQL去执行并返回结果，也可以获取Mapper的接口。类似于JDBC的Connection对象
###2.1.4.SQL Mapper：由一个Java接口和XML文件构成，需要给出对应的SQL和映射规则。负责发送SQL去执行，并返回结果

##2.1.SqlSessionFactoryBuilder（生命周期只存在与方法的局部，作用就是生成SqlSessionFactory）


##2.2.构建SqlSessionFactory(Mybatis应用的整个生命周期，采用单例模式)
###2.2.1.XML方式配置（定义数据库环境，事务管理方式，数据库链接信息，配置映射器）
###2.2.2.代码的方式

##2.3.创建SqlSession（请求数据库处理事务过程中。一个线程不安全的对象，在涉及多线程的时候应当心，注意其隔离级别，数据库锁特性，创建SqlSession必须及时关闭）


##2.4.映射器（在一个SqlSession事务方法中，是一个方法级别的东西）
###2.4.1.定义参数类型
###2.4.2.描述缓存
###2.4.3.描述SQL语句
###2.4.4.定义查询结果和POJO的映射关系


#三.生命周期
- SqlSessionFactoryBuilder：利用XML或者Java编码获得资源来构建SqlSessionFactory的，通过它可以构建多个SessionFactory。它的作用就是一个构建器，一旦构建SqlSessionFactory，作用就已经完结，此时就应该毫不犹豫的废弃它，将其回收，其生命周期只存在于方法的局部
- SqlSessionFactory：创建SqlSession，而SqlSession就是一个会话，相当于JDBC中的Connection对象，SqlSessionFactory应该在MyBatis应用的整个生命周期。如果我们多次创建同一个数据库的SqlSessionFactory,则每次创建SqlSessionFactory会打开更多的数据库连接资源，则其很快就会耗尽，因此SqlSessionFactory的责任是唯一的，就是创建SqlSession，应该采用单例模式
- SqlSession：一个会话，相当于JDBC的一个Connection对象，其生命周期应该是在请求数据库处理事务的过程中。其是一个线程不安全的对象，操作数据库需要注意其隔离级别，数据库锁等高级特性，每次创建SqlSession都必须及时关闭它，它的长期存在就会是数据库连接池的活动资源减少，对系统性能影响很大
- Mapper:它应该在一个SqlSession事务方法之内，是一个方法级别的东西，它的最大的范围和SqlSession是相同的。尽管我们想一直保留着Mapper，你会发现其很难控制，尽量在一个SqlSession事务的方法中使用它们，然后废弃掉
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBatis%E7%BB%84%E4%BB%B6%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

#三.配置
##3.1.配置Mybatis的XML文件的层次结构
    <?xml version="1.0" encoding="UTF-8"?>
	<configuration> <!--配置-->
		<properties/> <!--属性-->
		<setting/> <!--设置-->
		<typeAliases/> <!--类型别名-->
		<typeHandlers/> <!--类型处理器-->
		<objectFactory/> <!--对象工厂-->
		<plugins/> <!--插件-->
		<environments> <!--配置环境-->
			<environment> <!--环境配置-->
				<transactionManager/> <!--事务管理器-->
				<dataSource/> <!--数据源-->
			<environment/>
		<environments/>
		<databaseIdProvider/> <!--数据库厂商标识-->
		<mappers/> <!--映射器-->
	</configuration>
	
##3.2.properties元素
- property子元素
- properties配置文件
- 程序参数传递

##3.3.settings设置(配置内容较多需要根据具体情况科学配置)


##3.4.typeAliases
- 系统定义别名
- 自定义别名

##3.5.typeHandler类型处理器（javaType类型与JDBC类型）:其作用是将参数从javaType转化为jdbcType，或者从数据库取出结果时把jdbcType转化为JavaType

##3.6.ObjectFactory：当MyBatis在构建一个结果返回时，都会使用ObjectFactory区构建POJO,在MyBatis中可以定制自己的对象工厂

##3.7.插件


##3.8.environments配置环境
- 配置环境可以注册多个数据源，每一个数据源分为两大部分：一个是数据库源的配置。另一个是数据库事务的配置

##3.9.databaseIdProvider


##3.10.映射器（Mapper）
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Mapper%E5%8E%9F%E7%90%86.png)
- 定义映射器的接口
- 给出XML文件
- 引入映射器
	-  文件路径引入
	-  包名引入
	-  类注册引入
	-  用userMapper.xml引入

#四.映射器
##4.1.select元素
###4.1.1.传递参数（使用Map传递；@Param注解，参数较少的情况；JavaBean方式）
###4.1.2.resultMap映射结果集


##4.2.insert元素

##4.3.update元素和delete元素

##4.4.参数

##4.5.sql元素

##4.6.resultMap（结果集映射的映射关系）：定义映射规则、级联的更新、定制类型转化器
###4.6.1.级联查询

##4.7.缓存cache：[https://www.jianshu.com/p/c553169c5921](https://www.jianshu.com/p/c553169c5921)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBaits%E7%9A%84%E7%BC%93%E5%AD%98.png)
###4.7.1.一级缓存（只是相对于同一个SqlSession而言）：相当于针对一个连接
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBaits%E4%B8%80%E7%BA%A7%E7%BC%93%E5%AD%98%E6%89%A7%E8%A1%8C%E6%97%B6%E5%BA%8F%E5%9B%BE.png)
###4.7.2.二级缓存：实现二级缓存时，Mybatis要求返回的POJO必须可序列化
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Mybaits%E4%BA%8C%E7%BA%A7%E7%BC%93%E5%AD%98%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
###4.7.3.自定义缓存



#五.动态SQL
##5.1.if元素

##5.2.choose、when、otherwise元素

##5.3.trim、where、set元素

##5.4.foreach元素


##5.5.test元素


##5.6.bind元素

#六.MyBatis解析和运行原理
##6.1.创建SqlSessionFactory
- 通过XMLConfigBuilder解析配置的XML文件，读出配置参数，并将读取的数据存入Configuration类中
- 使用Configuration对象创建SqlSessionFactory，Mybatis提供了一个默认的SqlSessionFactory实现类，这种创建方式是一种Builder模式


##6.2.SqlSession运行过程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/SqlSession%E5%86%85%E9%83%A8%E8%BF%90%E8%A1%8C%E5%9B%BE.png)
###6.2.1.映射器的动态代理
###6.2.2.SqlSession四大对象
- Executor:执行器，调度StatementHandler、ParameterHandler、ResultHandler等执行SQL
	- SIMPLE
	- REUSE:一种执行器重用预处理语句
	- BATCH:执行器重用语句和批量更新 
- StatementHandler：使用数据库的Statement(PreparedStatement)执行操作
- ParameterHandler：用于SQL对参数的处理
- ResultHandler是进行最后数据集的封装返回结果



#七.插件（模板模式）
##7.1.映射器
- MappedStatement:保存映射器的一个节点（select|insert|delete|update）。包括配置的SQL、SQL的id、缓存信息等
- SqlSource:提供BoundSql对象的地方，是MappedStatement的一个属性
- BoundSql:建立SQL和参数的地方，主要有三个属性
	- parameterMappings：它是一个List,每一个元素都是ParameterMapping的对象，这个参数描述我们的参数
	- parameterObject:参数本身，可以传递简单对象，POJO,Map或者@Param注解参数
	- sql：书写在映射器里面的一条SQL 
##7.2.插件接口Interceptor
    public inter Interceptor{
		//覆盖拦截对象原有的方法，参数Invocation，通过它可以反射调度原来对象的方法
		Object intercept(Invocation invocation) throws Throwable;
		//target是被拦截的对象，其作用是给被拦截对象生成一个代理对象，并返回它
		Object plugin(Object target);
		//允许在plugin元素中配置所需参数，方法在插件初始化的时候就被调用一次，然后把插件对象存入到配置中，以便后面取出
		void setProperties(Properties properties);
	}


#八.补充
##8.1.数据库BLOB字段读写（对于文件操作）
##8.2.批量更新：一旦使用批量执行器，在默认情况下，在commit后才发生SQL到数据库
##8.3.调用存储过程
##8.4.分页分表
##8.5.上传文件到服务器

#九.Mybaits流程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Mybaits%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
- 通过Reader对象读取Mybatis映射文件
- 通过SqlSessionFactoryBuilder对象创建SqlSessionFactory对象
- 获取当前线程的SQLSession
- 事务默认开启
- 通过SQLSession读取映射文件中的操作编号，从而读取SQL语句
- 提交事务
- 关闭资源

#十.补充知识点
##10.1.传统JDBC编程
- 使用JDBC编程需要连接数据库，注册驱动和数据库信息
- 操作Connection，打开Statement对象
- 通过Statement执行SQL，返回结果到ResultSet对象
- 使用ResultSet读取数据，通过代码转化为具体的POJO对象
- 关闭数据库相关资源

##10.2.#和$的区别
- 设置的参数#（name）在大部分的情况下MyBatis会用创建预编译的语句，然后MyBatis为其设值；$(name)传递SQL语句本身，而不是SQL需要的参数，不安全






















