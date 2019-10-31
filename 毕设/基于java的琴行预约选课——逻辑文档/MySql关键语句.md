## MySql关键语句

### 1.日期date

```
SELECT DATE_ADD(date,INTERVAL 50 day)
FROM date
WHERE id =1;
作用：指定id的时间增加50天
```

### 2.特殊事件

```
replace into 跟 insert 功能类似，不同点在于：replace into 首先尝试插入数据到表中， 
	1. 如果发现表中已经有此行数据（根据主键或者唯一索引判断）则先删除此行数据，然后插入新的数据。 
	2. 否则，直接插入新数据。
要注意的是：插入数据的表必须有主键或者是唯一索引！否则的话，replace into 会直接插入数据，这将导致表中出现重复的数据。
```

### 3.拿到最新插入的数据（用于公告）

```
select * 
from  `user`  
order by ID desc limit 1
```

### 4.模糊搜索

```
SELECT * 
FROM book
WHERE 1 AND INSTR(BOOKNAME,'红');
```

### 5.内连接、左、右连接

### 6.更新会员积分

```
UPDATE t_user_information SET score = score+50 WHERE id =121;
```

