# 😄 MyBatis

## 基础入门

### Mybatis简介

> MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

mybatis的学习建议配合[官方的文档](https://mybatis.org/mybatis-3/zh/index.html)进行学习，官方文档很是详细，也有中文版，对于新手来说很友好。

### 环境搭建

pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.21</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.6</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.16</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--
        在 environments 中可以配置多个不同的环境
        每一个环境都有一个id，可以使用default来选择一个环境
    -->
    <environments default="development">
        <environment id="development">
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/study?useSSL=false&amp;setUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC&amp;writeBatchedStatements=true"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    
    <!--
        映射器的配置有四种方式，但是有url的方式不建议使用，只讲解三种方式
        方式一：
            resource 用资源文件的全限定名来指定一个资源文件
        方式二：
            class 使用接口类的类路径来注册一个映射器，但是xml文件名和接口类名应该一应并且处于同一个包下
        方式三：
            package 使用package标签来注册同一个包下的所有xml文件
    -->
    <mappers>
        <package name="cn.happyOnion801.study.myBatis.dao"/>
    </mappers>
</configuration>
```

这样，一个简单Mybatis的环境就已经搭建起来了，mybatis-config.xml放置在resources文件夹下。

编写一个简单的Mapper接口，然后让我们开始简单的CURD操作。

### CURD

过多的描述我就不巴巴了，很多的注释，应该很容易看懂。

UserMapper.java

```java
/**
 * 可以通过注释添加sql语句，直接进行查询，简单查询很方便，但是复杂查询很难受，不建议使用这种方式
 * 如果通过注解进行接口的实现，请通过class的方式进行Mapper的注册
 *
 * #{} 和 ${} 的区别
 * #{} 相当于PrePareStatement ，可以很大程度上防止 sql 注入，建议使用
 * ${} 相当于Statement ,不能防止 sql 注入，不建议使用
 *
 * @Param 作用
 * 用于映射参数名和占位符名，如果有多个参数，在基本变量类型和String前必须使用
 */
public interface UserMapper {
    User getUserByName(String name);

    User getUserByNameLike(String name);

    List<User> getUserList();

    int insertUser(User user);

    int insertUserByMap(Map<String, Object> map);

    @Delete("delete from user where name = #{na}")
    int deleteUserByName(@Param("na") String name);

    int updateUserByName(User user);
}
```

这就是一个简单的Dao层接口，然后，你需要一个xml来通过MyBatis来实现上边的方法。xml文件的名称**建议**和接口的名称一样，这样可以进行自动包扫描配置，但是目前阶段都无无所谓。我将xml文件放置在了rescourses文件夹下。

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace 就是 Dao 接口所对应的全类名 -->
<mapper namespace="cn.happyOnion801.study.myBatis.dao.UserMapper">

    <!--
        如果 pojo 类的属性名和数据库的字段名不统一，可以使用 resultMap 的方式来进行统一
        每一个 resultMap 对应一个 id ，sql 标签应该使用 resultMap 来指定一个映射器
        resultMap 使用 type 来指定类型
    -->
    <resultMap id="UserMapper" type="cn.happyOnion801.study.myBatis.pojo.User">
        <result column="name" property="username"/>
    </resultMap>

    <!--
		id 是 namespace 对应的类的方法名
        resultType 的类型是返回集合的泛型的全类名
        中间是要执行的 sql 语句
        parameterType 是参数的类型
        在 sql 是用 #{变量名} 来充当占位符
        对于对象类型可以直接使用属性名来充当占位符变量
        可以通过 Map 来传递一个键值对
    -->
    <!--
        注意不能出现有同名的函数，即使参数不同也不行，在 xml 中会出错
    -->
    <select id="getUserList" resultMap="UserMapper">
        select *
        from user
    </select>

    <select id="getUserByName" resultMap="UserMapper" parameterType="String" resultType="cn.happyOnion801.study.myBatis.pojo.User">
        select *
        from user
        where name = #{name}
    </select>

    <select id="getUserByNameLike" resultMap="UserMapper">
        select *
        from user
        where name like #{username}"%"
    </select>

    <insert id="insertUser" parameterType="cn.happyOnion801.study.myBatis.pojo.User">
        insert into user(name, age) value (#{username},#{age})
    </insert>

    <insert id="insertUserByMap" parameterType="map">
        insert into user(name, age) value (#{姓名},#{年龄})
    </insert>

    <update id="updateUserByName" parameterType="cn.happyOnion801.study.myBatis.pojo.User">
        update user
        set age = #{age}
        where name = #{username};
    </update>

</mapper>
```

这样就成功使用mybatis实现了你的接口，mybatis可以自动为你生成代理对象来执行接口中的方法，下面就让我们编写一个测试程序来看看我们的代码是否能够正常的运行。

在运行你的代码之前，请不要忘记将你的UserMapper文件的位置告诉myBatis，这样它才能正常的完成你指定的工作。要想告诉myBatis你只mybatis-config.xml文件中添加如下的配置代码。

```xml
<mappers>
    <mapper class="cn.happyOnion801.study.myBatis.dao.UserMapper"/>
</mappers>
```

```java
public class UserMapperTest {
    @Test
    public void test(){
        try {
            // mybatis-config.xml 相对于src的位置
            String resource = "mybatis-config.xml";
            // 创建一个建造者来构造一个工厂
            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            // 通过配置文件来构造一个工厂
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream(resource));
            // 开启一个 SqlSession ，相当于 jdbc 中的 connection
            SqlSession session = sqlSessionFactory.openSession();
            // 通过 session 来获取刚刚定义的接口个一个代理对象
            UserMapper mapper = session.getMapper(UserMapper.class);
            // 通过代理对象来进行操作
            mapper.getUserList().forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

运行上边的代码，不出意外的情况下你会很惊讶发现这太神奇了，获取你还没有弄清楚改改发生了啥，但是你的确已经通过mybatis框架成功从数据库中经你的数据查询了出来。先不要感叹，我觉得你需要将你的代码再看一边。然后自己动手试一试其他的方法。

## 更多配置

没错，经过简单的几个文件你已经通过入门教程，让我们来看看还有那些更常用的配置

### properties

properties是一个很神奇的标签，你可以通过这个标签来设置一些变量并在配置文件中使用他们。

来看看吧一个简单的应用吧！首先创建一个db.properties文件。

```properties
username=root
password=Admin@123
```

在你的mybatis-config.xml文件的最上边（当然要在xml文件头的下边）添加如下内容：

```xml
<!--
    可以使用properties来声明配置
    使用resource来引用外置的配置文件
    可以使用property标签来注册新的属性变量
    如果在配置文件和标签中出现同名的变量则使用配置文件中的值
-->
<properties resource="db.properties">
    <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url"
              value="jdbc:mysql://localhost:3306/study?useSSL=false&amp;setUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC&amp;writeBatchedStatements=true"/>
    <property name="password" value="123456"/>
</properties>
```

然后将你的mybatis-config.xml的environments标签改成如下写法：

```xml
<environments default="development">
    <!--
        transactionManager的属性值有 JDBC | MANAGED 两个，默认是JDBC，另一个要知道
        dataSource的属性值有 UNPOOLED | POOLED | JNDI 三个，默认是POOLED，另外零个要知道
    -->
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>
</environments>
```

希望你能够成功运行你的程序，你可能发现中并没有太大的用处。不过或许以后用的到。

### 配置Log4J

配置Log4J是一个很简单的操作，首先你需要创建一个Log4j的配置文件

log4j.properties

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c] - %m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=[%p] [%d{yy-MM-dd}] [%c]%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

然后，你需要在你的mybatis配置文件中配置Log4j，着同样很简单，只需要在你的mybatis配置文件中添加如下的配置代码。

```xml
<!--
    settings 标签用来对 myBatis 进行配置
    通过name是配置的名字，value 是个配置的取值
    具体的配置名和取值请参见官方文档
-->
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

有一个问题就是你需要放在文档的正确位置，不然你的代码可能不会正常工作。你需要将setting标签放在properties标签的下边。

解决一些可能发生的问题后，你可能成功运行了你的代码，并看到了日志，你可能开始觉得这一切并没有我说的那样容易，不过似乎也并没有太坏。

### 设置别名

通过上边的代码，你会发现有一个东西会让你抓狂，那就是自定义类型要写类的全路径，这太烦了，那就给他起一个别名吧。

设置别名有两种方法。你需要将下边的配置信息插入到mybatis-config.xml的setting标签的下边，没错，就是刚刚配置Log4j的那个setting标签。

```xml
<!--
    设置别名
    可以用typeAlias来设置具体某一个类的别名
    可以使用package来设置一个包下的所有类的别名，默认是首字母小写的类名，可以使用@Alias来设置这种配置方式的具体别名
    myBatis有一些默认的别名，int -> _int , Integer -> int
    具体的信息请参考官方文档
-->
<typeAliases>
    <typeAlias type="cn.happyOnion801.study.myBatis.pojo.User" alias="user"/>
    <typeAlias type="cn.happyOnion801.study.myBatis.pojo.Book" alias="book"/>
    <package name="cn.happyOnion801.study.myBatis.pojo"/>
</typeAliases>
```

## 花花肠子

### 动态SQL

此处省略N个字，请自行查看官方文档的动态SQL部分，这部分看上去很难，但是不难，真的，动手试试就会了，我在这写一边也没有什么意义，官方已经很详细了。

### 一对多

好吧！ 我放了两天还是决定写这一部分，感觉也没有啥好写的，官网已经讲的很好了。

首先你要知道到啥是一对多，想一下你和你的课程，只有一个你，但是一个你有好多课程（如果你觉得这么说太让人悲伤，那可以说一门课程有好多学生），如果是java对象你会怎么写呢？我相信你会向下面这样：

```java
public class Student{
    private String name;
    private List<String> subject;
    ...
}
```

没错，一对多的一个典型表现就是bean拥有集合类型的属性，不管是list或者arr，这没什么区别。让我们来看看看应该这么写Mapper文件吧！

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--两种一对多关系映射-->

<mapper namespace="cn.happyOnion801.study.myBatis.dao.User2Mapper">

    <!--将两个查询都要用的部分抽取出来，将两个子 resultMap 继承这个 resultMap-->
    <resultMap id="user" type="user2">
        <result property="name" column="uname"/>
        <result property="age" column="uage"/>
    </resultMap>

    <!--
       通过 collection 来指定一个属性是一个集合，然后设置每一个对象的映射
       ofType 来指定集合的泛型类型
    -->
    <resultMap id="user1" type="User2" extends="user">
        <collection property="books" ofType="Book">
            <result property="name" column="bname"/>
        </collection>
    </resultMap>
    <select id="getUser1" resultMap="user1">
        select u.name uname, u.age uage, b.name bname
        from user u,
             book b
        where u.name = b.user;
    </select>

    <!--==================== 我是分割线 ====================-->

    <!--
        同样是通过 collection 来制定一个集合类型的属性值
        通过 javaType 来指定集合的类型，通过 ofType 来指定泛型的类型，通过 select 来关联一个子查询语句
        column 来指定一个相对应的列，我感觉有点像传参
    -->
    <select id="getBook" resultType="Book">
        select *
        from book
        where user = #{name}
    </select>
    <resultMap id="user2" type="User2" extends="user">
        <collection property="books" javaType="ArrayList" ofType="Book" select="getBook" column="name"/>
    </resultMap>
    <select id="getUser2" resultMap="user2">
        select *
        from user
    </select>

</mapper>
```

对！没错，你可能觉得这一大段有点多，但是幸运的是这两种方法是相同地位的，你可以先看一种，然后看另一种。

第一种其实很简单，通过sql将你想要的数据一次性全部查询出来，然后将对应的列封装成对象添加到你的集合属性之中，你不要管他是怎么识别两个相同对象的，你只要知道这样写就行了。

问题的关键在于第二种方法，你肯能懂它大概的意思，其实这里有来两个注意的地方：

* javaType 和 ofType
  * javaType：集合的类型，也就是对应的属性的java类型。
  * ofType：集合类的范型类型，也就是你要封装成的对象的类型。
* column有什么用？
  * 细心的会发现 collection 标签的 column，简单来说，他就是外键的 column 名。
  * 我们知道集合中肯定是另一种bean对象，所有肯定对应数据库的另一张表，那怎么表示这种关系呢，那就是外键。
  * 这种方法会有一个子查询，第一次只是查询主表，将外将查询出来，然后解析到collection标签时在通过外键的值去查询附表，然收将查询出来的结果封装成对象放在对应的集合之中。
  * 上面说的外键只是用来帮助你理解，如果你不喜欢外键，当然也可以不设置外键。
* 两种方法什么特点
  * 第一种方法会合并对象，example 一个学生有很多课程，当出现两个一样（所有字段的属性的值都是一样的，我想一般不会出现这种情况）的学生时，就算数据库中有两个学生你也只能查询出来一条，因为myBatis会认为这是同一个对象。（实际上就连你自己也分不清这是一个还是两个对象）。
  * 第二中方法会进行多次数据库查询，所以说有两个对象就不会合并（要是有缓存不知道会不会不一样？），因为他们不是同一时间查询出来的，myBatis不用进行合并工作，也就不存在刚刚的问题。

emmmm。。。。测试代码就不写了吧！真希望你成功的执行了你的代码，并且听懂了刚刚这一切。

### 多对一

现在，你需要将你的思维换过来。没错，就是有好多对象同时拥有一个对象。example，好多人都共享一辆自行车。

```java
public class Human{
    private String name;
    private Bicyle bicyle;
}
```

没错，实际情况下，你的数据库可能是这样的：

```json
Bicyles{
    {id:"001",name:"宝马"}
}
Humans{
    {name:"张三",bicyle:001},
    {name:"李四",bicyle:001},
    {name:"王五",bicyle:001}
}
```

他们共享了一辆宝马，所以。。。。怎么映射？

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--两种多对一关系查询-->

<!--
    缓存这个东西
    在 MyBatis 中分为两种缓存：一级缓存 、 二级缓存
    一级缓存：
        一级缓存是存放在 SqlSession 中的，也就是说，只有图一个 sqlSession 中才会有效
        只有当一个 sqlSession 被关闭后，他的缓存才会被刷新到二级缓存中
    二级缓存：
        二级缓存的的作用域是同一个 Mapper，不管是使用哪一个 sqlSession 创建的 Mapper 的接口，能够实现共享。
        要实现二级缓存，bean 对象应该实现 Serializable 接口。
    查询顺序：
        当执行一个查询语句时，受限查看是否存在以及缓存，如果有直接返回，如果没有就查看二级缓存，如果有就返回，如果没有就进行数据库的查询
    对数据库进行增删改操作会刷新缓存
    更多信息请参考官方文档
-->

<mapper namespace="cn.happyOnion801.study.myBatis.dao.BookMapper">
    <!--
        同样的道理，resultMap 要和 resultMap 标签的 id 相对应
        type=Book ，因为返回的列表的泛型就是 Book
        Book 的 user 属性是一个对象类型，并不是基本类型或者 String，所以使用 association 标签
        property 就是在 Book 对象中的属性名，javaType 是具体的 java 类型
        在 association 中再使用 result 来映射属性名和字段名
        我感觉这种方式是最好的，比较容易理解，有点像 Spring 的 bean 注入对象类型

        如果存在两个状态相同的 Book 对象，那么将会合并对象，只返回一个结果
    -->
    <resultMap id="Book1" type="Book">
        <result property="name" column="bname"/>
        <association property="user" javaType="user">
            <result property="username" column="uname"/>
            <result property="age" column="uage"/>
        </association>
    </resultMap>
    <select id="getList1" resultMap="Book1">
        select b.name bname, u.name uname, u.age uage
        from book b,
             user u
        where b.user = u.name
    </select>

    <!--========================== 我是分割线 ==========================-->

    <!--
        这种方式和上边的方式实现的功能是一样的
        在 association 标签中，依旧使用 property 和 column 来映射属性名和字段名
        通过 javaType 来制定该字段具体的 java 类型
        通过 select 来指定一个 sql 语句的 id，来设置该字段的对象
        在 select 子句的占位符参数来源与父查询的列名

        子 select 语句因该是一个行子查询，不然会提示 [Cause: org.apache.ibatis.executor.ExecutorException: Statement returned more than one row, where no more than one was expected.]
        由于缓存的存在，进行子查询时只有第一次是进行真正查询的，后面的查询只要是缓存没有被刷新，就不是真正的从数据库中查询
        所以用这种方式进行查询，每次执行的子查询都是一样的，所以如果是通一个 User 对象，返回的对象是同一个
    -->
    <select id="selectUser" resultType="User">
        select * from user where name = #{user};
    </select>
    <resultMap id="Book2" type="Book">
        <result property="name" column="name"/>
        <association property="user" column="user" javaType="User" select="selectUser"/>
    </resultMap>
    <select id="getList2" resultMap="Book2">
        select name,user from book
    </select>
</mapper>
```

还是有两种方法可以完成这种任务。你同样是应该知道怎么做，看看这 association 标签，是不是与 collection 一奶同胞，不过他并没有 ofType 属性，不过我想你知道为什么。

### 懒加载

懒加载是一个很神奇的东西，要实现这项技术我认为需要开发人员很多的努力，但是对于一个使用者，用起来真的很简单。

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
    <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

看见没，只要在你的mybaits-config.xml文件的setting标签中添加一句话，告诉myBatis你要使用懒加载功能就一切都搞定了。

懒加载可以然你在使用数据时才去查询，比如刚刚的学生对应的课程，如果你没有启用懒加载那么会一次性加载所有学生的所有课程，但是如果你启用了懒加载，哪么只有你使用一个学生的 subject 属性时，才会去查询他的课程。但是两种方法中只有一种才会有效果，我觉得这个需要添加断点调试一下你的程序，看一看输出的日志，然后自己动脑筋想一想这是为什么，并尝试着解释一下这是为什么。

### 缓存

缓存是个很有用的东西，这可能在很大程度上减轻你的数据压力。想一下这种场景：你连续执行两次查询操作。不用想，肯定是相同的结果，为什么要进行两次查询你的，是的，这没有必要，如果你开启了缓存，哪呢myBatis也并没有真正的去查寻，只不过返回给了你刚刚的数据，甚至连他们的内存地址都是一样的。

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
    <setting name="lazyLoadingEnabled" value="true"/>
    <setting name="cacheEnabled" value="true"/>
</settings>
```

开启缓存一样很简单，只需要简单的一条命令就好了，但是开启二级缓存后，你对应的bean要实现Serializable接口才行因为他要储存你的对象，他的储存可能并不只是局限于保存在你的本地内存中，一些高级的操作甚至能将他们储存在网络上的其他服务器上，所以实现Serializable接口还是很有必要的。如果那个查询方法你并不想进行缓存，只要在你的select变迁中将useCache设置为false就好了。

**注意**：只有在对统一张表进行增删改时才会清除缓存，不过这就是有可能发生一个BUG，那就是你不通过myBatis进行数据库修改，那myBatis将并不会进行缓存的刷新，你的数据一致性将受到破坏。关于缓存的其他的细节，请看上边代码中的注释。

### 自定义缓存

myBatis不仅有自己的缓存（好像就是一个Map）而且还支持第三方的缓存和自定义缓存，这听起来很酷，下边然我们来实现一个自己的缓存。

首先创建一个myCache类，你必须要实现他的接口才行。

```java
public class MyCache implements Cache {

    private Map<Object, Object> map;
    private String id;

    public MyCache(String id) {
        this.id = id;
        this.map = new HashMap<>();
    }

    @Override
    public String getId() {
        return this.id;
    }

    @Override
    public void putObject(Object o, Object o1) {
        map.put(o, o1);
    }

    @Override
    public Object getObject(Object o) {
        return map.get(o);
    }

    @Override
    public Object removeObject(Object o) {
        return map.remove(o);
    }

    @Override
    public void clear() {
        map.clear();
    }

    @Override
    public int getSize() {
        return map.size();
    }

    @Override
    public ReadWriteLock getReadWriteLock() {
        return null;
    }
}
```

然后去告诉myBatis你要使用哪一个类进行缓存就行了。

```xml
<cache type="cn.happyOnion801.study.myBatis.utils.MyCache"/>
```

你只需要在你需要配置自定义缓存的Mapper去添加这句配置代码就好了。

注意这并不是在myBatis-config.xml的配置文件中，而是在对应的Mapper的映射文件中。

### 插件

就是myBatis支持一些插件来减少你的代码，去百度吧！这里我就不想讲了！！！！！

* pageHelper
* 逆向工程

这是两个插件，去看看吧，还是挺有用的！

## 高级

### mybatis 原理

Mybatis 的核心是 SqlSessionFactory 和 SqlSession，SqlSessionFactory 用来生成 SqlSession 对象，通过 SqlSessionFactoryBuilder 来创建 SqlSessionFactory 时，将通过传入的流对象来生成 Configuration 和 Environment 来保存配置信息，每次创建 SqlSession 时将配置信息设置给 SqlSession 对象。SqlSessionFactory 相当于一个数据库连接池，它的生命周期应该贯穿整个应用的生命周期，每次进行分配 SqlSession 时将从中获取一个连接来赋值给 SqlSession。SqlSession 相当于 JDBC 中 Connection 类的高级抽象。

### SqlSessionFactoryBuilder

用来创建 SqlSessionFactory 对象。SqlSessionFactoryBuilder 将通过 build 方法传入的流来读取配置文件的内容，然后将生成 Configuration 和 Environment 对象，并设置给 SqlSessionFactory。

具体过程如下：

```java
			/**
             * SqlSessionFactoryBuilder 通过 Build 方法传入一个流对象
             * SqlSessionFactoryBuilder 的主要作用就是通过传入的流对象来解析出 Configuration 和其中的 Environment 对象
             *      Configuration 中封装了你的配置文件中的所有配置选项
             *      Environment 中封装了数据源信息：
             *          private final String id
             *          private final TransactionFactory transactionFactory
             *          private final DataSource dataSource
             *
             * 调用 SqlSessionFactoryBuilder 的
             *      SqlSessionFactory build(InputStream inputStream, String environment, Properties properties)
             *      environment 和 properties 使用 null
             *
             * 然后，通过
             *      XMLConfigBuilder(InputStream inputStream, String environment, Properties props)
             *      XMLConfigBuilder(XPathParser parser, String environment, Properties props)
             *      方法来创建一个 XMLConfigBuilder 对象来解析 xml 文件
             *      XPathParser 和 XNode 真正封装了解析 xml 文件的工作
             *
             * XMLConfigBuilder 的
             *      Configuration parse()
             *      void parseConfiguration(XNode root)
             *      负责解析 xml 文件，并标记一个文件是否解析过，如果解析过 将提示 "Each XMLConfigBuilder can only be used once."
             *      解析完成后，将返回一个 Configuration 对象，这个对象包含了 Environment 等其他环境相关的信息
             *
             * 最后，将这个 Configuration 赋值给 SqlSessionFactory 对象并返回
             */
```

### SqlSessionFactory

SqlSessionFactory 的主要工作就是保存最初读取的配置信息并创建 SqlSession 对象，它相当于一个数据连接池，数据源的信息保存在 Environment 中，每次进行 SqlSession 分配的时候将分配一个 Executor 对象。

```java
// 这是最终创建 SqlSession 的方法
private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
	Transaction tx = null;
    try {
      final Environment environment = configuration.getEnvironment();
      final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment); // 如果 environment 中有事务管理工厂就直接使用，如果没有就创建一个新的 ManagedTransactionFactory
      tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
      final Executor executor = configuration.newExecutor(tx, execType);
      return new DefaultSqlSession(configuration, executor, autoCommit); // 创建一个新的 DefaultSqlSession 返回
    } catch (Exception e) {
      closeTransaction(tx); // may have fetched a connection so lets call close()
      throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
}
```

### SqlSession

SqlSession 是对 JDBC 中 Connectin 的抽象，他的主要任务任务就是执行 sql、生成 Mapper 对象和缓存，mybatis的一级缓存就是存放在 SqlSession 中的。SqlSession 生成 Mapper 是通过 Configuration 中的 MapperRegistry 来完成的。MapperRegistry 中维护了一个 Map，映射了不同类型的 Mapper 对应的 MapperProxyFactory，当读取配置文件时，将自动扫面每个 Mapper 接口的 XML 配置文件，然后注册到该 Mapper 中。MapperProxyFactory 中同样维护了一个 Map 映射了 Method 和 MapperMethod，Method 就是接口中的方法，MapperMethod 就是代理执行的方法（他不是 java.lang.reflect.Method 的子类，而是通过 **MapperMethod.execute(SqlSession sqlSession, Object\[] args)** 来真正的执行 sql ）。获取 Mapper 时，先要从 Map 中获取对应的代理工厂，然后再通过代理工厂来创建一个 Mapper 的代理对象。

```java
//  MapperRegistry 中创建 Mapper 的方法
public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
    final MapperProxyFactory<T> mapperProxyFactory = (MapperProxyFactory<T>) knownMappers.get(type);
    if (mapperProxyFactory == null) {
      throw new BindingException("Type " + type + " is not known to the MapperRegistry.");
    }
    try {
      return mapperProxyFactory.newInstance(sqlSession);
    } catch (Exception e) {
      throw new BindingException("Error getting mapper instance. Cause: " + e, e);
    }
}

