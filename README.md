<h1><img src="https://github.com/brettwooldridge/HikariCP/wiki/Hikari.png"> HikariCP<sup><sup>&nbsp;It's Faster.</sup></sup><sub><sub><sup>Hi·ka·ri [hi·ka·'lē] &#40;<i>Origin: Japanese</i>): light; ray.</sup></sub></sub></h1><br>

[![][Build Status img]][Build Status]
[![][Coverage Status img]][Coverage Status]
[![][license img]][license]
[![][Maven Central img]][Maven Central]
[![][Javadocs img]][Javadocs]

快速，简单，可靠。HikariCP是一个“零开销”的生产就绪的JDBC连接池。 大约130Kb，库非常小。了解[我们怎么做到的](https://github.com/brettwooldridge/HikariCP/wiki/Down-the-Rabbit-Hole).

&nbsp;&nbsp;&nbsp;<sup>**"简单是可靠性的先决条件."**<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- *Edsger Dijkstra*</sup>

----------------------------------------------------

_Java 8 thru 11 maven artifact:_
```xml
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>3.3.1</version>
    </dependency>
```
_Java 7 maven artifact (*maintenance mode*):_
```xml
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP-java7</artifactId>
        <version>2.4.13</version>
    </dependency>
```
_Java 6 maven artifact (*maintenance mode*):_
```xml
    <dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP-java6</artifactId>
        <version>2.3.13</version>
    </dependency>
```
或者[这里下载](http://search.maven.org/#search%7Cga%7C1%7Ccom.zaxxer.hikaricp).

----------------------------------------------------

##### JMH 基准测试 :checkered_flag:

JMH是一种用于构建，运行和分析Java和其他语言编写的针对JVM的nano/micro/milli/macro基准测试框架。
[JMH微基准框架](http://openjdk.java.net/projects/code-tools/jmh/)被用来在非常小的尺度上测量池的开销。
你可以拉取[HikariCP基准测试项目](https://github.com/brettwooldridge/HikariCP-benchmark)，审查并运行这些基准测试来了解更多细节.

![](https://github.com/brettwooldridge/HikariCP/wiki/HikariCP-bench-2.6.0.png)

 * 一个*连接周期Connection Cycle*定义为单个 ``DataSource.getConnection()``/``Connection.close()``.
 * 一个*语句周期Statement Cycle*定义为单个 ``Connection.prepareStatement()``, ``Statement.execute()``, ``Statement.close()``.

<sup>
<sup>1</sup> 使用的版本: HikariCP 2.6.0, commons-dbcp2 2.1.1, Tomcat 8.0.24, Vibur 16.1, c3p0 0.9.5.2, Java 8u111 <br/>
<sup>2</sup> 英特尔酷睿i7-3770 CPU @ 3.40GHz <br/>
<sup>3</sup> 无竞争基准: 32 threads/32 connections, 竞争基准: 32 threads, 16 connections <br/>
<sup>4</sup> Apache Tomcat使用<i>StatementFinalizer</i>时，由于垃圾回收时间过长无法完成Statement基准测试 <a href="https://raw.githubusercontent.com/wiki/brettwooldridge/HikariCP/markdown/Tomcat-Statement-Failure.md"></a><br/>
<sup>5</sup> Apache DBCP由于垃圾收集时间过长无法完成Statement基准测试<a href="https://raw.githubusercontent.com/wiki/brettwooldridge/HikariCP/markdown/Dbcp2-Statement-Failure.md"></a>
</sup>

----------------------------------------------------
#### 分析 :显微镜:

#### 峰值需求下池的比较
<a href="https://github.com/brettwooldridge/HikariCP/blob/dev/documents/Welcome-To-The-Jungle.md"><img width="400" align="right" src="https://github.com/brettwooldridge/HikariCP/wiki/Spike-Hikari.png"></a>
与其他池相比，HikariCP v2.6与独特的“峰值需求”负载相关的分析.

客户的环境带来了获取新的连接的高成本，并且需要动态大小的池，但是还同时需要响应峰值的请求。 了解我们对峰值需求处理的[方式](https://github.com/brettwooldridge/HikariCP/blob/dev/documents/Welcome-To-The-Jungle.md).
<br/>
<br/>
#### 你[可能]做错了.
<a href=""><img width="200" align="right" src="https://github.com/brettwooldridge/HikariCP/wiki/Postgres_Chart.png"></a>
或者说，*“您可能对连接池调整大小不了解”*。 观看来自Oracle Real-world Performance组的视频，了解为什么连接池的大小不需要像往常认为的那样大。 事实上，超大的连接池对性能有明显的，可证明的*负面*影响; 在Oracle演示中，差异为50倍。 [这里可以找到答案](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing).
<br/>
#### WIX工程分析
<a href="https://www.wix.engineering/blog/how-does-hikaricp-compare-to-other-connection-pools"><img width="180" align="left" src="https://github.com/brettwooldridge/HikariCP/wiki/Wix-Engineering.png"></a>
我们要感谢来自WIX的人们主动编写的深刻的对比文章在他们的[工程博客](https://www.wix.engineering/blog/how-does-hikaricp-compare-to-other-connection-pools)上. 有时间可以看一看.
<br/>
<br/>
<br/>
#### Failure: Pools behaving badly
阅读我们有趣的[“数据库挂掉”池挑战](https://github.com/brettwooldridge/HikariCP/wiki/Bad-Behavior:-Handling-Database-Down).

----------------------------------------------------
#### "模仿是最真诚的剽窃方式" - <sub><sup>无名</sup></sub>
Open source software like HikariCP, like any product, competes in the free market.  We get it.  We understand that product advancements, once public, are often co-opted.  And we understand that ideas can arise from the zeitgeist; simultaneously and independently.  But the timeline of innovation, particularly in open source projects, is also clear and we want our users to understand the direction of flow of innovation in our space.  It could be demoralizing to see the result of hundreds of hours of thought and research co-opted so easily, and perhaps that is inherent in a free marketplace, but we are not demoralized.  *We are motivated; to widen the gap.*

----------------------------------------------------
##### 用户推荐

[![](https://github.com/brettwooldridge/HikariCP/wiki/tweet3.png)](https://twitter.com/jkuipers)<br/>
[![](https://github.com/brettwooldridge/HikariCP/wiki/tweet1.png)](https://twitter.com/steve_objectify)<br/>
[![](https://github.com/brettwooldridge/HikariCP/wiki/tweet2.png)](https://twitter.com/brettemeyer)<br/>
[![](https://github.com/brettwooldridge/HikariCP/wiki/tweet4.png)](https://twitter.com/dgomesbr/status/527521925401419776)

------------------------------
#### 配置参数 (是旋钮啊, baby!)

HikariCP明智的参数默认值，在大多数部署中都无需额外调整就表现良好。 
**除下面标明的"必要"外，每个属性都是可选的。**

<sup>&#128206;</sup>&nbsp;*HikariCP的所有时间单位都使用毫秒*

&#128680;&nbsp;HikariCP依靠精确的定时器来提高性能和可靠性。 *必须*您的服务器与时间源（如NTP服务器）同步。 *尤其是*如果您的服务器在虚拟机中运行。为什么？ [在此处阅读更多内容](https://dba.stackexchange.com/a/171020)。 **不要依赖虚拟机管理程序设置来"同步"虚拟机的时钟。在虚拟机内配置时间源同步。**如果您要在缺少时间同步的问题上寻求支持，您将在Twitter上被公开嘲笑。

##### 必要

&#128288;``dataSourceClassName``<br/>
这是JDBC驱动程序提供的``DataSource``类的名称。查阅用于获取此类名的特定JDBC驱动程序的文档，或参见下面的[table](https://github.com/brettwooldridge/HikariCP#popular-datasource-class-names)。
注意不支持XA数据源。 XA需要一个真正的事务管理器
[bitronix](https://github.com/bitronix/btm)。请注意，如果您使用此属性，则不需要此属性
``jdbcUrl``，用于“old-school”基于DriverManager的JDBC驱动程序配置。
*Default: none*

*- or -*

&#128288;``jdbcUrl``<br/>
此属性指示HikariCP使用“基于DriverManager的”配置。我们觉得基于DataSource
配置（上面）由于各种原因（见下文）是优越的，但对于许多部署而言
差别不大。 **将此属性与“旧”驱动程序一起使用时，您可能还需要进行设置
``driverClassName``属性，但首先尝试不使用。**请注意，如果使用此属性，您可以
仍然使用* DataSource *属性来配置驱动程序，实际上建议使用驱动程序参数
在URL本身中指定。
*Default: none*

***

&#128288;``username``<br/>
此属性设置从底层的driver获取* Connections *时使用的默认身份验证用户名。
请注意，对于DataSource们来说，这可以通过非常确定的方式工作，在其DataSource上
调用``DataSource.getConnection(*username*,password)``。
然而，对于Driver-based的配置来说，每个driver都不同。在Driver-based的情况下，
HikariCP将使用``Properties``中的``user``属性来设置``username``传递给
driver的``DriverManager.getConnection（jdbcUrl，props）``方法调用。如果您的程序不调用这个方法，
可以直接这样调用``addDataSourceProperty（"username",...）``。
*Default: none*

&#128288;``password``<br/>
此属性设置从底层的驱动程序获取* Connections *时使用的默认验证密码。
请注意，对于DataSource们来说，这可以通过非常确定的方式工作，在其DataSource上
调用``DataSource.getConnection(username,*password*)``。
然而，对于Driver-based的配置来说，每个driver都不同。在Driver-based的情况下，
HikariCP将使用``Properties``中的``password``属性来设置``password``传递给
driver的``DriverManager.getConnection（jdbcUrl,props）``方法调用。如果您的程序不调用这个方法，
可以直接这样调用``addDataSourceProperty（"pass",...）``
*Default: none*

##### 高频

&#9989;``autoCommit``<br/>
此属性控制从池返回的连接的默认自动提交行为。
它是一个布尔值。
*Default: true*

&#8986;``connectionTimeout``<br/>
此属性控制客户端（即您的程序）从池中得到连接需要等待的最大毫秒数。
如果超过此时间而依然没有可用的连接，将抛出SQLException。最低可接受的连接超时值是250毫秒.
*Default: 30000 (30 seconds)*

&#8986;``idleTimeout``<br/>
此属性控制一个连接在池中允许被闲置的最长时间。 
**此设置仅适用于``minimumIdle``被定义为小于``maximumPoolSize``的情况。**
一旦池达到"minimumIdle"(最小连接数)，空闲连接将不会被剔除。闲置连接是否退出的最大变量为+30秒，平均变量为+15秒。
在此闲置超时之前，连接永远不会作为空闲连接而被剔除。0表示永远不会从池中删除空闲连接。允许的最小值为10000毫秒
(10秒).
*Default: 600000 (10 minutes)*

&#8986;``maxLifetime``<br/>
此属性控制池中连接的最长生命周期。使用中的连接永远不会剔除，只有当它关闭时才会被删除。
在连接挨着连接的情况下，应用微小的负衰减可以避免池中连接的大量灭绝。 
**我们强烈建议设置此值，它应该设置成比任何数据库或基础设施自带的连接时间限制短几秒。**
值为0表示没有最大寿命（无限寿命），此时连接的生命取决于``idleTimeout``的设置。
*Default: 1800000 (30 minutes)*

&#128288;``connectionTestQuery``<br/>
**如果你的driver支持JDBC4,我们强烈建议不要设置这个属性.** This is for 
"legacy" drivers that do not support the JDBC4 ``Connection.isValid() API``.  This is the query that
will be executed just before a connection is given to you from the pool to validate that the 
connection to the database is still alive. *Again, try running the pool without this property,
HikariCP will log an error if your driver is not JDBC4 compliant to let you know.*
*Default: none*

&#128290;``minimumIdle``<br/>
此属性控制HikariCP维护的池中最小*空闲连接数*。如果空闲连接低于此值并且池中的总连接小于"maximumPoolSize"(最大池大小)，
HikariCP将尽最大努力快速有效地添加连接。
但是，为了获得最高性能和对峰值需求的响应，我们建议*不*设置此值，而是允许HikariCP充当*固定大小*连接池。
*Default: same as maximumPoolSize*

&#128290;``maximumPoolSize``<br/>
此属性控制允许池到达的最大大小，包括闲置的和使用中的连接。 
基本上这个值将决定实际连接到数据库后端的最大连接数量。 最合适的值由你的执行环境决定。
当池达到此大小时，并且没有空闲连接可用，对getConnection()的调用将阻塞最多``connectionTimeout``毫秒
请阅读[about pool sizing](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing).
*Default: 10*

&#128200;``metricRegistry``<br/>
此属性仅可通过编程(代码)配置或IoC容器获得。 这个属性允许你
指定一个*Codahale/Dropwizard*的``MetricRegistry``实例
用于记录池的各种指标。 详见[Metrics](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-Metrics)
*Default: none*

&#128200;``healthCheckRegistry``<br/>
此属性仅可通过编程(代码)配置或IoC容器获得。 这个属性允许你
指定一个*Codahale/Dropwizard*的``HealthCheckRegistry``实例
用于报告池的当前监控信息.详见[Health Checks](https://github.com/brettwooldridge/HikariCP/wiki/Dropwizard-HealthChecks)
*Default: none*

&#128288;``poolName``<br/>
此属性表示连接池的用户定义名称，主要显示在日志记录和JMX管理控制台中，以标识池和池配置。
*Default: auto-generated*

##### 低频

&#8986;``initializationFailTimeout``<br/>
This property controls whether the pool will "fail fast" if the pool cannot be seeded with
an initial connection successfully.  Any positive number is taken to be the number of 
milliseconds to attempt to acquire an initial connection; the application thread will be 
blocked during this period.  If a connection cannot be acquired before this timeout occurs,
an exception will be thrown.  This timeout is applied *after* the ``connectionTimeout``
period.  If the value is zero (0), HikariCP will attempt to obtain and validate a connection.
If a connection is obtained, but fails validation, an exception will be thrown and the pool
not started.  However, if a connection cannot be obtained, the pool will start, but later 
efforts to obtain a connection may fail.  A value less than zero will bypass any initial
connection attempt, and the pool will start immediately while trying to obtain connections
in the background.  Consequently, later efforts to obtain a connection may fail.
*Default: 1*

&#10062;``isolateInternalQueries``<br/>
This property determines whether HikariCP isolates internal pool queries, such as the
connection alive test, in their own transaction.  Since these are typically read-only
queries, it is rarely necessary to encapsulate them in their own transaction.  This
property only applies if ``autoCommit`` is disabled.
*Default: false*

&#10062;``allowPoolSuspension``<br/>
This property controls whether the pool can be suspended and resumed through JMX.  This is
useful for certain failover automation scenarios.  When the pool is suspended, calls to
``getConnection()`` will *not* timeout and will be held until the pool is resumed.
*Default: false*

&#10062;``readOnly``<br/>
This property controls whether *Connections* obtained from the pool are in read-only mode by
default.  Note some databases do not support the concept of read-only mode, while others provide
query optimizations when the *Connection* is set to read-only.  Whether you need this property
or not will depend largely on your application and database. 
*Default: false*

&#10062;``registerMbeans``<br/>
This property controls whether or not JMX Management Beans ("MBeans") are registered or not.
*Default: false*

&#128288;``catalog``<br/>
This property sets the default *catalog* for databases that support the concept of catalogs.
If this property is not specified, the default catalog defined by the JDBC driver is used.
*Default: driver default*

&#128288;``connectionInitSql``<br/>
此属性设置的是将在每次创建新连接之后添加到池中之前，执行的SQL语句。
如果此SQL无效或抛出异常，它将是作为连接失败处理，将遵循标准重试逻辑。
*Default: none*

&#128288;``driverClassName``<br/>
HikariCP将尝试通过DriverManager仅基于``jdbcUrl``解析driver，
但对于一些较旧的驱动程序，还必须指定``driverClassName``。
除非您会收到一条明显的错误消息，指出未找到驱动程序，否则忽略此属性。
*Default: none*

&#128288;``transactionIsolation``<br/>
This property controls the default transaction isolation level of connections returned from
the pool.  If this property is not specified, the default transaction isolation level defined
by the JDBC driver is used.  Only use this property if you have specific isolation requirements that are
common for all queries.  The value of this property is the constant name from the ``Connection``
class such as ``TRANSACTION_READ_COMMITTED``, ``TRANSACTION_REPEATABLE_READ``, etc.
*Default: driver default*

&#8986;``validationTimeout``<br/>
This property controls the maximum amount of time that a connection will be tested for aliveness.
This value must be less than the ``connectionTimeout``.  Lowest acceptable validation timeout is 250 ms.
*Default: 5000*

&#8986;``leakDetectionThreshold``<br/>
This property controls the amount of time that a connection can be out of the pool before a
message is logged indicating a possible connection leak.  A value of 0 means leak detection
is disabled.  Lowest acceptable value for enabling leak detection is 2000 (2 seconds).
*Default: 0*

&#10145;``dataSource``<br/>
此属性仅可通过编程配置或IoC容器获得。这个属性
允许您直接设置要由池包装的``DataSource``实例，而不是
让HikariCP通过反射构建它。这在一些依赖注入构架中很有用
。指定此属性时，``dataSourceClassName``属性和所有
特定于DataSource的属性将被忽略。
*Default: none*

&#128288;``schema``<br/>
This property sets the default *schema* for databases that support the concept of schemas.
If this property is not specified, the default schema defined by the JDBC driver is used.
*Default: driver default*

&#10145;``threadFactory``<br/>
This property is only available via programmatic configuration or IoC container.  This property
allows you to set the instance of the ``java.util.concurrent.ThreadFactory`` that will be used
for creating all threads used by the pool. It is needed in some restricted execution environments
where threads can only be created through a ``ThreadFactory`` provided by the application container.
*Default: none*

&#10145;``scheduledExecutor``<br/>
This property is only available via programmatic configuration or IoC container.  This property
allows you to set the instance of the ``java.util.concurrent.ScheduledExecutorService`` that will
be used for various internally scheduled tasks.  If supplying HikariCP with a ``ScheduledThreadPoolExecutor``
instance, it is recommended that ``setRemoveOnCancelPolicy(true)`` is used.
*Default: none*

----------------------------------------------------

#### 缺失的旋钮

正如你在上面看到的那样，HikariCP有很多“旋钮”(配置项)，但比其他一些池要少。
这是一种设计理念。 HikariCP设计美学是极简主义。 与
*简单更好*或*更少更多*设计理念保持一致，有些配置维度被故意遗漏。

#### Statement缓存

许多连接池，包括Apache DBCP,Vibur,c3p0和其他连接池都提供了``PreparedStatement``缓存。
HikariCP没有。为什么?

在连接池层``PreparedStatements``只能缓存*每个连接*。如果你的申请有250个常用查询和20个连接池，
您要求数据库保留5000个查询执行计划 - 同样地，池必须缓存这么多``PreparedStatements``和它们
对象的相关图。

大多数主要的数据库JDBC驱动程序已经有一个可以配置的Statement缓存，包括PostgreSQL,
Oracle,Derby,MySQL,DB2等等。 JDBC驱动程序处于利用特定于数据库的独特功能位置，几乎所有的缓存实现
都能够*跨连接*共享执行计划。这意味着在内存和相关的执行计划中，不是5000个语句，而是250个常用的执行
查询在数据库中生成250个执行计划。
聪明的实现甚至不保留``PreparedStatement``在驱动程序级别的内存中对象，而只是将新实例附加到现有的计划ID。

在池化层使用语句缓存是[反模式](https://en.wikipedia.org/wiki/Anti-pattern)，
与Driver提供的缓存相比，会对应用程序性能产生负面影响。

#### 记录语句文本 / 慢查询日志

和语句缓存一样，大多数数据库都支持Statement缓存，像Oracle, MySQL, Derby, MSSQL等，一些甚至支持慢查询日志，所以我们不再做处理。如果一些少量的库不支持，也有很多可选方案来达成目的。
比如，我们收到的[p6spy工作良好的一份报告](https://github.com/brettwooldridge/HikariCP/issues/57#issuecomment-354647631),
也注意到[log4jdbc](https://github.com/arthurblake/log4jdbc)和[jdbcdslog-exp](https://code.google.com/p/jdbcdslog-exp/).

#### 快速恢复
请阅读[快速恢复向导](https://github.com/brettwooldridge/HikariCP/wiki/Rapid-Recovery)来确定要如何配置你的Driver和系统以从数据库重启和网络分区事件中正确恢复。

----------------------------------------------------

### 连接池初始化

举个栗子，你可以像这样用``HikariConfig`` <sup>1</sup>:
```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/simpsons");
config.setUsername("bart");
config.setPassword("51mp50n");
config.addDataSourceProperty("cachePrepStmts", "true");
config.addDataSourceProperty("prepStmtCacheSize", "250");
config.addDataSourceProperty("prepStmtCacheSqlLimit", "2048");

HikariDataSource ds = new HikariDataSource(config);
```
&nbsp;<sup><sup>1</sup> 这是例子, 不要完全复制，按您自己的配置来.</sup>

或者直接像这样实例化一个 ``HikariDataSource``:
```java
HikariDataSource ds = new HikariDataSource();
ds.setJdbcUrl("jdbc:mysql://localhost:3306/simpsons");
ds.setUsername("bart");
ds.setPassword("51mp50n");
...
```
或者从属性文件读配置:
```java
// Examines both filesystem and classpath for .properties file
HikariConfig config = new HikariConfig("/some/path/hikari.properties");
HikariDataSource ds = new HikariDataSource(config);
```
属性文件示例:
```ini
dataSourceClassName=org.postgresql.ds.PGSimpleDataSource
dataSource.user=test
dataSource.password=test
dataSource.databaseName=mydb
dataSource.portNumber=5432
dataSource.serverName=localhost
```
或者设置 ``java.util.Properties`` :
```java
Properties props = new Properties();
props.setProperty("dataSourceClassName", "org.postgresql.ds.PGSimpleDataSource");
props.setProperty("dataSource.user", "test");
props.setProperty("dataSource.password", "test");
props.setProperty("dataSource.databaseName", "mydb");
props.put("dataSource.logWriter", new PrintWriter(System.out));

HikariConfig config = new HikariConfig(props);
HikariDataSource ds = new HikariDataSource(config);
```

这里也有一个系统属性参数, ``hikaricp.configurationFile``, 它可以用来指定属性文件的位置. 
如果您打算使用此选项，使用默认构造函数构造一个``HikariConfig``或``HikariDataSource``
的实例时，属性文件将被自动加载。

### 性能提示
[MySQL性能提示](https://github.com/brettwooldridge/HikariCP/wiki/MySQL-Configuration)

### 流行的DataSource类名

我们建议使用``dataSourceClassName``代替``jdbcUrl``, 但两者都可以接受. 我们再说一遍, *两者都可以接受*.

&#9888;&nbsp;*Note: Spring Boot自动配置用户，需要使用基于``jdbcUrl``的配置.*

&#9888;&nbsp;已知MySQL的DataSource在网络超时方面的支持被打破。请改用``jdbcUrl``配置。

流行的JDBC *DataSource* 类列表:

| Database         | Driver       | *DataSource* class |
|:---------------- |:------------ |:-------------------|
| Apache Derby     | Derby        | org.apache.derby.jdbc.ClientDataSource |
| Firebird         | Jaybird      | org.firebirdsql.ds.FBSimpleDataSource |
| H2               | H2           | org.h2.jdbcx.JdbcDataSource |
| HSQLDB           | HSQLDB       | org.hsqldb.jdbc.JDBCDataSource |
| IBM DB2          | IBM JCC      | com.ibm.db2.jcc.DB2SimpleDataSource |
| IBM Informix     | IBM Informix | com.informix.jdbcx.IfxDataSource |
| MS SQL Server    | Microsoft    | com.microsoft.sqlserver.jdbc.SQLServerDataSource |
| ~~MySQL~~        | Connector/J  | ~~com.mysql.jdbc.jdbc2.optional.MysqlDataSource~~ |
| MariaDB          | MariaDB      | org.mariadb.jdbc.MariaDbDataSource |
| Oracle           | Oracle       | oracle.jdbc.pool.OracleDataSource |
| OrientDB         | OrientDB     | com.orientechnologies.orient.jdbc.OrientDataSource |
| PostgreSQL       | pgjdbc-ng    | com.impossibl.postgres.jdbc.PGDataSource |
| PostgreSQL       | PostgreSQL   | org.postgresql.ds.PGSimpleDataSource |
| SAP MaxDB        | SAP          | com.sap.dbtech.jdbc.DriverSapDB |
| SQLite           | xerial       | org.sqlite.SQLiteDataSource |
| SyBase           | jConnect     | com.sybase.jdbc4.jdbc.SybDataSource |

### Play框架插件

注意，Play 2.4现在默认使用HikariCP.一个新的Play框架插件发布了; [play-hikaricp](http://edulify.github.io/play-hikaricp.edulify.com/).  如果您使用优秀的Play框架，您的应用程序应该得到HikariCP.  Thanks Edulify Team!

### Clojure封装

一个新的Clojure封装创建于[tomekw](https://github.com/tomekw)，项目在[这里](https://github.com/tomekw/hikari-cp).

### JRuby封装

一个新的JRuby封装创建于[tomekw](https://github.com/tomekw)，项目在[这里](https://github.com/tomekw/hucpa).

----------------------------------------------------

### 支持 <sup><sup>&#128172;</sup></sup>

Google讨论组 [HikariCP here](https://groups.google.com/d/forum/hikari-cp), growing [FAQ](https://github.com/brettwooldridge/HikariCP/wiki/FAQ).

[![](https://raw.github.com/wiki/brettwooldridge/HikariCP/twitter.png)](https://twitter.com/share?text=Interesting%20JDBC%20Connection%20Pool&hashtags=HikariCP&url=https%3A%2F%2Fgithub.com%2Fbrettwooldridge%2FHikariCP)&nbsp;[![](https://raw.github.com/wiki/brettwooldridge/HikariCP/facebook.png)](http://www.facebook.com/plugins/like.php?href=https%3A%2F%2Fgithub.com%2Fbrettwooldridge%2FHikariCP&width&layout=standard&action=recommend&show_faces=true&share=false&height=80)

### Wiki

不要忘了还有[Wiki](https://github.com/brettwooldridge/HikariCP/wiki)关于更多信息:
 * [FAQ](https://github.com/brettwooldridge/HikariCP/wiki/FAQ)
 * [Hibernate 4.x Configuration](https://github.com/brettwooldridge/HikariCP/wiki/Hibernate4)
 * [MySQL Configuration Tips](https://github.com/brettwooldridge/HikariCP/wiki/MySQL-Configuration)
 * 等.

----------------------------------------------------

### 要求

 &#8658; Java 8+ (Java 6/7 artifacts are in maintenance mode)<br/>
 &#8658; slf4j library<br/>

### 赞助商
高性能项目永远不会有太多工具！我们要感谢以下公司:

Thanks to [ej-technologies](https://www.ej-technologies.com) for their excellent all-in-one profiler, [JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html).

YourKit supports open source projects with its full-featured Java Profiler.  Click the YourKit logo below to learn more.<br/>
[![](https://github.com/brettwooldridge/HikariCP/wiki/yklogo.png)](http://www.yourkit.com/java/profiler/index.jsp)<br/>


### 贡献

请执行更改从``dev``分支而不是``master``提交pull请求。请将编辑器设置为使用空格而不是制表符，并遵循您正在编辑的代码的明显样式。如果您希望得到最新的代码，那么``dev``分支总是比``master``更"前".

[Build Status]:https://travis-ci.org/brettwooldridge/HikariCP
[Build Status img]:https://travis-ci.org/brettwooldridge/HikariCP.svg?branch=dev

[Coverage Status]:https://codecov.io/gh/brettwooldridge/HikariCP
[Coverage Status img]:https://codecov.io/gh/brettwooldridge/HikariCP/branch/dev/graph/badge.svg

[license]:LICENSE
[license img]:https://img.shields.io/badge/license-Apache%202-blue.svg

[Maven Central]:https://maven-badges.herokuapp.com/maven-central/com.zaxxer/HikariCP
[Maven Central img]:https://maven-badges.herokuapp.com/maven-central/com.zaxxer/HikariCP/badge.svg

[Javadocs]:http://javadoc.io/doc/com.zaxxer/HikariCP
[Javadocs img]:http://javadoc.io/badge/com.zaxxer/HikariCP.svg
