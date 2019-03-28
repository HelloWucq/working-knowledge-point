##1.不要使用==运算符测试字符串的相等性，==运算符只能确定两个字符串是否放置在同一个位置
##2.每次连接字符串都会构建一个新的string对象，费时费空间。使用StringBuilder类可以避免这个问题
##3.格式化输出
##4.对象和类
###4.1.一个对象变量没有实际包含一个对象，仅仅引用一个对象，显示将对象变量设置为null，表明这个对象变量目前没有引用任何对象
###4.2.不要在构造器中定义与实例域重名的局部变量
###4.3.静态域：如果将域定义为static，每个类中只有一个这样的域。而每一个对象对于所有的实例域却都有一份自己的拷贝
###4.4.静态方法是一种不能向对象实施操作的方法。建议使用类名，而不是对象来调用静态方法
###4.5.使用静态方法（1.一个方法不需要访问对象的状态，其所需的参数都是通过显示参数提供的；2.一个方法只需要访问类的静态域）
###4.6.使用静态工厂方法来构造对象
###4.7.Java程序设计总是采用按值调用
###4.8.Java中方法参数（1.一个方法不能修改一个基本数据类型的参数；2.一个方法可以改变一个对象参数的状态；3.一个方法不能让对象参数引用一个新的对象）
###4.9.初始化块
###4.10.类设计技巧（1.保证数据私有；2.对数据初始化；3.不要在类中使用过多的基本类型；4.不是所有的域都需要独立的访问域访问器和域更改器；5.将职责过多的类进行分解；6.类名和方法名要能够体现他们的职责；7.优先使用不可变的类）

##五.继承
###5.1.关键字this(1.引用隐式参数；2.调用该类其他的构造器)；super(1.调用超类的方法；2.调用超类的构造器)
###5.2.将一个类声明为final,只有其中的方法自动成为final,而不包括域
###5.3.只能在继承层次内进行类型转换；在将超类转化为子类之前，应该使用instanceof进行检查
###5.4.反射：能够分析类能力的程序
###5.5.Field,Method和Constructor


##六.接口
###6.1.回调：可以指出某个特定事件发生时应该采取的动作

##七.内部类
###7.1.内部类方法可以访问该类定义所在的作用域的数据，包括私有的数据；内部类可以对同一个包中的其他类隐藏起来；当想要定义一个回调函数且不想编写大量代码时
###7.2.内部类中声明的所有静态域都必须是final
###7.3.局部内部类
###7.4.匿名内部类：实现事件监听器和其他回调
###7.5.静态内部类：
##7.6.内部类的作用
###7.6.1.更好的封装性，因为内部类可以被访问限定符修饰，所以可以完全对外部其他类隐藏
###7.6.2.弥补Java单继承的问题，利用内部类，可以在类里利用内部类再继承其他的抽象类或实体类，提高代码的复用率
###7.6.3.内部类可以直接访问所在作用域中的其他数据，包括私有数据，这是直接新建一个普通类做不到的
###7.6.4.内部类可以简化代码，在Android中最常用的就是匿名内部类，例如注册Button的Onclick时，使用匿名内部类简化代码，不用再单独建一个类来实现抽象类或接口

##八.代理
###8.1.

##九.集合

##十.Java中的锁（https://www.jianshu.com/p/68ca420adf5a）
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E6%82%B2%E8%A7%82%E9%94%81%E4%B8%8E%E4%B9%90%E8%A7%82%E9%94%81%E7%9A%84%E5%8C%BA%E5%88%AB.jpg)

###10.1.公平锁：是指多个线程按照申请锁的顺序来获取锁
###10.2.非公平锁：是指多个线程获取锁的顺序并不是按照申请锁的顺序，有可能后申请的线程比先申请的线程优先获取锁。有可能，会造成优先级反转或者饥饿现象
###10.3.可重入锁：可重复可递归调用的锁，在外层使用锁之后，在内层仍然可以使用，并且不发生死锁（前提得是同一个对象或者class），这样的锁就叫做可重入锁
###10.4.不可重入锁
###10.5.独享锁 / 共享锁
###10.6.互斥锁 / 读写锁
###10.7.乐观锁 / 悲观锁
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/学习图片/乐观锁与悲观锁.png)
###10.8.我们一般有三种方式降低锁的竞争程度： 1、减少锁的持有时间 2、降低锁的请求频率 3、使用带有协调机制的独占锁，这些机制允许更高的并发性
###10.9.偏向锁 / 轻量级锁 / 重量级锁
###10.10.CAS问题
####10.10.1.ABA(版本号)
####10.10.2.循环时间长开销大
####10.10.3.只能保证一个共享变量的原子操作
###10.11.自旋锁与非自旋锁
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E8%87%AA%E6%97%8B%E9%94%81%E4%B8%8E%E9%9D%9E%E8%87%AA%E6%97%8B%E9%94%81.png)

