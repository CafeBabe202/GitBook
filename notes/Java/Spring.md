# 😄 Spring

## IOC

### 创建bean对象

```xml
<bean id="user" class="cn.happyonion801.study.spring.bean.User"></bean>
```

### 属性注入

#### 使用setter

```xml
<bean id="book1" class="cn.happyonion801.study.spring.bean.Book">
	<!--使用property标签
		name : 属性名
        value : 注入的值
	-->
	<property name="name" value="书名1"></property>
	<property name="author" value="张浩"></property>
</bean>
```

#### 使用构造器

```xml
<bean id="book2" class="cn.happyonion801.study.spring.bean.Book">
		<constructor-arg name="name" value="书名2"></constructor-arg>
        <constructor-arg name="author" value="张浩"></constructor-arg>
</bean>
```

#### 使用固定值

```xml
<bean id="book4" class="cn.happyonion801.study.spring.bean.Book">
	<property name="name" value="书名4"></property>
	<property name="author">
		<null></null>
	</property>
</bean>
```

#### 设置特殊字符

```xml
<bean id="book5" class="cn.happyonion801.study.spring.bean.Book">
	<!--使用转义符号-->
	<property name="name" value="&lt;&lt;书名5&gt;&gt;"></property>
	<!--使用CDATA-->
	<property name="author">
		<value><![CDATA[<<张浩>>]]]></value>
	</property>
</bean>
```

#### 注入外部Bean

```xml
<bean id="user1" class="cn.happyonion801.study.spring.bean.User">
	<property name="name" value="张浩"></property>
</bean>
<bean id="book6" class="cn.happyonion801.study.spring.bean.Book">
	<property name="name" value="书名6"></property>
	<property name="author" value="张浩"></property>
	<property name="user" ref="user1"></property>
</bean>
```

#### 注入内部Bean

```xml
<bean id="emp1" class="cn.happyonion801.study.spring.bean.Emp">
	<property name="name" value="张浩"></property>
	<property name="gender" value="男"></property>
	<property name="dept">
		<bean id="dept1" class="cn.happyonion801.study.spring.bean.Dept">
			<property name="name" value="财务部"></property>
		</bean>
	</property>
</bean>
```

#### 级联赋值

```xml
<bean id="dept2" class="cn.happyonion801.study.spring.bean.Dept">
	<property name="name" value="安保部"></property>
</bean>
<bean id="emp2" class="cn.happyonion801.study.spring.bean.Emp">
	<property name="name" value="张浩"></property>
	<property name="gender" value="男"></property>
	<property name="dept" ref="dept2"></property>
	<!--要设置Emp的getDept方法才行，只有可以获取dept才能进行赋值-->
	<property name="dept.name" value="技术部"></property>
</bean>
```

#### 注入集合

```xml
<bean id="stu1" class="cn.happyonion801.study.spring.bean.Stu">
	<!--注入数组-->
	<property name="course">
		<array>
			<value>语文</value>
			<value>数学</value>
		</array>
	</property>
	<!--注入set-->
	<property name="set">
		<set>
			<value>ele1</value>
			<value>ele2</value>
		</set>
	</property>
	<!--注入list-->
	<property name="list">
		<list>
			<value>list1</value>
			<value>list2</value>
		</list>
	</property>
	<!--注入map-->
	<property name="map">
		<map>
			<entry key="k1" value="v1"></entry>
			<entry key="k2" value="v2"></entry>
		</map>
	</property>
</bean>
```

#### 在集合中注入对象

```xml
<bean id="stu2" class="cn.happyonion801.study.spring.bean.Stu">
	<property name="list" ref="list"></property>
	<property name="books">
		<list>
			<ref bean="book1"></ref>
		</list>
	</property>
</bean>
```

#### 将集合单独提出来

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util
                           http://www.springframework.org/schema/util/spring-util.xsd">
<util:list id="list">
    <value>list1</value>
    <value>list2</value>
