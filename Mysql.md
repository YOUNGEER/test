# Mysql

## 介绍

- 关系型数据库 sql，比如mysql，oracle，sql server，sqlite等
- 非关系型数据库 no sql，比如redis，mongdb

### 基本的命令行操作

```mysql
//所有的sql命令，都需要;结尾，才表示一个语句结束
//连接数据库  mysql 用户名  密码
mysql -uroot -p123456;

//查看数据库
show databases;

//切换进该表
use school;

//查看数据库中的表
show tables;

//查看具体某个表,比如表student
describe student;

//创建一个数据库，如school
create database if not exist school;

//删除一个数据库
drop database if exists school;

```

### 数据类型

>数值型

- tinyint	1个字节，  最大255
- smallint     2个字节，最大65535
- mediumint    3个字节
- **int    4个字节**，最大十几亿
- bigint     8个字节

>浮点型

- float   4个字节
- double   8个字节
- Decimal    字符串数值型，涉及钱的可以用该类型

>字符串

- char，最多存储255个字符，定长，一般设置了长度后，无论数据是否达到该长度，在内存中都是这么长，比如手机号，身份证号的存储，优点查询快，缺点，可能会浪费一些内存
- varchar，最多65535个字符，不定长，优点，根据内容变动存储大小，缺点，查询相对慢些

- tinytext，最多存储255个字符，无默认值
- text，最多储存65535个字符，和varchar的区别是，varchar在超多255个字符后，会默认转成text来进行存储，超多65535，会自动转longtext，所以在使用的时候，varchar和text区别不大，建议使用varchar，毕竟会自动转

>时间

- date ：格式为YYYY:MM:DD，表示日期
- time：格式为HH:mm:ss，表示时间
- datetime：格式为YYYY:MM:DD HH:mm:ss，最常用的时间格式
- tiemstamp：时间戳，
- year：表示年

>null表示没有

- 没有值，未知

### 字段属性

- Unsigned

  - 无符号的整数，
  - 表示该列不能为负数

- zerofill

  - 零填充
  - 不足的位数，用0填充，如int(3)，存储为0003

- 自增

  - 一般的主键需要，在上一列的基础上+1
  - 必须是整数类型
  - 可以自定义主键的起始值和步长

- 非空. null  /not null

  - Null，如何不设置值，默认为null

  - not null，如果不设置值，会报错

- 默认值

  - 没有设置值，取默认值

## mysql数据管理

### 外键

- 用于建立和加强两个表数据之间的一致性
- 一个表的外键一般就是另一个表的主键，在删除一张表的主键的时候，一定要注意其他表是否把其作为外键
- 这样表的外键引用是物理引用，不推荐使用这样方式
- 可以通过代码的方式来实现，让数据库只存数据，不存逻辑关系

### DML

#### 插入

```mysql
//插入一条记录
insert into table(字段1，字段2...) values(值1，值2...)
//插入多条记录
insert into table(字段1，字段2...) values(值1，值2...),(值1，值2...),(值1，值2...)
```

#### 修改

```mysql
//更新 update ...  set ...  where
update table set value='value1',value2='value2' where name='name1' 
```

#### 删除

```mysql
//删除 delete from 表 条件
delete from table where...

//truncate 清空表,表的数据和索引不会变
truncate student
```

#### 查询

```mysql
select [all|distinct] *|table.field [as alian1].... 
from table [as table1]
{left|right|inner join table2 on }
{where ...}
{group by ...}
{having ...}
{order by ...}
{limit [offset] , raw_count}
```

#### where子句



## 常见函数

### 内置函数

#### 数学函数

```mysql
select abs(-8)  绝对值
select ceiling(4.5)  向上取整
select floor(4.5) 向下取整
select sign(x)  x的正负号，0->0；-10->-1；10-?1
```

#### 字符串