#十一.基础知识
##11.1.this与super的区别
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/this%E4%B8%8Esuper%E7%9A%84%E5%8C%BA%E5%88%AB.png)
##11.2.类关系:类关系中的组合是一种完全绑定的关秀，生命周期一致；聚合是一种可以拆分的整体与部分的关系，是非常松散的暂时组合，部分可以被拆出来给另一个整体；依赖是除组合与聚合外的类与类之间的单向弱关系，使用另一个类的属性、方法。或以其作为方法的参数输入，或以其作为方法的返回值输出，依赖往往是模块解耦的最佳点；关联即是互相平等的依赖关系，可以在关联点上进行解耦

###11.2.1.继承（extends）:is-a
###11.2.2.实现（implement）：can-do
###11.2.3.组合：类是成员变量：contains-a
###11.2.4.聚合：类是成员变量：has-a
###11.2.5.依赖：是除组合与聚合外的单向弱关系。比如使用另一个类的属性、方法、或以其作为方法的参数输入，或以其作为方法的返回值输出（depends-a）
###11.2.6.关联：是互相平等的依赖关系（links-a）

##11.3.如何避免空指针
###11.3.1.字符串比较，常量放在前面
###11.3.2.初始化默认值
###11.3.3.返回空集合
###11.3.4.断言
###11.3.5.Optional

##11.4.Java的异常机制
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Java%E5%BC%82%E5%B8%B8%E6%9C%BA%E5%88%B6.png)

#十五.Java基础知识总结
##15.1.变量：内存中的一个存储空间，用于存储常量数据（数据类型；变量名称；变量的初始化值）
##15.2.创建一个对象在内存中做了什么
###15.2.1.先将硬盘上指定位置的.class文件加载进内存。
###15.2.2.执行main方法时，在栈内存中开辟了main方法的空间(压栈—进栈)，然后在main方法的栈区分配了一个变量p。
###15.2.3.在堆内存中开辟一个实体空间，分配了一个内存首地址值。new
###15.2.4.在该实体空间中进行属性的空间分配，并进行了默认初始化。
###15.2.5.对空间中的属性进行显示初始化。
###15.2.6.进行实体的构造代码块初始化。
###15.2.7.调用该实体对应的构造函数，进行构造函数初始化。
###15.2.8.将首地址赋值给p ，p变量就引用了该实体。(指向了该对象)
##15.3.当成员函数没用访问过对象中特定数据，那么这个方法需要被静态修饰；静态代码块、构造代码块、构造函数同时存在时的执行顺序：静态代码块 ———> 构造代码块 ———> 构造函数；
##15.4.可以可以通过一个关键字 instanceof ;//判断对象是否实现了指定的接口或继承了指定的类
##15.5.字符串
###15.5.1.字符串一旦被初始化，就不可以被改变，存放在方法区中的常量池中。
###15.5.2.StringBuffer字符串缓冲区，可以对字符串内容进行修改（线程安全，多线程操作）
###15.5.3.StringBuilder字符串缓冲区（线程不安全，单线程操作，效率高）
##15.6.流操作
###15.6.1.明确源与目的（数据源：就是需要读取，可以使用两个体系：InputStream、Reader；数据汇：就是需要写入，可以使用两个体系：OutputStream、Writer；）
###15.6.2.操作的数据是否是纯文本（如果是：数据源：Reader,数据汇：Writer;如果不是：数据源：InputStream,数据汇：OutputStream）
###15.6.3.明确操作的数据设备（数据源对应的设备：硬盘(File)，内存(数组)，键盘(System.in)；数据汇对应的设备：硬盘(File)，内存(数组)，控制台(System.out)）
##15.7.数据的序列化：静态数据不能被序列化，因为静态数据不在堆内存中，是存储在静态方法区中；如何将非静态数据不进行序列化，用transient关键字修饰
##15.8.反射技术
###15.8.1.反射技术可以对一个类进行解剖
###15.8.2.基本步骤（获得Class对象，就是获取到指定的名称的字节码文件对象；实例化对象，获取类的属性、方法或构造函数；访问属性、调用方法、调用构造函数创建对象）
###15.8.3.获取这个Class对象的方式（1.每一个数据类型都有一个静态的属性class，必须要先明确该类，主要用于传参；2.通过每个对象都具备的方法getClass获取，必须要创建该类对象，才可以调用getClass方法，用于获得对象的类型；3.使用的Class类中的方法，静态的forName方法，指定什么类名，就获取什么类字节码文件对象，扩展性强，用于类加载）