</util:list>
```

### 创建工厂Bean

```java
public class MyBean implements FactoryBean<Book> {
    @Override
    public Book getObject() throws Exception {
        Book book = new Book();
        return book;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

```xml
<!--在spring中有两种bean：一种是普通bean，另一种是工厂bean-->
<!--
	普通bean：返回的类型与xml中定义的一样
	工厂bean：返回的类型与返回的类型可以不一样，工厂 bean 是一个工厂，用来创建 bean 对象，当你获取一个工厂 bean 时，spring 将调用该工厂的 getObject 方法获取一个 bean 对象。如果你的工厂 bean 的 isSingleton 方法返回了 true，spring 会将你的第一次调用 getObject 获取的对象进行保存，那不论你获取多少次对象，都将返回同一个 bean 对象
-->
<!--演示工厂bean-->
<!--
	创建一个bean，实现FactoryBean
	实现接口中的方法，在实现的方法中定义返回值
-->
<bean id="myBean1" class="cn.happyonion801.study.spring.bean.MyBean"></bean>
```

### Bean的作用域

```xml
<!--bean的作用域： 可以设置是单实例还是多实例对象；默认是单实例对象==获取多个对象的地址相同==-->
<!--scope属性值
	singleton 单实例 加载配置文件时创建对象 默认
	prototype 多实例 在获取对象时才创建对象
	requests
	session
-->
<!--设置多实例对象-->
<bean id="book7" class="cn.happyonion801.study.spring.bean.Book" scope="prototype"></bean>
```

### Bean的生命周期

```java
public class MyBeanPost implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean,String beanName) throws BeansException {
        System.out.println("3、把bean实例传递给bean的前置处理器 ==postProcessBeforeInitialization()==");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean,String beanName) throws BeansException {
        System.out.println("5、把bean实例传递给bean的后置处理器 ==postProcessAfterInitialization()==");
        return bean;
    }
}
```

```xml
<!--bean的后置处理器 ==添加后置处理后的生命周期==
	1、通过构造器创建对象
	2、为bean设置属性和其他bean的引用 ==通过set方法==
	3、把bean实例传递给bean的后置处理器 ==postProcessBeforeInitialization()== 注意方法名
	4、调用bean的初始化方法 ==需要进行配置==
	5、把bean实例传递给bean的后置处理器 ==postProcessAfterInitialization()==  注意方法名
	6、bean可以使用了
	7、当容器关闭时，调用bean的销毁方法 ==需要进行配置==
-->

<!--创建后置处理器：
	1、实现BeanPostProcessor
	2、实现方法
-->

<!--配置后置处理器 ==创建后置处理器后会为所有对象都使用后置处理器==-->
<bean id="myBeanPost1" class="cn.happyonion801.study.spring.bean.MyBeanPost"></bean>
```

### 自动装配

```xml
<!--什么是自动装配
	根据指定的装配规则（属性名称，或者属性类型），Spring自动将匹配的属性值进行自动注入
-->

<!--autowire
	byName 根据名称注入，注入的bean的值和类型的名称一样
	byType 根据类型注入，对应类型的bean只能有一个才行
-->
<bean id="dept" class="cn.happyonion801.study.spring.bean.Dept">
		<property name="name" value="自动注入"></property>
</bean>
<bean id="emp3" class="cn.happyonion801.study.spring.bean.Emp" autowire="byName"></bean>
```

### 引用外部配置文件

```xml
<!--引入外部properties文件
	1、添加context命名空间
	2、创建一个properties文件
	3、将properties文件引入xml
	4、使用变量表达式代替固定值
-->
<context:property-placeholder location="classpath:demo.properties" file-encoding="UTF-8"></context:property-placeholder>
<bean id="book8" class="cn.happyonion801.study.spring.bean.Book">
	<property name="name" value="${bookName}"></property>
	<property name="author" value="${authorName}"></property>
</bean>
```

### 注解

#### 使用注释创建bean

```xml
<!--什么是注解
	1、注解是代码的特殊标记
	2、注解可以作用在类、方法、属性上边
	3、使用注解简化xml配置
-->

<!--Spring针对bean管理中的注解
    1、@Component：最普通的组件，可以被注入到spring容器进行管理
    2、@Service：作用于业务逻辑层
    3、@Controller：作用于表现层（spring-mvc的注解）
    4、@Repository：作用于持久层
-->

<!--上边四个功能是一样的，都可以用来创建bean对象-->

<!--使用注解
    1、添加aop依赖
    2、开启组件扫面 （要添加context命名空间）
-->

<!--开启组件扫描-->
<!--想要扫描多个包，可以使用逗号分开-->
<context:component-scan base-package="cn.happyonion801.study.spring.bean.scan,cn.happyonion801.study.spring.bean.dao"></context:component-scan>

<!--组件扫面的细节配置
    use-default-filters="false" 设置默认全部不扫描
    context:include-filter 设置扫描那些
    type 设置过滤方式 ==annotation：基于注解的进行过滤==
    expression 设置符合过滤的条件 ==org.springframework.stereotype.Component：制定的注解类==
-->
<context:component-scan base-package="cn.happyonion801.study.spring.bean.dao" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
</context:component-scan>

<!--
    不设置use-default-filters默认是true，默认全部扫描
    context:exclude-filter 设置不扫描那些
    type 同上
    expression 同上
-->
<context:component-scan base-package="cn.happyonion801.study.spring.bean.dao">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Component"/>
</context:component-scan>
<!--基于注解的属性注入
    @AutoWired 根据类型进行自动注入
    @Qualifier 根据名称进行自动注入 ==要和@AutoWired一起使用==
    @Resource 既可以根据属性类型进行注入，又可以根据名称进行注入
    @Value 注入普通类型属性
-->
```

```java
@Service
public class UserService {
    @Autowired @Qualifier(value = "userDao1")
    private UserDao userDao1;
    @Value(value = "name")
    private String name;
    public void add(){
        System.out.println(this.name + ": UserService add...");
        userDao1.add();
    }
}
```

```java
// value 值可以不写，默认是首字母小写的类名
@Component(value = "userDao1")  //<bean id="userDao1" class="cn.happyonion801.study.spring.bean.dao.USerDao"></bean>
public class UserDao {
    public void add(){
        System.out.println("UserDao add...");
    }
}
```

#### 完全注释开发

```java
@Configuration  //声明是配置文件
@ComponentScan(basePackages = {"cn.happyonion801.study.spring.bean"}) //开启扫描
public class Config {
}
```

## AOP

面向切面编程（面向方面编程）

### 原理

#### 1、底层是用动态代理

（1）动态代理的两种情况

* 有接口的情况，使用JDK的动态代理；创建接口类的代理对象，增强类中的方法
* 没有接口的情况，使用CGLIB的动态代理；通过创建子类来增强类中的方法

#### 2、使用JDK实现动态代理

```java
public interface UserDao {
    public int add(int a,int b);

    public String update(String id);
}
```

```java
public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        System.out.println("执行方法");
        return a+b;
    }

    @Override
    public String update(String id) {
        return null;
    }
}
```

```java
public class JDKProxy {
    public static void main(String[] args) {
        UserDao userDao = (UserDao) Proxy.newProxyInstance(
                JDKProxy.class.getClassLoader(),
                new Class[]{UserDao.class},
                new UserDaoProxy(new UserDaoImpl()));
        int res = userDao.add(1,1);
        System.out.println("res=" + res);
    }
}

class UserDaoProxy implements InvocationHandler {

    //被代理类对象
    private Object obj;

    //把被代理对象传递进来
    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        /**
         * proxy：代理类对象
         * method：被代理类的方法对象
         * args：形参
         */
        System.out.println("在方法执行前");
        //注意是执行obj（被代理对象）对象的方法，而不是proxy（代理对象）对象的方法
        Object res = method.invoke(obj,args);
        System.out.println("在方法执行后");
        return res;
    }
}
```

### 术语

连接点：类里面那些方法能够被增强

切入点：实际真正被增强的方法

通知（增强）：实际增强的逻辑部分；分为：前置通知、后置通知、环绕通知、异常通知、最终通知

切面：把通知应用到切入点的过程

### AOP准备

1、Spring框架一般都是基于AspectJ实现AOP操作

​ AspecJ不是Spring的组成部分，是独立的AOP框架，一般AspectJ和Spring框架一起使用，进行AOP操作。

2、基于AspectJ实现AOP

（1）基于xml方式实现

（2）基于注解方式实现

3、添加依赖

4、切入点表达式

（1）切入点表达式作用：知道对那个类里面的方法进行增强

（2）语法结构：execution( \[权限修饰符] \[返回类型] \[全类路径] \[方法名] (\[参数类表]) )

5、切入点表达式举例

（1）对cn.happyonion801.study.spring.DAO.BookDao的add方法进行增强

execution(\* cn.happyonion801.study.spring.DAO.BookDao.add(...))

（2）对cn.happyonion801.study.spring.DAO.BookDao的所有方法进行增强

execution(\* cn.happyonion801.study.spring.DAO.BookDao.\*(...))

（3）对cn.happyonion801.study.spring.DAO.包中的所有类的所有方法进行增强

execution(\* cn.happyonion801.study.spring.DAO.\* .\*(...))

### 基于xml实现增强

1、添加aop命名空间

2、创建代理类和被代理类的bean对象

3、配aop，设置切入点和切面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="userDaoImpl" class="cn.happyonion801.study.spring.aop.UserDaoImpl"></bean>
    <bean id="userDaoImplProxy" class="cn.happyonion801.study.spring.aop.UserDaoImplProxy"></bean>
    <!--开启AspectJ生成对象-->
    <aop:aspectj-autoproxy  proxy-target-class="true"></aop:aspectj-autoproxy>
    <aop:config>
        <!--设置切入点-->
        <aop:pointcut id="add" expression="execution(* cn.happyonion801.study.spring.aop.UserDaoImpl.add(..))"/>
        <aop:aspect ref="userDaoImplProxy">
            <aop:before method="after" pointcut-ref="add"/>
        </aop:aspect>
    </aop:config>
</beans>
```

```java
public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        System.out.println("执行方法");
        return a+b;
    }

