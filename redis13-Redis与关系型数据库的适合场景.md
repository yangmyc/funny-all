# 书签系统mysql数据库设计
```mysql
create table book (
bookid int,
title char(20)
)engine myisam charset utf8;

insert into book values 
(5 , 'PHP圣经'),
(6 , 'ruby实战'),
(7 , 'mysql运维')
(8, 'ruby服务端编程');

create table tags (
tid int,
bookid int,
content char(20)
)engine myisam charset utf8;

insert into tags values 
(10 , 5 , 'PHP'),
(11 , 5 , 'WEB'),
(12 , 6 , 'WEB'),
(13 , 6 , 'ruby'),
(14 , 7 , 'database'),
(15 , 8 , 'ruby'),
(16 , 8 , 'server');
```
> 既有web标签,又有PHP,同时还标签的书,要用连接查询

select * from tags inner join tags as t on tags.bookid=t.bookid
where tags.content='PHP' and t.content='WEB';

# 书签系统redis数据库设计
换成key-value存储，用kv 来存储。
```mysql
set book:5:title 'PHP圣经'
set book:6:title 'ruby实战'
set book:7:title 'mysql运难'
set book:8:title ‘ruby server’

sadd tag:PHP 5
sadd tag:WEB 5 6
sadd tag:database 7
sadd tag:ruby 6 8
sadd tag:SERVER 8
```
> 查: 既有PHP,又有WEB的书

Sinter tag:PHP tag:WEB  #查集合的交集

> 查: 有PHP或有WEB标签的书

Sunin tag:PHP tag:WEB

> 查:含有ruby,不含WEB标签的书

Sdiff tag:ruby tag:WEB #求差集