##16.软引用与强引用（软引用只在创建和FGC之前有效，FGC之后，不管有没有继续引用，都直接释放内存的一种机制）

##17.ThreadLocal基本知识(用于线程间的数据隔断)
- ThreadLocal 是多线程都需要使用一个变量，但是这个变量的值不需要各个线程间共享，每个线程都有自己的这个变量的值。
###17.1.每个Thread类都有一个ThreadLocalMap，每个ThreadLocalMap里面都有一个Entry[] table，每个Entry就是一个键值对，key就是ThreadLocal而且是软引用，Value是普通对象是强引用，由于value无法被回收，容易造成OOM，使用完ThreadLocal之后，记得调用remove方法。
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/ThreadLocal%E7%94%A8%E6%B3%95.png)
###17.2.ThreadLocal内部结构图
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/ThreadLocal%E5%86%85%E9%83%A8%E7%BB%93%E6%9E%84%E5%9B%BE.png)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/ThreadLocal%E5%86%85%E5%AD%98%E6%A8%A1%E5%9E%8B.png)
###17.3.ThreadLocal源码分析([https://juejin.im/post/5a5efb1b518825732b19dca4](https://juejin.im/post/5a5efb1b518825732b19dca4))
####17.3.1.成员属性
    public class ThreadLocal<T> {
    /**
     * 自定义哈希码(ThreadLocalMaps的),降低哈希冲突
     */
    	private final int threadLocalHashCode = nextHashCode();

    /**
     * 生成下一个哈希码的hashCode,操作是原子的,从0开始
     */
    	private static AtomicInteger nextHashCode =
        new AtomicInteger();

    /**
     * 连续分配的两个ThreadLocal实例的threadLocalHashCode值的增量
     */
    	private static final int HASH_INCREMENT = 0x61c88647;

    /**
     * 返回下一个哈希码的hashCode,此方法是一个原子类不停地去加上斐波那契散列数,使得哈希值分布均匀
     */
    	private static int nextHashCode() {
        	return nextHashCode.getAndAdd(HASH_INCREMENT);
    	}

	}
