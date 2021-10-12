## 介绍

> Spring是一个分层的(一站式) 轻量级开源框架 Spring的核心是控制反转（IoC）和面向切面（AOP）



#### ioc

> IoC -- Inverse of Control，控制反转，将对象的创建权反转给Spring！！ 使用IOC可以解决的程序耦合性高的问题！



#### 控制反转（DI:依赖注入）

>  假设我需要做一个功能，在这个功能当中我需要调用servic层，然后再调用dao层，去取数据。在传统的javaEE开发中我就直接去new一个service 然后再new一个dao。但是在spring框架中，我们吧new service和new dao的权利交个spring框架，假设我需要使用我就直接去spring框架中寻找。等于说我的资源创建的权利交给了spring框架，这就叫做控制反转。



#### 解耦

> 刚刚我们说资源创建交给了sring，我们需要什么就找spring。这过程就像是工厂模式。但是在spring框架中它需要创建哪些对象，它需要一个配置文件。这个配置文件告诉spring，需要创建哪些资源。

例如：假设我需要去数据库查询数据显示页面

> 程序启动，spring框架去找配置文件创建资源，把资源放置再一个容器中，开始运行，前端请求数据，在spring中找controller层，再找service层，再找dao层要数据，最后数据原路返回controller，再显示到页面上。其中service被spring注入到controlller层，dao层被spring注入到service层。这个过程分工明确。每一层各司其职。传统的一个开发，在servlet中直接new然后去查数据，然后数据返回到界面上。万一操作一多所有的判断，查询不同的表，这个servlet的代码变得十分的臃肿。不说开发慢，你开发完了看代码也费劲。            所以说控制反转可以用来解耦





1. 搭建Spring环境

开发spring至少需要使用的jar：

> spring-aop.jar		//开发AOP特性时需要的jar
>
> spring-beans.jar	//处理Bean的jar
>
> spring-context.jar	//处理spring上下文的jar
>
> spring-core.jar		//spring核心jar
>
> spring-expression.jar	//spring表达式

以及三方提供的日志jar

> commons-logging.jar	//日志





2. 编写配置文件

资源目录创建appliactioncontext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```





3. 开发spring程序(ioc)

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--  该文件中所产生的所有对象，被spring放入了一个 称之为 spring ioc容器的地方  -->
    <bean id="student" class="com.springdemo.demo.entity.Student">
        <property name="id" value="2"></property>
        <property name="name" value="ls"></property>
        <property name="age" value="20"></property>
        <property name="address" value="guangdong"></property>
    </bean>

</beans>
```

Test.java

```java
package com.springdemo.demo.test;

import com.springdemo.demo.entity.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        //以往创建对象的方式
        Student student = new Student(1,"zs",20,"jiangxi");
        System.out.println(student.toString());

        //ioc创建对象的方式
        ApplicationContext context = new  ClassPathXmlApplicationContext("applicationContext.xml");     //获取spring上下文对象
        Student student1 = (Student) context.getBean("student");   //执行从springIOC容器中获取一个id为student的对象
        System.out.println(student1.toString());


    }
}

```

IOC(控制反转)也可以称之为DI(依赖注入)

> 控制反转:将 创建对象，属性赋值 的方式进行了反转，从new,setXXX() 反转为了 从springIOC容器getBean()
>
> 依赖注入: 将属性值注入给了属性， 将属性注入给了bean, 将bean注入给了ioc容器

 总结：ioc/di，无论要什么对象，都可以去spring ioc 容器中获取，而不需要自己操作(new/setXXX())

> 
>
> IOC容器赋值:如果是简单类型(8个基本+String)用value，如果是复杂类型用ref="需要引用的id值"





常见的依赖注入方式：

1. set方式的依赖注入：

   ```xml
   <bean id="student" class="com.springdemo.demo.entity.Student">
       <property name="id" value="2"></property>
       <property name="name" value="ls"></property>
       <property name="age" value="20"></property>
       <property name="address" value="guangdong"></property>
   </bean>
   ```

赋值，默认使用的是set方法()；

依赖注入底层是通过反射实现



2.构造器注入(首先得有构造方法):

```xml
<bean id="student" class="com.springdemo.demo.entity.Student">
    <constructor-arg value="3" index="0"></constructor-arg>
    <constructor-arg value="ww" name="name"></constructor-arg>
    <constructor-arg value="21"></constructor-arg>
    <constructor-arg value="jiangxi"></constructor-arg>
</bean>
```

> 构造器注入的顺序需和构造方法一致，如不一致时可通过index=”参数下标“(从0开始)，name="形参名"或type(”数据类型“)(参数不得重复时可使用)进行指定



3.p命名空间注入

> 引入p命名空间
>
> xmlns:p="http://www.springframework.org/schema/p"

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"	p命名空间
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <bean id="student" class="com.springdemo.demo.entity.Student" p:id="4" p:name="zl" p:age="23" p:address="广西"></bean>

