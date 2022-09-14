## 1，JDBC概述

在开发中我们使用的是java语言，那么势必要通过java语言操作数据库中的数据。这就是接下来要学习的JDBC。

### 1.1  JDBC概念

> JDBC   就是使用Java语言操作关系型数据库的一套API
>
> 全称：( Java DataBase Connectivity ) Java 数据库连接

<img src="C:\Users\86138\Desktop\java\javaweb学习\day03-JDBC\ppt\assets\image-20210725130537815.png" alt="image-20210725130537815" style="zoom:80%;" />

我们开发的同一套Java代码是无法操作不同的关系型数据库，因为每一个关系型数据库的底层实现细节都不一样。如果这样，问题就很大了，在公司中可以在开发阶段使用的是MySQL数据库，而上线时公司最终选用oracle数据库，我们就需要对代码进行大批量修改，这显然并不是我们想看到的。我们要做到的是同一套Java代码操作不同的关系型数据库，而此时sun公司就指定了一套标准接口（JDBC），JDBC中定义了所有操作关系型数据库的规则。众所周知接口是无法直接使用的，我们需要使用接口的实现类，而这套实现类（称之为：驱动）就由各自的数据库厂商给出。

### 1.2  JDBC本质

* 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口
* 各个数据库厂商去实现这套接口，提供数据库驱动jar包
* 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

### 1.3  JDBC好处

* 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
* 可随时替换底层数据库，访问数据库的Java代码基本不变

以后编写操作数据库的代码只需要面向JDBC（接口），操作哪儿个关系型数据库就需要导入该数据库的驱动包，如需要操作MySQL数据库，就需要再项目中导入MySQL数据库的驱动包。如下图就是MySQL驱动包

![image-20220907212721277](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907212721277.png)

## 2，JDBC快速入门

![image-20220907212711876](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907212711876.png)

第一步：编写Java代码

第二步：Java代码将SQL发送到MySQL服务端

第三步：MySQL服务端接收到SQL语句并执行该SQL语句

第四步：将SQL语句执行的结果返回给Java代码

2.1  编写代码步骤

- 创建工程，导入驱动jar包

![image-20220907213146955](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213146955.png)

* 注册驱动

  ```sql
  Class.forName("com.mysql.jdbc.Driver");
  ```

* 获取连接

  ```sql
  Connection conn = DriverManager.getConnection(url, username, password);
  ```

* Java代码需要发送SQL给MySQL服务端，就需要先建立连接

* 定义SQL语句

  ```sql
  String sql =  “update…” ;
  ```

* 获取执行SQL对象

* 执行SQL语句需要SQL执行对象，而这个执行对象就是Statement对象

  ```sql
  Statement stmt = conn.createStatement();
  ```

* 执行SQL

  ```sql
  stmt.executeUpdate(sql);  
  ```

* 处理返回结果

* 释放资源

### 2.2  具体操作

* 创建新的空的项目

![image-20220907213512585](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213512585.png)

* 定义项目的名称，并指定位置

![image-20220907213750654](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213750654.png)

* 对项目进行设置，JDK版本、编译版本

![image-20220907213835547](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213835547.png)

* 创建模块，指定模块的名称及位置

![image-20220907213843541](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213843541.png)

- 导入驱动包

-将mysql的驱动包放在模块下的lib目录（随意命名）下，并将该jar包添加为库文件

![image-20220907213903892](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907213903892.png)

在添加为库文件的时候，有如下三个选项

* Global Library  ： 全局有效
* Project Library :   项目有效
* Module Library ： 模块有效

![image-20220907214024372](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907214024372.png)

* 在src下创建类

![image-20220907214220093](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907214220093.png)

- 编写代码如下

```java
/**
 * JDBC快速入门
 */
public class JDBCDemo {

    public static void main(String[] args) throws Exception {
        //1. 注册驱动
        //Class.forName("com.mysql.jdbc.Driver");
        //2. 获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);
        //3. 定义sql
        String sql = "update account set money = 2000 where id = 1";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //5. 执行sql
        int count = stmt.executeUpdate(sql);//受影响的行数
        //6. 处理结果
        System.out.println(count);
        //7. 释放资源
        stmt.close();
        conn.close();
    }
}
```

## 3，JDBC API详解

### 3.1  DriverManager

DriverManager（驱动管理类）作用：

- 注册驱动

![image-20220907214657247](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907214657247.png)

registerDriver方法是用于注册驱动的，但是我们之前做的入门案例并不是这样写的。而是如下实现

```java
Class.forName("com.mysql.jdbc.Driver");
```

