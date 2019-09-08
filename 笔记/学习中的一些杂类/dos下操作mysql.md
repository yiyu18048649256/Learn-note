#### 1.打开数据库

cd C:\Program Files\MySQL\MySQL Server 5.7

mysql -uroot -p

#### 2.查看数据库（表）

show databases; 

show tables;

#### 3.创建数据库

create database 数据库名

#### 4.创建表

```
create table 表名{
    id int,
    name varchar(20),
    gender varchar(4),
    job varchar(40),
}character set utf8 collate utf8_general_ci;
```

#### 5.删除数据库（表）

drop database 数据库名

drop table 表名

#### 6.添加表数据

insert into 数据表名 values(num0,num1,num2,…);

#### 7.批量从本地文本插入表

 load data local infile "E:/data.txt" into table countrylanguage fields terminated by ',';

load data local infile  "txt文件根目录" into table 表名 fileds terminated by ',';

#### 8.查找数据

```
select * from 表名
where 条件
```

#### 9.退出sql环境

exit；