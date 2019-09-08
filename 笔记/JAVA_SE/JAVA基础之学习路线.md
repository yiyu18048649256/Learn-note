### JAVA_SE基础路线

n  对象导论：如何用面向对象的思路来开发

n  深入JVM：Java运行机制以及JVM原理

n  面向对象的特征：封装、继承、抽象、多态

n  数组和容器：容器的线程安全问题

n  I/O和NIO：NIO工作原理和应用

n  并发编程：concurrent包

n  网络编程：TCP/IP+BIO/NIO UDP/IP+BIO/NIO

n  JDBC框架和反射：JNDI、连接池、annotation等

n  其他：正则表达式、字符串等

      其实对于后续学习Java EE或者是Android来说，Java SE可能只要学会皮毛就足够应付了，当然这里的皮毛是要求你熟练Java语法结构和基本CRUD操作。

可是这样真的又够了么？好多工作了一两年的程序员肯定又会慢慢怀疑自己的基础是否足够扎实，于是他们又拿出了《Thinking In Java》重新通读一遍。基础的重要性毋庸置疑，
越到后面越会觉得框架什么的对自己的提升帮助很小。而真正有用的还是对Java的深入理解。在这一阶段，应该看看专门针对每一章节讲解的书，比如：《Java Concurrency in Practice》、
《Java NIO》、《深入Java虚拟机》等。资料很多，但是需要我细细的去琢磨。

第一阶段：

Java

核心部分

JavaSE 

      Java核心语法、Java核心API、面向对象程序设计、Java容器类(集合)、GUI 用户界面编程、I/O体系结构、多线程并发模型、网络编程、数据结构. 掌握Java核心语法与面向对象思想，
能熟练运用常用设计模式与编程技巧完成桌面应用或

网络通信类程序的开发. 

 ............................................................... 

 第二阶段：数据库编程

      Oracle/SQL语言以SQL为平台，介绍SQL数据库的安装、SQL体系结构、物理组件、权限分配、数据管理、分析各种关系数据库设计的常见问题，深入讲解数据库设计范式.

全面讲解各类SQL语句的使用和优化策略.深入学习SQL数据库对象:index(索引)、view(视图)、sequence(序列)、tirgger(触发器)、comment(注释). 理解SQL数据库体系结构,

掌握SQL数据库基本操作，数据库设计，开发和管理知识，熟练掌握SQL和Oracle对象使用. 

      PL/SQL 
    
      PL/SQL语法，作用.使用游标、存储过程、函数、触发器解决数据库性能问题. 掌握PL/SQL的使用,能够使用存储过程开发高效的数据处理系统,解决数据库性能瓶颈并实现数据优化

. MySql 以MySql为平台，介绍MySql数据库的安装、权限分配、数据管理.数据库使用. 掌握MySql数据库的使用

. JDBC 使用Java操作数据库,包括：数据库连接、结果集处理、存储过程调用、元数据、大数据类型处理、事务管理，批更新，可滚动、可更新的结果集，SQL3.0新特性，连接池技术，数据库应用架构. 

熟练掌握Java数据库编程技巧，能使用高级API、DAO编程模式编写高性能的数据持久层应用.



----------------------------------------------------
第一部分 Java语言基础

　说白了，Java语言的基础部分，其主要就指代JavaSE，这也是学习Java这么语言的核心部分，其主要包括异常、IO流、多线程、集合类等等。　

Java语言基础部分和面向对象思维，学习Java的第一点，其掌握的程度是将来去基础学习，以及运用Java开发等一系列的根本，所以在这两点是重中之重，面向对象，主要有封装，继承，多态这三部分组成，这三点也是以后做项目必不可少的知识点；
IO流和集合类（注：后面会详解），二者是走向JavaEE开发的一个起点，也是针对以后JavaEE开发的思维拓展的开始，其中IO流包括了各种输入输出流，在JavaSE学习阶段，掌握字节流和字符流，其包括InputStream、 OutputStream、FileInputStream、FileOutputStream、DataInputStream、 DataOutputStream、BufferedInputStream、BufferedOutputStream、Reader、Writer、 InputStreamReader、OutputStreamWriter、BufferReader、BufferedWriter、 ObjectInputStream、ObjectOutputStream，集合类分为单列集合和双列集合，主要掌握Set、Collection、Map、List、Iterator、Enumeration接口的使用和HashSet、ArrayList、Vector、HashMap、HashTable类的使用，以及Collections工具类的使用方法；
多线程，反射等，第一，掌握多线程实现的两种实现方法，分别是继承Thread类与实现Runnable接口，理解线程间的同步与互斥，第二，了解多线程在实际运用上的意义，反射就是通过字节码获取一个类（运行状态中）的各种信息。这两点是以后面对JavaWeb开发的出发点，以后项目开发很少用到，有封装好的现成可以用，所以在此学的是一个思维而已；
  第二部分 JavaWeb部分

Web前端
　　基于静态网络的开发，如html等标记语言来做一个本地的网页的开发，其主要掌握HTML的运用，来用xml解析，发送文件。 



