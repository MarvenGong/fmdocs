## 安装fastmock

#### 1.下载源码
```shell
git clone https://github.com/MarvenGong/fastmock.git && cd fastmock
```
#### 2.安装依赖
```shell
npm i
```
#### 3.修改配置
###### 修改config/default.js
```javascript
const Op = require('sequelize').Op;
module.exports = {
  'wwwBaseUrl': 'http://localhost:3000',
  'db': {
    'connectionLimit' : 10,
    'host'            : '改成mysql的host',
    'user'            : '修改成你为fastmock创建的mysql用户',
    'database'        : '修改成你为fastmock创建的mysql数据库名'
  },
  'sequeliszeOptions': {
    'dialect': 'mysql',
    'host': '改成mysql的host',
    'port': 3306,  // 改成设置的mysql端口，默认3306
    ***
    'pool': {
      'max': 5,
      'min': 0,
      'acquire': 30000,
      'idle': 10000
    },
    'logging': console.log,
    'dialectOptions': {
      'supportBigNumbers': true,
      'bigNumberStrings': true,
      'charset': "utf8",
      'collate': "utf8_general_ci",
    }
  },
  'radis': null
```
###### 修改config/production.js
```javascript
module.exports = {
  'wwwBaseUrl': '改成要上线的域名+端口',
  // 例 'wwwBaseUrl': 'www.example.com',
  'enviroment': 'prod',
  'db': {
    'password'        : '改成default.js设置的mysql用户对应的密码',
    'socketPath'      : '改成mysql.sock的绝对路径'
  },
  'radis': {
    'host': '改成redis的host',
    'port': '改成你设置的redis端口 默认6379',
    'ttl': 7200,  // 缓存有效时间
    'logErrors': false
  },
  'sequeliszeOptions': {
    'logging': false,
    'dialectOptions': {
      'socketPath': '改成mysql.sock的绝对路径'
    }
  }
}
```
###### 修改entity/index.js
```javascript
const Sequelize = require('sequelize');
const config = require('config');
const dbConfig = config.get('db');
const sequelizeOptions = config.get('sequeliszeOptions');
let sequelizeInstance = new Sequelize(dbConfig.database, dbConfig.user, dbConfig.password, sequelizeOptions);
var fs        = require('fs');
var path      = require('path');
var basename  = path.basename(__filename);
let entities = {};
fs.readdirSync(__dirname).filter(file => {
    return (file.indexOf('.') !== 0) && (file !== basename) && (file.slice(-3) === '.js');
  }).forEach(file => {
    var model = sequelizeInstance['import'](path.join(__dirname, file));
    entities[model.name] = model;
  });

Object.keys(entities).forEach(modelName => {
  if (entities[modelName].associate) {
    entities[modelName].associate(entities);
  }
});

/*注意begin*/
sequelizeInstance.sync().then(() => {});   // 添加此行，在系统启动时会创建数据库表
/*注意end*/

entities.sequelizeInstance = sequelizeInstance;
module.exports = entities;
```
#### 4.安装pm2
```shell
npm i pm2 -D
```
#### 5.运行fastmock
```shell
npm run prod
```
#### 6.结束fastmock
```shell
npx pm2 stop id  // id是fastmock启动成功显示的id
```