我们查询MySQL提供的Driver类，看它是如何实现的，源码如下：

![image-20220907214830441](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907214830441.png)

在该类中的静态代码块中已经执行了 `DriverManager` 对象的 `registerDriver()` 方法进行驱动的注册了，那么我们只需要加载 `Driver` 类，该静态代码块就会执行。而 `Class.forName("com.mysql.jdbc.Driver");` 就可以加载 `Driver` 类。

> ==提示：==
>
> * MySQL 5之后的驱动包，可以省略注册驱动的步骤
> * 自动加载jar包中META-INF/services/java.sql.Driver文件中的驱动类

获取数据库连接

![image-20220907215051684](C:\Users\86138\AppData\Roaming\Typora\typora-user-images\image-20220907215051684.png)

参数说明：

* url ： 连接路径

  > 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2…
  >
  > 示例：jdbc:mysql://127.0.0.1:3306/db1
  >
  > ==细节：==
  >
  > * 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称?参数键值对
  >
  > * 配置 useSSL=false 参数，禁用安全连接方式，解决警告提示

* user ：用户名

* poassword ：密码

### 3.2  Connection

Connection（数据库连接对象）作用：

* 获取执行 SQL 的对象
* 管理事务

#### 3.2.1  获取执行对象

* 普通执行SQL对象

  ```sql
  Statement createStatement()
  ```

* 入门案例中就是通过该方法获取的执行对象。

* 预编译SQL的执行SQL对象：防止SQL注入

```java
PreparedStatement  prepareStatement(sql)
```

通过这种方式获取的 `PreparedStatement` SQL语句执行对象是我们一会重点要进行讲解的，它可以防止SQL注入。

- 执行存储过程的对象

  ```sql
  CallableStatement prepareCall(sql)
  ```

* 通过这种方式获取的 `CallableStatement` 执行对象是用来执行存储过程的，而存储过程在MySQL中不常用，所以这个我们将不进行讲解。

#### 3.2.2  事务管理

先回顾一下MySQL事务管理的操作：

* 开启事务 ： BEGIN; 或者 START TRANSACTION;
* 提交事务 ： COMMIT;
* 回滚事务 ： ROLLBACK;

> MySQL默认是自动提交事务

接下来学习JDBC事务管理的方法。

Connection几口中定义了3个对应的方法：

* 开启事务 ： BEGIN; 或者 START TRANSACTION;
* 提交事务 ： COMMIT;
* 回滚事务 ： ROLLBACK;

> MySQL默认是自动提交事务

接下来学习JDBC事务管理的方法。

Connection几口中定义了3个对应的方法：

* 开启事务

  ![image-20210725173444628](C:\Users\86138\Desktop\java\javaweb学习\day03-JDBC\ppt\assets\image-20210725173444628.png)

  参与autoCommit 表示是否自动提交事务，true表示自动提交事务，false表示手动提交事务。而开启事务需要将该参数设为为false。

* 提交事务

  ![image-20210725173618636](C:\Users\86138\Desktop\java\javaweb学习\day03-JDBC\ppt\assets\image-20210725173618636.png)

* 回滚事务

  ![image-20210725173648674](C:\Users\86138\Desktop\java\javaweb学习\day03-JDBC\ppt\assets\image-20210725173648674.png)

具体代码实现如下：

```java
/**
 * JDBC API 详解：Connection
 */
public class JDBCDemo3_Connection {

    public static void main(String[] args) throws Exception {
        //1. 注册驱动
        //Class.forName("com.mysql.jdbc.Driver");
        //2. 获取连接：如果连接的是本机mysql并且端口是默认的 3306 可以简化书写
        String url = "jdbc:mysql:///db1?useSSL=false";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);
        //3. 定义sql
        String sql1 = "update account set money = 3000 where id = 1";
        String sql2 = "update account set money = 3000 where id = 2";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();

        try {
            // ============开启事务==========
            conn.setAutoCommit(false);
            //5. 执行sql
            int count1 = stmt.executeUpdate(sql1);//受影响的行数
            //6. 处理结果
            System.out.println(count1);
            int i = 3/0;
            //5. 执行sql
            int count2 = stmt.executeUpdate(sql2);//受影响的行数
            //6. 处理结果
            System.out.println(count2);

            // ============提交事务==========
            //程序运行到此处，说明没有出现任何问题，则需求提交事务
            conn.commit();
        } catch (Exception e) {
            // ============回滚事务==========
            //程序在出现异常时会执行到这个地方，此时就需要回滚事务
            conn.rollback();
            e.printStackTrace();
        }

        //7. 释放资源
        stmt.close();
        conn.close();
    }
}
```

