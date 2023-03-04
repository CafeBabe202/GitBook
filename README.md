# 😃 MySQL笔记

## MySQL笔记

### 数据库简介

**数据库相关概念**

**一、数据库好处**

1、可以持久化数据到本地

2、结构化查询

**二、数据库常见概念**

DB：DataBase简写，数据库，储存数据的容器

DBMS：DataBase Management System简写，数据库管理系统，又称为数据库软件或数据库产品，用于创建或管理数据库

SQL：结构化查询语言，用于和数据库通信的语言，不是某个数据库软件特有的，而是几乎所有的数据库主流软件通信的语言

**三、数据存储数据的特点**

1、数据存放到表中，然后表在存放在数据库中

2、一个库中可以有多张表，每张表具有唯一的表名来表示自己

3、表中有一个或者多个列，列又称为”字段“，相当于Java中的”属性“

4、表中的每一行数据，相当于Java中的”对象“

**四、常见数据库管理系统**

MySQL、Oracle、DB2、SqlServer

**MySQL的介绍**

**一、MySQL的背景**

前身是瑞典的一家公司，MySQL AB，08年被收购，09年sun被oracle收购

**二、MySQL的优点**

1、开源、免费、成本低

2、性能高、移植性好

3、体积小，便于安装

**三、MySQL服务的登录和退出**

登录：mysql 【-h 主机名 -P 端口号】 -u 用户名 -p密码

退出：exit或ctrl+c

### DQL语言

**基础查询**

**一、语法**

```mysql
select 查询字段 from 表名;
```

**二、特点**

1、查询的可以是字段、常量、表达式、函数、也可以是多个

2、查询结果是一张虚表

**三、示例**

1、查询单个字段：

```mysql
select 字段名 from 表名
```

2、查询多个字段

```mysql
select 字段名1,字段名2,字段名n from 表名
```

3、查询所有字段

```mysql
select * from 表名
```

4、查询常量

```mysql
select 常量值
```

```
注：字符型和日期型常量值必须用单引号引起来，数值型不需要
```

5、查询函数

```mysql
select 函数名(实参列表)
```

6、查询表达式

```mysql
select 100/10
```

7、起别名

```mysql
方式一：select 字段名 as 别名 from 表名
```

```mysql
方式二：select 字段名 别名 from 表名
```

8、去重

```mysql
select distinct 字段名 from 表名
```

**四、补充**

1、关于 +

```
作用：做加法运算

select 数值+数值；直接运算

select 字符+数值；先试图将字符型转换成数值型，如果成功，则继续运算，否则转换成0，在做运算

select null+值；结果都是null
```

**条件查询**

**一、语法**

```mysql
select 字段名 from 表名 where
```

**二、筛选条件的分类**

1、简单条件查询：

```
< > = <= >= != <=> 
```

2、逻辑运算符：

|  逻辑运算符 |  && | \|\| |  ！  |
| :----: | :-: | :--: | :-: |
| SQL关键字 | and |  or  | not |

3、模糊查询：

```
like ：一般搭配通配符使用，可以判断字符型或数值型

通配符：%表示任意多个字符；_ 表示任意一个字符

between and：在某一区间

in：在某一集和

is null / is not null ：用于判断null值
```

**排序查询**

**一、语法**

```mysql
select 字段名 from 表名 where 筛选条件 order by 排序字段【asc|desc】
```

**二、特点**

1、asc：升序，可以不写；desc：降序

2、排序字段支持单个字段、多个字段、函数、表达式、别名

3、order by的位置一般放在查询语句的最后（limit语句之外）

**常见函数**

**一、概述**

```
功能：类似于java中的方法

好处：提高重用性和隐藏实现细节

调用：select 函数名(实参列表)
```

**二、单行函数**

1、字符函数

|                 函数名                 |                      功能                      |           函数名           |                功能               |
| :---------------------------------: | :------------------------------------------: | :---------------------: | :-----------------------------: |
|        concat(str1,str2,...)        |                     连接字符串                    |         substr()        |               截取字串              |
|              upper(str)             |                      变大写                     |        lower(Str)       |               变小写               |
|           replace(str,a,b)          |                  把str中的a替换成b                 |       length(str)       |             获取字符串长度             |
|              trim(str)              |                    去除前后空格                    |          instr          |           获取子串第一次出现的索引          |
|           lpad(str,len,s)           |                用s左填充str到len个宽度               |     rpad(str,len,s)     |         用s右填充str到len个宽度         |
|              ascii(str)             |               返回字符串最左侧字符的ascii值              |     bit\_length(str)    |         以bit为单位，返回字符串长度         |
| concat\_ws(separator,str1,str2,...) |            用特定字符separator连接参数组成字符串           |  insert(str,x,y,instr)  | 将字str从x位置开始的y个字符替换成instr，位置从1开始 |
|      find\_in\_set(str,strlist)     | 查找制定字符串str在字符集strlist（被“,”分割的字串组成的一个字符串）中的位置 |      left(str,len)      |        返回字符串str左侧的len个字符        |
|            right(str,len)           |                返回字符串右侧的len个字符                |        rtrim(str)       |            去除字符串右侧的空格           |
|              ltrim(str)             |                  去除字符串左侧的空格                  | position(substr IN str) |      查询指定字符串substr在字符串中的位置      |
|          repeat(str,count)          |               返回重复count次的str字符串              |       reverse(str)      |             返回str的反转            |
|          strcmp(str1,str2)          |                    比较两个字符串                   |                         |                                 |

