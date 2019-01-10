## 接口的预览和使用
#### 接口预览
在项目详情页面，接口列表表格中，点击最右侧眼睛图标，即可预览接口
![接口预览](http://fastmock.ufile.ucloud.com.cn/fastmock-api-use.png)
fastmock使用了restc这个中间件实现了类似于postman的接口测试功能，凡是通过浏览器地址栏访问fastmock下的/mock/*规则的接口，浏览器都会打开restc的界面
> restc相关资料请移步 [restc中文文档](https://elemefe.github.io/restc/intro/)

![接口restc预览结果](http://fastmock.ufile.ucloud.com.cn/fastmock-restc.png)

#### 使用接口
复制图一中矩形框中项目根路径加接口中接口地址，及为接口的完整请求路径，如上图中的“登录验证”接口地址为
~~~html
http://129.204.116.48:3000/mock/9ef51f691a45733efc0e99f5eb58ecd5/first/login
~~~

将此地址粘贴到postman中请求结果为

![接口postman请求](http://fastmock.ufile.ucloud.com.cn/fastmock-postman.png)

