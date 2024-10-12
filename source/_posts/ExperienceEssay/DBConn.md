---
title: 常用数据库链接字符串
date: 2023-10-13 18:37
categories:
  - 经验随笔
tags:
  - DB
  - CONN
---

### 简介

常用数据库链接字符串

<!-- more -->

|数据库类型|连接方式|连接字符串|
|--------|--------|--------|
|Access|ODBC|"Driver={Microsoft Access Driver(*.mdb)}; Dbq=<\mdb file>; Uid=<\user name>; Pwd=<\password>;"|
|Access|OLEDB|"Provider=Microsoft.Jet.OLEDB4.0; Data Source=<\mdb file>; User Id=<\user name>; Password=<\password>"|
|SQL Server|ODBC|"Driver={SQL Server}; Server=<\server name>; Database=<\database name> Uid=<\user name>; Pwd=<\password>;"|
|SQL Server|OLEDB|"Provider=SQLOLEDB; Data Source=<\server name>; Initial Catalog=<\database name>; Integrated Security=SSPI; User |Id=<\user name>; Password=<\password>;"|
|MySql|ODBC|"Draver={mySQL}; Server=<\server name>; Option=16384; Database=<\database name>; Uid=<\user name>; Pwd=<\password>;"|
|MySql|OLEDB|"Provider=MySQLProv; Data Source=<\database name>; User Id=<\user name>; Password=<\password>;"|
|Oracle|ODBC|"Driver={Microsoft ODBC for Oracle}; Server=OracleServer.world; Uid=<\user name>; Pwd=<\password>;"|
|Oracle|OLEDB|"Provider=OraOLEDB.Oracle; Data Source=<\database name> User Id=<\user name>; Password=<\password>;"|
|SQLite|ODBC|Basic:"Data Source=Application.db;Cache=Shared"|
|SQLite|ODBC|加密:"Data Source=Encrypted.db;Password=MyEncryptionKey"|
|SQLite|ODBC|只读:"Data Source=Reference.db;Mode=ReadOnly"|
|SQLite|ODBC|内存中:"Data Source=:memory:"|
|SQLite|ODBC|可共享的内存:"Data Source=Sharable;Mode=Memory;Cache=Shared"|