2、数学函数

|       函数名      |       功能       |     函数名    |      功能     |
| :------------: | :------------: | :--------: | :---------: |
|      ceil      |      向上取整      | round(x,y) |  将x保留小数点后y位 |
|    mod(x,y)    |       x%y      |  floor(x)  | 返回不大于x的最大整数 |
| truincate(x,y) | 返回舍去x中小数点y位后的数 |   rand()   |    获取随机数    |
|   ceiling(x)   |   返回不小于x的最大整数  |    pi()    |    返回圆周率    |

3、日期函数

|    函数命名    |      功能     |        函数名       |               功能              |
| :--------: | :---------: | :--------------: | :---------------------------: |
|  year(str) |     返回年     |        now       |            返回日期+时间            |
| month(str) |     返回月     |      curdate     |             返回当前日期            |
|  day(str)  |     返回日     |      curtime     |             返回当前时间            |
|  hour(str) |     返回小时    |   date\_format   |           将日期转变成字符串           |
|   minute   |     返回分钟    |   str\_to\_data  |           将字符串转换成日期           |
|   second   |     返回秒     |     datediff     |          返回两个日期相差的天数          |
|  monthname | 以英文形式返回月分名称 | dateofweek(date) | 返回指定日期是一周中的的几天，周日是1，周一是2，以此类推 |

4、其他函数

|    函数名    |       功能       |      函数名      |      功能     |
| :-------: | :------------: | :-----------: | :---------: |
| version() |   返回当前数据库服务版本  |   database()  |   当前打开的数据库  |
|   user()  |      当前用户      | password(str) | 返回该字符串的密码形式 |
|  MD5(str) | 返回该字符串的MD5加密形式 |               |             |

**三、流程控制函数**

三目运算符

```
if(条件表达式，表达式1，表达式2)：如果条件表达式成立，返回表达式1，否则返回表达式2
```

case用法一

```mysql
case 变量或表达式或字段
when 常量1 then 值1
when 常量2 then 值2
...
else 值n
end
```

case用法二

```mysql
case
when 条件1 then 值1
when 条件2 then 值2
...
else 值n
end
```

**四、分组函数**

1、函数

|  函数名  |  功能  | 函数名 |  功能 |
| :---: | :--: | :-: | :-: |
|  max  |  最大值 | min | 最小值 |
|  sum  |  求和  | avg | 平均值 |
| count | 计算个数 |     |     |

2、特点

①语法：

```mysql
select max(字段) from 表名；
```

②支持的类型

```
sum和avg一般用于处理数值型
max、min、count可以处理任何数据类型
```

③count函数

```
count(字段)：统计该字段非空的个数
count(*)：统计结果集的个数
```

④其他

```
1、以上分组函数都忽略null
2、count(1)：统计结果集的行数
3、和分组函数一同查询字段，要求group by后出现的字段
4、都可以搭配distinct使用，实现去重
	select sum(distinct 字段) from 表；
5、效率：
	MyISAM储存引擎：count(*)效率最高
	InnoDB储存引擎：count(*)=count(1)>count(字段)
```

**分组查询**

**一、语法**

```mysql
select 分组函数，分组后的字段
from 表
where 筛选条件
group by 分组的字段
having 分组后的筛选
order by 排序字段
```

**连接查询**

**一、含义**

当查询中涉及了多个表的字段，需要使用多个表连接

**二、语法**

```mysql
select 字段1，字段2
from 表1，表2
```

**三、笛卡尔积**

当查询多个表时，如果没有添加连接条件，结果为所连接表的笛卡尔积

**四、SQL92语法**

1、等值连接

语法：

```mysql
select 字段
from 表1 别名，表2 别名
where 表1.key=表2.key
and 筛选条件
group by 分组字段
having 分组后的筛选
order by 排序字段
```

特点：

```
1、一般为表起别名
2、多表的顺序可以调换
3、n表连接至少需要n-1个连接条件
4、等值连接的结果是多表的交集部分
```

2、非等值连接

```mysql
语法:
select 查询字段
from 表1 别名，表2，别名
where 非等值连接条件
and 筛选条件
group by 分组字段
having 分组后筛选条件
order by 排序字段
```

3、自连接

```mysql
语法：
select 查询字段
from 表1 别名1，表1 别名2
where 连接条件
and 筛选条件
group by 分组字段
having 分组后的筛选
order by 排序字段
```

**五、SQL99语法**

1、内连接

语法：

```mysql
select 查询字段
from 表1 别名
【inner】 join 表2 别名 on 连接条件
where 筛选条件
group by 分组字段
having 分组后筛选
order by 排序查询
limit 分页
```

特点：

```
1、表的顺序可以调换
2、内连接的结果=多表的交集
3、n表连接至少需要n-1个连接条件
```

2、外连接

语法：