####17.3.2.方法
- ThreadLocal 实例本身是不存储值，它只是提供了一个在当前线程中找到副本值的key(它自己就是ThreadLocalMap的key)
- 是ThreadLocal包含在Thread中，而不是Thread包含在 ThreadLocal 中
		  
		/**
     	* 此方法第一次调用发生在当线程通过 {@link #get} 方法访问此线程的ThreadLocal值时
     	* 除非线程先调用了 {@link #set}，在这种情况下,{@code initialValue} 方法才不会被这个线程调用
     	* 通常情况下,每个线程最多调用1次这个方法,但是也可能再次调用,比如 {@link #remove} 被调用后,调用get
     	* 这个方法仅仅简单返回null{@code null}; 如果程序想要它返回除了null之外的初始值,必须继承重写此方法,
     	* 通常使用匿名内部类的方式实现
     	* @return 返回当前ThreadLocal的初始值 null
     	*/
    	protected T initialValue() {
       	 	return null;
    	}

    	/**
     	* 返回当前线程中保存ThreadLocal的值
     	* 如果当前线程没有此ThreadLocal则通过 {@link #initialValue}方法进行初始化值
     	* @return 
     	*/
    	public T get() {
        	// 获取当前线程对象
        	Thread t = Thread.currentThread();
        	// 获取此线程对象中维护的`ThreadLocalMap`对象
        	ThreadLocalMap map = getMap(t);
        	// 如果Map存在
        	if (map != null) {
           	 	// 以当前ThreadLocal实例对象为key获取对应的存储实体e
            	ThreadLocalMap.Entry e = map.getEntry(this);
            	// 找到对应的存储实体e
            	if (e != null) {
               		 @SuppressWarnings("unchecked")
               		 // 获取e对应的value值,也就是我们想要的当前线程对应此ThreadLocal的值
                	T result = (T)e.value;
                	return result;
            	}
        	}
        	// 如果map不存在,证明此线程没有维护此ThreadlocalMap对象,进行一波初始化操作
        	return setInitialValue();
    	}

   	 	/**
     	* set的变样实现,用于初始化值initialValue
     	* 用来代替防止用户重写set而无法初始化
     	* @return 返回初始化后的值
    	 */
    	private T setInitialValue() {
        	T value = initialValue();
        	Thread t = Thread.currentThread();
        	ThreadLocalMap map = getMap(t);
        	// 如果此map村咋洗,调用map,set设置此实体entry
       	 	if (map != null)
            	map.set(this, value);
        	else
        	 // map不存在时,调用此方法进行ThreadLocalMap对象初始化并将此entry作为第一个值放进去
            	createMap(t, value);
         	// 返回设置的value值s
        	return value;
    	}

    	/**
     	* 设置此线程对应的ThreadLocal的值,大多数子类不需要重写此方法,
    	 * 只需要依赖{@link #initialValue} 方法代替设置当前线程对应的ThreadLocal值
     	* @param 将要保存在当前线程对应的ThreadLocal值
     	* 1. 获取当前线程`Thread`对象,进而获取此线程对象中维护的`ThreadLocalMap`对象
     	*2. 判断当前的`ThreadLocalMap`是否存在,如果存在就直接调用map.set设置entry,如果不存在就调用`createMap`进行`ThreadLocalMap`对象的初始化,并将此实体`entry`作为第一个值存放到`ThreadLocalMap`中。
     	*/
    	public void set(T value) {
        	Thread t = Thread.currentThread();
        	// 获取当前线程的ThreadLocalMap实例
        	ThreadLocalMap map = getMap(t);
        	// 存在就设置此entry
        	if (map != null)
            	map.set(this, value);
        	else
        	// 不存在就进行对象初始化并设置entry作为第一个值存入ThreadLocalMap中
            	createMap(t, value);
    	}

    	/**
     	* 删除当前线程中保存ThreadLocal对应的实体entry
     	* 如果此ThreadLocal变量在当前线程中调用{@linkplain #get read} 方法
     	* 则会通过调用{@link #initialValue} 方法进行初始化
     	* 除非此值value是通过当前线程内置调用set方法设置
     	* 这可能导致在当前线程中多次调用initialValue方法初始化
    	 * 1. 获取当前线程Thread对象，进而获取此线程对象中维护的ThreadLocalMap对象。
     	* 2.  判断`ThreadLocalMap`是否存在,如果存在,调用map.remove,以当前的`ThreadLocal`为key删除对应的`entry`
     	*/
    	 public void remove() {
        	 // 获取当前线程Thread对象,进而获取此线程对象中维护的`ThreadLocalMap`对象
         	ThreadLocalMap m = getMap(Thread.currentThread());
        	if (m != null)
         	// 如果`ThreadLocalMap`存在调用remove方法删除之,当前ThreadLocal对象为key
             	m.remove(this);
     	}

    	/**
     	* 获取当前对象Thread对应维护的ThreadLocalMap
     	* @param  当前线程
     	* @return 对应的ThreadLocalMap
    	 */
    	ThreadLocalMap getMap(Thread t) {
       	 	return t.threadLocals;
    	}
