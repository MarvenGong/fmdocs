## 常用返回规则示例
- 基本数据(固定json结构)

~~~javascript
{
  "code": "0000",
  "data": {
    "name": "张三",
    "age": 12
  },
  "desc": "成功"
}
~~~
- 基本数据(Mock随机json结构)

~~~javascript
{
  "code": "0000",
  "data": {
    "list|20": [{
      "name": "@name",
      "age": "@integer(20)"
    }],
    "url": "'11111'"
  },
  "desc": "成功"
}
~~~
- RESTFUL逻辑数据

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