---
title: mysql全文索引
date: 2017-09-18 17:25:02
tags: [mysql,数据库]
categories: 数据库
---
## 问题
在MySql中使用全文索引对于中文的支持不太理想,如果想使用支持中文内容的全文索引,不能直接在 <b>设计表</b> 页面 选择建立全文索引,因为默认的全文索引是以空格来分词的.
在MySql 5.7.6以后的版本中,可以使用插件ngram parser来解决这个问题.

## 新建全文索引
```sql
CREATE TABLE table(
   `id` int(11) DEFAULT NULL,
   `name` varchar(512) DEFAULT NULL,
   `content` text,
   FULLTEXT KEY idx_name(name),
   FULLTEXT KEY idx_content(content) WITH PARSER ngram
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
## 修改全文索引
```sql
ALTER TABLE `table` ADD FULLTEXT (`content`) WITH PARSER ngram;
```
### 参考资料: [2017MYSQL中文索引解决办法 自然语言处理(N-GRAM PARSER)](http://www.cnblogs.com/lgms2008/p/7196525.html)