    @Override
    public String update(String id) {
        return null;
    }

    public void add(){
        System.out.println("add...");
    }
}
```

```java
public class UserDaoImplProxy {
    public void before(){
        System.out.println("before...");
    }
}
```

### 基于配注解实现增强

1、创建配置类

2、注解生成bean对象

3、为代理类添加@Aspect注解

4、应用切面

```java
@EnableAspectJAutoProxy(proxyTargetClass = true)
@Configuration
public class Aop {
    @Bean
    public UserDaoImpl userDaoImpl(){
        return new UserDaoImpl();
    }
    @Bean
    public UserDaoImplProxy userDaoImplProxy(){
        return new UserDaoImplProxy();
    }
}
```

```java
public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        System.out.println("执行方法");
        return a+b;
    }

    @Override
    public String update(String id) {
        return null;
    }

    public void add(){
        System.out.println("add...");
    }
}
```

```java
@Aspect
@Order(1)
public class UserDaoImplProxy {

    @Pointcut(value = "execution(* cn.happyonion801.study.spring.aop.UserDaoImpl.add(..))")
    public void pointCut(){
    }

    @Before(value = "pointCut()")
    public void before(){
        System.out.println("before...");
    }

    @After(value = "pointCut()")
    public void after(){
        System.out.println("after...");
    }