```mysql
select char_length('你好，mysql')  字符串长度，8
select concat('张三','利好'，...)   字符串拼接
select insert('123456',1,3,'890')  字符串替换，位置1开始，数3个，结果890456，注意，下标从1开始
select replace('12345353','3','00')  替换制定的字符串，用00替换3 结果：12004500500
select substr('124456443',3,6)    返回子字符串，3，6表示坐标，结果：4456
select instr('123424234','2')  返回子字符串第一次出现的索引，结果 2
select lower('kang')   转小写
select upper('kkk')   转大写
select reverse('12345')   反转。结果：54321
```

#### 时间日期

```mysql
select current_date()/curdate()    返回当前日期，格式为2020-02-22
select now()   获取当前时间, 格式为 2020-03-22 20:08:35，有时分秒
select localtime()   获取本地时间, 格式为 2020-03-22 20:08:35，有时分秒
select sysdate()   返回系统时间, 格式为 2020-03-22 20:08:35，有时分秒

select year(localtime())  当前时间的年
select month(localtime())  当前时间的月
select day(localtime())  当前时间的日
select hour(localtime())  当前时间的时
select minter(localtime())  当前时间的分
select second(localtime())  当前时间的秒

```

#### 系统

```mysql
select system_user()/user()  当前用户
select version()  数据库的版本
```

### 聚合函数

#### count 计数

```mysql
select count('name') from table_1   返回字段name的个数，为null的行不参与统计
select count(*/1) from table_1  返回table_1的行数，*或者1都可以，为null的也参与统计
```

#### sum 求和

```mysql
select sum('age') as totalAge from table_1  求所有age的总和
```

#### Max/Min 最大/最小值

```mysql
select max/min('age') as age from table_1  求age的最大/最小值
```

#### avg 平均值

```mysql
select avg('age') as age from table_1  求age的平均值
```

#### 分组和过滤 group by /   having

```mysql
select lessonName , count(1),sum('score'),max('score'),min('score'),avg('score') as '平均分'
from score group by lessonName having '平均分'>80
此时的聚合函数，都是在组的集合中进行远算，比较，一般会返回多行,同时，结合having对分组后的结果集进行条件筛选
```

### 加密

```mysql
select * from table where name=md5('12345')  直接用md5函数
```



## 事务

## 索引

- 索引（index）是帮助mysql高效获取数据的**数据结构**

### 分类

>

- 主键索引 primary key
  
  - 唯一的标示，主键不可重复，只能有一个列作为主键
- 唯一索引 union key
  
  - 避免重复的列出现，可以有多个唯一索引，也就是可以多个列设置为union key
- 常规索引 key/index
  
  - 默认的是常规索引，设置为key/index
- 全文索引 fulltext
  
- 以前有些不支持，现在sql基本都是支持的
  
- 索引sql

  ```mysql
  //显示索引
  show index from table
  
  //添加索引
  alter table school.student add fulltext index indexname (fieldName)
  
  //添加索引
  create index indexname on table(fieldName)
  ```

### 索引的原则

- 索引不是越多越好
- 不要对经常变动的数据加索引
- 小数据量的表不需要加索引
- 索引一般用在常查询的字段上



## 用户

```mysql
//创建用户  create user 姓名 identified by 密码
create user username identified by password

//删除用户
drop user username

//修改当前用户密码  set password=password(密码)
set password=password(password)

//修改制定用户密码  set password for 用户名=password(密码)
set password for username=password(password)

//重命名 rename user 姓名1 to 姓名2
rename user name1 to name2

//用户权限授予 grant 权限(all privileges全部权限，除了不能给别人授权) on database.table(*.*表示所有的表) to 用户
grant all privileges on *.* to username

//撤销权限
revoke all privileges on *.* to username

//查询用户权限 show grants for 用户
show grants for user

```

## 数据导出/导入

```mysql
//导出 mysqldump -h 主机 -u 用户名 -p 密码 数据库 [表名1 表名2] > 物理磁盘/文件名
mysqldump -hlocalhost -uroot -p123456 school student > ~user/a.sql

//导入,登陆的情况下 source 文件地址
//未登陆情况 mysql -uroot -p123456 库名 < 文件地址

```

