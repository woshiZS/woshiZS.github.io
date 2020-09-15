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

### 索引分类
主要分为四种：
* non unique
* unique
* Primary Key
* Fulltext Index(全文索引)

增加或者删除索引的语法这里就不说了，很简单的，可以自己去查，接下来重点说一下show index这个语句中每个字段的具体意思。
```sql
SHOW INDEX FROM CountryLanguage;
```
|     field     |                           meaning                            |
| :-----------: | :----------------------------------------------------------: |
|     Table     |                     represent table name                     |
|  Non-unique   |                if it is a unique index or not                |
|   Key name    |                     alias to this index                      |
| seq_in_index  |          联合索引的位置（如果是单个索引的话就是1）           |
|  Colun_name   |                     indexed column name                      |
|   collation   | 每个collation都归属于一个字符集，每个字符集合都至少有一个collation,collation的作用就是帮助数据库进行排序 |
|  cardinality  |                 unique value in this column                  |
|   sub_part    | 如果该列只有部分建立索引，则显示被索引部分的下表，全部都有索引的话则显示null |
|    Packed     |      Indiacate how the key is packed.Null if it is not       |
|     NULL      |  如果该列可能含有null值的话就显示yes，如果不含有的话就为空   |
|  Index_type   |                      index method used                       |
| Index_comment |          创建index的时候你如果加了注释就有这个字段           |
|    comment    |              额外的注释信息，例如disable,老实说这个没看懂2333  |
|    visible    |              if the optimizer can see the index              |

### Using Index
使用索引的时候有一些要注意的点：
* indexed column最好是非NULL的，检索时对NULL进行比较需要额外的判断
* 不要过度使用索引，首先添加索引是需要消耗空间的，并且如果索引太多的话，每次对表进行更新都需要花费大量的时间和资源去维护已有的索引。
* 老生常谈的，索引列的值要是大部分都是一致的，索引的效率就会接近于table scan
* 选择合适的索引类型，比如说某一列是enum类型的话，就最好选择non_unique索引类型
* 减少建立索引字段的长度，就比如说mysql会在内存里面缓存一些rows来避免过多的磁盘I/O，如果选择整个列的字段作为索引的话，所占空间相对来说就会变较大，所以我们可以选择部分字段来建立索引。
* 不要建立互有重叠的索引，比如说两个联合索引初始列是相同的，因为最左前缀匹配原则。

### Indexing column Prefixes
使用整个字段进行索引建立会有如下三个缺点：
* 需要从磁盘中读取更多的信息
* 较长的值需要花费较长的时间去比较
* index cache会因为字段太长而只能存储比较少的字段，导致优化效率变低。

所以我们可以使用如下语法来建立prefix索引
```sql
CREATE TABLE t(
    name char(255),
    INDEX (name(25))
);
```
但是这样又会引发一个问题，字段如果太短，会造成index字段差异不够大，就是说name前多少个字符相差都不大，这样的话就会降低索引的性能，所以我们应该在长度和uniqueness之间做一个折中。\
我们可以使用如下语句进行比较
```sql
SELECT

    COUNT(*) AS 'Total Rows',

    COUNT(DISTINCT name) AS 'Distinct Values',

    COUNT(*) - COUNT(DISTINCT name) AS 'Duplicate Values'

FROM t;

SELECT

    COUNT(DISTINCT LEFT(name,n)) AS 'Distinct Prefix Values',

    COUNT(*) - COUNT(DISTINCT LEFT(name,n)) AS 'Duplicate Prefix Values'

FROM t;
```

通过不断调整n的大小来对两组数据进行对比，来进行一个折中。
**还有一点需要注意的是，如果你对primary key或者unique key进行prefix indexing 的时候，要确保前缀也是不相同的，否则就要改变index的类型。**

### Leftmost Index Prefixes
最左前缀匹配可以减少重复的索引建立，比如说面试题中的a,b,c作为composite index，查询时仅使用a,b是使用到index的。
具体是否经过索引可以使用explain进行检测。

### Fulltext Indexes
注意事项：
* 全文索引目前只有MyISAM
* 数据类型只能是char,varchar或者text类型，不能是binary
* 全文索引是case insensitive的。
创建全文索引的语法
```sql
CREATE TABLE t (name CHAR(40), FULLTEXT (name));

ALTER TABLE t ADD FULLTEXT name_idx (name);

ALTER TABLE t DROP INDEX name_idx;

CREATE FULLTEXT INDEX name_idx ON t (name);

DROP INDEX name_idx ON t;

```

* 全文索引不支持前缀索引，如果使用前缀，mysql会忽略这个请求
* 因为全文索引不支持前缀索引，所以全文索引也不支持最左前缀匹配，要使用不同的组合的话，要分别建立索引

使用示例：
```sql
SELECT * FROM t WHERE MATCH(name) AGAINST('Wendell');
```
这条语句用来检索，name列中含有Wendell的字段。记住，要是对不同列或者组合进行全文索引查询，需要分别建立索引。

本文参考了[ Index Optimization and Index Usage](http://books.gigatux.nl/mirror/mysqlcertification/0672326329/ch13lev1sec1.html#:~:text=3.2%20Leftmost%20Index%20Prefixes,initial%20columns%20of%20the%20index.)以及mysql使用手册。