</beans>
```

> p命名空间注入如果是简单类型则直接写属性名=”属性值“ ，若是复杂类型则写字段名-ref="beanID"

> 
>
> 注意
>
> 注入的属性值默认为字符串，所以在注入时没有指定type或者name时，优先赋值给字符串类型的属性







#### 各种集合类的注入方式

1.set方式注入

set、list、数组   各自都有自己的标签<set> <list> <array>,但是也可以混着用。

```xml
 <bean class="com.springdemo.demo.entity.AllCollectionType" id="allCollectionType">
        <!--        1.set方式注入-->
        <property name="listElement">
            <list>
                <value>足球</value>
                <value>篮球</value>
                <value>羽毛球</value>
                <value>乒乓球</value>
            </list>
        </property>

        <property name="arrayElement">
            <array>
                <value>唱</value>
                <value>跳</value>
                <value>rap</value>
                <value>篮球</value>
            </array>
        </property>

        <property name="setElement">
            <set>
                <value>java</value>
                <value>c语言</value>
                <value>html</value>
            </set>
        </property>

        <property name="mapElement">
            <map>
                <entry>
                    <key><value>foot</value></key>
                    <value>足球</value>
                </entry>
                <entry>
                    <key><value>basket</value></key>
                    <value>篮球</value>
                </entry>
            </map>
        </property>
        <property name="propertiesElement">
            <props>
                <prop key="name">wen</prop>
                <prop key="zs">张三</prop>
                <prop key="ls">李四</prop>
            </props>
        </property>
    </bean>
```

```java
public static void collectionDemo(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        AllCollectionType type = (AllCollectionType) context.getBean("allCollectionType");
        System.out.println(type.toString());
    }
```





#### 特殊符号的处理

大于号小于号使用\&lt;  	\&rt;

```xml
<property name="name" value="l&lt;s"></property>
<property name="name"><value>l&lt;s</value></property>
```

或者使用value标签才有的	\<![CDATA[特殊字符]]>

```xml
<property name="name"><value>ls<![CDATA[特殊字符]]></value></property>
```





**处理空值**

使用	\<null/>标签

```xml
<property name="name">
	<null/>
</property>
```







#### 自动装配（只对ref复杂类型有效）

**autowire="byName"**	：	Course类中有一个ref属性teacher(属性名)，并且该ioc容器中恰好有一个bean的id也是teacher

其他bean的id值=类的属性名 就会自动注入到类的属性中,不需要再通过set，构造方法或者p空间去赋值



**autowire="byType"**	：	其他bean的类型(class) 是否与 该Course的ref属性类型一致，注意此种方式 必须满足：当前Ioc容器中 只能有一个Bean满足条件

**autowire="byConstructor"**	：	其他bean的类型(class) 是否与 该Course的构造方法参数的类型一致，此种方法的本质就是Type方法

```xml
<bean id="teacher" class="com.springdemo.demo.entity.Teacher" p:name="zl" p:age="23">
</bean>

<bean id="course" class="com.springdemo.demo.entity.Course"  autowire="byName">
        <constructor-arg value="c语言"></constructor-arg>
        <constructor-arg value="200"></constructor-arg>
        <!--<constructor-arg ref="teacher"></constructor-arg>-->
</bean>
```





一次性设置所有的bean自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"
        default-autowire="byName"
       >
</beans>
```

标签内的属性 可以 覆盖 全局的自动装配

自动装配虽然可以减低代码量，但是会降低可读性







**Spring 中常用的注解如下。**

**1）@Component**

可以使用此注解描述 Spring 中的 Bean，但它是一个泛化的概念，仅仅表示一个组件（Bean），并且可以作用在任何层次。使用时只需将该注解标注在相应类上即可。

**2）@Repository**

用于将数据访问层（DAO层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**3）@Service**

通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**4）@Controller**

通常作用在控制层（如 Struts2 的 Action、SpringMVC 的 Controller），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。

**5）@Autowired**

可以应用到 Bean 的属性变量、属性的 setter 方法、非 setter 方法及构造函数等，配合对应的注解处理器完成 Bean 的自动配置工作。默认按照 Bean 的类型进行装配。

**6）@Resource**

作用与 Autowired 相同，区别在于 @Autowired 默认按照 Bean 类型装配，而 @Resource 默认按照 Bean 实例名称进行装配。

**@Resource 中有两个重要属性：name 和 type**。

Spring 将 name 属性解析为 Bean 的实例名称，type 属性解析为 Bean 的实例类型。如果指定 name 属性，则按实例名称进行装配；如果指定 type 属性，则按 Bean 类型进行装配。如果都不指定，则先按 Bean 实例名称装配，如果不能匹配，则再按照 Bean 类型进行装配；如果都无法匹配，则抛出 NoSuchBeanDefinitionException 异常。

**7）@Qualifier**

与 @Autowired 注解配合使用，会将默认的按 Bean 类型装配修改为按 Bean 的实例名称装配，Bean 的实例名称由 @Qualifier 注解的参数指定。





#### 注解定义Bean

通过注解的形式，将Bean以及相应的属性值放入ioc容器



1.配置扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--  配置扫描器  -->
    <context:component-scan base-package="com.demo2.wen.Dao"></context:component-scan>