```mysql
select 查询字段
from 表1 别名
left|right|full|【outer】 join 表2 别名 on 连接条件
where 筛选条件
group by 分组条件
having 分组后的筛选
order by 排序
limit 分页
```

特点：

```
1、查询的结果=主表所有的行，如果从表和他匹配的将显匹配行，如果从表没有匹配则显示null
2、left join左边的表是主表，right join右边的表是主表，full join两边全是主表
3、一般用于查询除了交集部分的剩下的不匹配的行
```

3、交叉连接

语法：

```mysql
select 查询字段
from 表1 别名
cross join 表2 别名
```

特点：

```
类似于笛卡尔乘积
```

**子查询**

**一、含义**

1、嵌套在其他语句内部的select语句称为子查询或内查询

2、外面的语句可以是insert、update、delete、select、等，一般是select作为外面语句比较多

3、如果外面为select语句，则此语句成为外查询或卓查询

**二、分类**

标量子查询：结果集为一行一列

列子查询：结果集为多行一列

行子查询：结果集为一行多列

表子查询：结果集为多行多列

**三、语法**

```mysql
select：标量子查询
from：表子查询
where：标量子查询|列子查询|行子查询
having：标量子查询|列子查询|行子查询
exists：标量子查询|列子查询|行子查询|表子查询
```

**分页查询**

**一、应用场景**

当要查询的条目数太多，一页显示不全时

**二、语法**

```mysql
select 查询字段
from 表
limit 【offset，】size
```

注意：offset代表的是起始索引，默认从0开始；size代表的是显示的条目数

**联合查询**

**一、含义**

union：合并、联合，将多查询结果合并成一个结果

**二、语法**

```mysql
语句1
union 【all】
语句2
union 【all】
...
```

**三、意义**

1、将一条比较复杂的查询语句查分成多条语句

2、适用于查询多个表的时候，查询的列基本一致

**四、特点**

1、要求多条件查询的列数必须一致

2、多表查询的类型、顺序最好一致

3、union去重；union all包含重复项

**查询总结**

**一、语法**

```sql
select	查询字段	⑦
from	表 别名	①
连接类型 join 表2	②
on		连接条件	③
where	查询条件	④
group by 分组条件	⑤
having	筛选条件	⑥
order by 排序条件	⑧
limit	起始索引，条目数	⑨
```

### DML语言

**插入**

**一、方式一**

语法

```mysql
insert into 表名（字段名1，字段名2，...） values（值1，值2，...）
```

特点

```
1、要求值的类型和字段的类型要一致或兼容
2、字段的个数和顺序不一定与原始表中的字段个数顺序一致，但是必须保证值和字段一一对应
3、假如表中又可以为null的字段，可以不插入字段名，也可以设置值为null,但是不能写了字段，但是值留空
4、字段和值的个数必须保持一致
5、字段名可以圣洛，默认所有列
```

**二、方式二**

语法

```mysql
insert into 表名 set 字段=值，字段=值...
```

**三、两种方式的区别**

1、方式一支持一次插入多行，语法如下

```mysql
insert into 表名【（字段1，字段2，...）】
values（值1，值2，...）,(值1，值2，...)，...;
```

2、方式一支持子查询，语法如下

```mysql
insert into 表名
查询语句
```

**修改**

**一、修改单行表的记录**

```mysql
update 表名 set 字段=值，字段=值 【where 筛选条件】
```

**二、修改多表的记录**

```mysql
update 表1 别名
left|right|inner join 表2 别名
on 连接条件
set 字段=值，字段=值
where 筛选条件
```

**删除**

**一、方式一**

1、删除单行表的记录

```mysql
delete from 表名
where 筛选条件
limit 条目数
```

2、级联删除

```mysql
delete 别名1，别名2 from 表1 别名1
inner|left|right| join 表2 别名2
on 连接条件
where 筛选条件
```

**二、方式二**

```mysql
truncate table 表名
```

三、两种方式的区别

1、truncate删除后，如果在插入，表示列从1开始；delete删除后，如果再插入，表示列从断点开始

2、delete可以删除删选条件；truncate不可以

3、truncate效率较高

4、truncate没有返回值；delete可以返回受影响的行数

5、truncate不可以回滚；delete可以回滚

### DDL语言

**库的管理**

**一、创建库**

```mysql
create database 【if not exists】 库名 【character set 字符集】
```

**二、修改库**

```mysql
alter database 库名 charset 字符集名
```

**三、删除库**

```mysql
drop database 【if exists】 库名
```

**表的管理**

**一、创建表**

```mysql
create table 【if not exists】 表名（
	字段名 字段类型 【约束】，
	...
	字段名 字段类型 【约束】
）;
```

**二、修改表**

1、添加列

```mysql
alter table 表名 add column 列明 类型 【first|after 字段名】
```

2、修改列的类型或约束

```mysql
alter table 表名 modify column 列名 新类型 【新约束】
```

3、修改列名

```mysql
alter table 表名 change column 旧列名 新列名 类型
```

4、删除列

```mysql
alter table 表名 drop column 列名
```

5、修改表

```mysql
alter table 表名 rename 【to】 新表名
```

