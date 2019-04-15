#一.对象池[https://blog.csdn.net/liuxiao723846/article/details/78881040](https://blog.csdn.net/liuxiao723846/article/details/78881040)
- 对于那些被频繁使用的对象，在使用完后，不立即将它们释放，而是将它们缓存起来，以供后续的应用程序重复使用，从而减少创建对象和释放对象的次数，进而改善应用程序的性能。事实上，由于对象池技术将对象限制在一定的数量，也有效地减少了应用程序内存上的开销。
##1.1.核心原理：缓存和共享

#二.Java和操作系统交互细节[https://my.oschina.net/u/4106059/blog/3029071](https://my.oschina.net/u/4106059/blog/3029071)

#三.反射技术
##3.1.简介
1. 定义：Java语言中一种动态（运行时）访问、检测以及修改它本身的能力
2. 作用：动态（运行时）获取类的完整结构信息以及调用对象的方法
> 类的结构信息包括：变量、方法等
>
> 正常情况下，Java类在编译前，就已经被加载到JVM中；而反射机制使得程序运行时还可以动态地去操作类的变量、方法等信息

##3.2.特点
###3.2.1.优点：灵活性高。反射属于动态编译，只有到运行时才动态创建以及获取对象实例
###3.2.2.缺点：执行效率低，反射的操作主要通过JVM执行，时间成本会高于直接执行相同的操作；容易破坏类结构，因为反射操作绕过了源码，容易干扰类原有的内部逻辑
> 因为接口的通用性，Java的invoke方法是传object和object[]数组的。基本类型参数需要装箱和拆箱，产生大量额外的对象和内存开销，频繁促发GC。
> 
> 编译器难以对动态调用的代码提前做优化，比如方法内联。
> 
> 反射需要按名检索类和方法，有一定的时间开销

##3.3.应用场景
1. 动态获取 类文件结构信息（如变量、方法等） & 调用对象的方法
2. 常用的需求场景有：动态代理、工厂模式优化、Java JDBC数据库操作等
##3.4.具体使用
###3.4.1.实现手段
1. 主要通过操作java.lang.Class类
> 在Java程序运行时，Java虚拟机为所有类型维护一个java.lang.Class对象
> 
> 该Class对象存放着所有关于该对象的 运行时信息
> 
> 泛型形式为Class<T>
##3.5.使用步骤
1. 获取目标类型的Class对象

    	// 获取 目标类型的`Class`对象的方式主要有4种

		<-- 方式1：Object.getClass() -->
    	// Object类中的getClass()返回一个Class类型的实例 
   		Boolean carson = true; 
    	Class<?> classType = carson.getClass(); 
    	System.out.println(classType);
    	// 输出结果：class java.lang.Boolean  

		<-- 方式2：T.class 语法    -->
    	// T = 任意Java类型
    	Class<?> classType = Boolean.class; 
    	System.out.println(classType);
    	// 输出结果：class java.lang.Boolean  
    	// 注：Class对象表示的是一个类型，而这个类型未必一定是类
    	// 如，int不是类，但int.class是一个Class类型的对象

		<-- 方式3：static method Class.forName   -->
    	Class<?> classType = Class.forName("java.lang.Boolean"); 
    	// 使用时应提供异常处理器
    	System.out.println(classType);
    	// 输出结果：class java.lang.Boolean  

		<-- 方式4：TYPE语法  -->

   	 	Class<?> classType = Boolean.TYPE; 
    	System.out.println(classType);
    	// 输出结果：boolean 