    @Around(value = "pointCut()")
    public void around(ProceedingJoinPoint joinPoint)throws Throwable{
        System.out.println("around before...");
        joinPoint.proceed();
        System.out.println("around after...");
    }

    @AfterThrowing(value = "pointCut()")
    public void afterThrowing(){
        System.out.println("afterThrowing...");
    }

    @AfterReturning(value = "pointCut()")
    public void afterReturning(){
        System.out.println("afterReturning...");
    }
}

```

### 注解说明

@Configuration 声明是配置文件

@ComponentScan 开启扫描

@EnableAspectJAutoProxy(proxyTargetClass = true) 自动生成代理对象

@Component 创建bean对象

@Aspect 声明是增强类

@Before 前置通知

@After 后置通知

@Around 环绕通知

@AfterThrowing 异常通知

@AfterReturning 最终通知

@PointCut 声明切入点

@Order 声明优先级数值越小，优先级越大

## JDBCTemplate

### 什么是JDBCTemplate

Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

### 准备工作

1、添加 jar 包依赖

2、创建 com.alibaba.druid.pool.DruidDataSource 数据库连接池 bean 对象

3、创建 org.springframework.jdbc.core.JdbcTemplate 模板 bean 对象

4、将 datasource 注入给 jdbcTemplate

5、创建 Dao 层类，将 jdbcTemplate 注入给 Dao 层对象

6、将 Dao 层对象注入给 Service 层

### JDBCTemlpate 实现 CURD

这一部分比较简单，所以就过多的进行解释了，直接贴上了代码，写了点简单的注释，应该很容易看懂。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--开启组件扫描-->
<context:component-scan
        base-package="cn.happyonion801.study.spring.jdbc"></context:component-scan>
<!--创建dataSource-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/spring?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC&amp;useSSL=false"></property>
    <property name="username" value="root"></property>
    <property name="password" value="Admin@123"></property>
</bean>
<!--创建 jdbcTemplate -->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>
</beans>
```

