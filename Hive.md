# Hive

前期准备：

JDK安装、Hadoop安装

- [x] ## 内置derby版

1. 解压hive安装包
2. bin/hive   启动即可
3. 缺点：不同路径启动hive，每个hive拥有一套自己的元数据，无法共享

- [ ] ## mysql版 

1. 安装mysql

   1. yum在线安装

      > yum install mysql mysql-service mysql-devel
      >
      > /etc/init.d/mysqld start		#启动mysql
      >
      > > use mysql
      > >
      > > update user set password=password('123456') where user='root';
      > >
      > > flush  privileges
      >
      > > grant all privileges on  `*.*`  to 'root'@‘%’  identified  by  '密码'  with   grant  option;           #远程管理mysql
      > >
      > > flush  privileges
      >
      > mysql -u root -p
      >
      > 以下为格式模板：
      >
      > grant all privileges on  `*.*`  to '用户名'@‘%’  identified  by  '密码'  with   frant  option;           #远程管理mysql

      > `service  mysqld start`
      >
      > `chkconfig mysqld on`
      >
      > `chkconfig mysqld --list`

   2. 离线安装

2. 上传、解压、修改配置文件

3. 配置Hive

   1. 配置HIVE_HOME环境变量

      con/目录下

      > cp hive-env.sh.template	hive-env.sh
      >
      > vi conf/hive-env.sh
      >
      > 配置其中的hadoop_home
      >
      > ​	`export HADOOP_HOME=opt/hadoop-2.7.1`

   2. 配置元数据库信息

      1. conf/目录

      2. vi hive-site.xml

         ```xml
         <configuration>
             <property>
                 <name>javax.jdo.option.ConnectionURL</name>
                 <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
                 <description>JDBC connect string for a JDBC metastore</description>
             </property>
             <property>
                 <name>javax.jdo.option.ConnectionDriverName</name>
                 <value>com.mysql.jdbc.Driver</value>
                 <description>Driver class name for a JDBC metastore</description>
             </property >
             <property>
                 <name>javax.jdo.option.ConnectionUserName</name>
                 <value>root</value>
                 <description>username to use against metastore database</description>
             </property>
              <property>
                 <name>javax.jdo.option.ConnectionPassword</name>
                 <value>123456</value>
                 <description>password to use against metastore database</description>
             </property>
         </configuration>
         ```

         > 连接驱动：将连接驱动放入hive的lib目录里
         >
         > bin/hive		#启动测试

4. 配置MySQL元数据库信息

5. Hive几种使用方式

   1. hive交互shell

   2. hive jdbc服务    java  jdbc连接mysql

   3. hive 启动为一个服务器，来对外提供服务

      > bin/hiveserver2
      >
      > 启动成功后，别的节点用beeline去连接（scp文件到此机器）
      >
      > bin/beeline  -u jdbc:hive2://开启服务机器的ip:10000  -n  root
      >
      > 或者
      >
      > bin/beeline 
      >
      > !  connect   jdbc:hive2://开启服务机器的ip:10000

   4. hive命令

      > hive  -e   'sql'
      >
      > bin/hive  -e  'select * from t_test'