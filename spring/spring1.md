# Spring IOC

共计11道题

### Q1: loC 是什么?

loC即控制反转，简单来说就是把原来代码里需要实现的对象创建、依赖反转给容器来帮忙实现，需要
创建一个容器并 且需要-种描述让容器知道要创建的对象间的关系，在Spring中管理对象及其依赖关
系是通过Spring的loC容器实现的。
loC的实现方式有依赖注入和依赖查找，由于依赖查找使用的很少，因此IoC也叫做依赖注入。依赖注
入指对象被动地接受依赖类而不用自己主动去找，对象不是从容器中查找它依赖的类，而是在容器实例
化对象时主动将它依赖的类注入给它。假设-一个Car类需要一个Engine的对象，那么-般需要需要手
动new一个Engine,利用loC就只需要定义一个私有的Engine类型的成员变量，容器会在运行时自
动创建一个Engine的实例对象并将引用自动注入给成员变量。



### Q2: loC 容器初始化过程?

基于XML的容器初始化
当创建-个ClassPathXmlAplicationContext时，构造方法做了两件事:①调用父容器的构造方法为
容器设置好Bean资源加载器。②调用父类的setConfi gLocations方法设置Bean配置信息的定位
路径。
ClassPathXmlApplicationContext通过调用父类AbstractApplicationContext的refresh 方法启动
整个loC容器对Bean定义的载入过程，refresh 是-一个模板方法，规定了IoC容器的启动流程。在
创建loC容器前如果已有容器存在，需要把已有的容器销毁，保证在refresh 方法后使用的是新创建
的loC容器。
容器创建后通过1oadBeanDefinitions方法加载Bean配置资源，该方法做两件事:①调用资源加
载器的方法获取要加载的资源。②真正执行加载功能，由子类XmlBeanDefinitionReader实现。加载
资源时首先解析配置文件路径，读取配置文件的内容，然后通过XML解析器将Bean配置信息转换成
文档对象，之后按照Spring Bean的定义规则对文档对象进行解析。
Spring loC容器中注册解析的Bean信息存放在一个HashMap集合中，key 是字符串,值是
BeanDefinition,注册过程中需要使用synchronized保证线程安全。当配置信息中配置的Bean被解
析且被注册到loC容器中后，初始化就算真正完成了，Bean 定义信息已经可以使用且可被检索。
Spring loC容器的作用就是对这些注册的Bean定义信息进行处理和维护，注册的Bean定义信息是控
制反转和依赖注入的基础。
基于注解的容器初始化
分为两种:①直接将注解Bean注册到容器中，可以在初始化容器时注册，也可以在容器创建之后手动
注册，然后刷新容器使其对注册的注解Bean进行处理。②通过扫描指定的包及其子包的所有类处理,
在初始化注解容器时指定要自动扫描的路径。



### Q3:依赖注入的实现方法有哪些?

构造方法注入: loC Service Provider会检查被注入对象的构造方法，取得它所需要的依赖对象列表,
进而为其注入相应的对象。这种方法的优点是在对象构造完成后就处于就绪状态，可以马上使用。缺点
是当依赖对象较多时，构造方法的参数列表会比较长，构造方法无法被继承，无法设置默认值。对于非
必需的依赖处理可能需要引入多个构造方法，参数数量的变动可能会造成维护的困难。
setter方法注入:当前对象只 需要为其依赖对象对应的属性添加setter方法，就可以通过setter方法
将依赖对象注入到被依赖对象中。setter 方法注入在描述性上要比构造方法注入强，并且可以被继承,
允许设置默认值。缺点是无法在对象构造完成后马上进入就绪状态。
接口注入:必须实现某个接口，接口提供方法来为其注入依赖对象。使用少，因为它强制要求被注入对
象实现不必要接口，侵入性强。



### Q4:依赖注入的相关注解?

@Autowi red:自动按类型注入，如果有多个匹配则按照指定Bean的id查找，查找不到会报错。
@qualifier:在自动按照类型注入的基础上再按照Bean的id注入，给变量注入时必须搭配
@Autowi red,给方法注入时可单独使用。
@Resource :直接按照Bean的id注入，只能注入Bean类型。
@value :用于注入基本数据类型和String类型。



### Q5:依赖注入的过程?

getBean方法获取Bean实例，该访***调用doGetBean ，doGetBean 真正实现从loC容器获取
Bean的功能，也是触发依赖注入的地方。
具体创建Bean对象的过程由ObjectFactory 的createBean 完成，该访法主要通过
createBeanInstance方法生成Bean包含的Java对象实例和populateBean方法对Bean属性的
依赖注入进行处理。
在populateBean方法中，注入过程主要分为两种情况:①属性值类型不需要强制转换时，不需要解
析属性值，直接进行依赖注入。②属性值类型需要强制转换时，首先解析属性值，然后对解析后的属性
值进行依赖注入。依赖注.入的过程就是将Bean对象实例设置到它所依赖的Bean对象属性上，真正的
依赖注入是通过setPropertyvalues 方法实现的，该方法使用了委派模式。
BeanWrapperlmpl类负责对完成初始化的Bean对象进行依赖注入，对于非集合类型属性，使用JDK
反射，通过属性的setter方法为属性设置注入后的值。对于集合类型的属性，将属性值解析为目标类型
的集合后直接赋值给属性。
当容器对Bean的定位、载入、解析和依赖注入全部完成后就不再需要手动创建对象，loC 容器会自动
为我们创建对象并且注入依赖。





