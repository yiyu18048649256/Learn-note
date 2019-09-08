### JAVA——注解

对各种常用注解的含义进行记录、

#### JDK自带注解

- @Override：向编译器说明被注解元素是重写的父类的一个元素。在重写父类元素的时候此注解并非强制性的，不过可以在重写错误时帮助编译器产生错误以提醒我们。比如子类方法的参数和父类不匹配，或返回值类型不同。

#### Spring

- @Autowired 自动装配
- @Service 用于标注业务层组件
- @Repository 用于标注数据访问组件，即DAO组件
- @Controller 用于标注控制层组件（如struts中的action）
- @Component 泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

#### Mybatis(还没接触，暂时看不懂..)

- @SelectProvider(type = TestSqlProvider.class, method = "getSql") ： 提供查询的SQL语句，如果你不用这个注解，你也可以直接使用@Select("select * from ....")注解，把查询SQL抽取到一个类里面，方便管理，同时复杂的SQL也容易操作，type = TestSqlProvider.class就是存放SQL语句的类，而method = "getSql"表示get接口方法需要到TestSqlProvider类的getSql方法中获取SQL语句。
- @InsertProvider(type = TestSqlProvider.class, method = "insertSql") ：用法和含义@SelectProvider一样，只不过是用来插入数据库而用的。
- @UpdateProvider(type = TestSqlProvider.class, method = "updateSql") ：用法和含义@SelectProvider一样，只不过是用来更新数据库而用的。
- @DeleteProvider(type = TestSqlProvider.class, method = "deleteSql") ：用法和含义@SelectProvider一样，只不过是用来删除数据而用的。
- @Options(useCache = true, flushCache = false, timeout = 10000) ： 一些查询的选项开关，比如useCache = true表示本次查询结果被缓存以提高下次查询速度，flushCache = false表示下次查询时不刷新缓存，timeout = 10000表示查询结果缓存10000秒。