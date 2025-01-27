---
redirect_from: /
published: true
---
自建wiki， 整理并记录一些工作学习中遇到的知识点

# 基础知识补完计划

- [redis基本知识](https://gc1423.github.io/git-wiki-1/2022-06-26-redis)


# docker overlay2 清理
> 背景：公司服务器上磁盘报警， 排查发现是 /var/lib/docker/overlay2 文件夹占用磁盘过多
>> 查询相关发现： overlay2 是docker使用的一种文件存储驱动， 不能随便删除， 可用 docker system prune 命令清理不用的数据文件

>> 来源: https://stackoverflow.com/questions/46672001/is-it-safe-to-clean-docker-overlay2

Docker uses /var/lib/docker to store your images, containers, and local named volumes. Deleting this can result in data loss and possibly stop the engine from running. The overlay2 subdirectory specifically contains the various filesystem layers for images and containers.

To cleanup unused containers and images, see docker system prune. There are also options to remove volumes and even tagged images, but they aren't enabled by default due to the possibility of data loss:
```bash
$ docker system prune --help

Usage:  docker system prune [OPTIONS]

Remove unused data

Options:
  -a, --all             Remove all unused images not just dangling ones
      --filter filter   Provide filter values (e.g. 'label=<key>=<value>')
  -f, --force           Do not prompt for confirmation
      --volumes         Prune volumes
```
What a prune will never delete includes:
- running containers (list them with docker ps)
- logs on those containers (see this post for details on limiting the size of logs)
- filesystem changes made by those containers (visible with docker diff)

# http 302重定向问题

>302状态码 临时重定向, 浏览器处理方式如下
>>HTTP1.0阶段 ： 客户端发出POST请求后，收到服务端302状态码后，不能自动向新的URL发送重复请求，部分浏览器会跟客户确认是否重发，因为第二次POST时，是非幂等请求，可能再次发送POST请求不符合用户预期。也有部分直接选择以GET形式重发。()

>>HTTP1.1阶段：客户端发出非GET、HEAD请求后，收到服务端的302状态码，那么就不能自动的向新URI发送重复请求，除非得到用户的确认。但是，很多浏览器都把302当作303处理了（注意，303是HTTP1.1才加进来的，其实从HTTP1.0进化到HTTP1.1，浏览器什么都没动），它们获取到HTTP响应报文头部的Location字段信息，并发起一个GET请求。

python requests包处理采用了转为GET请求的方式，因此，如果参数allow_redirects设置为True然后POST一个api得到302请求时，会导致重定向之后的请求编程GET请求。
```python
    def rebuild_method(self, prepared_request, response):
        """When being redirected we may want to change the method of the request
        based on certain specs or browser behavior.
        """
        method = prepared_request.method

        # https://tools.ietf.org/html/rfc7231#section-6.4.4
        if response.status_code == codes.see_other and method != 'HEAD':
            method = 'GET'

        # Do what the browsers do, despite standards...
        # First, turn 302s into GETs.
        if response.status_code == codes.found and method != 'HEAD':
            method = 'GET'

        # Second, if a POST is responded to with a 301, turn it into a GET.
        # This bizarre behaviour is explained in Issue 1704.
        if response.status_code == codes.moved and method == 'POST':
            method = 'GET'

        prepared_request.method = method

```

保留POST请求的方式是allow_redirects设置为False， 手动处理跳转
```python
resp = requests.post(url, data=data, headers={'Content-Type': 'application/json'}, allow_redirects=False)
while resp.headers.get('location'):
    url = resp.headers['location']
    resp = requests.post(url, data=data, headers={'Content-Type': 'application/json'}, allow_redirects=False)
```


# curl命令重定向问题
```
curl -H "Content-Type:application/json" -X POST -d '{"token": "a1c73241d5d0e86d4aa20c2e6276bda8","timestamp": 1639376219,  "tp": 3,   "id": 12345}' http://api.baike.sogou.com/sg/push/tianyancha

```
添加-L参数允许curl重定向
```
curl -L -H "Content-Type:application/json" -X POST -d '{"token": "a1c73241d5d0e86d4aa20c2e6276bda8","timestamp": 1639376219,  "tp": 3,   "id": 12345}' http://api.baike.sogou.com/sg/push/tianyancha
```

# crontab编写格式

> 作者：十二楼中月
>> 链接：https://www.jianshu.com/p/f7e052e5c58a
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