### Q6: Bean 的生命周期?

在loC容器的初始化过程中会对Bean定义完成资源定位,加载读取配置并解析，最后将解析的Bean
信息放在一个HashMap集合中。当loC容器初始化完成后，会进行对Bean实例的创建和依赖注入过
程，注入对象依赖的各种属性值，在初始化时可以指定自定义的初始化方法。经过这-系列初始化操作
后Bean达到可用状态，接下来就可以使用Bean了，当使用完成后会调用destroy方法进行销毁，此
时也可以指定自定义的销毁方法，最终Bean被销毁且从容器中移除。
XML方式通过配置bean标签中的init-Method 和destory-Method指定自定义初始化和销毁方法。
注解方式通过@PreConstruct 和QPostConstruct 注解指定自定义初始化和销毁方法。

### Q7: Bean 的作用范围?

通过scope属性指定bean的作用范围，包括:
①singleton:单例模式，是默认作用域，不管收到多少Bean请求每个容器中只有一个唯- -的Bean实
例。
②prototype:原型模式，和singleton相反，每次Bean请求都会创建一个新的实例。
③request:每次HTTP请求都会创建一个新的Bean并把它放到request域中，在请求完成后Bean
会失效并被垃圾收集器回收。
④session:和request类似，确保每个session中有一个Bean实例，session 过期后bean会随之失
效。
⑤global session:当应用部署在Portlet容器时，如果想让所有Portlet共用全局存储变量，那么该变
量需要存储在global session中。

### Q8:如何通过XML方式创建Bean?

默认无参构造方法，只需要指明bean标签中的id和class属性，如果没有无参构造方***报错。
静态工厂方法，通过bean标签中的class属性指明静态工厂，factory-method 属性指明静态工厂方
法。
实例工厂方法，通过bean标签中的factory-bean属性指明实例工厂，factory-method 属性指明实例
工厂方法。

### Q9:如何通过注解创建Bean?

@Component把当前类对象存入Spring容器中，相当于在xml中配置一个bean标签。value 属性指
定bean的id,默认使用当前类的首字母小写的类名。
@Contro1ler，@Service， @Repository 三个注解都是@Component 的衍生注解，作用及属性都
是一模一样的。只是提供了更加明确语义，@Contro1ler 用于表现层，@Service 用于业务
层，@Repository用于持久层。如果注解中有且只有一个value属性要赋值时可以省略value。
如果想将第三方的类变成组件又没有源代码，也就没办法使用@Component进行自动配置，这种时候
就要使用@Bean注解。被@Bean注解的方法返回值是一个对象，将会实例化，配置和初始化- -个新
对象并返回，这个对象由Spring的IoC容器管理。name属性用于给当前@Bean注解方法创建的对象
指定一个名称，即bean的id。当使用注解配置方法时，如果方法有参数，Spring 会去容器查找是否有
可用bean对象，查找方式和@Autowired一样。



### Q10:如何通过注解配置文件?

@Configuration用于指定当前类是-个spring配置类，当创建容器时会从该类上加载注解，value
属性用于指定配置类的字节码。
@ComponentScan用于指定Spring在初始化容器时要扫描的包。basePackages 属性用于指定要扫描
的包。.
@PropertySource用于加载. properties文件中的配置。value 属性用于指定文件位置，如果是在
类路径下需要加上classpath。
@Import用于导入其他配置类，在引入其他配置类时可以不用再写@Configuration 注解。有
@Import的是父配置类，引入的是子配置类。value属性用于指定其他配置类的字节码。



### Q11: BeanFactory、FactoryBean和ApplicationContext的区别?

BeanFactory是一个Bean工厂，使用简单工厂模式，是Spring loC容器顶级接口，可以理解为含有
Bean集合的工厂类,作用是管理Bean,包括实例化、定位、配置对象及建立这些对象间的依赖。
BeanFactory实例化后并不会自动实例化Bean,只有当Bean被使用时才实例化与装配依赖关系，属
于延迟加载，适合多例模式。
FactoryBean是- -个工厂Bean, 使用了工厂方法模式，作用是生产其他Bean实例，可以通过实现该
接口，提供一个工厂方法来自定义实例化Bean的逻辑。FactoryBean 接口由BeanFactory中配置的对
象实现，这些对象本身就是用于创建对象的工厂，如果-个Bean实现了这个接口，那么它就是创建对
象的工厂Bean，而不是Bean实例本身。
ApplicationConext是BeanFactory的子接口，扩展了BeanFactory的功能，提供了支持国际化的文本
消息，统一的资源文件读取方式，事件传播以及应用层的特别配置等。容器会在初始化时对配置的
Bean进行预实例化，Bean的依赖注入在容器初始化时就已经完成，属于立即加载，适合单例模式，一
般推荐使用。