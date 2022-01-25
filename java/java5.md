# JUC

共计11题

### Q1:什么是CAS?

CAS表示Compare And Swap，比较并交换，CAS 需要三个操作数，分别是内存位置V、旧的预期值A
和准备设置的新值B。CAS指令执行时，当且仅当V符合A时，处理器才会用B更新V的值，否则它
就不执行更新。但不管是否更新都会返回V的旧值，这些处理过程是原子操作，执行期间不会被其他线
程打断。
在JDK 5后，Java 类库中才开始使用CAS操作，该操作由Unsafe类里的compareAndswapInt 等几
个方法包装提供。HotSpot 在内部对这些方法做了特殊处理，即时编译的结果是一条平台相关的处理器
CAS指令。Unsafe类不是给用户程序调用的类，因此jDK9前只有Java类库可以使用CAS,譬如juc
包里的AtomicInteger类中compareAndset 等方法都使用了Unsafe类的CAS操作实现。

### Q2: CAS 有什么问题?

CAS从语义上来说存在一个逻辑漏洞:如果V初次读取时是A，并且在准备赋值时仍为A，这依旧不能
说明它没有被其他线程更改过，因为这段时间内假设它的值先改为B又改回A,那么CAS操作就会误
认为它从来没有被改变过。
这个漏洞称为ABA问题，juc 包提供了-个AtomicStampedReference,原子更新带有版本号的引用类
型，通过控制变量值的版本来解决ABA问题。大部分情况下ABA不会影响程序并发的正确性，如果需
要解决，传统的互斥同步可能会比原子类更高效。

### Q3:有哪些原子类?

JDK 5提供了java.util.concurrent.atomic 包，这个包中的原子操作类提供了-种用法简单、性能高
效、线程安全地更新-个变量的方式。到JDK 8该包共有17个类，依据作用分为四种:原子更新基本类
型类、原子更新数组类、原子更新引用类以及原子更新字段类，atomic 包里的类基本都是使用Unsafe
实现的包装类。
AtomicInteger原子更新整形、AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。
AtomicIntegerArray,原子更新整形数组里的元素、AtomicLongArray 原子更新长整型数组里的元
素、AtomicReferenceArray 原子更新引用类型数组里的元素。
AtomicReference原子更新引用类型、AtomicMarkableReference 原子更新带有标记位的引用类型,
可以绑定一个boolean标记、AtomicStampedReference 原子更新带有版本号的引用类型，关联一个
整数值作为版本号，解决ABA问题。
AtomicIntegerFieldUpdater原子更新整形字段的更新器、AtomicLongFieldUpdater 原子更新长整形
字段的更新器AtomicReferenceFieldUpdater原子更新引用类型字段的更新器。



### Q4: AtomicIntger 实现原子更新的原理是什么?

AtomicInteger原子更新整形、AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。
getAndIncrement以原子方式将当前的值加1，首先在for死循环中取得AtomicInteger里存储的数
值，第二步对AtomicInteger当前的值加1，第三步调用compareAndSet 方法进行原子更新，先检
查当前数值是否等于expect,如果等于则说明当前值没有被其他线程修改，则将值更新为next,否则
会更新失败返回false,程序会进入for循环重新进行compareAndSet 操作。
atomic包中只提供了三种基本类型的原子更新，atomic 包里的类基本都是使用Unsafe实现的,
Unsafe只提供三种CAS方法: compareAndSwapInt 、compareAndSwaplong 和
compareAndSwapobject，例如原子更新Boolean是先转成整形再使用compareAndSwapInt 。



### Q4: AtomicIntger 实现原子更新的原理是什么?

AtomicInteger原子更新整形、AtomicLong 原子更新长整型、AtomicBoolean 原子更新布尔类型。
getAndIncrement以原子方式将当前的值加1，首先在for死循环中取得AtomicInteger里存储的数
值，第二步对AtomicInteger当前的值加1，第三步调用compareAndSet 方法进行原子更新，先检
查当前数值是否等于expect,如果等于则说明当前值没有被其他线程修改，则将值更新为next,否则
会更新失败返回false,程序会进入for循环重新进行compareAndSet 操作。
atomic包中只提供了三种基本类型的原子更新，atomic 包里的类基本都是使用Unsafe实现的,
Unsafe只提供三种CAS方法: compareAndSwapInt 、compareAndSwaplong 和
compareAndSwapobject，例如原子更新Boolean是先转成整形再使用compareAndSwapInt 。

### Q5: CountDownLatch 是什么?

CountDownLatch是基于执行时间的同步类，允许一个或多个线程等待其他线程完成操作，构造方法接
收一个int参数作为计数器，如果要等待n个点就传入n。每次调用countDown 方法时计数器减
1，await方***阻塞当前线程直到计数器变为0，由于countDown 方法可用在任何地方，所以n个
点既可以是n个线程也可以是一个线程里的n个执行步骤。



### Q6: CyclicBarrier 是什么?

