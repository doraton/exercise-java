# 集合

共计7题

### Q1:说一说ArrayList

ArrayList是容量可变的非线程安全列表，使用数组实现，集合扩容时会创建更大的数组，把原有数组
复制到新数组。支持对元素的快速随机访问，但插入与删除速度很慢。ArrayList 实现了RandomAcess
标记接口，如果-个类实现了该接口，那么表示使用索引遍历比迭代器更快。
elementData是ArrayList 的数据域，被transient修饰，序列化时会调用writeObject写入流，反序
列化时调用readObject重新赋值到新对象的elementData。原因是elementData容量通常大于实际
存储元素的数量，所以只需发送真正有实际值的数组元素。
size是当前实际大小，elementData 大小大于等于size。
modCount记录了ArrayList结构性变化的次数，继承自AbstractList。 所有涉及结构变化的方法都会
增加该值。expectedModCount 是迭代器初始化时记录的modCount值，每次访问新元素时都会检查
modCount和expectedModCount是否相等，不相等就会抛出异常。这种机制叫做fail-fast,所有集
合类都有这种机制。

### Q2:说一说LinkedList

LinkedList本质是双向链表，与ArrayList 相比插入和删除速度更快，但随机访问元素很慢。除继承
AbstractList外还实现了Deque接口，这个接口具有队列和栈的性质。成员变量被transient修饰，原
理和ArrayList类似。
LinkedList包含三个重要的成员: size、 first 和last。size 是双向链表中节点的个数，first 和last分别
指向首尾节点的引用。
LinkedList的优点在于可以将零散的内存单元通过附加引用的方式关联起来，形成按链路顺序查找的线
性结构，内存利用率较高。



### Q3: Set有什么特点，有哪些实现?

Set不允许元素重复且无序，常用实现有HashSet、LinkedHashSet 和TreeSet。
HashSet通过HashMap实现，HashMap 的Key即HashSet存储的元素，所有Key都使用相同的
Value，一个名为PRESENT的Object类型常量。使用Key保证元素唯一-性， 但不保证有序性。由于
HashSet是HashMap实现的，因此线程不安全。
HashSet判断元素是否相同时，对于包装类型直接按值比较。对于引用类型先比较hashCode是否相
同，不同则代表不是同一个对象，相同则继续比较equals,都相同才是同-一个对象。
LinkedHashSet继承自HashSet，通过LinkedHashMap实现，使用双向链表维护元素插入顺序。
TreeSet通过TreeMap实现的，添加元素到集合时按照比较规则将其插入合适的位置，保证插入后的
集合仍然有序。

### Q4: TreeMap有什么特点?

TreeMap基于红黑树实现，增删改查的平均和最差时间复杂度均为O(logn)，最大特点是Key有序。
Key必须实现Comparable接口或提供的Comparator比较器，所以Key不允许为null。
HashMap依靠hashcode 和equals 去重，而TreeMap依靠Comparable或Comparator.
TreeMap排序时，如果比较器不为空就会优先使用比较器的compare 方法，否则使用Key实现的
Comparable的compareTo 方法，两者都不满足会抛出异常。
TreeMap通过put和deleteEntry 实现增加和删除树节点。插入新节点的规则有三个:①需要调整
的新节点总是红色的。②如果插入新节点的父节点是黑色的，不需要调整。③如果插入新节点的父节
点是红色的，由于红黑树不能出现相邻红色，进入循环判断，通过重新着色或左右旋转来调整。
TreeMap的插入操作就是按照Key的对比往下遍历，大于节点值向右查找，小于向左查找，先按照二
叉查找树的特性操作，后续会重新着色和旋转，保持红黑树的特性。

### Q5: HashMap有什么特点?

JDK8之前底层实现是数组+链表, JDK8 改为数组+链表/红黑树，节点类型从Entry变更为Node。主
要成员变量包括存储数据的table数组、元素数量size、加载因子loadFactor。
table数组记录HashMap的数据，每个下标对应一条链表，所有哈希冲突的数据都会被存放到同- -条
链表，Node/Entry 节点包含四个成员变量: key、 value、 next 指针和hash值。
HashMap中数据以键值对的形式存在，键对应的hash值用来计算数组下标，如果两个元素key的
hash值一样，就会发生哈希冲突，被放到同一个链表上，为使查询效率尽可能高，键的hash值要尽可
能分散。
HashMap默认初始化容量为16，扩容容量必须是2的幂次方、最大容量为1<<30、默认加载因子为
0.75。

### Q6: HashMap相关方法的源码?

jDK8之前
hash:计算元素key的散列值
①处理String类型时，调用stringHash32 方法获取hash值。
②处理其他类型数据时，提供一个相对于 HashMap实例唯一不变的随机值hashSeed作为计算初始
量。
③执行异或和无符号右移使hash值更加离散，减小哈希冲突概率。
indexFor:计算元素下标
将hash值和数组长度-1进行与操作，保证结果不会超过table数组范围。
get:获取元素的value值
①如果key为null,调用getForNu11Key 方法，如果size为0表示链表为空，返回null。如果size
不为0说明存在链表，遍历table[0]链表,如果找到了key为null 的节点则返回其value,否则返回
null.
②如果key为不为null,调用getEntry 方法，如果size为0表示链表为空，返回null值。如果
size不为0，首先计算key的hash值,然后遍历该链表的所有节点，如果节点的key和hash值都和
要查找的元素相同则返回其Entry节点。
③如果找到了对应的Entry节点，调用getvalue方法获取其value并返回,否则返回null。

