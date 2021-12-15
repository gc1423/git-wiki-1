---
redirect_from: /
published: true
---
自建wiki， 整理并记录一些工作学习中遇到的知识点

# crontab编写格式

> 作者：十二楼中月
> 链接：https://www.jianshu.com/p/f7e052e5c58a
> 来源：简书
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```
*　　*　　*　　*　　*　　command
前面的五个星号分别代表：M分 H时 D天 m月 d星期
M: 分钟（0-59）。
H：小时（0-23）。
D：天（1-31）。
m: 月（1-12）。
d: 一星期内的天（0~6，0为星期天）。
```

# awk split函数的使用方法
> The awk function split(s,a,sep) splits a string s into an awk array a using the delimiter sep

```
set time = 12:34:56
set hr = `echo $time | awk '{split($0,a,":" ); print a[1]}'` # = 12
set sec = `echo $time | awk '{split($0,a,":" ); print a[3]}'` # = 56
```

# mysql中各个字段类型在jdbc中对应编号

> 解析canal-kafka的json格式时遇到， kafka的消息中sqlTypes字段以编号的形式标注了字段类型
> 数据来自: https://blog.csdn.net/lxacdf/article/details/77579203

|mysql数据库字段类型|jdbc类型编号|
|  ----  | ----  |
|tinyint|-6|
|smallint|5|
|mediumint|4|
|int|4|
|integer|4|
|bigint|-5|
|bit|-7|
|real|8|
|double|8|
|float|7|
|numeric|3|
|decimal|3|
|char|1|
|varchar|12|
|date|91|
|time|92|
|year|91|
|timestamp|93|
|datetime|93|
|tinyblob|-2|
|blob|-4|
|mediumblob|-4|
|longblob|-4|
|tinytext|12|
|text|-1|
|mediumtext|-1|
|longtext|-1|
|enum|1|
|set|1|
|binary|-2|
|varbinary|-3|
|point|1111|
|linestring|1111|
|polygon|1111|
|geometry|-2|
|multipoint|1111|
|multilinestring|1111|
|multipolygon|1111|
|geometrycollection|1111|
