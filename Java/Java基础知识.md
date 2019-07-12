# Java基础知识

## Java基础
#### 1.Java内存模型
	Java内存模型(Java Memory Model，简称JMM)的主要目标是定义程序中各个变量的访问规则，即在虚拟机中将变量存储到内存和从内存中取出变量这样底层细节。此处的变量与Java编程时所说的变量不一样，指包括了实例字段、静态字段和构成数组对象的元素，但是不包括局部变量与方法参数，后者是线程私有的，不会被共享。
#### 2.为什么重写`equals()`还要重写`hashcode()`?
	HashMap中要比较key是否相等，要同时使用这两个函数。因为自定义的类的hashcode()方法继承于Object类，其hashcode码为默认的内存地址，这样即便有相同含义的两个对象，比较也是不相等的。在HashMap中比较key,首先求key的hashCode(),比较其值是否相等，若相等再比较equals()，若相等则认为是相等的，若equals()不相等则认为不相等。若只重写hashcode()不重写equals(),当比较equals()时只是判断它们是否是同一对象（仅比较内存地址）
#### 3.`Object`类的`hashcode()`不重写的话，`hashcode()`如何计算的
	该方法是本地方法，即用C/C++语言实现的，该方法直接返回对象的内存地址
#### 4. `==`和`equals()`比较的各是什么
	"=="对比两个对象基于内存引用，当两个对象的引用相同（指向同一个对象）时，为true,否则为false。"=="如果比较基本类型，就是比较数值是否相等。不重写的情况下，equals()比较的是对象的地址
#### 5.一个十进制数在内存中是怎么存的
	补码的形式存储

## 关键字
#### 6.`volatile`关键字
	它是一个类型修饰符，是被设计用来修饰被不同线程访问和修改的变量。是用来保证有序性和可见性的。
	被volatile修饰的变量，系统每次用到它时都会直接从对应的内存中提取，而不会利用到缓存。
	总之，在使用了volatile修饰成员变量后，所有线程在任何时候所看到的变量的值都是相同的。
	volatile变量规则：对一个变量的写操作先行发生于后面对这个变量的读操作
#### 7.`synchronized`和`lock`
	synchronized是Java的关键字，当它用来修饰一个方法或者代码块的时候，能够保证在同一时刻最多只有一个线程执行代码。
	Lock是一个接口。
	利用synchronized实现同步的基础：Java中每一个对象都可以作为锁，具体表现为：
	①对于普通的同步方法，锁是当前实例对象
	②对于静态同步方法，锁是当前类的Class对象，即class.Class
	③对于同步方法块，锁是synchronized括号里配置的对象
	两者的区别：
	①synchronized是托管给JVM执行的，而Lock的锁定是通过代码实现的，Lock需要显示地指定初始位置和终止位置。
	②性能不同：在资源竞争不是很激烈的情况下，synchronized的性能要优于ReetrantLock。当资源竞争很激烈的情况下，synchronized性能下降很快，而 ReetrantLock性能基本不变
	③锁机制不同：synchronized获得锁和释放锁都是在块结构中，当获取多个锁时，必须以相反的顺序释放，并且是自动解锁。Lock需要自己手动的释放锁，必须在finally块中释放，否则会引起死锁。
	
## 面向对象
#### 8.不可变对象什么意思
	不可变对象是指一个对象的状态在对象创建之后就不再变化。不可改变的意思就是说：不能改变对象内的成员变量，包括基本类型数值也不能改变，引用类型的变量不能指向其他的对象，引用类型指向的对象的状态也不能改变。
#### 9.如何通过反射创建对象
	①通过类对象调用newInstance()方法，例如：String.class.newInstance();
	②通过类的getConstructor()或getDeclaredConstructor()方法获得构造器 例如：String.class.getConstructor(String.class).newInstance("hello);

## 集合部分
#### 10.`HashMap`底层实现
	在JDK1.6, JDK1.7中，HashMap采用位桶+链表实现，使用链表处理冲突。JDK1.8中HashMap采用位桶+链表+红黑树实现。当链表元素大于8时，链表转换成树结构
	若桶中的链表元素个数小于等于6时，树结构还原成链表。
	实现原理：首先有一个数组，每个元素都是链表。当添加一个元素（k-v)时，就首先计算key的hashcode，以确定插入数组中的位置。若同一hash值的元素已经被放
	置，这时候就添加到同一hash值的元素的后面，它们在数组同一位置，但是形成了链表。当链表长度过大，转化为红黑树，提高了查找效率。
#### 11.若`HashMap`的key是一个自定义的类，怎么办？
	必须重写hashcode()跟equals()
#### 12.`TreeMap`底层，红黑树原理
	TreeMap的实现就是红黑树的数据结构，也就是说是一棵自平衡的二叉树
	红黑树性质：
	①每个节点要么是红色，要么是黑色
	②根节点永远是黑色
	③所有的叶节点都是空节点(null)，并且是黑色的
	④每个红色节点的两个子节点都是黑色
	⑤从任一节点到其子树中每个叶子节点的路径都包含相同数量的黑色节点
#### 13.`concurrentHashMap`底层实现
	concurrentHashMap是线程安全的。JDK1.7中，concurrentHashMap采用Segment数组+HashEntry数组的方式实现的，lock加在Segment上面。Segment是一种可重入锁，HashEntry则用于
	存储键值对数据。一个concurrentHashMap包含一个Segment数组，它的结构类似HashMap,是一种数组和链表结构。一个Segment数组里包含一个HashEntry数组，每个HashEntry是一个
	链表结构的元素，每个Segment守护一个HashEntry里的元素。当对HashEntry数组的数据进行修改时，首先必须获得与它对应的Segment锁。
	JDK1.8中，concurrentHashMap采用Node+CAS+synchronized来实现。JDK1.8中使用一个volatile类型的变量baseCount记录元素的个数，当插入数据或者删除数据时，会通过
	addCount()方法更新baseCount,通过累加baseCount和counterCells数组的数量，即可得到元素总个数。
#### 14.`Collection`跟`Collections`的区别
	Collection是集合类的上级接口，继承它的接口主要有Set和List
	Collections是针对集合的一个帮助类，提供一系列静态方法实现对各种集合的搜索、排序等操作
	
