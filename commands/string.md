# string

redis中所有键都是字符串类型，其他几种数据类型都是在字符串类型基础上构建的；

值最大不能超过512MB；

## 内部编码
- int：8字节的长整型
- embstr：小于等于39字节的字符串
- raw：大于39字节的字符串

## [命令](http://redisdoc.com/string/index.html)
