
# JDBC



> JDBC(Java DataBase Connectivty,Java数据库连接)API,是一种用于执行Sql语句的Java API,可以为关系型数据库提供统一的访问,其由一组Java编写的类和接口组成.


## JDBC驱动程序



> **起初,SUN公司推出JDBC API希望能适用于所有数据库,但实际中是不可能实现的,**各个厂商提供的数据库差异太大,SUN公司于数据库厂商协同之后决定:**由SUN公司提供一套访问数据库的API,各个厂商根据规范提供一套访问自家数据库API的接口,**SUN公司提供的规范API称之为**JDBC**,厂商提供的自家数据库API接口称之为 **驱动**


### JDBC原理


* 经过SUN公司于各个数据库厂商的协同,JDBC的结构如下图:


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033324878-1865629228.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033324878-1865629228.png)


### JDBC驱动的原理



> 我们知道了JDBC运行的原理,那么JDBC驱动是怎么运行的呢,我们开始进行探究


* **JDBC驱动的类型**



> 根据访问数据库数据库技术不同,JDBC驱动程序被访问四类


	+ Type1: `JDBC-ODBC`桥驱动程序
		- 此类驱动程序由JDBC\-ODBC桥金额一个ODBC驱动程序组成
	+ Type2:部分Java本地JDBC API驱动程序
		- 此类驱动程序必须在本地计算机上先安装好特点的驱动程序才能进行使用
	+ **Type3:纯Java的数据库中间件驱动程序(目前主流)**
		- `Pure Java Driver for Database Middleware`使用此类驱动时,不需要再本地计算机上安装任何附加软件,但必须再安装数据库管理系统的服务器端加装中间件`(Middleware)` ,这个中间件负责所有存取数据库时的必要转换.**中间件的工作原理是:**驱动程序将JDBC访问转换为于数据库无关的标准网络协议(通常是HTTP或HTTPS)送出,然后再由中间件服务器将其转换为数据库专用的访问指令,完成对数据库的操作.中间件可以支持对多种数据库的访问.
	+ **Type4:纯Java的JDBC驱动程序(最理想的驱动程序)**
		- `Direct-toDatabasePureJavaDriver` 此类驱动程序是之间面向数据库的Java驱动程序,即所谓的”瘦”驱动程序.使用该驱动程序无需安装如何附加软件(包括本地计算机或是数据库服务器端),所有存取的数据库操作都直接由JDBC驱动程序来完成
* **JDBC驱动工作动作**


	+ 对于第三类Type3驱动程序来说,其是由纯Java语言开发的,此类驱动程序体积最小
		- 下面给出Type3驱动程序的结构图


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033339162-1442825343.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033339162-1442825343.png)



```
    - 可见,Type 3,JDBC驱动程序为两层结构,分别为驱动程序客户端和驱动程序服务端,客户端直接于用户交互,**其为用户提供符合JDBC规范的数据库统一编程接口,**客户端将数据请求通过**特定的网络请求**发送值服务器.服务器作为中间件的角色,其负责接收和处理用户的请求,JDBC驱动程序本身不进行直接与数据库的交互,而是借助其他已经实现的驱动,称之为”雇佣”.当然”雇佣”的数量越多,可支持的数据库数量就越多.”雇佣”的数量和成员可以动态的改变,以满足业务的扩展,这也是Type3JDBC性能强大的原因

```

## 将JDBC驱动导入idea(以Mysql为例)


1. 第一步肯定是下载JDBC驱动程序了,各个厂商把特定的驱动程序打包成.jar发布在其官网上大伙可用到官网下载
	* **常用的各大厂商驱动下载地址**
	
	
		1. **Mysql数据库**
		
		
		[https://dev.mysql.com/downloads/connector/](https://github.com)
		2. **Oracle数据库**
		
		
		[https://www.oracle.com/database/technologies/appdev/jdbc\-downloads.html](https://github.com)
		3. **SQL Server 数据库**
		
		
		[https://docs.microsoft.com/zh\-cn/sql/connect/jdbc/download\-microsoft\-jdbc\-driver\-for\-sql\-server?view\=sql\-server\-2017](https://github.com)
	* **Mysql驱动下载**
	
	
		+ 通过选择 Connector/J→选项选择Platform Independent→在根据自身电脑配置下载


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033352038-1546669150.jpg)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033352038-1546669150.jpg)



```
    下载好后,解压至文件夹备用

```

2. 打开idea,在要导入驱动的项目中新建一个名为lib的文件夹(建议命名)


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033404780-877600858.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033404780-877600858.png)


