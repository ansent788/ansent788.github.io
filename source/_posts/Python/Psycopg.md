---
title: Psycopg
date: 2021-12-30 10:28:31
categories:
  - 经验随笔
tags:
  - Python
  - PostgreSQL
  - Psycopg
---

## 简介

Psycopg 3是一个新设计的PostgreSQL数据库适配器，用于Python编程语言。

<!-- more -->

## 安装

> pip > 20.3
> python > 3.6
> PostgreSQL > 10

```console
pip install psycopg
pip install psycopg_binary
```

## 基本用法

```python
import psycopg

conn = psycopg.connect("dbname=<dbname> user=<username> password=<password>")
try:

    with conn.cursor() as cur:
        for record in cur.execute("SELECT * FROM test", binary=True):
            print(record)
        for record in cur.execute("SELECT now()", binary=True).fetchone():
            print(record)

    with conn.transaction():
        for record in conn.execute("SELECT * FROM test", binary=True):
            print(record)
        for record in conn.execute("SELECT now()", binary=True).fetchone():
            print(record)

    print(conn.execute("SELECT (%s %% 2) = 1 AS even", (10,), binary=True).fetchone())

except BaseException:
    conn.rollback()
else:
    conn.commit()
finally:
    conn.close()
```