####17.3.3.ThreadLocal的内部类ThreadLocalMap
- ThreadLocalMap 其内部利用 Entry 来实现 key-value 的存储，类似 HashMap 的结构 如下代码，从上面代码中可以看出 Entry 的 key 就是 ThreadLocal ，而value 就是值。

		static class Entry extends WeakReference<ThreadLocal<?>> {
     		/** The value associated with this ThreadLocal. */
    		 Object value;

    		 Entry(ThreadLocal<?> k, Object v) {
        		 super(k);
         		value = v;
     		}
 		}

		private void set(ThreadLocal<?> key, Object value) {

    		ThreadLocal.ThreadLocalMap.Entry[] tab = table;
    		int len = tab.length;

    		// 根据 ThreadLocal 的散列值，查找对应元素在数组中的位置
    		int i = key.threadLocalHashCode & (len-1);

    		// 采用“线性探测法”，寻找合适位置
    		for (ThreadLocal.ThreadLocalMap.Entry e = tab[i];
        		e != null;
        		e = tab[i = nextIndex(i, len)]) {

        		ThreadLocal<?> k = e.get();

       			 // key 存在，直接覆盖
        		if (k == key) {
            		e.value = value;
            		return;
        		}

        		// key == null，但是存在值（因为此处的e != null），说明之前的ThreadLocal对象已经被回收了
        		if (k == null) {
            		// 用新元素替换陈旧的元素
           			 replaceStaleEntry(key, value, i);
            		 return;
        		}
    		}

    		// ThreadLocal对应的key实例不存在也没有陈旧元素，new 一个
    		tab[i] = new ThreadLocal.ThreadLocalMap.Entry(key, value);

    		int sz = ++size;

    		// cleanSomeSlots 清楚陈旧的Entry（key == null）
    		// 如果没有清理陈旧的 Entry 并且数组中的元素大于了阈值，则进行 rehash
    		if (!cleanSomeSlots(i, sz) && sz >= threshold)
        		rehash();
		}

		/**用了开放定址法，所以当前key的散列值和元素在数组的索引并不是完全对应的，首先取一个探测数（key的散列值），如果所对应的key就是我们所要找的元素，则返回，否则调用getEntryAfterMiss()，如下：*/
		private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
    		Entry[] tab = table;
    		int len = tab.length;

    		while (e != null) {
        		ThreadLocal<?> k = e.get();
        		if (k == key)
            		return e;
        		if (k == null)
            		expungeStaleEntry(i);
        		else
            		i = nextIndex(i, len);
       			e = tab[i];
    		}
    		return null;
		}
- 为了避免内存的泄露,每次使用完 ThreadLocal 的时候都需要调用 remove() 方法来擦除数据。

##18.乐观锁与悲观锁
###18.1.悲观锁
###18.2.乐观锁：使用版本号机制和CAS算法实现

##19.Java中的null
###19.1.null不属于任何类型，可以被转换为任何类型，但是用instanceof永远返回false
###19.2.null永远不能和八大基本数据类型进行赋值运算等,否则不是编译出错,就是运行出错
###19.3.null可以和字符串进行运算
###19.4.同种类型的null,比较都返回true,null==null也返回true


##20.线程状态
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81.png)

##21.IO模型
###21.1.IO一般有两个阶段：（1）数据准备阶段，磁盘文件读入到内核空间。（2）数据拷贝阶段，内核空间拷贝到用户空间
###21.2.用户程序在IO数据准备阶段调用read()方法后，若内核未直接返回结果，就是阻塞；若直接返回结果，则是非阻塞；IO数据拷贝阶段若是用户程序负责，就是同步，若是操作系统负责，就是异步。
![](https://i.imgur.com/YL8mWUp.png)
![](https://i.imgur.com/h1hwV0r.png)

##22.线程池基本运作流程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%9F%BA%E6%9C%AC%E8%BF%90%E4%BD%9C%E6%B5%81%E7%A8%8B.png)
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%A4%84%E7%90%86%E6%B5%81%E7%A8%8B.png)

#23.Java中关键字学习
##23.1.Synchronized原理
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Synchronized%E5%8E%9F%E7%90%86.jpg)
###23.1.1.应用场景：保证线程安全，解决多线程中的并发同步问题（修饰 实例方法 / 代码块时，（同步）保护的是同一个对象方法的调用 & 当前实例对象；修饰 静态方法 / 代码块时，（同步）保护的是 静态方法的调用 & class 类对象）
##23.2.Synchronized是通过对象内部的一个叫做监视器锁（monitor）来实现的。但是监视器锁本质又是依赖于底层的操作系统的Mutex Lock来实现的。而操作系统实现线程之间的切换这就需要从用户态转换到内核态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么Synchronized效率低的原因。
###23.2.volatile：内存可见性，禁止指令重排序

