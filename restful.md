## 根据入参数据动态返回mock内容
某些场景中，我们可能需要根据接口的入参规则，加入适当的逻辑处理后再返回数据。一个简单的场景就是登录场景，需要根据用户名密码，判断是否登录成功。再或者，我们需要根据产品ID动态返回产品信息，等等。现在fastmock提供了这种场景的解决方案，下图中展示了如何如果在mock规则中获取请求中的各个部分的数据然后再返回，其中包括了四种数据。
1. restful链接参数，如/user/:id 当请求/user/1时 对应数据为{id: 1}。获取方式为_req.params.id
2. query查询参数，如/user?id=1 获取方式为_req.query.id
3. body请求体数据，在请求的request body中 获取方式为_req.body.id
4. headers 头部信息，常用的场景是接口的token验证 获取方式为_req.headers.token
![根据入参数据动态返回mock内容](http://fastmock.cn-bj.ufileos.com/fastmock-restful.png)

> 注意：在您的规则中配置好了对应请求方式的参数获取，那你在访问的时候一定要以对应的请求入参方式传入请求数据，要不然fastmock在解析规则的时候会抛出错误

### 使用方法

- 在原来的json数据的基础上，需要动态返回的字段对应的值不再是固定值或者固定的mock规则，而是传入一个函数。
- 这个函数接收两个参数，_req和Mock  注意：这两个变量名不能改动
- 在函数体中返回该字段对应的值，在返回之前做相应的逻辑处理
- _req参数中包含了四个对象，_req.query ,  _req.params ,  _req.body  ,  _req.headers可以从这四个对象中获取上述的四种数据。
- Mock对象就是mock.js 原生对象，可以用它做mock.js中Mock对象可以做的事情，如Mock.mock({name: '@cname'})等等
如：上图中的对应接口录入规则为

~~~javascript
{
  "code": "0000",
  "data": {
    "token": function({_req, Mock}) {
      return _req.headers.token;
    },
    "id": function({_req, Mock}) {
      return _req.params.id;
    },
    "name": function({_req, Mock}) {
      return _req.body.name;
    },
    "age": function({_req, Mock}) {
      return _req.query.age;
    }
    
  },
  "desc": "成功"
}
~~~

再举一个验证登录信息的例子：

~~~javascript
{
  "code": "0000",
  "data": {
    "verifySuccess": function({_req, Mock}) {
      let body = _req.body;
      return body.username === 'admin' && body.password === '123456';
    },
    "userInfo": function({_req, Mock}) {
      let body = _req.body;
      if (body.username === 'admin' && body.password === '123456') {
        return Mock.mock({
          username: "admin",
          email: "@email",
          address: "@address"
        });
      } else {
        return null;
      }
    },
  },
  "desc": "成功"
}
~~~

上面的规则中定义了登录接口只有请求体{username: 'admin', password: '123456'}时，才会返回用户信息，且带有mock生成的随机邮箱地址和居住地址
