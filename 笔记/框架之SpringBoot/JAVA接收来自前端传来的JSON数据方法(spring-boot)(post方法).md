### JAVA接收来自前端传来的JSON数据方法(spring-boot)(post方法)

- 添加json依赖

  ```
  <dependency>
              <groupId>com.alibaba</groupId>
              <artifactId>fastjson</artifactId>
              <!--<version></version>-->
  </dependency>
  ```

  ---

- controller里写方法，添加@RequestBody注解接收，并且前端传来的json数据是一组一组的，传来的是一个包含json的对象，将前端传来的对象转换为一个java对象，通关赋值，把java对象的值变成java对象

  ```
    @ResponseBody
      @RequestMapping(value = "/update", method = RequestMethod.POST)  //post方法
      public int updateFoodType(@RequestBody Param param){   //将前端json对象转换成java对象

          FoodType food = new FoodType();    

          food.setId(param.id);     //赋值
          food.setTypeName(param.name);   //赋值
          return foodTypeService.updateFoodType(food); 
      }
      
      class Param{
      public int id;
      public String name;
  }

  ```

  ​