3. 将下载的zip文件解压,找到其中的mysql\-connector\-java\-8\.0\.26\.jar包,**将其复制**


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033417757-863773867.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033417757-863773867.png)


4. 复制其中的mysql\-connector\-java\-8\.0\.18\.jar文件，在lib文件夹上右键，粘贴到IDEA中，刚刚新建的lib文件夹里


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033428564-1234213568.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033428564-1234213568.png)


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033437414-834406845.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033437414-834406845.png)


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033447197-178201426.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033447197-178201426.png)


### 使导入的驱动生效


1. 在idea点击File→Project Structure


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033458478-1232631169.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033458478-1232631169.png)


1. 在Modules中选择Denpendencies


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033514360-990551992.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033514360-990551992.png)


2. 点击左侧\+号,选择 JARS or directories


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033648206-391565047.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033648206-391565047.png)


3. 在弹出的窗口中选择刚刚导入 lib 文件夹的驱动，点击Ok


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033655213-355184841.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033655213-355184841.png)


1. 可以看到Module模块中，多出了一个mysql驱动，选择之后点击Apply，然后Ok


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033702862-2113051902.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033702862-2113051902.png)


## JDBC中的的类及其应用



> JDBC API中包含四个常用的接口和一个类,分别是`Connection`接口,`Statement`接口,`PreparedStatement`接口,`ResultSet`接口,`DriverManager`类,jar包中已经包含这些接口的实现类直接使用即可


### Statement接口



> Statement接口是Java程序执行数据库操作的重要接口,用于已经建立了数据库连接的基础上,向数据库发送要执行的Sql语句


* **作用**:执行不带参数的简单Sql语句
* **主要方法**
	+ `void addBatch(String sql )throws SQLException` :该方法用于将Sql语句添加到Statement对象的当前命令列表中,**用于Sql语句的批量处理**
	+ `void clearBatch() throws SQLException`:立即**释放**Statement对象中的命令列表
	+ `boolean excute(String sql) throws SQLException` :**执行指定的Sql语句**,成功返回`true`否则返回`false`
	+ `int[] excuteBatch() throws SQLException`:**将命令列表中的sql命令提交执行**,返回一个int数组表示每个sql语句影响的行数
	+ `ResultSet excuteQuery(String sql) throws SQLException`:该方法用于**执行查询类型(Select类型)的Sql语句,**返回的查询所获取的结果集`ResultSet`对象
	+ `void close() throws SQLException`:用于立即释放此Statement对象的数据库和JDBC资源


### Connection接口



> Connection接口位于java.sql包中,是用于与数据库连接的对象,只有获取了与数据库连接的对象后,才能访问数据库进行操作


* **作用**:与数据库进行连接
* **主要方法**


	+ `Statement createStatement() throws SQLException`:用于创建一个Statement对象,用于执行Sql语句
	+ `PreparedStatement prepareStatement(String sql) throws SQLException`: 创建一个PreparedStatement对象,用于执行预编译的Sql语句
	+ `CallableStatement prepareCall(String sql)throws SQLException`:创建一个CallableStatement对象用于执行存储过程或函数
	+ `void commit() throws SQLException`:提交当前事务
	+ `void rollback() throws SQLException`:回滚事务
	+ `void close() throws SQLException`:关闭连接
	
	
	
	> 在进行数据库连接的时候还要用到DriverManager类中的`getConnection(url,username,password)`方法E.g:



```
        String url = "jdbc:mysql://localhost:3306/demo";
        String username = "root";
        String password = "root";
        //建立连接
        Connection connection = DriverManager.getConnection(url, username, password);
        String sql = "SELECT * FROM dept";
        //创建Statement对象执行查询操作
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery(sql);
        //处理查询结果
        while (resultSet.next()) {
            String dId = resultSet.getString("d_id");
            String dName = resultSet.getString("d_name");
            String loc = resultSet.getString("loc");
            System.out.println(dId + " " + dName + " " + loc);
        }
        connection.close();
        statement.close();
        resultSet.close();
    }

```


### DriverManager类



> DriverManager类是JDBC API的核心,该类中包含了与数据库交互操作的方法,类中的方法都由数据库厂商提供


