# JDBC

## 概念

### 定义

- JDBC(Java Database Connectivity)是一个独立于特定数据库管理系统，通用的SQ数据库存储和操作的公共接口(一组API)，定义了用来访问数据库的标准Java类库。
- JDBC为访问不同的数据库提供了统一的途径，为开发者屏蔽了一些细节。
- JDBC的目标是使Java程序猿使用JDBC可以访问任何链接了JDBC驱动程序的数据库系统，这样可以使得程序员无需对数据库有更多的了解，大大简化和加快了开发过程。

### Driver 驱动

- jdbc:mysql://localhost:3306/test
- jdbc:mysql：指JDBC连接方式
- localhost：数据库主机的地址/ip
- 3306：SQL数据库的端口号；
- test：你要连接的数据库的库名

#### 连接驱动

- 配置配置文件+反射类=连接数据库



## PreparedStatement实现CRUD

- PreparedStatement是预编译的,对于批量处理可以大大提高效率.也叫JDBC存储过程
- statement每次执行sql语句，相关数据库都要执行sql语句的编译，preparedstatement是预编译得,preparedstatement支持批处理
- sql中的参数，可以用？表示，然后再进行相关类型的设置



## java和sql的数据映射

- boolean->bit
- Byte->tinyint
- Short->smallint
- Int->integer
- Long->bigint
- String->char/varchar/longvarchar
- byte array->binary/var binary
- Date->date
- Time->time
- Timestamp->timestamp



## 事务

- 事务：一组逻辑操作单元，使数据从一种状态变换到另一种状态
- 事务操作：保证所有事务都作为一个工作单元来执行，即使出了故障，都不能改变这种执行方式
- 事务提交commit：一个工作单元，如果顺序完成，记录就会被完整的保存在数据库中
- 事务回滚rollback：如果执行单元出错，即使已经的sql，也会会退到最初的状态



## 连接池

- 基本思想：为数据库建立一个缓冲池，预先在缓冲池中放入一定数量的连接，当需要建立数据库连接池时，只需在缓冲池中取一个，用完了再还回去
- 作用：数据库连接池负责分配，管理和释放数据库连接，它允许数据库使用一个现有的连接，而不是重新建立一个。
- DBCP：Apatch提供的数据库连接池，Tomcat服务器自带dbcp连接池。相对c3p0速度更快，但是自身存在bug，hibernate本身不在支持
- C3P0：稳定性很好，速度相对差点，是hibernate的官方推荐使用
- Druid：阿里开源的连接池，兼顾稳定和速度，推荐使用