</beans>
```

\<context:component-scan>	Spring在启动的时候，会根据 base-package 在该包中扫描所有类，查找这些类是否有注解 @Component("***")，如果有，则将该类加入Spring Ioc容器

```java
package com.demo2.wen.Dao;

import com.demo2.wen.entity.Student;
import org.springframework.stereotype.Component;

@Component("StudentDao")    //<bean id="StudentDao" class="com.demo2.wen.Dao.StudentDaoImpl"></bean>
public class StudentDaoImpl {

    public void addStudent(Student student){
        System.out.println("新增学生....");
    }
}
```

@Component细化：

dao层注解：**@Repository**

service层注解：**@Service**

控制层注解：**@Controller**




注解定义Bean例子：
Dao：
```java
package com.example.demo.Dao.impl;

import com.example.demo.Dao.UserDao;
import org.springframework.stereotype.Repository;

@Repository("UserDao")
public class UserDaoImpl implements UserDao {
    @Override
    public void addUser() {
        System.out.println("新增用户...");
    }

    @Override
    public void updateUser() {
        System.out.println("修改用户...");
    }
}
```

Service：
```java
package com.example.demo.Service.impl;

import com.example.demo.Dao.UserDao;
import com.example.demo.Service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service("UserService")
public class UserServiceImpl implements UserService {

    private UserDao userDao;

    @Autowired
    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void addUser() {
        userDao.addUser();
    }

    @Override
    public void updateUser() {
        userDao.updateUser();
    }
}
```

test:
```java
package com.example.demo.Test;

import com.example.demo.Service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void TestService(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContext.xml");
        UserService service = (UserService) context.getBean("UserService");
        service.addUser();
    }

    public static void main(String[] args) {
        TestService();
    }
}
```





#### 注解注入参数属性

1.字段注入 （不推荐使用字段注入）：
```java
@Autowired
private UserDao userDao;
```

字段注入可能会导致：
1). 对象的外部可见性

2). 可能导致循环依赖

3). 无法设置注入的对象为final，也无法注入静态变量


2.构造器注入
```java
 @Autowired
public UserServiceImpl(UserDao userDao) {
    this.userDao = userDao;
}
```

当依赖的对象很多时，需要严格按照构造器的顺序去填写依赖的对象，这将导致代码可读性和可维护性变得很差。这时候可以引入Setter方法进行注入，Setter方法和构造器注入很像，不过Setter更具有可读性。



3.set注入

```java
@Autowired
@Qualifier("UserDao")   //默认使用ByType,想要使用ByName时加上@Qualifier()注解并写bean的id值
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```








#### 使用注解实现事务(声明式事务) 

通过事务实现一些方法要么全成功，要么全失败，事务一般在service层写

**1.导入jar包：**
spring事务jar:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
```

数据库jar：
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.25</version>
</dependency>
```

commons-dbcp连接池使用的数据源jar：
```xml
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>
```

连接池jar:
```xml
<dependency>
    <groupId>commons-pool</groupId>
    <artifactId>commons-pool</artifactId>
    <version>1.5.4</version>
</dependency>
```

spring-jdbc jar:
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.17.RELEASE</version>
</dependency>
```

aop jar：
```xml
 <dependency>
    <groupId>aopalliance</groupId>
    <artifactId>aopalliance</artifactId>
    <version>1.0</version>
</dependency>
```

**2.配置**
jdbc/mybatis/spring

配置对事务的支持：
(1.)增加事务tx的命名空间
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd"
>
</beans>
```

(2.)配置spring处理数据库的信息:
```xml
<!--   1. 配置数据库相关 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/user"></property>
    <property name="username" value="root"></property>
    <property name="password" value="wen1106."></property>
    <!-- 连接池的最大连接时间：-->
    <property name="maxActive" value="10"></property>
    <!--连接池最大空闲时间-->
    <property name="maxIdle" value="6"></property>
</bean>

<!--   2. 配置事务管理器-->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--   3. 增加对事务的支持（主要）-->
<tx:annotation-driven transaction-manager="txManager"/>
```

**3.使用**

将需要 成为事物的方法前 增加注解

```java
@Transactional()
```

@Transactional的属性：

1).readOnly	：	是否只读

2).





例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd"
>
  
    <!--   1. 配置数据库相关 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/user"></property>
        <property name="username" value="root"></property>
        <property name="password" value="wen1106."></property>
        <!-- 连接池的最大连接时间：-->
        <property name="maxActive" value="10"></property>
        <!--连接池最大空闲时间-->
        <property name="maxIdle" value="6"></property>
    </bean>

    <!--   2. 配置事务管理器-->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--   3. 增加对事务的支持-->
    <tx:annotation-driven transaction-manager="txManager"/>

</beans>
```

service

```java
@Transactional(readOnly = false,propagation = Propagation.REQUIRED,rollbackFor = {SQLException.class,ArithmeticException.class})    //事务注解：readOnly是否只读
    @Override
    public void addUser() {
        userDao.addUser();
    }
```











#### aop

面向方面编程

切入点：每次调用某方法后，自动执行其他方法