* **作用:**管理和协调不同的JDBC驱动程序
* **主要方法**
	+ `public static Connection getConnection(String url, String user, String password)throws SQLException`:根据指定的数据库url,用户名以及密码建立数据库连接
	+ `public static Connection getConnection(String url,Properties info)`:根据指定的数据库url以及连接属性建立数据库连接
	+ `public static synchronized void deregisterDriver(Driver driver) throws SQLException`:从DriverManager管理列表中删除一个驱动,driver参数是要删除的驱动对象


### PreparedStatement接口



> PreparedStatement接口位于java.servlet包中,其继承了Statement接口


* 与Statement的区别:
	+ **执行速度较快:**PreparedStatement对象是已经预编译过的,执行速度快于Statement.**因此若要执行大量的Sql语句时使用PreparedStatement以提高效率**
* **主要方法**
	+ **setXXX()**
	
	
	
	> 此类方法都是设置sql语句中传入的参数里类型
	
	
		- `void setBinaryStream(int parameterIndex,InputStream x) throws SQLException`:将二进制流作为sql语句传入的参数,二进制流可用高效地处理图片,音频,视频登媒介,`parameterIndex` 是参数位置索引
		- `void setBoolean(int parameterIndex,boolean x) throws SQLException`:将boolen作为sql传入的参数类型,parameterIndex为参数位置索引
		- `void setByte(int parameterIndex,byte x) throws SQLException`:将byte作为sql传入的参数类型
		- `void setDate(int parameterIndex,Date x) throws SQLException`: 将java.sql.Date值x做为SQL语句中的参数值
		- `void setDouble(int parameterIndex,double x)` :将double值x做为SQL语句中的参数值
		- `void setInt(int parameterIndex,int x) throws SQLException` :将int值x做为SQL语句中的参数值
		- `void setObject(int parameterIndex,Object x) throws SQLException` :将object对象x做为SQL语句中的参数值
		- `void setString(int parameterIndex,String x) throws SQLException`: 将String值x做为SQL语句的参数值
		- `void setTimestamp(int parameterIndex,Timestamp x) throws SQLException`: 将java.sql.Timestamp值x做为SQL语句中的参数值
	+ `int executeUpdate() throws SQLException`:`executeUpdate()` 方法返回的 `int` 值表示受影响的行数。如果返回值为 0，则可能表示没有符合条件的记录被修改;执行`INSERT`、`UPDATE` 或 `DELETE`这些DML语句时同理


### ResultSet接口



> 是用于接收查询的结果,是结果集合,当你执行一个SELECT语句时DBMS会返回一个包含查询结果的数据表,ResultSet接收用于表现这个数据表的对象


* **作用:**表示查询后的返回值
* **主要方法**
	+ `Boolean next() throws SQLException`:移动游标到结果集的下一行,并返回一个boolen值,结尾返回false
	+ `getXXX(String columnLabel)`:获取指定列名的值,XXX表示Java数据类返回XXX类型,如getString(),`columnLabel`表示列名
	+ `getXXX(int columnIndex)`：获取指定列索引的值，列索引从 1 开始


# SQL注入



> 所谓SQL注入,是值通过把恶意SQL语句插入到Web表单提交或页面请求的查询字符串,最终达到欺骗服务器的结果


## SQL注入实例


* 对于一个简单的登入功能,关键函数如下:



```
 static boolean noProtectLogin(String username, String password, Statement statement) throws SQLException {
        //username="abc";
        //password = "or '1'='1'";
        String sql = "SELECT *FROM  user WHERE username= '" + username + "'AND password=+''";
        ResultSet resultSet = statement.executeQuery(sql);
        return resultSet.next();
    }

```

* 方法中的username于password没有进行任何处理,直接接受前端传入的数据,这样拼接的SQL语句会发送注入漏洞
* 若把password参数修改成`"or '1'='1'"`,username为任意值,那么这条语句结果为`SELECT *FROM user WHERE username= 'abc' AND password= 'or '1'='1''`显然这条语句的一直是true,这样可以把user表中的所有用户信息查询到,就可以成功实现无密码登入


## SQL预编译



> 也称之为SQL预处理是一种,可以提高sql语句安全性和性能的技术


