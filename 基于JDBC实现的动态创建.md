#### 基于JDBC实现的动态创建表

应用场景：解析html表时得到的字段值(key-value)、excel字段值(key-value)、或者一定格式的txt文件解析(key-value)、或者数据采集时，显示指定字段

##### 回顾：JDBC执行sql顺序



1. 加载驱动(基于反射)

   ```
   Class.forName("com.mysql.jdbc.Driver");
   ```

2. 获取连接

   ```
   Connection conn=DriverManager.getConnection(url,user,passwd);
   ```

3. 执行sql

   ```
   Statement st=conn.createStatemnet(sql);
   ```

4. 处理结果集

   ```

   ```

5. 关闭连接

   ```
   rs!=null,rs.close();
   st!=null,st.close();
   conn!=null,conn.close();
   ```

##### 实现动态创建表

基于方法参数的动态创建表

**方法一：StringBuffer的字符串拼接**

```
//StringBuffer字符拼接不占用新的堆空间
/**
*ArrayList数组长度可动态扩展
*通配符指明数组数据类型包括pojo、原生数据类型
*create table 需在字段列之前
*StringBuffer转String需使用toString方法
*@author param tableName
*@author param params,列名
*@return sb  返回sql
*/
public static String showSql(String tableName,ArrayList<Object>[] params){
    if(tableName!=null&&tableName.length()>0){
    StringBuffer sb=new StringBuffer("create table if not exists "+tableName);
    StringBuffer s=new StringBuffer("");
    for(int i=0;i<params.length;i++){
         s.append(" "+params[i]+" varchar(25)");   
    }
    sb.append(s.toString()+");");
    //执行sql
    return sb;
    }
    return "";
}
```

```
输出示例：


```

**方法二：创建表结构时的alter的动态 增加列(基于DDL)**

```
缺点：在实现时，即create table 时，必须同时创建一列字段，否则后续增加列操作无法进行
public static void createTalbe(String tableName,ArrayList<Object>[] params){
     StringBuffer sb=new StringBuffer("");
     StringBuffer addColumn=new StringBuffer("");
    if(tableName!=null&&tableName.length()>0){
      sb.append("create table if not exists " + tableName + " (" + params[0].toString() + "  varchar(50) "+");").toString();
          //执行sql(创建表并没插入数据)
     for(int i=0;i<params.length-1;i++){
         //继续定义表结构
         addColumn.append("alter table "+tableName+" add column "+params[i].toString()+" varchar(20);"+"\n").toString();
     }   
    }
}
```

##### 实现insert、update、delete方法封装

```
//JDBC实现insert、update、delete
/**
*@author param sql
*
*/
public static int executeAll(String sql, Object[] params) {
		try {
			conn = ds.getConnection();
			conn.setAutoCommit(false);
			ps = conn.prepareStatement(sql);
			for (int i = 0; params != null && i < params.length; i++) {
				ps.setObject(i + 1, params[i]);
			}
			int a = ps.executeUpdate();
			conn.commit();
			return a;
		} catch (Exception e) {
			try {
				conn.rollback();
			} catch (SQLException e1) {
				e1.printStackTrace();
			}
			e.printStackTrace();
		} finally {
			release(null, ps, conn);
		}
		return 0;
	}
```

##### 资源释放的封装：

```

```

##### 实现查询方法的封装：

```

```

