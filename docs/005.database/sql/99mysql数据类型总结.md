# mysql数据类型总结

分为三类number,string,date/time

## number

* 整数：tinyint（1）,smallint（2）,mediumint（3）,int（4）,bigint（8）
* 浮点数(不精确)  ：float（4）,double（8）/double precision（等价）为了兼容性使用float和double precision且不指定宽度
* 定点数（精确）：DECIMAL(5,2) 值的范围 -999.99 to 999.99 等同于NUMERIC  decimal(M) 等于decimal(M,0(D)) 其中M为长度，D为小数位数
* 布尔：不支持使用tinyint(1)来表示布尔值，0表示false，1表示true
* bit：bit(1-64) 赋值用 b'1010'这样的二进制字面值

## string(char/varchar/text的长度单位是字符,binary/varbinary/blob的长度单位是字节)

* 字符串：char（定长）/varchar（变长）例如varchar(4)可能长度为1-5字节（有一个是长度字段，超过255长度字段是两个字节）
* 二进制：binary（定长）/varbinary（变长）
* 长文本和二进制数据：text/blob（都有四种类型tiny,medium,long和自身）
* 特殊[REF](https://segmentfault.com/a/1190000019795336)
  * 枚举：[name] enum('a','b','c',etc......)  只能插入任意一个值
  * 集合：[name] set('a' 'b') 可以插入任意多个值 'a,b,b' 'a,b'等价

## date/time

* date:'0000-00-00' '1000-01-01' to '9999-12-31'
* time:'00:00:00'  '-838:59:59.000000' to '838:59:59.000000'
* datetime:'0000-00-00 00:00:00'  '1000-01-01 00:00:00.000000' to '9999-12-31 23:59:59.499999' 默认值为 NULL
* timestamp:'0000-00-00 00:00:00'   '1970-01-01 00:00:01.000000' UTC to '2038-01-19 03:14:07.499999' UTC 默认值为 0
* year: 4-digit strings in the range '1901' to '2155' or 4-digit numbers in the range 1901 to 2155.