```java
@Component
public class BookDaoImpl implements BookDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public int addBook(Book book) {
        String sql = "insert into book values(?,?,?)";
        /**
         * 更新操作，用来增删改
         * arg 1 ： sql语句
         * arg ... ： sql参数
         */
        return jdbcTemplate.update(sql, book.getId(), book.getBook_status(), book.getBook_name());
    }

    @Override
    public int count() {
        /**
         * 用来获取单一的数值，比如查看一共多少条数据
         * arg 1 ： sql语句
         * arg 2 ： 返回值的类型的Class
         */
        String sql = "select count(*) from book";
        return jdbcTemplate.queryForObject(sql, Integer.class);
    }

    @Override
    public Book get(int id) {
        /**
         * 用来查询一个对象
         * arg 1 ： sql语句
         * arg 2 ： 一个BeanPropertyRowMapper，相当于设置返回值类型
         * arg ... ： sql参数
         */
        String sql = "select * from book where id = ?";
        Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>(Book.class), id);
        return book;
    }

    @Override
    public List<Book> list() {
        /**
         * 用来查询集合
         * arg 1 ： sql语句
         * arg 2 ： 一个BeanPropertyRowMapper，相当于设置返回值类型
         * arg ... ： sql参数 （ 由于这里没有参数，所以没有写 ）
         */
        String sql = "select * from book";
        return jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>(Book.class));
    }

    @Override
    public int[] batchAdd(List<Book> books) {
        /**
         * 批量添加操作,删改也是同样的操作，就不演示了；查询的话不涉及批量操作
         * arg 1 ： sql语句
         * arg 2 ： 是一个List<Object[]>类型，每一个Object[]就一组args，然后遍历list循环进行操作
         */
        String sql = "insert into book value(?,?,?)";
        List<Object[]> args = new ArrayList<>();
        for (int i = 0; i < books.size(); i++) {
            Book book = books.get(i);
            Object[] arg = {book.getId(), book.getBook_status(), book.getBook_name()};
            args.add(arg);
        }
        return jdbcTemplate.batchUpdate(sql, args);
    }
}
```

