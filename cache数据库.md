---
typora-root-url: ..\..
---

# Cache数据库

[TOC]

#### Cache数据库下载与安装：



#### Cache数据库定义：

​       美国Intersystems公司产品，后关系型数据库(Post Relational database)中的领头羊；`支持关系型数据库和对象型数据库`。

#### 特性：

1. 相较于其他数据库，`查询速度快`；
2. 使用简单、`支持标准的sql语句`，并且还`支持odbc接口`；
3. 与其他系统进行数据交换非常容易，同时还`可以将数据输出为文本文件的形式供其它系统调问访用`
4. `对象型编辑`，数据库定义自己想要的对象，然后再其他ide中调用对象属性和方法完成开发工作
5. 支持web开发，`自带web开发工具`，使用和开发方便

#### 数据类型：

![](../kind.png)



#### 连接方式：

JDBC（JAVA）、ODBC(C)

JDBC：

###### `1.加载驱动：`

```
Class.forName ("com.intersys.jdbc.CacheDriver").newInstance();
```

###### `2.获取连接：`

​      URL地址组成：`驱动方式+主机地址+端口号+数据库名`

​      如：String URL="jdbc:Cache://172.18.37.52:1972/abc"

​     由于CacheDataSource类中getConnection(String user,String password)方法调用了getURL()方法，所以进行获取连接时，参数区别于其他数据库获取Connection方式；

![connection](../connection.png)

```
ds.setURL("jdbc:Cache://172.18.37.52:1972/abc");
Connection dbconn = ds.getConnection("root","123456");
```

###### `3.执行sql:`  :

Statement、PrepareStatement、CallableStatement

```
Statement：
new Connection(user,pwd).createStatement();//得到Statement对象
参数说明:
空：默认格式；
(int，int):光标移动方向和修改是否敏感、并发中可否修改；
(int，int,int):光标移动方向和修改是否敏感、并发中可否修改、修改提交时ResultSet是否关闭
```

![1](../1.png)

![createStatement](../createStatement.png)

```
PrepareStatement：
new Connection(user,pwd).prepareStatement(sql);//得到PrepareStatement对象

```

![prepareStatement](../prepareStatement.png)

```
CallableStatement：
继承于PreparedStatement
针对存储过程进行的优化，类似于PrepareStatement
```

![prepareCall](../prepareCall.png)

###### Statement总结：

官方注解

都是为了将sql语句发送到database数据库；

![state](../state.png)

Statement用来执行方法没有sql的语句。

PreparedStatement通常用来`相同sql的多次执行和预编译处理`；

![canshu](../canshu.png)

​       `sql有或者没有都将被预编译到PreparedStatement对象中`；若驱动支持预编则被发送到数据库，如果不支持则不会发送到数据库，直到PreparedStatement被执行。

![fin](../fin.png)

​    其默认的返回值`TYPE_FORWARD_ONLY，光标只能向前移动`、并发中，`CONCUR_READ_ONLY，结果集不会被更新`

CallableStatement

```
     继承于PreparedStatement
     针对`存储过程`进行的优化，类似于PrepareStatement
```

###### `4.结果处理：`

execute、executeQuery、executeUpdate、executeLargeUpdate

```
execute:
在Statement中，execute执行单一的sql并返回多个结果集和更新列，且必须是execute(sql);
在PreparedStatement中，execute执行单一或多个的sql并返回多个结果集和更新列;

返回值为：true表示第一个result是ResultSet对象；false则表示第一个是跟新数或者没有结果集
```

![execute](../execute.png)

getResultSet：判断结果是不是结果集ResultSet

。。。

getUpdateCount：

。。。

getMoreResults：

。。。

```
executeQuery:
```

```
executeUpdate:执行数据库的DDL语言或数据库的DML语言，且返回值为影响行数或0
```

![executeUpdate](../executeUpdate.png)

```
executeLargeUpdate:
```

###### `5.关闭连接：`

rs——>pst——>conn

```
finally{
  if(xxx != null){
  xxx.close();
  }
}
```

###### 类关系继承图：

![extends](../extends.png)

###### `代码示例：`

```
package com.my.all.kinds.jdbc;
import com.intersys.jdbc.CacheDataSource;
import java.sql.*;
public class CacheJdbc {
    public static void main(String[] args) throws SQLException {
        Connection conn = null;
        PreparedStatement pstmt = null;
        Statement st = null;

        try {
            Class.forName("com.intersys.jdbc.CacheDriver");
            CacheDataSource ds = new CacheDataSource();
            ds.setURL("jdbc:Cache://172.18.37.52:1972/abc");
            conn = ds.getConnection("root", "123456");
            System.out.println(conn);
            //可滚动的、数据变化敏感
            int scroll = ResultSet.TYPE_SCROLL_SENSITIVE;
            //并发模式，可被更新
            int update = ResultSet.CONCUR_UPDATABLE;

//*************************PrepareStatement*******************
            String sql1 = "insert into SQLUser.test(id,name) values(1,'prepareStatement') ";
            pstmt = conn.prepareStatement(sql1, scroll, update);
            int i = pstmt.executeUpdate();
            System.out.println(i);
            
//*************************Statement*******************
            String sql2 = "insert into SQLUser.test(id,name) values(2,'createStatement') ";
            st = conn.createStatement(scroll, update);
            int j = st.executeUpdate(sql2);
            System.out.println(j);

//*************************CallableStatement*******************

        } catch (Exception e) {
            System.out.println("error");
        } finally {
            if (st != null|pstmt!=null) {
                st.close();
                pstmt.close();
            }if(conn!=null){
                conn.close();
            }
        }
    }
}
```

#### 获取源数据：



#### 建表：



#### DDL:



#### DML：