循环屏障是基于同步到达某个点的信号量触发机制，作用是让一组线程到达一个屏障时被阻塞，直到最
后一个线程到达屏障才会解除。构造方法中的参数表示拦截线程数量，每个线程调用await方法告诉
CyclicBarrier自己已到达屏障，然后被阻塞。还支持在构造方法中传入一个Runnable任务，当线程到
达屏障时会优先执行该任务。适用于多线程计算数据，最后合并计算结果的应用场景。
CountDownL acth的计数器只能用一次，而CyclicBarrier的计数器可使用reset方法重置，所以
CyclicBarrier能处理更为复杂的业务场景，例如计算错误时可用重置计数器重新计算。



### Q7: Semaphore 是什么?

信号量用来控制同时访问特定资源的线程数量，通过协调各个线程以保证合理使用公共资源。信号量可
以用于流量控制，特别是公共资源有限的应用场景，比如数据库连接。
Semaphore的构造方法参数接收一个int值，表示可用的许可数量即最大并发数。使用acquire方法
获得一个许可证，使用release方法归还许可，还可以用tryAcquire 尝试获得许可。



### Q8: Exchanger 是什么?

交换者是用于线程间协作的工具类，用于进行线程间的数据交换。它提供一个同步点，在这个同步点两
个线程可以交换彼此的数据。
两个线程通过exchange 方法交换数据，第一个线程执行exchange 方法后会阻塞等待第二个线程执
行该方法，当两个线程都到达同步点时这两个线程就可以交换数据，将本线程生产出的数据传递给对
方。应用场景包括遗传算法、校对工作等。



### Q9: JDK7的ConcurrentHashMap原理?

ConcurrentHashMap用于解决HashMap的线程不安全和HashTable的并发效率低，HashTable 之
所以效率低是因为所有线程都必须竞争同一把锁，假如容器里有多把锁，每- -把锁用于锁容器的部分数
据，那么多线程访问容器不同数据段的数据时，线程间就不会存在锁竞争，从而有效提高并发效率，这
就是ConcurrentHashMap的锁分段技术。首先将数据分成Segment数据段，然后给每一个数据 段配
一把锁，当一个线程占用锁访问其中一个段的数据时，其他段的数据也能被其他线程访问。
get实现简单高效，先经过一次再散列， 再用这个散列值通过散列运算定位到Segment,最后通过散列
算法定位到元素。get的高效在于不需要加锁，除非读到空值才会加锁重读。get 方法中将共享变量定
义为volatile,在get操作里只需要读所以不用加锁。
put必须加锁，首先定位到Segment,然后进行插入操作，第-步判断是否需要对Segment里的
HashEntry数组进行扩容，第二步定位添加元素的位置，然后将其放入数组。
size操作用于统计元素的数量，必须统计每个Segment的大小然后求和，在统计结果累加的过程中,
之前累加过的count变化几率很小，因此先尝试两次通过不加锁的方式统计结果，如果统计过程中容器
大小发生了变化，再加锁统计所有Segment大小。判断容器是否发生变化根据modCount确定。



### Q10: JDK8的ConcurrentHashMap原理?

主要对jJDK7做了三点改造:①取消分段锁机制，进一步降低冲突概率。②引入红黑树结构，同一个哈
希槽上的元素个数超过一定阈值后，单向链表改为红黑树结构。③使用了更加优化的方式统计集合内的
元素数量。具体优化表现在:在put、resize 和size方法中设计元素总数的更新和计算都避免了锁，使
用CAS代替。
get同样不需要同步，put 操作时如果没有出现哈希冲突，就使用CAS添加元素，否则使用
synchronized加锁添加元素。
当某个槽内的元素个数达到7且table容量不小于64时，链表转为红黑树。当某个槽内的元素减少到
6时，由红黑树重新转为链表。在转化过程中，使用同步块锁住当前槽的首元素，防止其他线程对当前
槽进行增删改操作，转化完成后利用CAS替换原有链表。由于TreeNode节点也存储了next 引用，因
此红黑树转为链表很简单，只需从first元素开始遍历所有节点，并把节点从TreeNode转为Node类
型即可，当构造好新链表后同样用CAS替换红黑树。



### Q11: ArrayList 的线程安全集合是什么?

可以使用CopyOnWriteArrayList代替ArrayList,它实现了读写分离。写操作复制一个新的集合，在新
集合内添加或删除元素，修改完成后再将原集合的引用指向新集合。这样做的好处是可以高并发地进行
读写操作而不需要加锁，因为当前集合不会添加任何元素。使用时注意尽量设置容量初始值，并且可以
使用批量添加或删除，避免多次扩容，比如只增加一个元素却复制整个集合。
适合读多写少，单个添加时效率极低。CopyOnWriteArrayList 是fail-safe的，并发包的集合都是这种
机制，fail-safe 在安全的副本.上遍历，集合修改与副本遍历没有任何关系，缺点是无法读取最新数据。
这也是CAP理论中C和A的矛盾，即一致性与可用性的矛盾。