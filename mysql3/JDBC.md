# 21. JDBC

1. 概念：JDBC	(Java Database Connectivity) Java数据库连接

   *JDBC本质：其实是（Sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商来实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包的实现类。*

   ![image-20200614084854696](https://images.shiguangping.com/imgs/20200614084854.png)

2. 快速入门：

   *步骤：*

   1. 导入驱动jar包
   2. 注册驱动 class.forName("com.mysql.cj.jdbc.Driver");
   3. 获取数据库连接对象 Connection connection, Connection是接口
   4. 定义sql
   5. 获取执行sql语句的对象Statement 
   6. 执行sql，接收返回的结果
   7. 处理结果
   8. 释放资源

代码示例：

```java
/**
 * JDBC快速入门
 */
public class JdbcDemo1 {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/itcast";
    private static final String DB_USER = "root";
    private static final String DB_PASS = "Liyan1234";

    public static void main(String[] args) throws ClassNotFoundException {
        //1. 导入驱动jar包
        try {
            //2.注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver");
            //3.获取连接对象
            Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
            //4.定义sql语句
            String sql = "update account set balance = 1000 where name='张三'";
            //5.获取Statement对象
            Statement stmt = conn.createStatement();
            //6.执行sql语句
            int count = stmt.executeUpdate(sql);
            //7.处理结果
            System.out.println(count);
            //释放资源
            stmt.close();
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```



### 逐个解析：

#### 1.DriverManager类

* registerDriver()方法：注册驱动：

  class.forName("com.mysql.cj.jdbc.Driver");加载jdbc中的Driver实现类，该实现类中有一个静态代码块，加载驱动实际上是调用了DriverManager里的注册驱动registerDriver()方法：

  ```java
  static {
      try {
          DriverManager.registerDriver(new Driver());
      } catch (SQLException var1) {
          throw new RuntimeException("Can't register driver!");
      }
  }
  ```

  

* getConnection()方法：获取Connection对象

  获取连接对象是调用DriverManager类中的静态方法getConnection()返回了一个Connection对象：

  ```java
  Connection conn = DriverManager.getConnection(DB_URL, DB_USER, DB_PASS);
  ```

  DB_URL:指定连接路径

  路径格式：`jdbc:mysql://ip地址:端口号/数据库名称`

  如果连接本地本地数据库，可简写成：`jdbc:mysql:///数据库名称`

#### 2.获取数据库Connection对象

Interface Connection

- createStatement()方法：获取执行sql语句的Statement对象

  ```java
  Statement stmt = conn.createStatement();
  ```

  Connection接口对象调用了实现类ConnectionImpl中的createStatement()方法，返回一个Statement对象

- prepareStatement(String sql)方法：用来生成PreparedStatement对象

- 管理事务：

  开启事务：setAutoCommit(boolean autoCommit)：该方法设置参数为false，即开启事务

  提交事务：commit()

  回滚事务：rollback()



#### 3.获取执行sql语句Statement对象

Interface Statement 用于执行静态SQL语句并返回其生成的结果的对象。

- execute(String sql) 执行任意sql语句

- executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL（create、alter、drop）语句

  返回值int类型：影响的行数，可以通过这个印象的行数来判断DML语句是否执行成功。如果返回值>0则执行成功。

- executeQuery(String sql) ：执行DQL（select）语句，返回ResultSet对象。



#### 4.ResultSet：结果集对象

executeQuery()方法执行sql查询，会返回一个结果集对象

- next()方法：游标向下移动一行，如果返回true，则有下一行；如果返回flase，则表示没有更多行。
- getString("字段名") ：得到该行指定列的数据（String）



#### 5.PreparedStatement对象

是Statement的子接口，可防止SQL注入问题

- PreparedStatement pstmt = conn.prepareStatement(String sql);

  通过Connection接口中的prepareStatement(String sql)方法创建对象，需要传入sql语句

- SQL语句的写法：String sql = "select * from user where username=? and password=?";

- setString(int index,String string);为SQL语句中的问号赋值，index为第几个问号，从1开始。

- pstmt.executeQuery(); 这里不需要传入SQL语句

*代码演示：用户名密码校验*

```java
/**
     * 登陆判断用户名密码
     * @param username
     * @param password
     * @return bool
     */
    public boolean login(String username, String password) {
        if (username == null || password == null) {
            return false;
        }
        PreparedStatement pstmt = null;
        Connection conn = null;
        ResultSet rs = null;
        try {
            conn = JDBCUtils.getConnection();
            String sql = "select * from user where username=? and password=?";
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1,username);
            pstmt.setString(2,password);
            rs = pstmt.executeQuery();
            return rs.next();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            JDBCUtils.closs(rs, pstmt, conn);
        }
        return false;
    }
```



### JDBC控制事务

1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
2. 操作：
   1. 开始事务
   2. 提交事务
   3. 回滚事务
3. 使用Connection对象来管理事务
   * 开启事务：setAutoCommit(boolean autoCommit); 参数为false即开始事务
   * 提交事务：commit()
   * 回滚事务：rollback()

