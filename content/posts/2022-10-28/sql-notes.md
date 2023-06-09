---
title: "sql 笔记"
date: 2022-10-28
type: "post"
tags: ["linux"]
showTableOfContents: true
---

# 数据库基础

## SQL语句分类

- DDL(Data Definition Language):DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。
- DML(Data Manipulation Language):DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。
- DQL(Data Query Language):DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。

# 数据查询

## 基本查询

``` sql
SELECT * 
FROM <表名>
```

## 条件查询

``` sql
SELECT * 
FROM <表名> 
WHERE <条件表达式>
```

## 投影查询

投影查询就是只需要显示部分的列

``` sql
SELECT 列1, 列2, 列3 ...
```

## 排序

``` sql
SELECT * 
FROM <表名>
ORDER BY <需要排序的值>
```
如果要反向排序需要 `` DESC ``

## 分页查询

``` sql
SELECT *
FROM <表名>
LIMIT <每页显示多少> OFFSET <从哪一页开始>
```

## 聚合查询

| 函数 | 说明                                   |
| ---- | -------------------------------------- |
| SUM  | 计算某一列的合计值，该列必须为数值类型 |
| AVG  | 计算某一列的平均值，该列必须为数值类型 |
| MAX  | 计算某一列的最大值                     |
| MIN  | 计算某一列的最小值                     |

分组聚合使用 `` GROUP BY ``子句

## 多表查询

笛卡尔乘积

``` sql
SELECT *
FROM 表1, 表2
```

## 连接查询

``` sql
SELECT *
FROM <表名>
INNER JOIN <连接的表>
ON <连接表达式>
```

- INNER JOIN    两张都在的记录
- LEFT JOIN     左表存在的记录
- RIGHT JOIN    右表存在的记录
- FULL OUTER JOIN 两张表都存在的记录

## 子查询

``` sql
-- SELECT中的子查询必须是一个只有一个列的结果
SELECT (
    SELECT COUNT(*)
    FROM <表名>
) as <重名>

-- FROM中的子查询可以作为一个子表存在也可以内联别的表
SELECT *
    FROM (
    SELECT uid
    FROM supplier
) as <重名>
```