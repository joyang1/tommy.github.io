---
layout:     post
title:      "JavaWeb 中 POJO、BO、VO、DO、DTO、DAO、PO 详细介绍"
subtitle:   " \"POJO、BO、VO、DO、DTO、DAO、PO\""
date:       2019-10-31 21:36:13
author:     "Tommy"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
    - MySQL
    - Interview
---

# MySQL 查询优化
## 优化之EXPLAIN
使用 EXPLAIN 可以帮助分析自己写的 SQL 语句，看看我们是否用到了索引。

### 具体用法

```
mysql> EXPLAIN SELECT * FROM tbl_name WHERE FALSE\G

执行结果：

id: 1
select_type: SIMPLE
table: tbl_name
partitions: NULL
type: ALL
possible_keys: NULL
key: NULL
key_len: NULL
ref: NULL
rows: 1693
filtered: 19.00
Extra: Using where

```

以上执行结果各行表示的含义：
- id: SELECT 查询的标识符，每个 SELECT 都会自动分配一个唯一的标识符
- select_type: SELECT 查询的类型
- table: 查询的是哪个表
- partitions: 匹配的分区
- type: join 类型
- possible_keys: 此次查询中可能选用的索引
- key: 此次查询中确切使用到的索引
- ref: 哪个字段或常数与 key 一起被使用
- rows: 显示此查询一共扫描了多少行，这个是一个估计值
- filtered: 表示此查询条件所过滤的数据的百分比
- extra: 额外的信息

### select_type
`select_type` 表示了查询的类型, 它的常用取值有:
- SIMPLE： 表示此查询不包含 UNION 查询或子查询
- PRIMARY： 表示此查询是最外层的查询
- UNION： 表示此查询是 UNION 的第二或随后的查询
- DEPENDENT UNION： UNION 中的第二个或后面的查询语句, 取决于外面的查询
- UNION RESULT： UNION 的结果
- SUBQUERY： 子查询中的第一个 SELECT
- DEPENDENT SUBQUERY： 子查询中的第一个 SELECT, 取决于外面的查询. 即子查询依赖于外层查询的结果

最常见的应该是 SIMPLE，当我们的查询 SQL 里面没有 UNION 查询或者子查询的时候，那么通常就是 SIMPLE 类型。

### type
`type` 字段比较重要，它提供了判断查询是否高效的重要依据。通过 type 字段，我们可以判断此次查询是**全表扫描**，还是**索引扫描**等。

`type` 常用取值有：
- system
    
    表中只有一条数据，这个类型是特殊的 const 类型。

- const
    
    针对主键或唯一索引的等值查询扫描，最多只返回一行数据，const 查询速度非常快，因为它仅仅读取一次即可。
    
- eq_ref
    
    此类型通常出现在多表的 join 查询，表示对于前表的每一个结果，都只能匹配到后表的一行结果，并且查询的比较操作通常是 =，查询效率较高。
    
- ref
    
    此类型通常出现在多表的 join 查询，针对于非唯一或非主键索引，或者是使用了**最左前缀**规则索引的查询。
    
- range

    表示使用索引范围查询，通过索引字段范围获取表中部分数据记录，这个类型通常出现在 =、 <>、 >、 >=、 <、 <=、 IS NULL、 <=>、 BETWEEN、 IN 操作中。
    当 type 是 range 时，那么 EXPLAIN 输出的 ref 字段为 NULL, 并且 key_len 字段是此次查询中使用到的索引的最长的那个。
    
- index
    
    表示全索引扫描(full index scan)，和 ALL 类型类似，只不过 ALL 类型是全表扫描，而 index 类型则仅仅扫描所有的索引，而不扫描数据。
    index 类型通常出现在：所要查询的数据直接在索引树中就可以获取到，而不需要扫描数据。当是这种情况时，Extra 字段 会显示 Using index。
    
- ALL

    表示全表扫描，这个类型的查询是性能最差的查询之一。通常来说，我们的查询不应该出现 ALL 类型的查询，因为这样的查询在数据量大的情况下，对数据库的性能是巨大的灾难。
    如一个查询是 ALL 类型查询，那么一般来说可以对相应的字段添加索引来避免。
    
### type 类型的性能比较
通常来说, 不同的 type 类型的性能关系如下:
`ALL < index < range ~ index_merge < ref < eq_ref < const < system`

ALL 类型因为是全表扫描，因此在相同的查询条件下，它是速度最慢的。
而 index 类型的查询虽然不是全表扫描，但是它扫描了所有的索引，因此比 ALL 类型的稍快。
后面的几种类型都是利用了索引来查询数据，因此可以过滤部分或大部分数据，因此查询效率就比较高了。

#### possible_keys
`possible_key` 表示 MySQL 在查询时，可能使用到的索引。即使有些索引出现在 possible_key 中，但是并不表示此索引一定会被 MySQL 使用到。MySQL 在查询时具体使用到那些索引，与 key 和你写的 SQL 有关。

### key
此字段表示 MySQL 在当前查询时所真正会使用到的索引。

### key_len
表示查询优化器使用了索引的字节数。这个字段可以评估组合索引是否完全被使用，或只有最左部分字段被使用到。
key_len 的计算规则如下：

- 字符串
    - char(n)：n 字节长度
    - varchar(n)：如果是 utf8 编码，则是 3n + 2字节；如果是 utf8mb4 编码，则是 4n + 2 字节。

- 数值类型
    - TINYINT： 1字节
    - SMALLINT： 2字节
    - MEDIUMINT: 3字节
    - INT: 4字节
    - BIGINT: 8字节

- 时间类型
    - DATE：3字节
    - TIMESTAMP：4字节
    - DATETIME：8字节

- 字段属性
    NULL 属性 占用一个字节。如果一个字段是 NOT NULL 的，则没有此属性。
    
### rows
rows 也是一个重要的字段。MySQL 查询优化器根据统计信息，估算 SQL 要查找到结果集需要扫描读取的数据行数。这个值非常直观显示 SQL 的效率好坏，原则上 rows 越少越好。

### Extra
EXPLAIN 中的很多额外的信息会在 Extra 字段显示，常见的有以下几种内容：
- Using filesort

    当 Extra 中有 Using filesort 时，表示 MySQL 需额外的排序操作，不能通过索引顺序达到排序效果。一般有 Using filesort，都建议优化去掉，因为这      样的查询 CPU 资源消耗大。

- Using index

    **覆盖索引扫描**，表示查询在索引树中就可查找所需数据，不用扫描表数据文件，往往说明性能不错。

- Using temporary

    查询有使用临时表，一般出现于排序，分组和多表 join 的情况，查询效率不高，建议优化。