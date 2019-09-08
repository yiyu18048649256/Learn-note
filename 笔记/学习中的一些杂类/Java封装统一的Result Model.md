# Java封装统一的Result Model(返回值模型)

- ### 为什么会需要这个东西

  ​	在开发过程中，有时候会需要使用错误码+错误信息的形式，来返回某些业务操作的错误结果信息，来代替效率较低的异常传递。
  	这样就需要封装一个统一的Result model作为返回值，代替直接返回数据等结果。

  ​

- #### 实现Result

  ​	这里定义的是service层的Result。有时候在controller层只会作一些简单的参数校验，在service层会作进一步的校验，
  	如果controller中需要统一返回一个JsonResult可以将ServiceResult作一个简单的转换器进行转换。

  ```
  	错误码可能是String、Integer、Long 等类型，也可能是enum类型。因此这里使用泛型来代替，错误码类型，可以提高灵活性。
  ```

  ```
  public class Result<T> {

      private T data;//    数据

      private Integer code;//    状态码

      private String message;//    状态信息

      public T getData() {
          return data;
      }

      public void setData(T data) {
          this.data = data;
      }

      public Integer getCode() {
          return code;
      }

      public void setCode(Integer code) {
          this.code = code;
      }

      public String getMessage() {
          return message;
      }

      public void setMessage(String message) {
          this.message = message;
      }

      public void setAllData(T data,Integer code,String message,Object... args){
          this.data=data;
          this.code=code;
          this.message=message;
      }

      public void setAllData(T data,Integer code,String message){
          setAllData(data,code,message,null);
      }
      public void setAllData(Integer code,String message){
          setAllData(null,code,message);
      }

      public void setOriginalData(Integer code,String message,T data){
          this.data=data;
          this.code=code;
          this.message=message;
      }

  }
  ```

  ​