```java
@Repository
public class BookService {
    @Autowired
    private BookDao bookDao;

    //添加方法
    public int addBook(Book book){
        return bookDao.addBook(book);
    }

    public int count(){
        return bookDao.count();
    }

    public Book get(int id){
        return bookDao.get(id);
    }

    public List<Book> list(){
        return  bookDao.list();
    }

    public int[] batchAdd(List<Book> books){
        return bookDao.batchAdd(books);
    }
}
```

```java
public class Test {
    @org.junit.Test
    public void add(){
        Book book = new Book(2,"a","bc");
        ApplicationContext context = new ClassPathXmlApplicationContext("jdbc.xml");
        BookService bookService = context.getBean("bookService", BookService.class);
        System.out.println(bookService.addBook(book));
    }

    @org.junit.Test
    public void count(){
        ApplicationContext context = new ClassPathXmlApplicationContext("jdbc.xml");
        BookService bookService = context.getBean("bookService", BookService.class);
        System.out.println(bookService.count());
    }

    @org.junit.Test
    public void get(){
        ApplicationContext context = new ClassPathXmlApplicationContext("jdbc.xml");
        BookService bookService = context.getBean("bookService", BookService.class);
        System.out.println(bookService.get(1));
    }

    @org.junit.Test
    public void list(){
        ApplicationContext context = new ClassPathXmlApplicationContext("jdbc.xml");
        BookService bookService = context.getBean("bookService", BookService.class);
        System.out.println(bookService.list());
    }

    @org.junit.Test
    public void batchAdd(){
        List<Book> books = new ArrayList<>();
        books.add(new Book(3,"3","3"));
        books.add(new Book(4,"4","4"));
        books.add(new Book(5,"5","5"));
        ApplicationContext context = new ClassPathXmlApplicationContext("jdbc.xml");
        BookService bookService = context.getBean("bookService", BookService.class);
        System.out.println(Arrays.toString(bookService.batchAdd(books)));
    }
}
```

### 事务

#### 什么是事务

一条或多条SQL语句组成的执行单位，一组SQL要么都执行，要么都不执行

#### 事务的特点

原子性（A）：一个事务是不可拆分的整体，要么都执行，要么都不执行

一致性（C）：一个事务可以是数据从一个一致性状态到另一个一致性状态

隔离性（I）：一个事务不受其他事务事务干扰，多个事务相互隔离

持久性（D）：一个事务一旦提交了，则永久的持久化到本地

#### 操作事物的基本流程

1、开启事务操作

2、进行业务操作

3、没有异常就提交任务

4、有异常就是进行任务回滚

#### spring 进行任务管理的介绍

1、事务一般添加在 javaEE 三层中的业务逻辑层上

2、在 spring 中进行事务管理的方式

* 编程式；不推荐使用
* 声明式；推荐使用

3、声明式事务管理

* 基于注解方式；简单，方便
* 基于 xml 方式

4、在 spring 中进行声明式管理，底层使用 AOP

5、spring 事务管理中相关的 API

* 提供了一个接口，代表事务管理器，这个接口针对不同的框架实现了不同的实现类。

#### 在 spring 中进行事务声明

1、在 spring 中创建事务管理器

2、添加 tx 的命名空间

2、开启事务注解

3、在类或方法上边添加事务注解 @Transactional

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--开启组件扫描-->
    <context:component-scan
            base-package="cn.happyonion801.study.spring.jdbc"></context:component-scan>
    <!--创建dataSource-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/spring?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC&amp;useSSL=false"></property>
        <property name="username" value="root"></property>
        <property name="password" value="Admin@123"></property>
    </bean>
    <!--创建 jdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 创建事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 开始事务注解 -->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