2. 通过Class对象分别获取Constructor类对象、Method类对象以及Field类对象

		// 即以下方法都属于`Class` 类的方法。

		<-- 1. 获取类的构造函数（传入构造函数的参数类型）->>
  		// a. 获取指定的构造函数 （公共 / 继承）
  		Constructor<T> getConstructor(Class<?>... parameterTypes)
  		// b. 获取所有的构造函数（公共 / 继承） 
  		Constructor<?>[] getConstructors(); 
  		// c. 获取指定的构造函数 （ 不包括继承）
  		Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes) 
  		// d. 获取所有的构造函数（ 不包括继承）
 		 Constructor<?>[] getDeclaredConstructors(); 
		// 最终都是获得一个Constructor类对象

		// 特别注意：
  		// 1. 不带 "Declared"的方法支持取出包括继承、公有（Public） & 不包括有（Private）的构造函数
  		// 2. 带 "Declared"的方法是支持取出包括公共（Public）、保护（Protected）、默认（包）访问和私有（Private）的构造方法，但不包括继承的构造函数
  

		<--  2. 获取类的属性（传入属性名） -->
  		// a. 获取指定的属性（公共 / 继承）
   		Field getField(String name) ;
  		// b. 获取所有的属性（公共 / 继承）
   		Field[] getFields() ;
  		// c. 获取指定的所有属性 （不包括继承）
  		 Field getDeclaredField(String name) ；
  		// d. 获取所有的所有属性 （不包括继承）
   		Field[] getDeclaredFields() ；
		// 最终都是获得一个Field类对象

		<-- 3. 获取类的方法（传入方法名 & 参数类型）-->
  		// a. 获取指定的方法（公共 / 继承）
   		Method getMethod(String name, Class<?>... parameterTypes) ；
  		// b. 获取所有的方法（公共 / 继承）
  		 Method[] getMethods() ；
  		// c. 获取指定的方法 （ 不包括继承）
   		Method getDeclaredMethod(String name, Class<?>... parameterTypes) ；
  		// d. 获取所有的方法（ 不包括继承）
   		Method[] getDeclaredMethods() ；
		// 最终都是获得一个Method类对象

		<-- 4. Class类的其他常用方法 -->
		getSuperclass(); 
		// 返回父类

		String getName(); 
		// 作用：返回完整的类名（含包名，如java.lang.String ） 
 
		Object newInstance(); 
		// 作用：快速地创建一个类的实例
		// 具体过程：调用默认构造器（若该类无默认构造器，则抛出异常 
		// 注：若需要为构造器提供参数需使用java.lang.reflect.Constructor中的newInstance（）