put:添加元素
①如果key为null,直接存入table[0]。
②如果key不为null,计算key的hash值。
③调用indexFor 计算元素存放的下标i。
④遍历table[i]对应的链表，如果key已存在，就更新value然后返回旧value。
⑤如果key不存在，将modCount值加1,使用addEntry 方法增加一个节点并返回null.
resize:扩容数组
①如果当前容量达到了最大容量，将阈值设置为Integer最大值，之后扩容不再触发。
②否则计算新的容量，将阈值设为newCapacity x 1oadFactor 和最大容量+ 1的较小值。
③创建一个容量为newCapacity的Entry数组，调用transfer 方法将旧数组的元素转移到新数组。
transfer:转移元素
①遍历旧数组的所有元素，调用rehash 方法判断是否需要哈希重构，如果需要就重新计算元素key
的hash值。
②调用indexFor 方法计算元素存放的下标i,利用头插法将旧数组的元素转移到新数组。

JDK8
hash:计算元素key的散列值
如果key为null 返回0,否则就将key的hashcode 方法返回值高低16位异或，让尽可能多的位参与
运算，让结果的0和1分布更加均匀，降低哈希冲突概率。
put:添加元素
①调用putval 方法添加元素。
②如果table为空或长度为0就进行扩容，否则计算元素下标位置，不存在就调用newNode 创建一个
节点。
③如果存在且是链表，如果首节点和待插入元素的hash和key都-样，更新节点的value。
④如果首节点是TreeNode类型，调用putTreevaT 方法增加一个树节点，每一次都比较插入节点和
当前节点的大小，待插入节点小就往左子树查找，否则往右子树查找，找到空位后执行两个方
法: balanceInsert 方法，插入节点并调整平衡、moveRootToFront 方法，由于调整平衡后根节点
可能变化，需要重置根节点。
⑤如果都不满足，遍历链表，根据hash和key判断是否重复，决定更新value还是新增节点。如果遍
历到了链表末尾则添加节点，如果达到建树阈值7,还需要调用treei fyBin把链表重构为红黑树。

⑥存放元素后将modCount加1,如果++size > thresho1d， 调用resize扩容。
get:获取元素的value值
①调用getNode 方法获取Node节点，如果不是null 就返回其value值，否则返回null。
②getNode 方法中如果数组不为空且存在元素，先比较第一个节点和要查找元素的hash和key,如
果都相同则直接返回。
③如果第二个节点是TreeNode类型则调用getTreeNode 方法进行查找，否则遍历链表根据hash和
key查找,如果没有找到就返回null。
resize:扩容数组
重新规划长度和阈值，如果长度发生了变化，部分数据节点也要重新排列。
重新规划长度
①如果当前容量o1dCap > 0且达到最大容量，将阈值设为Integer最大值，return 终止扩容。
②如果未达到最大容量，当oldCap << 1不超过最大容量就扩大为2倍。
③如果都不满足且当前扩容阈值o1dThr > 0，使用当前扩容阈值作为新容量。
④否则将新容量置为默认初始容量16,新扩容阈值置为12。
重新排列数据节点
①如果节点为null 不进行处理。
②如果节点不为null且没有next节点，那么通过节点的hash值和新容量-1进行与运算计算下标存入
新的table数组。

③如果节点为TreeNode类型，调用split方法处理，如果节点数hc达到6会调用untreeify |方
法转回链表。
④如果是链表节点，需要将链表拆分为hash值超出旧容量的链表和未超出容量的链表。对于hash &
o1dCap == 0的部分不需要做处理，否则需要放到新的下标位置上，新下标=旧下标+旧容量。

### Q7: HashMap为什么线程不安全?

jDK7存在死循环和数据丢失问题。
数据丢失:
●并发赋值被覆盖:在createEntry方法中，新添加的元素直接放在头部，使元素之后可以被更
快访问，但如果两个线程同时执行到此处，会导致其中一个线程的赋值被覆盖。
●已遍历区间新增元素丢失:当某 个线程在transfer 方法迁移时，其他线程新增的元素可能落在
已遍历过的哈希槽 上。遍历完成后，table 数组引用指向了newTable,新增元素丢失。
●新表被覆盖:如果resize完成，执行了table = newTable,则后续元素就可以在新表上进
行插入。但如果多线程同时resize，每个线程都会new一个数组，这是线程内的局部对象，线
程之间不可见。迁移完成后resize的线程会赋值给table线程共享变量，可能会覆盖其他线程的
操作，在新表中插入的对象都会被丢弃。
死循环:扩容时resize调用transfer使用头插法迁移元素，虽然newTable是局部变量，但原先
table中的Entry链表是共享的，问题根源是Entry的next指针并发修改，某线程还没有将table设为
newTable时用完了CPU时间片，导致数据丢失或死循环。
JDK8在resize 方法中完成扩容,并改用尾插法，不会产生死循环，但并发下仍可能丢失数据。可用
ConcurrentHashMap或co1lections . synchronizedMap包装成同步集合。