###23.3.ThreadLocal内存模型
####23.3.1.当需要存储线程私有变量的时候，可以考虑使用ThreadLocal来实现
####23.3.2.当需要实现线程安全的变量时，可以考虑使用ThreadLocal来实现
####23.3.3.当需要减少线程资源竞争的时候，可以考虑使用ThreadLocal来实现
####23.3.4.注意Thread实例和ThreadLocal实例的生存周期，因为他们直接关联着存储数据的生命周期 
#24Java中的引用
##24.1.强引用
##24.2.软引用：描述一些有用当不是必需的对象，只有在内存不足的情况下JVM才会回收该对象，适合实现缓存
##24.3.弱引用：描述非必需对象，当JVM进行垃圾回收时，无论内存是否充足，都会回收被弱引用关联的对象
##24.4.虚引用：为了被虚引用关联的对象在被垃圾回收器回收时，能够收到一个系统通知。虚引用必须和引用队列关联使用，当垃圾回收器准备回收一个对象时,如果发现该对象具有虚引用，那么在回收之前会首先把该对象的虚引用加入到与之关联的引用队列中。

#25.反射的作用：Java反射机制是在运行状态中,对于任意一个类,都能够知道这个类的所有属性和方法;对于任意一个对象,都能够调用它的任意方法和属性;这种动态获取信息以及动态调用对象的功能称为Java语言的反射机制
###25.1.在运行时检测对象的类型；
###25.2.动态构造某个类的对象；
###25.3.检测类的属性和方法；
###25.4.任意调用对象的方法；
###25.5.修改构造函数、方法、属性的可见性等

#26.notify与notifyAll的区别
##26.1.Java中对象锁的模型：JVM会为一个使用内部锁（synchronized）的对象维护两个集合，Entry Set和Wait Set；
###26.1.1.对于Entry Set：如果线程A已经持有了对象锁，此时如果有其他线程也想获得该对象锁的话，它只能进入Entry Set，并且处于线程的BLOCKED状态；对于Wait Set：如果线程A调用了wait()方法，那么线程A会释放该对象的锁，进入到Wait Set，并且处于线程的WAITING状态；
###26.1.2.某个线程B想要获得对象锁，一般情况下有两个先决条件，一是对象锁已经被释放了（如曾经持有锁的前任线程A执行完了synchronized代码块或者调用了wait()方法等等），二是线程B已处于RUNNABLE状态
###26.1.3.对于Entry Set中的线程，当对象锁被释放的时候，JVM会唤醒处于Entry Set中的某一个线程，这个线程的状态就从BLOCKED转变为RUNNABLE。
###26.1.4.对于Wait Set中的线程，当对象的notify()方法被调用时，JVM会唤醒处于Wait Set中的某一个线程，这个线程的状态就从WAITING转变为RUNNABLE；或者当notifyAll()方法被调用时，Wait Set中的全部线程会转变为RUNNABLE状态。所有Wait Set中被唤醒的线程会被转移到Entry Set中
###26.1.5.每当对象的锁被释放后，那些所有处于RUNNABLE状态的线程会共同去竞争获取对象的锁，最终会有一个线程（具体哪一个取决于JVM实现，队列里的第一个？随机的一个？）真正获取到对象的锁，而其他竞争失败的线程继续在Entry Set中等待下一次机会。
##26.2.为什么要把wait()方法放在循环而不是if判断里?
##26.2.答：因为wait()的线程永远不能确定其他线程会在什么状态下notify()，所以必须在被唤醒、抢占到锁并且从wait()方法退出的时候再次进行指定条件的判断，以决定是满足条件往下执行呢还是不满足条件再次wait()呢


