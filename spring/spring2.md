# AOP

共计四道

### Q1: AOP是什么?

AOP即面向切面编程，简单地说就是将代码中重复的部分抽取出来，在需要执行的时候使用动态代理技
术，在不修改源码的基础上对方法进行增强。
Spring根据类是否实现接口来判断动态代理方式，如果实现接口会使用jDK的动态代理，核心是
InvocationHandler接口和Proxy类，如果没有实现接口会使用CGLib动态代理，CGLib 是在运行时动
态生成某个类的子类，如果某个类被标记为final,不能使用CGLib。
JDK动态代理主要通过重组字节码实现，首先获得被代理对象的引用和所有接口，生成新的类必须实现
被代理类的所有接口，动态生成Java 代码后编译新生成的.class 文件并重新加载到JVM运行。jDK
代理直接写Class字节码，CGLib 是采用ASM框架写字节码，生成代理类的效率低。但是CGLib调用
方法的效率高，因为jJDK使用反射调用方法，CGLib 使用FastClass机制为代理类和被代理类各生成一
个类，这个类会为代理类或被代理类的方法生成一个index,这个index可以作为参数直接定位要调用
的方法。
常用场景包括权限认证、自动缓存、错误处理、日志、调试和事务等。



### Q2: AOP的相关注解有哪些?

@Aspect :声明被注解的类是一个切面Bean。
@Before:前置通知，指在某个连接点之前执行的通知。
@After :后置通知，指某个连接点退出时执行的通知(不论正常返回还是异常退出)。
@AfterReturning:返回后通知，指某连接点正常完成之后执行的通知，返回值使用returning属性接
收。
@AfterThrowing:异常通知，指方法抛出异常导致退出时执行的通知，和@AfterReturning只会有
-个执行，异常使用throwing属性接收。





### Q3: AOP的相关术语有什么?

Aspect:切面，一个关注点的模块化，这个关注点可能会横切多个对象。
Joinpoint:连接点，程序执行过程中的某-行为，即业务层中的所有方法。。
Advice:通知，指切面对于某个连接点所产生的动作，包括前置通知、后置通知、返回后通知、异常
通知和环绕通知。
Pointcut:切入点，指被拦截的连接点，切入点一-定是连接点，但连接点不一定是切入点。
Proxy:代理，Spring AOP中有jDK动态代理和CGLib代理，目标对象实现了接口时采用jJDK动态代
理，反之采用CGLib代理。
Target:代理的目标对象，指一个或多个切面所通知的对象。
Weaving :织入，指把增强应用到目标对象来创建代理对象的过程。



### Q4: AOP的过程?

Spring AOP由BeanPostProcessor后置处理器开始，这个后置处理器是-个***，可以监听容器触发
的Bean生命周期事件，向容器注册后置处理器以后，容器中管理的Bean就具备了接收loC容器回调
事件的能力。BeanPostProcessor 的调用发生在Spring loC容器完成Bean实例对象的创建和属性的
依赖注入后，为Bean对象添加后置处理器的入口是initializeBean方法。
Spring中JDK动态代理通过JdkDynamicAopProxy调用Proxy的newInstance 方法来生成代理类,
JdkDynamicAopProxy也实现了InvocationHandler接口，invoke 方法的具体逻辑是先获取应用到此
方法.上的拦截器链,如果有拦截器则创建MethodInvocation并调用其proceed 方法，否则直接反射
调用目标方法。因此SpringAOP对目标对象的增强是通过拦截器实现的。