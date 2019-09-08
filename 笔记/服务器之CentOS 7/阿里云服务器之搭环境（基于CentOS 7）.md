## 阿里云服务器之搭环境（基于CentOS 7）

- ### JDK环境搭建

  通过yum安装Jdk （剧情需要，我装的jdk1.8版本）

  第一步：检查所有jdk

  ``` 
  yum list java-1.8* 
  ```

  第二步：安装jdk（中间有2次会让你选择 全输Y确定）

  ```
  yum install java-1.8.0-openjdk* -y
  ```

  最后安装完成记得检查

  ```
  java -version
  ```

  ![1542192001392](C:\Users\Administrator\Desktop\宿孟\笔记\CentOS 7\1-1.png)

- ### MySQL搭建

  通过yum安装MySQL 5.7

  第一步：下载mysql的yum源

  ```
   wget http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
  ```

  第二步：安装下载的5.7源

  ```
  yum localinstall mysql57-community-release-el7-11.noarch.rpm
  ```

  第三步：安装MySQL

  ```
  yum install mysql-community-server
  ```

  第四步：启动mysql

  ```
   systemctl start mysqld
  ```

  第五步：登录Mysql，这步复杂点，由于没有设置密码，就通过由系统生成的默认密码登录，然后登录后在修改。

  ```
  /* 查看默认密码 */ 
  grep 'temporary password' /var/log/mysqld.log
  update user set password=password("201314") where user="root";  
  /* 登录 */
  J,zguWDPk9g%

  mysql –u root –p默认密码
  update user set authentication_string=password("123456") where User='root';
  /* 修改密码   注意：大小写字母、数字和特殊符号，并且长度不能少于8位。 */  
  set password for 'root'@'localhost'= password('你所修改的密码');
   UPDATE user SET Password = '201314' WHERE User = 'root'; 
  set password for 'root'@'localhost'= password('201314');

  /* 如果不想把密码弄那么复杂，可以通过修改密码策略 */
  set global validate_password_policy=0;

  set global validate_password_mixed_case_count=0;

  set global validate_password_special_char_count=0;

  set global validate_password_length=6;

  /* 重启使之生效 */ 
  systemctl restart mysqld
  ```

  由于CentOS属于Linux，所以没有图形界面，通过命令来操作，剩下的就跟在windows 下的dos操作数据库一样了

- ### Tomcat搭建


  通过解压用户上传的tomcat压缩包得以配置Tomcat环境	 

  第一步：在自己电脑下载一个tomcat包（剧情需要：这里用的8.0.50的版本）

  ```
  这里给一个百度云链接: https://pan.baidu.com/s/1yKf0f_v074hcAQcy6cNf0g 提取码: syak 
  ```

  第二步：将自己下载的包通过ssh的软件上传到服务器的文件夹里，记住位置

  第三步：解压自己上传的文件夹

  ```
  /* 格式为：tar -zxvf 文件目录/文件名字(要带完整后缀)
  # tar -zxvf 文件目录/apache-tomcat-8.0.50.tar.gz
	tar -zxvf /root/apache-tomcat-8.0.50.tar.gz
  /* 解压完成后，找到解压好的文件夹，移动到usr/local/tomcat里 */
  # mv apache-tomcat-8.0.50 /usr/local/tomcat
  ```

  第四步：修改配置文件（tomcat安装后默认端口号为8080，想改成其他的自己百度修改）

  第五步：启动tomcat

  ```
  /* 首先进入bin文件夹 */
  cd /usr    //这里我也不知道为啥第一次进入要在usr前面加反斜杠，
  cd local/tomcat/bin   //这里的local不能加 加了就报错 想知道原因的自行百度

  /* 进入bin后，执行启动 */ 
  ./startup.sh
  ```

  第六步：开端口（由于防火墙的原因，所以需要把端口打开才能访问到）

  ```
  首先，去网页的阿里云里面，找到自己服务器，在安全组里面把端口加上去，就我自己，我加了3306跟8080，tcp协议。
  然后，在CentOS里面，添加8080,3306端口开放
  
  
  
rm -rm /usr/lib64/mysql
rm -rm /usr/share/mysql
rm -rm /etc/selinux/targeted/active/modules/100/mysql
rm -rm /var/lib/mysql
rm -rm /var/lib/mysql/mysql

  ```

  ```
  /* 输入命令查看防火墙的状态 */
  firewall-cmd --state

  /* 如果没有开，就输入命令打开（not running） */
  systemctl start firewalld.service

  /* 防火墙启动后，添加8080,3306端口开放（success） */
  firewall-cmd --zone=public --add-port=8080/tcp --permanent
  firewall-cmd --zone=public --add-port=3306/tcp --permanent

  /* 然后重启防火墙 */
  systemctl restart firewalld.service

  /* 使添加端口生效 */
  firewall-cmd --reload

  /* 查看是否开放成功 */ 
  firewall-cmd --list-ports
  ```

  ​