**三、删除表**

```mysql
drop table 【if exists】 表名
```

**四、复制表**

1、复制表结构

```mysql
create table 表名 like 旧表
```

2、复制表的结构+数据

```mysql
create table 表名
select 查询字段 from 旧表 【where 筛选条件】
```

**五 、查看表结构**

```sql
desc table_name;
```

**数据类型**

**一、整型**

1、数据类型

| 数据类型 | tinyint | smallint | mediumint | int/integer | bigint |
| :--: | :-----: | :------: | :-------: | :---------: | :----: |
| 字节长度 |    1    |     2    |     3     |      4      |    8   |

2、特点

> 1. 都可以设置无符号和有符号，默认有符号，通过unsigned设置符号
> 2. 如果超出了范围，会报out or range异常，插入临界值
> 3. 长度可以不指定，默认会有一个长度
> 4. 注：长度代表显示的最大宽度，如果不够则左边用0填充，但是需要搭配zerofill，并且默认变为无符号整性

**二、浮点型**

1、数据类型

| 数据类型 | decimal(M,D) | float(M,D) | double(M,D) |
| :--: | :----------: | :--------: | :---------: |
| 字节长度 |     _定点数_    |      4     |      8      |

2、特点

> 1. M代表整数部分+小数部分，D代表小数部分
> 2. 如果超出范围，会报out or range异常，并且插入临界值
> 3. M，D都可以省略，但对于定点数，M默认是10，D默认是0
> 4. 如果精度要求较高，则优先考虑使用定点数

**三、字符型**

|       数据类型      |               字节长度              |
| :-------------: | :-----------------------------: |
|     char(M)     |       M个字节，0<=M<=255，固定长度       |
|    varchar(M)   |  L+1个字节，其中L\<M&&0<65535，可以小于长度M |
|     tinytext    |         L+1个字节长度，其中L<2^8        |
|       text      |        L+2个字节长度，其中L<2^16        |
|    mediumtext   |        L+3个字节长度，其中L<2^24        |
|     longtext    |        L+4个字节长度，其中L<2^32        |
| enum(值1，值2，...) | 枚举，1或2个字节，取决于枚举的个数，最多(2^16)-1个值 |
|  set(值1，值2，...) |     集和，1，2，3，4，8个字节，取决于成员个数     |

**四、日期型**

|    数据类型   | 字节长度 |         日期格式        |                    日期范围                    |
| :-------: | :--: | :-----------------: | :----------------------------------------: |
|  datetime |   8  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00 \~ 9999-12-31 23:59:59 |
| timestamp |   4  | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:01 \~ 2038-01-19 03:14:07 |
|    time   |   3  |       HH:MM:SS      |            -838:59:59\~838:59:59           |
|    date   |   4  |      YYYY-MM-DD     |          1000-01-01 \~ 9999-12-31          |
|    year   |   1  |         YYYY        |                1901 \~ 2155                |

注：timestamp类型比较容易受时区，语法模式，版本的影响，但是更能反应当前时区的真实时间

**常见约束**

**一、常见的约束**

|      约束     |                 说明                |
| :---------: | :-------------------------------: |
|   NOT NULL  |            非空，该字段的值必须填写           |
|    UNIQUE   |            唯一，该字段的值不能重复           |
|   DEFAULT   |         默认，该字段的值不用手动插入有默认值        |
|    CHECK    |            检查，mySQL不支持            |
| PRIMARY KEY | 主键，该字段的值非空，不可重复，UNIQUE + NOT NULL |
| FOREIGN KEY |         外键，该字段的值引用了另一张表的字段        |

**二、主键和唯一对比**

|      相同点     |          区别          |
| :----------: | :------------------: |
|    都具有唯一性    | 一个表最多有一个主键，但是可以有多个唯一 |
| 都支持组合键，但是不推荐 |     主键不允许为空，唯一可以     |

**三、外键**

1. 用于限制两个表的关系，从表的字段引用了主表的某字段值
2. 外键列和主表的被引用列要求类型一致，意义一样，名称无要求
3. 主表的被引用列要求是一个key（一般是主键）
4. 插入数据，先插入主表；删除数据先删除从表

**四、自增长列**

1. 不用手动插入值，可以自动提供序列值，默认从1开始，步长为1；如果要修改起始值可以手动插入；修改步长可以使用 `set auto_increment=值`
2. 一个表最多有一个自增长列
3. 自增长列只能支持数值型
4. **自增长列必须为一个key**

### TCL语言

**事务**

**一、含义**

一条或多条SQL语句组成的执行单位，一组SQL要么都执行，要么都不执行

**二、特点**

原子性（A）：一个事务是不可拆分的整体，要么都执行，要么都不执行

一致性（C）：一个事务可以是数据从一个一致性状态到另一个一致性状态

隔离性（I）：一个事务不受其他事务事务干扰，多个事务相互隔离

持久性（D）：一个事务一旦提交了，则永久的持久化到本地

**三、事务的使用步骤**

1. 隐式事务：没有明显的开启和结束，本身就是一条事务可以自动提交，比如insert、update、delete
2. 显式事务：具有明显的开启和结束
3. 使用显式事务

