# 使用Servlet+JDBC 开发java web应用

一说到java web 应用的开发，很多人肯定想到的是用spring mvc 、Struts2  这些热门框架去开发，简单高效，只要配置好框架，剩下的工作就是体力活了。

但是很多人用惯了框架，一但开发中遇到问题，就束手无策，究其原理是根本不理解java web 最基础、最核心的东西。其实那些所谓的框架，其本质是在servlet和jdbc的

基础上扩展功能，封装常用的函数、以xml配置的方式提供给开发者使用。

 

言归正传，下面就介绍servlet和jdbc的使用：

 

在web2.5以前，servlet都是在web.xml中配置使用的，相信这个大家都很熟悉，如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<servlet>
    <servlet-name>testServlet</servlet-name>
    <servlet-class>com.qthh.web.servlet.TestServlet</servlet-class>
</servlet>
 
<servlet-mapping>
    <servlet-name>testServlet</servlet-name>
    <url-pattern>/test/list</url-pattern>
</servlet-mapping>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

web3.0后，servlet的使用更加方便，支持注解了，如下：

```
@WebServlet("/test/list")
public class TestServlet extends HttpServlet {
```

这样就不用再web.xml中配置那么一大堆了。

要开发java web 应用 ，首先你得搞清楚后台要做哪些事，也就是大家常说的分层。

最基本的分层就是：控制层+数据访问层  

扩展一点的话就是：控制层（servlet）+服务层（业务逻辑）+数据访问层（dao）+模型（java bean）

![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170309235123844-616816868.png)

知道了这些，我们就开始一层一层的去解决，首先从数据访问层开始。

 

 

 **一.数据访问层**

​       顾名思义，就是要操作数据库，既然要操作数据库，我们就要写个类去做这个事，封装常用的方法，JDBC为我们提供了操作数据库的方法，我们需要了解JDBC的使用方法

​       步骤如下：

​       1.加载驱动：我们熟知的Class.forName

​       2.驱动管理器建立连接：DriverManager.getConnection

​       3.创建Statement（或PreparedStatement）对象

​       4.执行操作：query或者update

​       为了方便大家，直接贴出封装部分代码： 

