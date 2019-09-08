## Session初试

- #### session定义

  - Session，有始有终的一系列动作/消息的意思，其实我们举一个小例子来说:比如我们一次打电话，从接起电话到你挂断电话的一系列过程可以称为一个Session。Session在Web开发环境下的语义又有了新的扩展，它的含义是指一类用来在客户端与服务器端之间保持状态的解决方案。有时候Session也用来指这种解决方案的存储结构。

- #### session标识传递方式

  - 当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否包含了一个session标识(即sessionId),如果已经包含一个sessionId则说明以前已经为此客户创建过session，服务器就按照session id把这个session检索出来使用(如果检索不到，可能会新建一个，这种情况可能出现在服务端已经删除了该用户对应的session对象，但用户人为地在请求的URL后面附加上一个JSESSION的参数)。如果客户请求不包含sessionId，则为此客户创建一个session并且生成一个与此session相关联的sessionId，这个sessionId将在本次响应中返回给客户端保存。

- #### session的存储方式

  - 使用Cookie: 保存session id的方式可以采用cookie，这样在交互过程中浏览器可以自动的按照规则把这个标识发送给服务器，这种cookie称为session cookie
  - URL重写: 由于cookie可以被人为的禁用，必须有其它的机制以便在cookie被禁用时仍然能够把session id传递回服务器，经常采用的一种技术叫做URL重写，就是把session id附加在URL路径的后面，附加的方式也有两种，一种是作为URL路径的附加信息，另一种是作为查询字符串附加在URL后面。网络在整个交互过程中始终保持状态，就必须在每个客户端可能请求的路径后面都包含这个session id。

- #### session的创建跟删除

  - Session创建
    (1). 对于Jsp: 若当前页面为浏览器（客户端）访问web应用的第一个资源页面且Jsp的Page指定的Session属性的值为true。
    (2). 对于Servlet: 若当前Servlet为浏览器（客户端）访问web应用的第一个资源时，使用request.getSession()或request.getSession(true)创建。
    Session删除
  - Session删除
    (1). 调用session.invalidate()方法
    (2). 卸载web应用程序
    (3). 超过了HttpSession的过期时间