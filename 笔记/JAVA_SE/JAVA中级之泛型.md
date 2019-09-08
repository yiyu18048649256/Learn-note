## JAVA中级之泛型

- #### 什么是泛型

  ​	泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。Java语言引入泛型的好处是安全简单。

- #### 泛型的作用

  ​	在编写程序时，经常遇到两个模块的功能非常相似，只是一个是处理int数据，另一个是处理string数据，或者其他自定义的数据类型，但我们没有办法，只能分别写多个方法处理每个数据类型，因为方法的参数类型不同。有没有一种办法，在方法中传入通用的数据类型，这样不就可以合并代码了吗？泛型的出现就是专门解决这个问题的。

- #### 泛型的用法

  泛型的用法是在容器后面添加**(<Type>)大叔大婶**
  Type可以是类，抽象类，接口

  - ##### 未使用泛型：

  ```
  //ADHero（物理攻击英雄） APHero（魔法攻击英雄）都是Hero的子类
  //ArrayList 默认接受Object类型的对象，所以所有对象都可以放进ArrayList中
  //所以get(0) 返回的类型是Object
  //接着，需要进行强制转换才可以得到APHero类型或者ADHero类型。
  //如果软件开发人员记忆比较好，能记得哪个是哪个，还是可以的。 但是开发人员会犯错误，比如第20行，
  //会记错，把第0个对象转换为ADHero,这样就会出现类型转换异常

  public class TestGeneric {
   
      public static void main(String[] args) {
           
          ArrayList heros = new ArrayList();
           
          heros.add(new APHero());
          heros.add(new ADHero());
           
          APHero apHero =  (APHero) heros.get(0);
          ADHero adHero =  (ADHero) heros.get(1);
           
          ADHero adHero2 =  (ADHero) heros.get(0);
      }
  }
  ```
   - ##### 使用了泛型：
  ```
  泛型的用法是在容器后面添加<Type>
  Type可以是类，抽象类，接口
  泛型表示这种容器，只能存放APHero，ADHero就放不进去了。

  public class TestGeneric {
   
      public static void main(String[] args) {
          ArrayList<APHero> heros = new ArrayList<APHero>();
           
          //只有APHero可以放进去    
          heros.add(new APHero());
           
          //ADHero甚至放不进去
          //heros.add(new ADHero());
           
          //获取的时候也不需要进行转型，因为取出来一定是APHero
          APHero apHero =  heros.get(0);
           
      }
  }
  ```
  - ##### 所以当规定了类型（type）后，就只能放入指定的类型，其他类型都不能放

- ##### 泛型的子类泛型可以转换成父类，但是父类不能转换成子类，因为父类里面的东西类型不一定全是子类泛型的type

  - 子类转换成父类

    ```
    public class TestGeneric {
     
        public static void main(String[] args) {
            ArrayList<Hero> hs =new ArrayList<>();
            ArrayList<ADHero> adhs =new ArrayList<>();
     
            //子类泛型转父类泛型
            hs = adhs;
        }
    }
    ```
   - 父类转换成子类

     ```
     public class TestGeneric {
       
         public static void main(String[] args) {
             ArrayList<Hero> hs =new ArrayList<>();
             ArrayList<ADHero> adhs =new ArrayList<>();
       
             //父类泛型转子类泛型，能否成功？为什么？
             adhs = hs;
              
         }
       
     } 

     //编译出错，因为类型不确定
     ```

