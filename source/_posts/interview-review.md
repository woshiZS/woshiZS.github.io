---
title: 面试复盘(联合索引)
toc: true
mathjax: true
date: 2020-09-14 21:42:46
password:
summary:
tags:
    - mysql
    - leftmost prefix index
categories:
    - interview
---
### preface
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;今天一面感觉还算比较典型，面试官也没有为难的意思，问的题目大多都比较典型，主要类型是数据库和网络，然后问到了一个联合索引的问题。\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大致意思是在a,b,c三个列上建立联合索引，然后问
```sql
SELECT * FROM users 
WHERE a>A AND b>B; 
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这个查询语句是否会经过索引。
<!--more-->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;正确答案是会经过索引，这里所涉及到的一个知识点就是最左前缀匹配。
### 索引及其性能优化
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;基本好处：
* 首先就是会使查找变快，这种加速在进行多表连接的时候尤为明显。
* 索引会显著减少磁盘IO次数，并且如果搜查结果只包含索引列，那么查找甚至都不会检索数据行而是直接读取索引。**请注意这一点，如果你的所有数据列都建了索引，那没你的任何一次查找都会走索引，就不会读磁盘（即人们常说的回表）。**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;未完待续
