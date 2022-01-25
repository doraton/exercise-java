# Mybatis

共计5道

### Q1: Mybatis 的优缺点?

优点
相比JDBC减少了大量代码量，减少冗余代码。
使用灵活，SQL 语句写在XML里，从程序代码中彻底分离，降低了耦合度，便于管理。
提供XML标签，支持编写动态SQL语句。.
提供映射标签，支持对象与数据库的ORM字段映射关系。
缺点
SQL语句编写工作量较大，尤其是字段和关联表多时。
SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。

### Q2: Mybatis 的XML文件有哪些标签属性?

select、insert、 update、 delete 标签分别对应查询、添加、更新、删除操作。
parameterType属性表示参数的数据类型，包括基本数据类型和对应的包装类型、String 和Java
Bean类型，当有多个参数时可以使用#{argn} 的形式表示第n个参数。除了基本数据类型都要以全
限定类名的形式指定参数类型。
resu1tType表示返回的结果类型，包括基本数据类型和对应的包装类型、String 和Java Bean类型。
还可以使用把返回结果封装为复杂类型的resultmap 。



### Q3: Mybatis 的一级缓存是什么?

-级缓存是SqlSession级别，默认开启且不能关闭。
操作数据库时需要创建SqlSession对象，对象中有一个HashMap存储缓存数据，不同SqlSession之
间缓存数据区域互不影响。
-级缓存的作用域是SqlSession范围的，在同一个SqlSession中执行两次相同的SQL语句时，第一次.
执行完毕会将结果保存在缓存中，第二次查询直接从缓存中获取。
如果SqlSession执行了DML操作(insert、 update、 delete) ，Mybatis 必须将缓存清空保证数据有
效性。



### Q4: Mybatis 的二级缓存是什么?

二级缓存是Mapper级别，默认关闭。
使用二级缓存时多个SqlSession使用同一个Mapper的SQL语句操作数据库,得到的数据会存在二级
缓存区，同样使用HashMap进行数据存储，相比于一-级缓存，_ _级缓存范围更大，多个SqlSession
可以共用二级缓存，作用域是Mapper的同一个namespace，不同SqlSession两次执行相同的
namespace下的SQL语句，参数也相等，则第一次执行成功后会将数据保存在二级缓存中，第二次可
直接从二级缓存中取出数据。
要使用二级缓存，需要在全局配置文件中配置<setting name="cacheEnabled" value=" true"/>
，再在对应的映射文件中配置一个<cache/>标签。

### Q5: Mybatis #{}和${}的区别?

使用${}相当于使用字符串拼接，存在SQL注入的风险。

使用#{}相当于使用占位符，可以防止SQL注入，不支持使用占位符的地方就只能使用${}，典型情况就是动态参数。