## 把本地的MySQL文件上传的服务器里

- #### 把本地的数据库转储为sql文件保存

- #### 用ssh工具把文件上传到服务器

- #### 登录服务器的mysql

- #### 创建一个同名的数据库

- #### use 数据库  （set names utf8; //设置格式为utf-8）

- #### source /root/program.sql;    // source + sql文件目录

- #### 然后就ok了

- #### 中文乱码的话，输入指令 ：set character_set_results=gb2312;