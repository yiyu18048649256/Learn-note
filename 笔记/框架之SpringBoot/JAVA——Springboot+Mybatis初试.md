## JAVA——Springboot+Mybatis初试

#### 1.Mybatis对数据库操作，在mapper里，不能传多个参数，但是可以传一组数据

```
*mapper
int updateFoodType(FoodType type);//更新菜系

*controller
 public int updateFoodType(@RequestBody Param param){

        FoodType food = new FoodType();

        food.setId(param.id);
        food.setTypeName(param.name);
        return foodTypeService.updateFoodType(food);
    }
```

#### 2.Mybatis对数据库操作,在映射文件中，查询语句需要有个返回参数，resultMap或者resultType

```
mybatis中使用resultMap完成高级输出结果映射。如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系
```

```
使用resultType进行输出映射，只有查询出来的列名和pojo（实体bean）中的属性名一致，该列才可以映射成功。
```

##### 总结：使用resultType进行输出映射，只有查询出来的列名和pojo中的属性名一致，该列才可以映射成功。如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系。

------

#### 3.在定义模型里的函数方法时，多个参数，可以定义类对象，如果单个参数可以直接定义。确定一个函数的作用，如果查询类，需要返回的方法，因为返回的是json数据，一组一组的，所以用数组或者List集合；如果不需要返回前端，直接对数据操作，可以直接用int类的基础数据类型。