# Spring MVC

共计3道

### Q1: Spring MVC的处理流程?

Web容器启动时会通知Spring初始化容器，加载Bean的定义信息并初始化所有单例Bean，然后遍历
容器中的Bean,获取每一个Controller中的所有方法访问的URL，将URL和对应的Controller保存
到一个Map集合中。
所有的请求会转发给DispatcherServlet前端处理器处理，DispatcherServlet 会请求HandlerMapping
找出容器中被@Controler 注解修饰的Bean以及被@RequestMappi ng修饰的方法和类，生成
Handler和HandlerInterceptor并以一个HandlerExcutionChain处理器执行链的形式返回。
之后DispatcherServlet使用Handler找到对应的HandlerApapter,通过HandlerApapter调用
Handler的方法，将请求参数绑定到方法的形参上，执行方法处理请求并得到ModelAndView。
最后DispatcherServlet根据使用ViewResolver试图解析器对得到的ModelAndView逻辑视图进行解
析得到View物理视图，然后对视图渲染，将数据填充到视图中并返回给客户端。

### Q2: Spring MVC有哪些组件?

Dispatcherservlet : SpringMVC 中的前端控制器，是整个流程控制的核心，负责接收请求并转发给
对应的处理组件。
Handler:处理器，完成具体业务逻辑，相当于Servlet或Action。
HandlerMapping:完成URL到Controller映射，DispatcherServlet 通过HandlerMapping将不同.
请求映射到不同Handler.
HandlerInterceptor:处理器拦截器，是一个接口，如果需要完成-些拦截处理，可以实现该接
口。
HandlerExecutionChain:处理器执行链，包括两部分内容: Handler 和HandlerInterceptor.
HandlerAdapter:处理器适配器，Handler执行业务 方法前需要进行一系列操作，包括表单数据验
证、数据类型转换、将表单数据封装到JavaBean等，这些操作都由HandlerAdapter完成。
DispatcherServlet通过HandlerAdapter来执行不同的Handler.
Mode lAndview:装载模型数据和视图信息，作为Handler处理结果返回给DispatcherServlet。
ViewResolver:视图解析器，DispatcherServlet 通过它将逻辑视图解析为物理视图，最终将渲染的
结果响应给客户端。

### Q3: Spring MVC的相关注解?

@Controller:在类定义处添加，将类交给IoC容器管理。
@RequtestMapping:将URL请求和业务方法映射起来，在类和方法定义上都可以添加该注
解。value属性指定URL请求的实际地址，是默认值。method 属性限制请求的方法类型，包括GET、
POST、PUT、DELETE等。如果没有使用指定的请求方法请求URL，会报405 Method Not Allowed错
误。1 params 属性限制必须提供的参数，如果没有会报错。
@RequestParam :如果Controller方法的形参和URL参数名-致可以不添加注解，如果不一致可以使
用该注解绑定。value 属性表示HTTP请求中的参数名。requi red属性设置参数是否必要，默认
false。defau1tvalue 属性指定没有给参数赋值时的默认值。
@PathVariable : Spring MVC支持RESTful风格URL，通过@PathVariab1e 完成请求参数与形参
的绑定。