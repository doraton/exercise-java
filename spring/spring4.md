# Spring Data JPA

共计4道

### Q1: ORM是什么?

ORM即Object- Relational Mapping，表示对象关系映射，映射的不只是对象的值还有对象之间的关
系，通过ORM就可以把对象映射到关系型数据库中。操作实体类就相当于操作数据库表，可以不再重
点关注SQL语句。



### Q2: JPA如何使用?

只需要持久层接口继承JpaRepository即可，泛型参数列表中第一个参数是实体类类型，第二个参数是
主键类型。
运行时通过JdkDynami CAopProxy的invoke 方法创建了-一个动态代理对象
SimpleJpaRepository，SimpleJpaRepository 中封装了JPA 的操作，通过hibernate (封装 了
JDBC)完成数据库操作。

### Q3: JPA实体类相关注解有哪些?

@Entity:表明当前类是一一个实体类。
@TabTe :关联实体类和数据库表。
@co1umn :关联实体类属性和数据库表中字段。
@Id :声明当前属性为数据库表主键对应的属性。
@Generatedvalue :配置主键生成策略。
@OneToMany:配置-对多关系，mappedBy属性值为主表实体类在从表实体类中对应的属性名。
@ManyToone :配置多对一-关系，targetEntity 属性值为主表对应实体类的字节码。
@Joi ncolumn:配置外键关系，name 属性值为外键名称，referencedColumnName 属性值为主表主
键名称。

### Q4:对象导航查询是什么?

通过get方法查询一个对象的同时，通过此对象可以查询它的关联对象。
对象导航查询一到多默认使用延迟加载的形式，关联对象是集合，因此使用立即加载可能浪费资源。
对象导航查询多到一默认使用立即加载的形式，关联对象是一个对象， 因此使用立即加载。
如果要改变加载方式，在实体类注解配置加上fetch属性即可，LAZY表示延迟加载，EAGER 表示立即
加载。