## 高级部分

### 四层架构

- 连接层
- 服务层
- 引擎层
- 存储层

### innodb和myisam

- innodb支持主外键，事物，行锁，缓存索引和数据，多物理内存要求高，关注事务
- myisam不支持主外键，不支持事物，支持表锁，只支持索引，内存空间小，关注性能

### 七种join

### 索引

- mysql中除了数据，在维护着一种满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就可以在这些数据结构的基础上实现高效查找算法。这种数据结构称为索引
- 一般索引本身也很大，一般是存储在索引文件中，不会在内存里面

##### 优点

- 提高数据的检索效率，降低数据库IO成本
- 降低数据的排序成本，降低CPU的消耗

##### 缺点：

- 索引是一种数据结构，会额外增加磁盘空间
- 索引本身在数据的更新，插入等操作时，会有一定的性能消耗
- 高质量的索引可以提高查询速度，低质量的索引会影响update，insert

#### 加索引原则

- 主键自动建立唯一索引
- 频繁作为查询条件的字段应该添加索引
- 查询中与其他表关联的字段，外键关系添加索引
- 频繁更新的字段不建议添加索引
- where条件中用不到的字段不添加索引
- 查询中排序的字段可以添加索引
- 查询中分组的字段可以添加索引
- 表数据太少没必要加索引
- 经常增删改的表建议不要加索引
- 字段值重复性太高，比如性别，不要给该字段加索引

#### Btree/B+tree

- 原理：



### EXPLAIN调优

>Id ,select_type , table, type,possible_key,key_key_len,ref

- id：查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序
  - id相同：表示查询顺序从上到下
  - id不同：如果是子查询，id会自增，id越大对应的表，会先执行
  - id相同，不同同时存在：id越大越先执行，相同的按照顺序执行
  
- select_type:
  - simple：最简单的select查询，不包含子查询或者union查询
  - primary：子查询中若包含各种子查询，最外层的查询会被标记为primary
  - subquery：在select或者where列表中包含了子查询
  - derived：在from列表中包含的子查询会被标记为derived，mysql会递归这些子查询，把结果放在临时表中
  - union：假若第二个select在union之后，则被标记为union
  - union result：union之后的结果
  
- table：操作的是哪张表

- type：
  - system：表只有一行记录，这是const的特例，一般很少出现，可以忽略不计

  - const：表示通过索引一次就能找到，一般是主键索引或者是唯一索引，一般一条记录，mysql可以将该查询快速转换为const常量,一般是单张表或者是连表查询

  - eq_ref：与const类似，也是匹配唯一索引或者是primary key，但是常见于多表一起查询时，比如

    ```
    SELECT * FROM sp_idai_make_idcard a,kaka_bak.sp_wechat_user b WHERE a.user_id=b.id
    ```

  - ref：非唯一性的索引值所匹配的多行结果，本质上也是索引匹配

  - range：只检索给定范围的行，key用的是索引，也就是where中用到了key值 in,<,>,between

  - index：对索引进行全遍历，比如

    ```
    select id from t1. //如果此时的id是个索引键
    ```

  - all：全表所描，应该禁止

    ```
    select * from ti
    ```

- possible_key：查询中涉及到的索引都会列出来，但是实际并不一定用到了

- key：实际查询中用到的索引key

- Key_len：表示索引中使用的字节数，在不损失精度的情况下，越短越好，大小是计算出来的长度，并不是实际使用的长度

- ref：显示索引的哪一列被使用了，如果可能，是一个常数。这些列或者常量被用于查找索引key上的值。

- rows：根据表统计信息，大致计算出找到所需要读取的行数

- extra：包含不在其他列显示但是十分重要的信息

  - Using filesort：mysql会使用一个外部的索引排序，而不是按照表内的索引顺序排序，建议优化
  - Using temporary：外部维持一个临时排序表，强烈建议优化
  - Using index：没有使用数据行，直接用的索引操作，效率不错，又称覆盖索引
  - using where：使用了where
  - using join buffer：使用了缓存

