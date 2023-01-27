# 14 Java数据库编程

## 14.1 JDBC简介
* Java数据库连接技术（Java Database Connective）是由Java提供的一组与平台无关的数据库的操作标准，本身由一组类与接口组成，并且在操作中将按照严格的顺序执行。

## 14.2 连接Oracle数据库
要进行数据库的连接操作，就要使用java.sql中提供的程序类，主要提供了以下的核心类与接口：
* java.sql.DriverManager类：提供数据库的驱动管理，主要负责数据库的连接对象取得。
* java.sql.Connection接口：用于描述数据库的连接，并且可以通过此接口关闭连接。
* java.sql.Statement接口：数据库的操作接口，通过连接对象打开。
* java.sql.PreparedStatement接口：数据库预处理操作的接口，通过连接对象打开。
* java.sql.ResultSet接口：数据查询结果集描述，通过此接口取得查询结果。