* 预编译SQL语句允许在运行之前定义SQL语句结构,同时使用占位符(通常是问号`?`)来动态表示数据部分
* 在Java中可以使用`PreparedStatement`来创建和执行预编译的SQL语句,刚刚的登入操作使用`PreparedStatement`操作的代码如下



```
static boolean noProtectLogin(String username, String password, Connection connection) throws SQLException {
//      username="abc";
//      password = "or '1'='1'";
        String sql = "SELECT * FROM  user WHERE username= ? AND password = ? ";
        PreparedStatement statement = connection.prepareStatement(sql);
        statement.setString(1, username);
        statement.setString(2, password);
        ResultSet resultSet = statement.executeQuery();
        return resultSet.next();
    }

```

	+ 这样我们在进行恶意的SQL注入,如把password定义成`"or '1'='1'"` 执行结果直接返回false


# 事务



> 什么是事务? 官方的说法:事务是访问数据库的一个操作序列,数据库应用系统通过执行业务集合要完成对数据库的存取,简单来说:**事务是执行工作操作中最小的不可再分的工作单位**,通常一个业务对应一个事务,多个操作同时进行要么同时成功,要么同时失败,这就是事务


## 事务的理解


### 事务的特性


* **原子性: 即不可分割,事务要么全部被执行,要么全部不执行**.若所有的事务都提交成功,那么数据库操作被提交,数据库状态发生变化,**若有一个子事务失败,那么其余事务的数据库操作都会回滚**,即数据库状态回到事务执行之前,保持状态不变
* 一致性:事务的执行使得数据库从一种正确状态转换为另一个正确状态
* 隔离性:在事务正确提交之前,不允许把事务对该数据的改变提交给其他事务,即在正确提交之前,其可能的结果不应该用于给其他事务
* 持久性:即事务正确提交之后,其结果会永远保存在数据库之中,即事务提交之后有了其他故障，事务的处理结果也会得到保存


### **事务的通俗例子**


* 假设张三要给李四转账,要完成这个操作,要执行两个事务,一个是:扣除张三的账户余额;另一个是:李四的账余额增加**,这两个事务是不可分割的**


### 事务的作用


* 主要作用:保证了用户的每一次操作都是可靠的,即便出现了异常的访问情况,也不会破坏后台的数据完整性,拿ATM机举例子,若ATM在操作过程中突然出现故障,此时事务必须确保故障前对账号的操作不生效,确保用户于银行的利益不受损


## JDBC中对事物进行管理



> 在JDBC中,Connection接口中定义了几个对事务操作的方法,我们一一讲解


* `void setAutoCommit*(*boolean autoCommit*)* throws SQLException`:在默认情况下JDBC连接处于自动提交模式,这意味着每个SQL语句执行后都会立即提交,这明显不符合事务的特性,要我们进行显性关闭,即将`autoCommit`设置为false
* `void commit()`:若所有操作都执行成功调用该方法提交事务
* `void rollback()`:若事务中任何操作失败或出现异常错误则需调用`rollback()`回滚事务


### 银行存/取款举例



> 拿刚刚的张三和李四的例子说明



```
try {
            String sql1 = "UPDATE bank SET balance = balance - ? WHERE b_id=?";
            String sql2 = "UPDATE bank SET balance = balance + ? WHERE b_id=?";

            stmt1 = conn.prepareStatement(sql1);
            stmt2 = conn.prepareStatement(sql2);
            //事务1执行

            stmt1.setDouble(1, 500);
            stmt1.setString(2, "01");
            int r1 = stmt1.executeUpdate();
            //事务2执行

            stmt2.setDouble(1, 500);
            stmt2.setString(2, "02");
            int r2 = stmt2.executeUpdate();

            if (r1 == 1 && r2 == 1) {
                System.out.println("业务执行成功");
                conn.rollback();
            } else {
                System.out.println("业务执行失败");
                conn.commit();
            }
        } catch (SQLException e) {
            e.printStackTrace();
            //有异常回滚事务
            conn.rollback();
        } finally {
            Objects.requireNonNull(stmt1).close();
            Objects.requireNonNull(stmt2).close();
            conn.close();
        }

```

# 连接池



> 连接池是创建和管理数据库连接的技术,这些连接随时准备被任何需要它的线程使用


## 连接池的原理


