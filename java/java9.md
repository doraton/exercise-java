❓Q：面向对象的三大特性？

三大特性：封装、继承、多态

封装明确允许外部访问的成员函数和数据项，调用者也不需要知道方法的细节，只需要知道它是用来干什么的

经典场景一：Javabean属性私有化，提供外部访问的get、set方法，比如说有一个需求它是用来给字符串加上一个统一的前缀，我使用set方法就可以保证所有命名的规范化

经典场景二：使用数据库只需要使用MyBatis就可以了，具体怎么样去建立连接，怎么样去执行SQL都不需要关心

继承：子类通过extends关键字去继承父类（合并同类项）的方法，并通过他自己的需求做出改变和拓展。

多态：





### 1.封装

隐藏细节，保护数据安全

![](https://cdn.jsdelivr.net/gh/su-zitong/imgs/imgs/20210515094322.png)

```java
//封装的实现
public class Encapsulation {
    // 1.成员属性私有化
    private String name;
    private String pwd;

    // 2.提供getter和setter方法来访问
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }
}

class TestEncapsulation {
    public static void main(String[] args) {
        Encapsulation test = new Encapsulation();
        // 3.通过setter方法设置属性值
        test.setName("封装");
        test.setPwd("666");
        // 4.通过getter方法获取值
        System.out.println("姓名：" + test.getName() + " -- 密码：" + test.getPwd());

    }
}
```



### 2.继承

在继承中代码的执行顺序为：

1.父类静态对象，父类静态代码块

2.子类静态对象，子类静态代码块

3.父类非静态对象，父类非静态代码块

4.父类构造函数

5.子类非静态对象，子类非静态代码块

6.子类构造函数

```java
public class Demo {
    class Super {
        int flag = 1;
        //父类构造方法
        Super() {
            test();
        }
        //实例方法
        void test() {
            System.out.println("Super.test() flag=" + flag);
        }
    }
    
    class Sub extends Super {
        //子类构造方法
        Sub(int i) {
            flag = i;
            System.out.println("Sub.Sub()flag=" + flag);
        }
        //实例方法
        void test() {
            System.out.println("Sub.test()flag=" + flag);
        }
    }
    
    public static void main(String[] args) {
        new Demo().new Sub(5);
    }
}

//执行顺序：父类构造方法、父类test()方法、子类test()方法、输出Sub.test() flag=1
//sub有参构造Sub.Sub() flag=5
```

不考虑反射机制，一个子类显式调用父类的构造器必须使用super关键字

当子类没有显示调用父类中的构造方法，系统默认会调用父类中的无参构造方法。

super用于代表子类的直接父类的特征

super可以访问：

父类的成员变量，成员方法，构造方法

使用super关键词的情况：

在子类中，存在与父类相同的属性和方法，由于访问子类中属性和方法的优先级高于父类，可以通过super关键词来访问父类中的属性和方法

在子类中，可以通过super关键词来显示的调用父类的构造方法

子类的构造方法默认调用父类的无参构造方法，当父类中不存在无参构造方法时，可以同过super来调用父类的有参构造芳法来避免编译时错误。



> ❓：子类要调用继承自父类的方法，必须使用super关键字？
>
> 💡：
>
> 1、子类构造函数调用父类构造函数用super
>
> 2、子类重写父类方法后，若想调用父类中被重写的方法，用super
>
> 3、未被重写的方法可以直接调用。

### 3.多态

#### 重写



#### 重载

一个类同时定义许多同名方法，参数个数、类型、顺序各不相同

与返回类型无关，返回类型可以相同也可以不同



❓Q：重载和重写的区别？

重载：方法名相同，参数列表不同

重写：子类重写父类的方法，方法名和参数列表都相同



## 二、内部类

在Java中，可以将一个类定义在另一个类里面或者一个方法里边，这样的类称为内部类，广泛意义上的内部类一般包括四种：成员内部类，局部内部类，匿名内部类，静态内部类 。

### 1.成员内部类

（1）该类像是外部类的一个成员，可以无条件的访问外部类的所有成员属性和成员方法（包括private成员和静态成员）；

（2）成员内部类拥有与外部类同名的成员变量时，会发生隐藏现象，即默认情况下访问的是成员内部类中的成员。如果要访问外部类中的成员，需要以下形式访问：【外部类.this.成员变量  或  外部类.this.成员方法】；

（3）在外部类中如果要访问成员内部类的成员，必须先创建一个成员内部类的对象，再通过指向这个对象的引用来访问；

（4）成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象；

（5）内部类可以拥有private访问权限、protected访问权限、public访问权限及包访问权限。如果成员内部类用private修饰，则只能在外部类的内部访问；如果用public修饰，则任何地方都能访问；如果用protected修饰，则只能在同一个包下或者继承外部类的情况下访问；如果是默认访问权限，则只能在同一个包下访问。外部类只能被public和包访问两种权限修饰。

### 2.局部内部类

（1）局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内；

（2）局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。

### 3.匿名内部类

（1）一般使用匿名内部类的方法来编写事件监听代码；

（2）匿名内部类是不能有访问修饰符和static修饰符的；

（3）匿名内部类是唯一一种没有构造器的类；

（4）匿名内部类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的实现或是重写。

### 4.静态内部类

（1）静态内部类是不需要依赖于外部类的，这点和类的静态成员属性有点类似；

（2）不能使用外部类的非static成员变量或者方法。