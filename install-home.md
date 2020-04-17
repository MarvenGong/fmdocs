## 本地部署fastmock
随着用户量的不断增加，您在fastmock在线平台的体验有可能会有所下降。不管是数据的返回速度还是接口的稳定性。我们也支持您和您的团队在您本地部署fastmock服务，您也可以进行二次开发来满足您的各种需求。

fastmock的架构很简单
- Node Express 作为后端服务器语言
- Mysql 数据库
- Redis 做session持久化
- Sequelize.js 实现ORM（对象关联模型，通过操作对象操作数据库，而不是手撸所有的sql）
- react 前端框架

因此，fastmock的部署也是非常简单的。

[开始部署fastmock](install-node.md)