* 连接池的基本思想是在系统初始化时,将数据库连接**作为对象存储在运行内存中**,**当用户需要访问数据库时,并非建立一个新的连接,而是从连接池中取出一个已建立的空闲连接对象.使用完毕后,用户也并非将连接之间关闭,而是将连接放回连接池中,提供给下一次请求访问使用**,而连接池的建立,断开都由连接池自身来管理.同时,还可以**通过设置连接池的参数来控制连接池中的初始连接数,连接的上下限数以及每个连接的最大使用次数,最大空闲时间等等**
* **连接池参数作用**
	+ **最小连接数:是连接池一直保持于数据库连接的数量**,因此若应用程序对数据库连接的使用量不大还设置较大的最小连接数,会造成大量的连接资源的浪费
	+ **最大连接数:是连接池能申请的最大连接数,若数据库连接请求超过最大连接数,后续的数据库连接请求将被加入到等待队列中**
	+ 若min连接于max连接相差很大时,那么最先连接请求将会获利,之后超过min连接的连接请求等价于新建一个数据库连接,但这些大于min连接的数据库连接在使用之后不会马上被释放,将被放入连接池中等待重复利用


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033830968-2081516631.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033830968-2081516631.png)


## C3P0



> C3P0是一个开放源代码的JDBC连接池,包括了jdbc3和jdbc2扩展规范说明的Connection和Statement池的DataSources对象


### C3P0的配置


* C3P0所需的两个必要jar包如下图:可以去官网直接下载zip文件:[c3p0:JDBC DataSources/Resource Pools download \| SourceForge.net](https://github.com)


导入方法于导入JDBC\-Mysql jar包类似这里不再赘述


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033845425-1551596337.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033845425-1551596337.png)


* 接着要配置c3p0\-config.xml文件(这里选择xml配置)→直接配置到src文件夹下,否则会报配置错误


[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033858950-1725211058.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033858950-1725211058.png)


* **c3p0\-config.xml文件的一般模板(可直接拷贝使用):**



```

    <default-config>
        
        "driverClass">com.mysql.cj.jdbc.Driver

        
        "jdbcUrl">jdbc:mysql://localhost:3306/demo

        
        "user">root

        
        "password">root

        
        "initialPoolSize">3

        
        "maxPoolSize">5
        
		
        "acquireIncrement">3
        
        "maxIdleTime">60

        
        "checkoutTimeout">0
    default-config>


```


### C3P0的使用方法



> `C3P0`中只有一个类,显然这个类是最主要的,其包含了C3P0所有对数据库连接的操作,接下来我们开始讲解


### ComboPooledDataSource类


* **主要方法**


	+ `Connection getConnection()`:从连接池中获取一个数据库连接,若当前没有空闲连接,则新建连接,直到达到最大连接数.其返回一个`Connection`对象
> 除了使用xml文件配置还可以在代码中直接使用`ComboPooledDataSource` 类中的方法配置,但一般使用xml文件,避免冗余


	+ `void setXXX()` :设置C3P0中的各类属性,XXX表示属性,例如`setUser(),setMinPoolSize(int min)` 等等
* **使用连接池测试更新语句**



```
public static void main(String[] args) throws SQLException {
        //建立连接池,获取连接
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        Connection conn = dataSource.getConnection();
        //用连接池测试更新语句
        String sql = "SELECT * FROM  user WHERE username= ? AND password = ? ";
        PreparedStatement stmt = conn.prepareStatement(sql);

        stmt.setInt(1, 11111);
        stmt.setString(2, "11111111");
        ResultSet resultSet = stmt.executeQuery();
        if (resultSet.next()) {
            int username = resultSet.getInt("username");
            String password = resultSet.getString("password");
            System.out.println("username:" + username + "password:" + password);
        }
        stmt.close();
        conn.close();
    }

```


### **Druid(最好用的Java连接池)**



> Druid是目前最好数据库连接池,在功能.性能.扩展性方面都吊打其他连接池,包括`DBCP,C3P0,BoneCP,Proxool`等等


### Druid配置


