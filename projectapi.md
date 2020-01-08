## 接口管理
进入项目后，项目详情页面会展示当前项目的基本信息和项目下的接口列表，为创建接口时，和创建项目一样，可以点击页面中间的创建按钮和右上角的加号创建接口。
![接口列表](http://fastmock.cn-bj.ufileos.com/fastmock-apis.jpg)
点击“创建接口后会打开创建接口的弹出层，所有接口信息的录入都在这里完成”
![新增接口的弹出层](http://fastmock.cn-bj.ufileos.com/fastmock-api-save.jpg)
接口的录入分为两个区域。

##### 左边是接口的基本信息录入区域，字段说明如下

- 接口名：接口的名称(如：获取xxx列表)。这个名称可以填任意字符，但是建议跟接口文档中的接口名称保持一致，方便查找。
- 接口类型：对应XHR的请求类型，目前支持的有GET、POST、PUT、DELETE。
- 接口的url：重要，接口的XHR访问相对地址，以斜杠开头，(如: /api/getUserList)
- 接口其他描述信息： 接口的其他说明，如，接口的更改说明，接口的访问注意事项等等

##### 右边是接口的返回数据规则编辑器区域。

每个接口都预置了接口返回的基础结构，在编辑器中录入你想要在当前接口返回的json对象

~~~javascript
{
  "code": "0000",
  "data": {},
  "desc": "成功"
}
~~~
###### 基本结构
如果你对mock.js一无所知，又想快速使用fastmock，也没关系，在编辑器中录入你想要返回的json对象的完整内容就行。如下面的登录验证接口：
~~~javascript
{
  "code": "0000",
  "data": {
    "userInfo": {
      "username": "zhangsan",
      "userId": 1,
      "avator": "http://www.xxx.com/upload/xxx.png",
      "token": "e10adc3949ba59abbe56e057f20f883e"
    }
  },
  "desc": "成功"
}
~~~
又比如返回一个产品列表的接口
~~~javascript
{
  "code": "0000",
  "data": {
    "pageNo": 1,
    "totalRecord": 1000,
    "pageSize": 10,
    "list": [{
      "name": "iphone xs",
      "title": "产品a",
      "descript": "这个产品是干什么的",
      "price": 100
    }，{
      "name": "ipad mini4",
      "title": "产品b",
      "descript": "这个产品是干什么的",
      "price": 120
    }，{
      "name": "macbook pro",
      "title": "产品a",
      "descript": "这个产品是干什么的",
      "price": 10
    }]
  },
  "desc": "成功"
}
~~~

###### Mock.js语法
fastmock 引入了mock.js的支持，支持所有的Mock.js随机数据的生成规则。在这里介绍几个基本规则，更多Mock.js内容移步[mockjs文档](https://github.com/nuysoft/Mock/wiki)
- 基础随机内容的生成

~~~javascript
{
  "string|1-10": "=", // 随机生成1到10个等号
  "string2|3": "=", // 随机生成2个或者三个等号
  "number|+1": 0, // 从0开始自增
  "number2|1-00.1-3": 1, // 生成一个小数，小数点前面1到10，小数点后1到3位
  "boolean": "@boolean( 1, 2, true )", // 生成boolean值 三个参数，1表示第三个参数true出现的概率，2表示false出现的概率
  "name": "@cname", // 随机生成中文姓名
  "firstname": "@cfirst", // 随机生成中文姓
  "int": "@integer(1, 10)", // 随机生成1-10的整数
  "float": "@float(1,2,3,4)", // 随机生成浮点数，四个参数分别为，整数部分的最大最小值和小数部分的最大最小值
  "range": "@range(1,100,10)", // 随机生成整数数组，三个参数为，最大最小值和加的步长
  "natural": "@natural(60, 100)", // 随机生成自然数（大于零的数）
  "email": "@email", // 邮箱
  "ip": "@ip" ,// ip
  "datatime": "@date('yy-MM-dd hh:mm:ss')" // 随机生成指定格式的时间
  // ......
}
~~~
- 列表数据

~~~javascript
{
  "code": "0000",
  "data": {
    "pageNo": "@integer(1, 100)",
    "totalRecord": "@integer(100, 1000)",
    "pageSize": 10,
    "list|10": [{
      "id|+1": 1,
      "name": "@cword(10)",
      "title": "@cword(20)",
      "descript": "@csentence(20,50)",
      "price": "@float(10,100,10,100)"
    }]
  },
  "desc": "成功"
}
~~~
- 图片

mockjs可以生成任意大小，任意颜色块，且用文字填充内容的图片，使我们不用到处找图片资源就能轻松实现图片的模拟展示
~~~javascript
"code": "0000",
  "data": {
    "pageNo": "@integer(1, 100)",
    "totalRecord": "@integer(100, 1000)",
    "pageSize": 10,
    "list|10": [{
      // 参数从左到右依次为，图片尺寸，背景色，前景色（及文字颜色）,图片格式，图片中间的填充文字内容
      "image": "@image('200x100', '#ffcc33', '#FFF', 'png', 'Fast Mock')" 
    }]
  },
  "desc": "成功"
~~~

<strong>在编辑器中，我们预置了几乎所有的mockjs函数的代码提示，在编辑中输入"mj"会得到编辑器的代码提示，如下图</strong>
![mockjs代码提示](http://fastmock.cn-bj.ufileos.com/fastmock-code-snippet.jpg)

编辑完成后点击左边的提交按钮即可保存当前的所有编辑，如果你希望编辑器帮你美化一下你的代码，可以点击左边的“美化”按钮，我们会按照js代码格式去美化你的代码。

点击编辑器最右边的悬浮按钮可以设置适合自己习惯的编辑器的主题和代码风格，
![编辑器设置](http://fastmock.cn-bj.ufileos.com/fastmock-aditor-set.jpg)