3. 通过Constructor类对象、Method类对象以及Field类对象分别获取类的构造函数、方法以及属性的具体信息，并进行后续操作

		'''// 即以下方法都分别属于`Constructor`类、`Method`类 & `Field`类的方法。

		<-- 1. 通过Constructor 类对象获取类构造函数信息 -->
  		String getName()；// 获取构造器名
  		Class getDeclaringClass()；// 获取一个用于描述类中定义的构造器的Class对象
 		int getModifiers()；// 返回整型数值，用不同的位开关描述访问修饰符的使用状况
 		 Class[] getExceptionTypes()；// 获取描述方法抛出的异常类型的Class对象数组
  		Class[] getParameterTypes()；// 获取一个用于描述参数类型的Class对象数组

		<-- 2. 通过Field类对象获取类属性信息 -->
 		String getName()；// 返回属性的名称
  		Class getDeclaringClass()； // 获取属性类型的Class类型对象
  		Class getType()；// 获取属性类型的Class类型对象
  		int getModifiers()； // 返回整型数值，用不同的位开关描述访问修饰符的使用状况
  		Object get(Object obj) ；// 返回指定对象上 此属性的值
  		void set(Object obj, Object value) // 设置 指定对象上此属性的值为value
 
		<-- 3. 通过Method 类对象获取类方法信息 -->
  		String getName()；// 获取方法名
 		 Class getDeclaringClass()；// 获取方法的Class对象 
  		int getModifiers()；// 返回整型数值，用不同的位开关描述访问修饰符的使用状况
  		Class[] getExceptionTypes()；// 获取用于描述方法抛出的异常类型的Class对象数组
 		 Class[] getParameterTypes()；// 获取一个用于描述参数类型的Class对象数组

		<--额外：java.lang.reflect.Modifier类 -->
		// 作用：获取访问修饰符

		static String toString(int modifiers)   
		// 获取对应modifiers位设置的修饰符的字符串表示

		static boolean isXXX(int modifiers) 
		// 检测方法名中对应的修饰符在modifiers中的值

##3.6.注意：访问权限问题
###3.6.1.背景：无法访问（private）私有的方法、字段
###3.6.2.冲突：Java安全机制只允许查看任意对象有哪些域，而不允许读取他们的值，若强制读取，将抛出异常
###3.6.3.解决方案：脱离Java程序中安全管理器的控制、屏蔽Java语言的访问检查，从而脱离访问控制


#4.内部类：定义在另一个类中的类
- 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据
- 内部类可以对同一个包中的其他类隐藏起来
- 当想定义一个回调函数且不想编写大量代码时，使用匿名内部类比较便捷

##4.1.成员内部类
- 虽然成员内部类可以无条件地访问外部类的成员，而外部类想访问成员内部类的成员却不是这么随心所欲了。在外部类中如果要访问成员内部类的成员，必须先创建一个成员内部类的对象，再通过指向这个对象的引用来访问
- 成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象。创建成员内部类对象的一般方式如下：

    	//第一种方式：
        Outter outter = new Outter();
        Outter.Inner inner = outter.new Inner();  //必须通过Outter对象来创建
         
        //第二种方式：
        Outter.Inner inner1 = outter.getInnerInstance();
##4.2.局部内部类：局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内。

##4.3.匿名内部类
- 匿名内部类是唯一一种没有构造器的类。正因为其没有构造器，所以匿名内部类的使用范围非常有限，大部分匿名内部类用于接口回调
- 创建格式

    
		new 父类构造器（参数列表）|实现接口（）  
   	 	{  
    	 //匿名内部类的类体部分  
   	 	}
- 匿名内部类仅能被使用一次，创建匿名内部类是他会立即创建一个该类的实例，该类的定义会立即消失，所以匿名内部类不能够重复使用
- 我们给匿名内部类传递参数的时候，若该形参在内部类中需要被使用，那么该形参必须要为final。也就是说：当所在的方法的形参需要被内部类里面使用时，该形参必须为final。
##4.4.静态内部类：静态内部类是不需要依赖于外部类的，这点和类的静态成员属性有点类似，并且它不能使用外部类的非static成员变量或者方法，这点很好理解，因为在没有外部类的对象的情况下，可以创建静态内部类的对象，如果允许访问外部类的非static成员就会产生矛盾，因为外部类的非static成员必须依附于具体的对象




#5.代理
- 代理类是在程序运行过程中创建的，一旦创建就变成了常规类，与虚拟机中的任何其他类没有区别
##5.1.静态代理
##5.2.动态代理
- java.lang.reflect.Proxy：这是 Java 动态代理机制的主类，它提供了一组静态方法来为一组接口动态地生成代理类及其对象。
- Proxy的静态方法
	-  // 方法 1: 该方法用于获取指定代理对象所关联的调用处理器
static InvocationHandler getInvocationHandler(Object proxy) 
	
	-  // 方法 2：该方法用于获取关联于指定类装载器和一组接口的动态代理类的类对象
static Class getProxyClass(ClassLoader loader, Class[] interfaces) 
 
	- // 方法 3：该方法用于判断指定类对象是否是一个动态代理类
static boolean isProxyClass(Class cl) 
 
	- // 方法 4：该方法用于为指定类装载器、一组接口及调用处理器生成动态代理类实例
static Object newProxyInstance(ClassLoader loader, Class[] interfaces, 
    InvocationHandler h)
- java.lang.reflect.InvocationHandler：这是调用处理器接口，它自定义了一个 invoke 方法，用于集中处理在动态代理类对象上的方法调用，通常在该方法中实现对委托类的代理访问。
-  InvocationHandler 的核心方法

	- // 该方法负责集中处理动态代理类上的所有方法调用。第一个参数既是代理类实例，第二个参数是被调用的方法对象
// 第三个方法是调用参数。调用处理器根据这三个参数进行预处理或分派到委托类实例上发射执行
Object invoke(Object proxy, Method method, Object[] args)
- java.lang.ClassLoader：这是类装载器类，负责将类的字节码装载到 Java 虚拟机（JVM）中并为其定义类对象，然后该类才能被使用。Proxy 静态方法生成动态代理类同样需要通过类装载器来进行装载才能使用，它与普通类的唯一区别就是其字节码是由 JVM 在运行时动态生成的而非预存在于任何一个 .class 文件中。
- 具体步骤
	- 通过实现 InvocationHandler 接口创建自己的调用处理器；
	- 通过为 Proxy 类指定 ClassLoader 对象和一组 interface 来创建动态代理类；
	- 通过反射机制获得动态代理类的构造函数，其唯一参数类型是调用处理器接口类型；
	- 通过构造函数创建动态代理类实例，构造时调用处理器对象作为参数被传入。

#6.创建和销毁对象
- 考虑使用静态工厂方法代替构造器
	- 它们有名称
	- 不必在每次调用它们的时候都创建一个新对象
	- 可以返回原返回类型的任何子类型的对象
	- 创建参数化类型实例的时候，代码更加简洁
	- 类如果不含公有的或者受保护的构造器，就不能被子类化
	- 它们与其他的静态方法实际上没有任何区别

- 遇到多个构造器参数时考虑使构建器
- 用私有构造器或者枚举类型强化Singleton属性
- 通过私有构造器强化不可实例化的能力
- 避免创建不必要的对象
- 消除过期的对象引用：
	- 只要类是自己管理内存，就应该警惕内存泄漏问题，一旦元素被释放掉，则该元素中包含的任何对象引用都应该被清空
	- 缓存对象
	- 监听器或其他回调

- 避免使用终结方法

#7.所有对象都通用的方法
- 覆盖equals时遵守的通用约定
	- 自反性
	- 对称性
	- 传递性
	- 一致性


> 使用==操作符检查参数是否为这个对象的引用
> 
> 使用instanceof操作符检查参数是否为正确的类型
> 
> 把参数转换为正确的类型
> 
> 对该类中的每个关键域，检查参数中的域是否与该对象中对应的域匹配 

> 覆盖equals时总要覆盖hashCode
> 
> 
> 不要企图让equals方法过于智能
> 
> 不要将equals声明中的Object对象替换为其他的类型

- 覆盖equals时总要覆盖hashCode

> hashCode（）在散列表中才有用，在其他情况下没用
>  
- 始终覆盖toString方法
- 谨慎的覆盖clone

#8.类和接口
- 使类和成员的可访问性最小化
- 在公有类中使用访问方法而不是公有域
- 使可变性最小化
	- 不要提供任何会修改对象状态的方法
	- 保证类不会被扩展
	- 使所有域都是final的 
	- 使所有域都成为私有的
	- 确保对于任何可变组件的互斥访问

- 复合优先于继承
- 要么为继承为设计，并提供文档说明，要么就禁止继承
- 接口优于抽象类
- 接口只用于定义类型
- 类层次优于标签类
- 用函数对象表示策略
- 优先考虑静态成员类

#9.泛型
#10.枚举和注解
- 用enum代替int常量
- 用实例域代替序数
- 用EnumSet代替位域
- 用EnumMap代替序数索引
- 用接口模拟可伸缩的枚举
- 注解优先于命名模式
- 坚持使用Override注解

#11.方法
- 检查参数的有效性
- 必要时进行保护性拷贝
- 谨慎设计方法签名
	- 谨慎的选择方法的名称
	- 不要过于追求提供便利的方法
	-  避免过长的参数列表

- 慎用重载
- 慎用可变参数
- 返回零长度的数组或者集合，而不是null
- 为所有导出的API元素编写文档注释

#12.通用程序设计
- 将局部变量的作用域最小化
- for-each循环优先于传统的for循环
- 了解和使用类库
- 如果需要精确的答案，避免使用float和double
- 基本类型优先于装箱基本类型
- 如果其他类型更加合适，尽量避免使用字符串
- 当心字符串连接的性能
- 通过接口引用对象
	- 应该使用接口而不是类作为参数的类型
	- 如果有合适的接口类型存在，那么对于参数、返回值、变量和域来说，就应该使用接口类型进行声明 
	- 如果没有合适的接口存在，完全可以用类而不是接口类来引用对象

- 接口优先于反射机制
- 谨慎使用本地方法
- 谨慎的进行优化
- 遵守普遍接受的命名惯例

#13.异常
- 之针对异常的情况才使用异常
- 对可恢复的情况使用受检异常，对编程错误使用运行时异常
- 避免不必要的使用受检的异常
- 优先使用标准异常
- 抛出与抽象相对应的异常
- 每个方法抛出异常都要有文档
- 在细节消息中包含能够捕获失败的信息
- 努力是失败保持原子性
- 不要忽略异常
- Exception与Error的区别
	- Exception是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理
	- Error指正常情况下，不大可能出现的情况，绝大部分的Error都会导致程序处于非正常、不可恢复状态，不便于也不需要捕获
	- Exception分为可检查异常和不可检查异常，可检查异常在源码里必须显示的进行捕获处理，编译检查的一部分
	- 不检查异常就是运行时异常，类似NullPointException、ArrayIndexOutOfBoundsException之类，通常可以编码避免的逻辑错误，不会在编译期强制要求

> try-catch代码段会产生额外的性能开销，往往会影响JVM对代码进行优化，所以建议捕获必要的代码段，尽量不要一个大的try包住整段代码
> 
> Java每实例化一个Exception，都会对当时的栈进行快照，这是一个相对比较重的操作

#14.final、finally、finalize的区别
- final可以用来修饰类、方法、变量，final修饰的class代表不可以继承扩展，final的变量不可以修改，final的方法也是不可以重写的
	- 使用final修饰参数或者变量，可以清楚地避免意外复制导致的编程错误，甚至，有人明确推荐将所有方法参数、本地变量、成员变量声明成final
	- final变量产生了某种程度的不可变的效果，可以用于保护只读数据，尤其在并发编程中，可以明确不能再赋值final变量，有利于减小额外的同步开销，可以省去一些防御性拷贝的必要
	- 利用final可能有助于JVM将方法内联等
- finally则是Java保证重点代码一定要被执行的一种机制，可以使用try-finally或者try-catch-finally来进行类似关闭JDBC连接、保证unlock锁等动作
- finalize是基础类Object的一个方法，它的设计目的是保证对象被垃圾收集前完成特定资源的回收

#15.各种引用的使用
> 不同的引用类型，主要体现在对象不同的可达性状态和对垃圾收集的影响

> 强引用
> 
> 软引用
> 
> 弱引用
> 
> 幻象引用：确保对象finalize以后，做某些事情的机制，比如，通常用来做所谓的Post-Mortem清理机制，也有人利用幻象引用监控对象的创建和销毁