* Druid执行要一个jar包大家可以去官网:[https://repo1\.maven.org/maven2/com/alibaba/druid/下载所需的jar包,在导入即可](https://github.com)
* **Druid的参数列表**




| **属性(Parameter)** | **默认值(Default)** | **描述(Description)** |
| --- | --- | --- |
| **username** | \*\*\*\* | **连接数据库的用户名** |
| **password** | \*\*\*\* | **连接数据库的密码** |
| **jdbcUrl** | \*\*\*\* | **同C3P0中的jdbcUrl属性** |
| **driverClassName** | **根据url自动识别** | **这一项可配可不配，如果不配置druid会根据url自动识别dbType，然后选择相应的driverClassName** |
| **initialSize** | **0** | \**初始化时建立物理连接的个数。初始化发生在显示调用init方法，或者第一次getConnection时                   *参见DBCP中的initialSize属性** |
| **maxActive** | **8** | **最大连接池数量**(Maximum number of Connections a pool will maintain at any given time. |
| **maxIdle** | **8** | **已经不再使用，配置了也没效果** |
| **minIdle** | \*\*\*\* | **最小连接池数量** |
| **maxWait** | \*\*\*\* | **获取连接时最大等待时间，单位毫秒。配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，如果需要可以通过配置useUnfairLock属性为true使用非公平锁。** |
| **poolPreparedState\- ments** | **false** | **是否缓存preparedStatement，也就是PSCache。PSCache对支持游标的数据库性能提升巨大，比如说oracle。** |
| **maxOpenPrepared\- Statements** | **\-1** | **要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。      在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100** |
| **testOnBorrow** | **true** | **申请连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能。** |
| **testOnReturn** | **false** | **归还连接时执行validationQuery检测连接是否有效，做了这个配置会降低性能** |
| **testWhileIdle** | **false** | **建议配置为true，不影响性能，并且保证安全性。申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。** |
| **validationQuery** | \*\*\*\* | **用来检测连接是否有效的sql，要求是一个查询语句。如果validationQuery为null，testOnBorrow、testOnReturn、 testWhileIdle都不会其作用。在mysql中通常为select 'x'，在oracle中通常为select 1 from dual** |
| **timeBetweenEviction\-RunsMillis** | \*\*\*\* | **1\) Destroy线程会检测连接的间隔时间 2\) testWhileIdle的判断依据** |
| **minEvictableIdle\- TimeMillis** | \*\*\*\* | **Destory线程中如果检测到当前连接的最后活跃时间和当前时间的差值大于minEvictableIdleTimeMillis，则关闭当前连接。** |
| **removeAbandoned** | \*\*\*\* | **对于建立时间超过removeAbandonedTimeout的连接强制关闭** |
| **removeAbandoned\-Timeout** | \*\*\*\* | **指定连接建立多长时间就需要被强制关闭** |
| **logAbandoned** | **false** | **指定发生removeabandoned的时候，是否记录当前线程的堆栈信息到日志中** |
| **filters** | \*\*\*\* | **属性类型是字符串，通过别名的方式配置扩展插件，常用的插件有： 1）监控统计用的filter:stat 2）日志用的filter:log4j  3）防御sql注入的filter:wall** |


	+ **红色属性为必要配置属性**
	+ 定义application.properties(名字可以顺便取但必须是properties),可以放置任意目录下[![](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033957705-1571790222.png)](https://img2024.cnblogs.com/blog/3423316/202409/3423316-20240907033957705-1571790222.png)


	+ application.properties文件一般模板如下
```
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/test?userSSL=false&serverTimezone=Asia/Shanghai
username=root
password=123456
initialSize=3
maxActive=5
maxWait=1000

```


### Druid使用方法



> Druid也是只要一个核心类`DruidDataSource`它实现了 `javax.sql.DataSource` 接口


* **主要类与方法**


	+ `DruidDataSource()构造方法`:构造一个默认的 DruidDataSource实例这个实例主要用于显性配置Druid属性
		- `setXXX():` 设置属性值,如`setUrl(String url)`,`setInitialSize(int initialSize)`等等
	+ `DruidDataSourceFactory`类:是一个工厂类，用于根据提供的配置信息创建 `DruidDataSource` 实例,这个类简化了从配置文件中加载配置信息并创建 `DruidDataSource` 的过程
		- `createDataSource(Properties properties)`:最常用的方法之一,其接受一个Properties对象作为参数,从该对象读取配置信息,并创建一个`DruidDataSource`
* **将Druid封装为JdbcUtils类**



```
public class JdbcUtil {
    private static DataSource dataSource;

    static {
        //先建立配置,连接连接池
        try {
            InputStream inputStream = JdbcUtil.class.getResourceAsStream("resource/application.properties");
            Properties props = new Properties();
            props.load(inputStream);
            dataSource = DruidDataSourceFactory.createDataSource(props);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() {
        Connection conn = null;
        try {
            conn = dataSource.getConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return conn;
    }

    public static void close(ResultSet rs, PreparedStatement stmt, Connection conn) {
        try {
            if (rs != null) {
                rs.close();
            }
            if (stmt != null) {
                stmt.close();
            }
            if (conn != null) {
                conn.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```
* 本文借鉴同平台许多作者如**@少平的博客人生,@chy\_18883701161,@滥好人**


  * [JDBC](#jdbc)
* [JDBC驱动程序](#jdbc%E9%A9%B1%E5%8A%A8%E7%A8%8B%E5%BA%8F)
* [JDBC原理](#jdbc%E5%8E%9F%E7%90%86)
* [JDBC驱动的原理](#jdbc%E9%A9%B1%E5%8A%A8%E7%9A%84%E5%8E%9F%E7%90%86)
* [将JDBC驱动导入idea(以Mysql为例)](#%E5%B0%86jdbc%E9%A9%B1%E5%8A%A8%E5%AF%BC%E5%85%A5idea%E4%BB%A5mysql%E4%B8%BA%E4%BE%8B)
* [使导入的驱动生效](#%E4%BD%BF%E5%AF%BC%E5%85%A5%E7%9A%84%E9%A9%B1%E5%8A%A8%E7%94%9F%E6%95%88)
* [JDBC中的的类及其应用](#jdbc%E4%B8%AD%E7%9A%84%E7%9A%84%E7%B1%BB%E5%8F%8A%E5%85%B6%E5%BA%94%E7%94%A8):[MeoMiao 萌喵加速](https://biqumo.org)
* [Statement接口](#statement%E6%8E%A5%E5%8F%A3)
* [Connection接口](#connection%E6%8E%A5%E5%8F%A3)
* [DriverManager类](#drivermanager%E7%B1%BB)
* [PreparedStatement接口](#preparedstatement%E6%8E%A5%E5%8F%A3)
* [ResultSet接口](#resultset%E6%8E%A5%E5%8F%A3)
* [SQL注入](#sql%E6%B3%A8%E5%85%A5)
* [SQL注入实例](#sql%E6%B3%A8%E5%85%A5%E5%AE%9E%E4%BE%8B)
* [SQL预编译](#sql%E9%A2%84%E7%BC%96%E8%AF%91)
* [事务](#%E4%BA%8B%E5%8A%A1)
* [事务的理解](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E7%90%86%E8%A7%A3)
* [事务的特性](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E7%89%B9%E6%80%A7)
* [事务的通俗例子](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E9%80%9A%E4%BF%97%E4%BE%8B%E5%AD%90)
* [事务的作用](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E4%BD%9C%E7%94%A8)
* [JDBC中对事物进行管理](#jdbc%E4%B8%AD%E5%AF%B9%E4%BA%8B%E7%89%A9%E8%BF%9B%E8%A1%8C%E7%AE%A1%E7%90%86)
* [银行存/取款举例](#%E9%93%B6%E8%A1%8C%E5%AD%98%E5%8F%96%E6%AC%BE%E4%B8%BE%E4%BE%8B)
* [连接池](#%E8%BF%9E%E6%8E%A5%E6%B1%A0)
* [连接池的原理](#%E8%BF%9E%E6%8E%A5%E6%B1%A0%E7%9A%84%E5%8E%9F%E7%90%86)
* [C3P0](#c3p0)
* [C3P0的配置](#c3p0%E7%9A%84%E9%85%8D%E7%BD%AE)
* [C3P0的使用方法](#c3p0%E7%9A%84%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)
* [ComboPooledDataSource类](#combopooleddatasource%E7%B1%BB)
* [Druid(最好用的Java连接池)](#druid%E6%9C%80%E5%A5%BD%E7%94%A8%E7%9A%84java%E8%BF%9E%E6%8E%A5%E6%B1%A0)
* [Druid配置](#druid%E9%85%8D%E7%BD%AE)
* [Druid使用方法](#druid%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)

   \_\_EOF\_\_

   ![](https://github.com/ihave2carryon)ihave2carryon  - **本文链接：** [https://github.com/ihave2carryon/p/18401274](https://github.com)
 - **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
 - **版权声明：** 本博客所有文章除特别声明外，均采用 [BY\-NC\-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
 - **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
     