</beans>
```

```java
@Repository
public class UserDaoImpl implements UserDao{
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public void addMoney() {
        String sql = "update user set money=money+? where user_name=?";
        jdbcTemplate.update(sql,100,"mary");
    }

    @Override
    public void reduceMoney() {
        String sql = "update user set money=money-? where user_name=?";
        jdbcTemplate.update(sql,100,"lucy");
    }

    public void insert(){
        String sql = "insert user values(?,?,?)";
        jdbcTemplate.update(sql,1,"mary",1000.0);
        jdbcTemplate.update(sql,2,"lucy",1000.0);
    }
}
```

```java
@Component
/**
 * 声明是一个事务
 * 如果注解添加在类上边，那么，该类的所有方法将都启用事务
 */
@Transactional
public class UserService {
    @Autowired
    private UserDao userDao;

    public void accountMoney(){
        userDao.reduceMoney();
        userDao.addMoney();
    }
}
```

#### @Transactional中的属性

1、propagation：事务的传播行为

2、ioslation：事务的隔离级别

* 为了解决隔离性的问题，保证多事务操作之间不会产生影响
* 有的三个问题：脏读、不可重复读、幻（虚）读
* 详细内容见Mysql笔记

3、timeout：超时时间

* 超过一定时间没有提交的事务自动提交

4、readOnly：是否只读

* 如果一个事务设置为只读，那么只能进行查询操作，将不能进行增删改操作

5、rollbackFor：回滚

* 设置出现那些异常时进行事务回滚

6、noRollbackFor：不回滚

* 设置出现那些异常是不进行事务回滚

#### 基于xml声明式进行事务管理

1、配置事务管理器

2、配置通知

3、配置切入点以及切面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--开启组件扫描-->
    <context:component-scan
            base-package="cn.happyonion801.study.spring.jdbc"></context:component-scan>
    <!--创建dataSource-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/spring?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC&amp;useSSL=false"></property>
        <property name="username" value="root"></property>
        <property name="password" value="Admin@123"></property>
    </bean>
    <!--创建 jdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 创建事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 开启事务注解 -->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

    <!--配置通知-->
    <tx:advice id="txadvice">
        <!--配置事务参数-->
        <tx:attributes>
            <tx:method name="accountMoney" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!--配置切入点和切面-->
    <aop:config>
        <!--配置切入点-->
        <aop:pointcut id="pt" expression="execution(* cn.happyonion801.study.spring.jdbc.service.UserService.*(..))"/>
        <!--配置切面-->
        <aop:advisor advice-ref="txadvice" pointcut-ref="pt"></aop:advisor>
    </aop:config>
</beans>
```

java代码与基于注解的一样，只不过不同添加 @Transactional 注解

#### 完全注解开发

配置类如下，其他的都一样

```java
@Configuration //声明这是一个配置类
@ComponentScan(basePackages = "cn.happyonion801.study.spring.jdbc") //开启组件扫描
@EnableTransactionManagement //启用事务管理
public class Jdbc {
    @Bean
    public DruidDataSource getDruidDataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/spring?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC&useSSL=false");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("Admin@123");
        return druidDataSource;
    }

    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //到IOC容器中找到相应类型的对象，进行注入
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return  dataSourceTransactionManager;
    }
}
```

## Spring5中的新功能

1. 整个 spring5 基于 java8，运行时兼容jdk9，将不建议的类和方法删除
2. spring5框架自带了通用的日志封装，也可以整合第三方日志框架
   * spring5移出了Log4jConfigListener，官方建议使用Log4j2
   * sprig5框架整合Log4j2
   * 引入依赖
     * 创建Log4j2配置文件（Log4j2.xml 固定的）
3. spring5支持@Nullable注解
   * 在方法上边，方法的返回值可以为空
   * 使用在方法参数里，方法的参数可以为空
   * 使用在属性上边，属性值可以为空

​