​      ![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170310002521734-1432421539.png)    

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public class DBHelper {
    
    private static final String DRIVENAME = "com.mysql.jdbc.Driver";
    private static final String URL = "jdbc:mysql://192.168.1.200:3306/web";
    private static final String USER = "root";
    private static final String PASSWORD = "123456";
    
    private Connection conn = null;
    private Statement st = null;
    private PreparedStatement ppst = null;
    private ResultSet rs = null;
    
    /**
     * 加载驱动
     */
    static{
        try {
            Class.forName(DRIVENAME).newInstance();
        } catch (Exception e) {
            System.out.println("驱动加载失败："+e.getMessage());
        }
    }
    

    /**
     * 连接数据库
     * @return
     */
    public Connection getConn(){
        try {
            conn =  DriverManager.getConnection(URL,USER,PASSWORD);
        } catch (SQLException e) {
            System.out.println("数据库连接失败："+e.getMessage());
        }
        return conn;
    }
    
    
    /**
     * 获取结果集（无参）
     * @param sql
     * @return
     */
    private ResultSet getRs(String sql){
        conn = this.getConn();
        try {        
            st = conn.createStatement();
            rs = st.executeQuery(sql);
        } catch (SQLException e) {
            System.out.println("查询（无参）出错:"+e.getMessage());
        }
        return rs;
    }
    
    
    /**
     * 获取结果集
     * @param sql
     * @param params
     * @return
     */
    private ResultSet getRs(String sql,Object[] params){
        conn = this.getConn();
        try {            
            ppst = conn.prepareStatement(sql);
            if(params!=null){
                for(int i = 0;i<params.length;i++){
                    ppst.setObject(i+1, params[i]);
                }
            }            
            rs = ppst.executeQuery();
        } catch (SQLException e) {
            System.out.println("查询出错："+e.getMessage());
        }
        
        return rs;
    }
    
        
    /**
     * 查询
     * @param sql
     * @param params
     * @return
     */
    public List<Object> query(String sql,Object[] params){
        
        List<Object> list = new ArrayList<Object>();
        ResultSet rs = null;
        if(params!=null){
            rs = getRs(sql, params);
        }else{
            rs = getRs(sql);
        }
        ResultSetMetaData rsmd = null;
        int columnCount = 0; 
            
        try {            
            rsmd = rs.getMetaData();  
            columnCount = rsmd.getColumnCount();            
            while(rs.next()){
                Map<String, Object> map = new HashMap<String, Object>();
                for(int i = 1;i<=columnCount;i++){
                    map.put(rsmd.getColumnLabel(i), rs.getObject(i));  
                }
                list.add(map);
            }
        } catch (SQLException e) {
            System.out.println("结果集解析出错："+e.getMessage());
        } finally {
            closeConn();
        }
        return list;
    }
    
    
    /**
     * 更新（无参）
     * @param sql
     */
    public int update(String sql){        
        int affectedLine = 0;//受影响的行数
        conn = this.getConn();
        try {        
            st = conn.createStatement();
            affectedLine = st.executeUpdate(sql);
        } catch (SQLException e) {
            System.out.println("更新（无参）失败："+e.getMessage());
        } finally {
            closeConn();
        }
        return affectedLine;
    }
    
    
    /**
     * 更新
     * @param sql
     * @param params
     * @return
     */
    public int update(String sql,Object[] params){
        int affectedLine = 0;//受影响的行数
        conn = this.getConn();
        try {
            ppst = conn.prepareStatement(sql);
            if(params!=null){
                for(int i = 0;i<params.length;i++){
                    ppst.setObject(i+1, params[i]);
                }
            }
            affectedLine = ppst.executeUpdate();
        } catch (SQLException e) {
            System.out.println("更新失败："+e.getMessage());
        } finally {
            closeConn();
        }
        return affectedLine;
    }
    
    
    private void closeConn(){
        
        if(rs!=null){
            try {
                rs.close();
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
        
        if(st!=null){
            try {
                st.close();
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
        
        if(ppst!=null){
            try {
                ppst.close();
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
        
        if(conn!=null){
            try {
                conn.close();
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            }
        }
    }

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

​      JDBC封装好了，接下来，就是在dao层中定义接口，然后去实现接口

​      ![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170310002448672-454500054.png)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public interface TestDao {
    
    List<Object> query(String sql,Object[] params);
    
    int update(String sql,Object[] params);

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public class TestDaoImpl implements TestDao {

    @Override
    public List<Object> query(String sql,Object[] params) {
        DBHelper db = new DBHelper();
        return db.query(sql, params);
    }

    @Override
    public int update(String sql,Object[] params) {
        DBHelper db = new DBHelper();
        return db.update(sql, params);
    }

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

以上就是一个很简单的dao层示例。实际开发中，会对常用的方法进行封装成baseDao,这里不做介绍。

 

**二.服务层**

​     服务层也叫业务层，编写业务逻辑的地方；关联控制层和dao层，负责从dao层取数据，然后返回给控制层。实现方式和dao层类似，先定义接口，然后去实现接口。

​     （这里肯定很多人有疑问，为啥非要定义接口，直接写个类不就行了么。当然你这么做也是能实现功能，但是从设计规范和安全的角度的来讲是不好的，以后也不方便维护。）

​     ![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170310002405734-1065230121.png)    

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public interface TestService {
    
    List<Object> getTestList();//获取列表
    
    int insertTest(Object[] params);//插入一条
    
    int modifyTest(Object[] params);//修改
    
    int deleteTest(Object[] params);//删除

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public class TestServiceImpl implements TestService {
    
    TestDao testDao = new TestDaoImpl();

    @Override
    public List<Object> getTestList() {
        String sql = "select * from test";
        return testDao.query(sql, null);
    }

    @Override
    public int insertTest(Object[] params) {
        String sql ="insert into test(name) values(?)";
        return testDao.update(sql, params);
    }

    @Override
    public int modifyTest(Object[] params) {
        String sql ="update test set name = ? where id = ?";
        return testDao.update(sql, params);
    }

    @Override
    public int deleteTest(Object[] params) {
        String sql ="delete from test where id = ?";
        return testDao.update(sql, params);
    }

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

**三.控制层**

​     这一层是我们最熟悉的地方，创建一个servlet，文章一开始我们也提到了，至于他的作用，就不用我多说了，直接贴代码

​     ![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170310003405719-1894122739.png)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
@WebServlet("/test/list")
public class TestServlet extends HttpServlet {
    
    private static final long serialVersionUID = 1L;
       
    TestService testService = new TestServiceImpl();
    
    
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json; charset=utf-8");
        List<Object> list = testService.getTestList();
        PrintWriter out = null;
        System.out.println("list:"+list.toString());
        try {
            out = response.getWriter();
            out.write(list.toString());
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        out.flush();
        out.close();
    }

    
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

​       因为是例子，所以写的很简单。关于servlet如何返回不同的数据格式，比如json，将在后面的篇章中介绍。

 

 

 **四. 模型** 

​       其实就是一个javabean，用来被填充数据的。如果我们做了orm（对象关系映射）的话，就不必操作数据库，直接操作这个对象，也就是这里的模型。

​       本案例没用到，就不说了。

 

 

​      麻雀虽小五脏俱全，到这里，web的整个后台已经完成了，接下来要做的就是发布到tomcat上测试一下。

​      ![img](https://images2015.cnblogs.com/blog/1119110/201703/1119110-20170310005051719-1945357419.png)

​      ok，正常运行。