#27.Object中的方法（[https://segmentfault.com/a/1190000014710646](https://segmentfault.com/a/1190000014710646)）
###27.1.clone()
###27.2.getClass()获取对象的class
###27.3.equals() 对象值比较,重写equals方法必须重写hashcode，对象的约定，例如不重写，hashMap的kv不一致；
###27.4.hashCode() 对象的hash值
###27.5..tostring() 默认方法是 包名@改对象的hashCode十六进制表示
###27.6.notify() 线程唤起
###27.7.notifyall() 线程全部唤起
###27.8.wait() 线程等待
###27.9.finalize() 基本没啥用，垃圾回收前调用的方法


##注意
- equals（）用来判断两个对象是否相等
	1. 若某各类没有覆盖equals()方法，当通过equals()比较两个对象时，实际上是比较两个对象是否是一个对象
	2. 当覆盖类的equals()方法时，使用equals()比较两个对象是否相等，通常的做法是：若两个对象的内容相等，则equals()方法放回true，否则返回false
- hashCode()确定该对象在哈希表中的索引位置（只有在散列表中才有用，在其他情况下没用）
	1. 如果两个对象相等，那么它们的hashCode()值一定要相同；
	2. 如果两个对象hashCode()相等，它们并不一定相等。注意：这是在散列表中的情况。在非散列表中一定如此！










#28.Java的对象
##28.1.Java的创建过程
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/Java%E7%9A%84%E5%88%9B%E5%BB%BA%E8%BF%87%E7%A8%8B.png)
##28.2.对象的内存布局（对象头+实例数据+对齐填充）

#29.Java的线程池
##29.0.目标
###29.0.1.线程池通过减少每次做任务的时候产生的性能消耗来优化执行大量的异步任务的时候的系统性能
###29.0.2.线程池还提供了限制和管理批量任务被执行的时候消耗的资源、线程的方法。另外ThreadPoolExecutor还提供了简单的统计功能，比如当前有多少任务被执行完了。

##29.1.作用
###29.1.1.降低资源消耗
###29.1.2.提高响应速度
###29.1.3.提高线程的可管理性

##29.2.工作顺序：corePoolSize -> 任务队列 -> maximumPoolSize -> 拒绝策略
##29.3.使用方式
###29.3.1.避免使用无界队列
###29.3.2.明确拒绝任务是的行为
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/%E7%BA%BF%E7%A8%8B%E6%B1%A0%E6%8B%92%E7%BB%9D%E7%AD%96%E7%95%A5.png)
###29.3.3.获取处理结果和异常
##29.4.创建和开启线程的开销：为线程栈分配内存，保存每个线程方法调用的栈帧；每个栈帧包括本地变量数组、返回值、操作栈和常量池；一些 JVM 支持本地方法，也将分配本地方法栈；每个线程获得一个程序计数器，标识处理器正在执行哪条指令；系统创建本地线程，与 Java 线程对应；和线程相关的描述符被添加到 JVM 内部数据结构；线程共享堆和方法区

#30.分布式环境中，任意时刻考虑3种状态：成功、失败、超时。





#31.Java代码优化
###31.1.尽量避免随意使用静态变量
###31.2.尽量避免过多的创建Java对象
###31.3.尽量使用final修饰符
#32.sleep()、yield()和wait()的区别
![](https://github.com/HelloWucq/working-knowledge-point/raw/master/%E5%AD%A6%E4%B9%A0%E5%9B%BE%E7%89%87/sleep()%E3%80%81yield()%20%E5%92%8C%20wait()%20%E7%9A%84%E5%8C%BA%E5%88%AB.png)

#32.CAS：CAS有三个操作数：内存值V、旧的预期值A、要修改的值B，当且仅当预期值A和内存值V相同时，将内存值修改为B并返回true，否则什么都不做并返回false。


#33.AQS:抽象队列同步器
##33.1.使用一个整型的volatile变量（命名为state）来维护同步状态，通过内置的FIFO队列来完成资源获取线程的排队工作。



#注意点
##1.记录异常日志的7条规则
###1.1.记录技术性异常而不是用户异常
###1.2.把数据存储在异常中以便于记录
###1.3.记录异常的描述信息
###1.4.输出导致异常的原因
###1.5.选择合理的日志级别
###1.6.不要记录后又重新向外抛出
###1.7.不要使用System.out或者System.err记录
##2.Java对象内存结构