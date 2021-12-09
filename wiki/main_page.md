---
redirect_from: /
published: true
---
自建wiki， 整理并记录一些工作学习中遇到的知识点

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