> 1.  开始事务
>
>     set autocommit = 0；
>
>     start transac；可以省略
> 2.  编写一组逻辑
>
>     insert、update、delete语句 设置回滚：savepoint 回滚点名；
> 3.  提交事务
>
>     提交：commit
>
>     回滚：rollback
>
>     回滚到指定地方：rollback to 回滚点名；

**四、并发事务**

1.  事务的并发问题是如何发生的？

    多个事务同时操作同一个数据库的相同数据时
2.  并发问题有哪些？

    脏读：一个事务读取了其他事务还没有提交的数据，读到其他事务更新但是没有提交任务的数据

    不可重复读：一个事务多次读取，结果不一样

    幻读：一个事务读取了其他事务还没有提交的数据，只是读到的是其他实物“插入”的数据
3.  如何解决并发问题

    通过设置隔离级别来解决并发问题
4. 隔离级别

|                       |  脏读 | 不可重复读 |  幻读 |
| :-------------------: | :-: | :---: | :-: |
| read uncommitted：读未提交 |  ×  |   ×   |  ×  |
|  read committed：读已提交  |  √  |   ×   |  ×  |
|  repeatable read：可重复读 |  √  |   √   |  ×  |
|    serializable：串行化   |  √  |   √   |  √  |

**触发器**

**一、触发器的优点**

1. 自动执行：触发器在操作数据时立即被激活
2. 级联更新：触发器可以通过数据库中的相关表进行层叠修改
3. 强化约束：触发器可以引用其他表中的列，能够实现比CHECK约束更复杂的约束
4. 跟踪变化：触发器可以阻止数据库中未经许可的指定更新和变化
5. 强化业务逻辑：触发器可用于执行管理任务，并强制影响数据库中的复杂业务规则

**二、创建触发器**

```sql
CREATE TRIGGER trigger_name
trigger_time
trigger_event ON tbl_name
FOR EACH ROW
trigger_stmt
```

```
trigger_name：触发器名称
trigger_time：触发器执行的时间，取值为BEFORE或AFTER
trriger_event：由那个事件触发，取值为INSERT、DELETE、UPDATE
tbl_name：作用表的表名
trigger_stmt：触发器程序体
```

```
NEW和OLD
NEW：总是表示新的数据	NEW.columnName
OLD：总是表示就的数据	OLD.columnName
```

**三、查看触发器**

```sql
SHOW TRIGGERS
```

**四、删除触发器**

```sql
DROP TRIGER [IF EXISTS] [schema_name.]trigger_name
```

### 其他

**视图**

**一、含义**

mysql5.1版本出现的新特征，本身就是一个虚拟表，他的数据来源于表，通过执行时动态生成

好处：简化了sql语句，提高了sql的重用性，保护了基表的数据，提高了安全性

**二、创建**

```mysql
create vier 视图名
as
查询语句
```

**三、修改**

方式一

```mysql
create or replace view 视图名
as
查询语句
```

方式二

```mysql
alter view 视图名
as
查询语句
```

四、插入

```sql
insert into 视图 values(值1，值2，...)
```

注意：values中值的顺序和类型必须和视图中的迅顺序一致

**五、删除**

```mysql
drop view 视图1，视图2，...
```

**六、查看**

```mysql
desc 视图名；
show create view 视图名；
```

**七、使用**

插入：insert；修改：update；删除：delete；查看：select

注：视图一般用来查询，而不是用来更新，所以具备以下特点的视图不允许用来更新

1. 包含分组函数、group by、distinct、having、union、join
2. 常量视图
3. where后的子查询用到了from中的表
4. 用到了不可更新的视图
5. 更新视图实际是更新数据表

**七、视图和表的对比**

|     |  关键字  |    是否占用物理空间    |   使用   |
| :-: | :---: | :------------: | :----: |
|  视图 |  view | 占用比较小，只保存sql逻辑 | 一般用于查询 |
|  表  | table |    保存实际使得数据    |  增删改查  |

**数据库备份与还原**

**一、数据库备份**

```sql
mysqldump -uusername -ppassword dbname>path:file.sql
```

**二、数据库还原**

```sql
musqldump -usrname -ppassword dbname<path:file.sql
```

注：直接在命令行执行即可，不用登陆数据库9

**用户管理**

**一、用户创建**

```mysql
create user 'username'@'host' identified by 'password'
```

**二、授权**

```mysql
grant privileges on datebasesname.table to 'username'@'host'
# privileges：用户操作的权限，如：select、insert、update等，如果要授予全部权限则使用ALL
# dataBaseName：数据库名
#tableName：表名
```

**三、撤销授权**

```mysql
revoke privilege on databasesname.tablename from 'username'@'host'
```

**四、设置密码**

```mysql
set password for 'username'@'host' = password('newpassword')
# 如果是当前登录的用户：
set password = password('password')
```

**五、删除用户**

```mysql
drop user 'username'@'host'
```

## JDBC

### 创建数据库连接

#### 方式一

