# sql中的正则表达式（标准没有关于正则表达式的规定所以都是特定于数据库的）

## 标准sql

* geeksforgeeks<https://www.geeksforgeeks.org/sql/regular-expressions-in-sql/>
* 在 SQL 中，有三种主要函数用于处理正则表达式：
  * REGEXP_LIKE
  * REGEXP_REPLACE
  * REGEXP_SUBSTR

## mysql 中的正则表达式（后面四个函数是8.0添加的）

| 姓名 | 描述 |
| :--- | :-- |
| REGEXP | 字符串是否匹配正则表达式 |
| RLIKE | 字符串是否匹配正则表达式 |
| NOT REGEXP | REGEXP 的否定 |
| REGEXP_LIKE() | 字符串是否匹配正则表达式 |
| REGEXP_REPLACE() | 替换匹配正则表达式的子字符串 |
| REGEXP_SUBSTR() | 返回匹配正则表达式的子串 |
| REGEXP_INSTR() | 子串匹配正则表达式的起始索引 |
