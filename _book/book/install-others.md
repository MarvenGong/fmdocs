## 其他问题

#### 1.修改端口
```javascript
修改 ecosystem.config.js

module.exports = {
  apps: [
    {
      // 生产环境
      name: "fastmockProd",
      // 项目启动入口文件
      script: "./bin/www",
      // 项目环境变量
      env: {
        "NODE_ENV": "production",
        "PORT": 80  // 在此修改端口即可
      },
      env_production : {
        NODE_ENV: 'production',  //使用production模式 pm2 start ecosystem.config.js --env production
      },
    },
]}
```
#### 2.使用云数据库TencentDB
```javascript
修改config/default.js

const Op = require('sequelize').Op;
module.exports = {
  'wwwBaseUrl': 'http://localhost:3000',
  'db': {
    'connectionLimit' : 10,
    'host'            : '改成外网访问地址',
    'user'            : '修改成你为fastmock创建的mysql用户',
    'database'        : '修改成你为fastmock创建的mysql数据库名'
  },
  'sequeliszeOptions': {
    'dialect': 'mysql',
    'host': '改成外网访问地址',
    'port': 62490,  // 改成外网访问端口
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
};

修改config/production.js

module.exports = {
  'wwwBaseUrl': 'http://localhost:3000',
  'enviroment': 'prod',
  'db': {
    'password'        : '改成default.js设置的mysql用户对应的密码',
    // 'socketPath'      : '/tmp/mysql.sock'  注释掉
  },
  'radis': {
    'host': 'localhost',
    'port': '6379',
    'ttl': 7200,
    'logErrors': false
  },
  'sequeliszeOptions': {
    'logging': false,
    // 'dialectOptions': {
    //   'socketPath': '/tmp/mysql.sock'   注释掉
    // }
  }
}
```