```java
public void testConnection1() throws SQLException {
	Driver driver = new com.mysql.jdbc.Driver();//创建数据库驱动对象
	String url = "jdbc:mysql://localhost:3306/test";//定义数据库链接地址
	Properties info = new Properties();
	info.setProperty("user", "root");
	info.setProperty("password", "abc123");
	Connection conn = driver.connect(url, info);//通过连接地质和配置获得数据库连接
	System.out.println(conn);
}
```

#### 方式二

```java
public void testConnection2() throws Exception {
    Class clazz = Class.forName("com.mysql.jdbc.Driver");//通过反射获得驱动class
	Driver driver = (Driver) clazz.newInstance();//通过反射创建驱动器对象
	String url = "jdbc:mysql://localhost:3306/test";
	Properties info = new Properties();
	info.setProperty("user", "root");
	info.setProperty("password", "abc123");
    Connection conn = driver.connect(url, info);//获取连接
	System.out.println(conn);
}
```

#### 方式三

```java
public void testConnection3() throws Exception {
    Class clazz = Class.forName("com.mysql.jdbc.Driver");
    Driver driver = (Driver) clazz.newInstance();//获取驱动器类
    String url = "jdbc:mysql://localhost:3306/test";
    String user = "root";
    String password = "abc123";
    DriverManager.registerDriver(driver);//通过驱动器管理类来注册驱动
    Connection conn = DriverManager.getConnection(url, user, password);//通过Driver.Manager来获取数据库连接
    System.out.println(conn);
}
```

#### 方式四

```java
public void testConnection4() throws Exception {
    String url = "jdbc:mysql://localhost:3306/test";
    String user = "root";
    String password = "abc123";
    Class.forName("com.mysql.jdbc.Driver");//加载驱动，在数据库连接的jar包中，已经存在注册驱动的代码了，所以可以不再注册驱动而直接使用
    Connection conn = DriverManager.getConnection(url, user, password);
    System.out.println(conn);
}
```

#### 方式五

```java
public void getConnection5() throws Exception{
	InputStream is = ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");//通过类加载器来获得配置文件的输入流
    Properties pros = new Properties();
    pros.load(is);//通过数据流来加载数据库连接时所需要的配置，方便使用
    String user = pros.getProperty("user");
    String password = pros.getProperty("password");
    String url = pros.getProperty("url");
    String driverClass = pros.getProperty("driverClass");
    Class.forName(driverClass);
    Connection conn = DriverManager.getConnection(url, user, password);
    System.out.println(conn);
}
```

### Statement与PreparedStatement

#### Statement

```java
Statement st = conn.createdStatement();
```

#### PreparedStatement

```java
PreparedStatement ps = conn.preparedStatement();
```

#### 比较

```
PreparedStatement解决了由字符串拼接引起的sql的注入问题
PreparedStatement解决了Blob类型的问题
PreparedStatement实现了更高效的批量操作问题
```

### JDBCUtils

#### getConnection

```java
public static Connection getConnection() throws Exception {
	InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
    Properties pros = new Properties();
    pros.load(is);
    String user = pros.getProperty("user");
    String password = pros.getProperty("password");
    String url = pros.getProperty("url");
    String driverClass = pros.getProperty("driverClass");
    Class.forName(driverClass);
    Connection conn = DriverManager.getConnection(url, user, password);
    return conn;
}
```

#### closeResource

```java
public static void closeResource(Connection conn,Statement ps){
    try {
        if(ps != null) ps.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    try {
        if(conn != null) conn.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}

public static void closeResource(Connection conn,Statement ps,ResultSet rs){
    try {
        if(ps != null) ps.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    try {
        if(conn != null) conn.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
    try {
        if(rs != null) rs.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```

### CURD操作

#### 查询

```java
public Customer queryForCustomers(Connection conn,String sql,Object...args){//传入sql语句和占位符对应的值，可变参数
    PreparedStatement ps = null;
    ResultSet rs = null;
    try {
        conn = JDBCUtils.getConnection();//获取连接
        ps = conn.preparedStatement(sql);//获取PrepareSatement对象
        for(int i = 0;i < args.length;i++){
            ps.setObject(i + 1, args[i]);//循环设置占位符的值，下标从1开始
        }
        rs = ps.executeQuery();//执行查询语句，返回ResultSet的结果集数据
        ResultSetMetaData rsmd = rs.getMetaData();//获得结果集元数据，包含了结果集的列数，列名称，列标签等信息
        int columnCount = rsmd.getColumnCount();//通过结果集元数据来获取列数
        if(rs.next()){//通过迭代器来判断是否存在数据
            Customer cust = new Customer();//创建结果对象
            for(int i = 0;i <columnCount;i++){//通过循环来遍历每一列,列号从1开始
                Object columValue = rs.getObject(i + 1);//获取每一列的值
				String columnName = rsmd.getColumnName(i + 1);//列号来获取列名
                String columnLabel = rsmd.getColumnLabel(i + 1);//通过列号来获取列标签
                Field field = Customer.class.getDeclaredField(columnLabel);//通过属性名来获取对象的Field对象
                field.setAccessible(true);//设置可访问性
                field.set(cust, columValue);//通过反射来设置对应的值
            }
            return cust;
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        JDBCUtils.closeResource(conn, ps, rs);//关闭资源，结果集也需要关闭
    }
    return null;
}
```

