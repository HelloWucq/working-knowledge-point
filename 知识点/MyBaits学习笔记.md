#一.简介
##1.1.ORM（对象关系映射）:数据库的表和简单的Java对象的映射关系模型，解决数据库数据和POJO对象的相互映射
##1.2.Hibernate的缺点
###1.2.1.全表映射带来的不便，比如更新时需要发送所有的字段
###1.2.2.无法根据不同的条件组装不同的SQL
###1.2.3.对多表关联和复杂SQL查询支持较差，需要自己写SQL，返回后，需要自己将数据组装成POJO
###1.2.4.不能有效支持存储过程
###1.2.5.虽有HQL,但性能较差。
##1.3.半自动映射框架（SQL,映射规则，POJO）


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
###3.2.1.property子元素
###3.2.2.properties配置文件
###3.2.3.程序参数传递


##3.3.settings设置(配置内容较多需要根据具体情况科学配置)


##3.4.typeAliases
###3.4.1.系统定义别名
###3.4.2.自定义别名


##3.5.typeHandler类型处理器（javaType类型与JDBC类型）

##3.6.ObjectFactory

##3.7.插件


##3.8.environments配置环境
###3.8.1.配置环境可以注册多个数据源，每一个数据源分为两大部分：一个是数据库源的配置。另一个是数据库事务的配置

##3.9.databaseIdProvider


##3.10.映射器（Mapper）
###3.10.1.定义映射器的接口
###3.10.2.给出XML文件
###3.10.3.引入映射器
####3.10.3.1.文件路径引入
####3.10.3.2.包名引入
####3.10.3.3.类注册引入
####3.10.3.4.用userMapper.xml引入


#四.映射器
##4.1.select元素
###4.1.1.传递参数（使用Map传递；@Param注解，参数较少的情况；JavaBean方式）
###4.1.2.resultMap映射结果集


##4.2.insert元素

##4.3.update元素和delete元素

##4.4.参数

##4.5.sql元素

##4.6.resultMap结果映射集
###4.6.1.级联查询

##4.7.缓存cache：[https://www.jianshu.com/p/c553169c5921](https://www.jianshu.com/p/c553169c5921)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/MyBaits%E7%9A%84%E7%BC%93%E5%AD%98.png)
###4.7.1.一级缓存（只是相对于同一个SqlSession而言）
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
###6.1.1.通过XMLConfigBuilder解析配置的XML文件，读出配置参数，并将读取的数据存入Configuration类中
###6.1.2.使用Configuration对象创建SqlSessionFactory，Mybatis提供了一个默认的SqlSessionFactory实现类，这种创建方式是一种Builder模式


##6.2.SqlSession运行过程
###6.2.1.映射器的动态代理
###6.2.2.SqlSession四大对象
####6.2.2.1.Executor:执行器，调度StatementHandler、ParameterHandler、ResultHandler等执行SQL
####6.2.2.2.StatementHandler：使用数据库的Statement(PreparedStatement)执行操作
####6.2.2.3.ParameterHandler：用于SQL对参数的处理
####6.2.2.4.ResultHandler是进行最后数据集的封装返回结果



#七.插件



#八.补充
##8.1.数据库BLOB字段读写（对于文件操作）
##8.2.批量更新：一旦使用批量执行器，在默认情况下，在commit后才发生SQL到数据库
##8.3.调用存储过程
##8.4.分页分表
##8.5.上传文件到服务器

#九.Mybaits流程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Mybaits%E6%B5%81%E7%A8%8B%E5%9B%BE.png)
