// MapperProxyFactory 中的 newInstance 方法
public T newInstance(SqlSession sqlSession) {
    
    final MapperProxy<T> mapperProxy = new MapperProxy<T>(sqlSession, mapperInterface, methodCache);
    return newInstance(mapperProxy);
}
```

```java
public class MapperProxy<T> implements InvocationHandler, Serializable {

  private static final long serialVersionUID = -6424540398559729838L;
  private final SqlSession sqlSession;
  private final Class<T> mapperInterface;
  private final Map<Method, MapperMethod> methodCache;

  public MapperProxy(SqlSession sqlSession, Class<T> mapperInterface, Map<Method, MapperMethod> methodCache) {}

  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    try {
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, args);
      } else if (isDefaultMethod(method)) {
        return invokeDefaultMethod(proxy, method, args);
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
    final MapperMethod mapperMethod = cachedMapperMethod(method);
    return mapperMethod.execute(sqlSession, args);
  }

  private MapperMethod cachedMapperMethod(Method method) {
    MapperMethod mapperMethod = methodCache.get(method);
    if (mapperMethod == null) {
      mapperMethod = new MapperMethod(mapperInterface, method, sqlSession.getConfiguration());
      methodCache.put(method, mapperMethod);
    }
    return mapperMethod;
  }

  @UsesJava7
  private Object invokeDefaultMethod(Object proxy, Method method, Object[] args)
      throws Throwable {
    final Constructor<MethodHandles.Lookup> constructor = MethodHandles.Lookup.class
        .getDeclaredConstructor(Class.class, int.class);
    if (!constructor.isAccessible()) {
      constructor.setAccessible(true);
    }
    final Class<?> declaringClass = method.getDeclaringClass();
    return constructor
        .newInstance(declaringClass,
            MethodHandles.Lookup.PRIVATE | MethodHandles.Lookup.PROTECTED
                | MethodHandles.Lookup.PACKAGE | MethodHandles.Lookup.PUBLIC)
        .unreflectSpecial(method, declaringClass).bindTo(proxy).invokeWithArguments(args);
  }

  private boolean isDefaultMethod(Method method) {}
}
```