#### 插入、删除、修改

```java
public int update(Connection conn,String sql,Object ...args){//针对于增删改的通用操作
    PreparedStatement ps = null;
    try {
        conn = JDBCUtils.getConnection();
        ps = conn.preparedStatement(sql);
        for(int i = 0;i < args.length;i++){
            ps.setObject(i + 1, args[i]);
        }
        return ps.execute();
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        JDBCUtils.closeResource(conn, ps);
    }
    return -1;
}
```

### 事务

```java
public void testUpdateWithTx() {
    Connection conn = null;
    try {
        conn = JDBCUtils.getConnection();
        conn.setAutoCommit(false);//取消事务的自动提交
        String sql1 = "update user_table set balance = balance - 100 where user = ?";
        update(conn,sql1, "AA");
        System.out.println(10 / 0);//模拟网络错误
        String sql2 = "update user_table set balance = balance + 100 where user = ?";
        update(conn,sql2, "BB");
        System.out.println("转账成功");
        conn.commit();//提交
    } catch (Exception e) {
        e.printStackTrace();
        try {
            conn.rollback();//回滚数据
        } catch (SQLException e1) {
            e1.printStackTrace();
        }
    }finally{
        JDBCUtils.closeResource(conn, null);
    }
}
```

### Blob

#### 插入

```java
public void testInsert() throws Exception{
    Connection conn = JDBCUtils.getConnection();
    String sql = "insert into customers(name,email,birth,photo)values(?,?,?,?)";
    PreparedStatement ps = conn.preparedStatement(sql);
    ps.setObject(1,"袁浩");
    ps.setObject(2, "yuan@qq.com");
    ps.setObject(3,"1992-09-08");
    FileInputStream is = new FileInputStream(new File("girl.jpg"));//创建字节输入流，读取文件
    ps.setBlob(4, is);//将对应的字段设置成对字节输入流上传文件
    ps.execute();
    JDBCUtils.closeResource(conn, ps);
}
```

#### 查询

```java
public void testQuery(){
    Connection conn = null;
    PreparedStatement ps = null;
    InputStream is = null;
    FileOutputStream fos = null;
    ResultSet rs = null;
    try {
        conn = JDBCUtils.getConnection();
        String sql = "select photo from customers where id = ?";
        ps = conn.preparedStatement(sql);
        ps.setInt(1, 21);
        rs = ps.executeQuery();
        if(rs.next()){
            Blob photo = rs.getBlob("photo");//获取对应属性的blob对象
            is = photo.getBinaryStream();//通过blob获取输入流
            fos = new FileOutputStream("zhangyuhao.jpg");//将数据保存在本地
            byte[] buffer = new byte[1024];
            int len;
            while((len = is.read(buffer)) != -1){
                fos.write(buffer, 0, len);
            }
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        try {
            if(is != null)
                is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if(fos != null)
                fos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        JDBCUtils.closeResource(conn, ps, rs);
    }
}
```

### 批量添加

#### 方式一

```java
public void testInsert2() {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
        conn = JDBCUtils.getConnection();
        String sql = "insert into goods(name)values(?)";
        ps = conn.preparedStatement(sql);
        for(int i = 1;i <= 1000000;i++){
            ps.setObject(1, "name_" + i);
            ps.addBatch();//攒sql命令
            if(i % 500 == 0){//设置每500条提交一次
                ps.executeBatch();//批量执行sql命令
                ps.clearBatch();//清空已经执行过的命令
            }
        }
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        JDBCUtils.closeResource(conn, ps);
    }
}
```

#### 方式二（更快）

```java
public void testInsert3() {
    Connection conn = null;
    PreparedStatement ps = null;
    try {
        conn = JDBCUtils.getConnection();
        conn.setAutoCommit(false);//设置不自动提交事务
        String sql = "insert into goods(name)values(?)";
        ps = conn.preparedStatement(sql);
        for(int i = 1;i <= 1000000;i++){
            ps.setObject(1, "name_" + i);
            ps.addBatch();
            if(i % 500 == 0){
                ps.executeBatch();
                ps.clearBatch();
            }
        }
        conn.commit();//提交事务，这样可以执行更快
    } catch (Exception e) {
        e.printStackTrace();
    }finally{
        JDBCUtils.closeResource(conn, ps);
    }
}
```

### BaseDAO

#### V1.0

```java
public abstract class BaseDAO {
	public int update(Connection conn, String sql, Object... args) {//通用的增删改操作
		PreparedStatement ps = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps);
		}
		return 0;
	}
    
	public <T> T getInstance(Connection conn, Class<T> clazz, String sql, Object... args) {//查询一条记录
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			ResultSetMetaData rsmd = rs.getMetaData();
			int columnCount = rsmd.getColumnCount();
			if (rs.next()) {
				T t = clazz.newInstance();
				for (int i = 0; i < columnCount; i++) {
					Object columValue = rs.getObject(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}
    
	public <T> List<T> getForList(Connection conn, Class<T> clazz, String sql, Object... args) {//查询多条记录
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			ResultSetMetaData rsmd = rs.getMetaData();
			int columnCount = rsmd.getColumnCount();
			ArrayList<T> list = new ArrayList<T>();
			while (rs.next()) {
				T t = clazz.newInstance();
				for (int i = 0; i < columnCount; i++) {
					Object columValue = rs.getObject(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				list.add(t);
			}
			return list;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}
    
	public <E> E getValue(Connection conn,String sql,Object...args){//查询特殊值
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			if(rs.next()){
				return (E) rs.getObject(1);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}	
}
```

#### V2.0

```java
public abstract class BaseDAO<T> {
	private Class<T> clazz = null;
    
    {
		Type genericSuperclass = this.getClass().getGenericSuperclass();//获取当前BaseDAO的子类继承的父类中的泛型
		ParameterizedType paramType = (ParameterizedType) genericSuperclass;
		Type[] typeArguments = paramType.getActualTypeArguments();//获取了父类的泛型参数
		clazz = (Class<T>) typeArguments[0];//泛型的第一个参数
	}
    
	public int update(Connection conn, String sql, Object... args) {
		PreparedStatement ps = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			return ps.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps);
		}
		return 0;
	}
    
	public T getInstance(Connection conn, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			ResultSetMetaData rsmd = rs.getMetaData();
			int columnCount = rsmd.getColumnCount();
			if (rs.next()) {
				T t = clazz.newInstance();
				for (int i = 0; i < columnCount; i++) {
					Object columValue = rs.getObject(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				return t;
			}
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}
    
	public List<T> getForList(Connection conn, String sql, Object... args) {
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for (int i = 0; i < args.length; i++) {
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			ResultSetMetaData rsmd = rs.getMetaData();
			int columnCount = rsmd.getColumnCount();
			ArrayList<T> list = new ArrayList<T>();
			while (rs.next()) {
				T t = clazz.newInstance();
				for (int i = 0; i < columnCount; i++) {
					Object columValue = rs.getObject(i + 1);
					String columnLabel = rsmd.getColumnLabel(i + 1);
					Field field = clazz.getDeclaredField(columnLabel);
					field.setAccessible(true);
					field.set(t, columValue);
				}
				list.add(t);
			}
			return list;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}
    
	public <E> E getValue(Connection conn,String sql,Object...args){
		PreparedStatement ps = null;
		ResultSet rs = null;
		try {
			ps = conn.preparedStatement(sql);
			for(int i = 0;i < args.length;i++){
				ps.setObject(i + 1, args[i]);
			}
			rs = ps.executeQuery();
			if(rs.next()){
				return (E) rs.getObject(1);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}finally{
			JDBCUtils.closeResource(null, ps, rs);
		}
		return null;
	}	
}
```

### 数据库连接池

#### C3P0

```java
public void getConnection() throws SQLException{
    ComboPooledDataSource cpds = new ComboPooledDataSource("helloc3p0");//通过配置文件创建C3P0的数据库连接池对象
    Connection conn = cpds.getConnection();//获取连接
    System.out.println(conn);
}
```

```xml
<!-- 文件目录及名称：src/c3p0-config.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<named-config name="helloc3p0">
		<!-- 提供获取连接的4个基本信息 -->
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="jdbcUrl">jdbc:mysql:///test</property>
		<property name="user">root</property>
		<property name="password">abc123</property>
		<!-- 进行数据库连接池管理的基本信息 -->
		<!-- 当数据库连接池中的连接数不够时，c3p0一次性向数据库服务器申请的连接数 -->
		<property name="acquireIncrement">5</property>
		<!-- c3p0数据库连接池中初始化时的连接数 -->
		<property name="initialPoolSize">10</property>
		<!-- c3p0数据库连接池维护的最少连接数 -->
		<property name="minPoolSize">10</property>
		<!-- c3p0数据库连接池维护的最多的连接数 -->
		<property name="maxPoolSize">100</property>
		<!-- c3p0数据库连接池最多维护的Statement的个数 -->
		<property name="maxStatements">50</property>
		<!-- 每个连接中可以最多使用的Statement的个数 -->
		<property name="maxStatementsPerConnection">2</property>
	</named-config>
</c3p0-config>
```

#### DBCP

```java
public void testGetConnection1() throws Exception{
    Properties pros = new Properties();
    pros.load(ClassLoader.getSystemClassLoader().getResourceAsStream("dbcp.properties"));//通过类加载器加载src/dbcp.properties
    DataSource source = BasicDataSourceFactory.createDataSource(pros);//创建数据库连接池
    Connection conn = source.getConnection();//通过数据库连接池获取连接
    System.out.println(conn);
}
```

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///test
username=root
password=abc123
initialSize=10
```

#### Druid

```java
public void getConnection() throws Exception{
    Properties pros = new Properties();
    pros.load(ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties"));//通过类加载器加载src/druid.properties
    DataSource source = DruidDataSourceFactory.createDataSource(pros);//创建数据连接池
    Connection conn = source.getConnection();//通过数据库连接池获取连接
    System.out.println(conn);
}
```

```properties
url=jdbc:mysql:///test
username=root
password=abc123
driverClassName=com.mysql.jdbc.Driver
initialSize=10
maxActive=10
```
