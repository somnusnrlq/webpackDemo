
## 1.安装环境
```
npm init -y --一步到位配置
```

安装webpack
```
npm install webpack webpack-cli --save-dev

```
本项目中查看安装版本
```
./node_modules/.bin/webpack -v
```

## 2. 手写一个webpack配置 webpack.config.js
```
const path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode:'production'
}

```

## 3. 通过npm script 运行webpack

原理：模块局部安装会在 node_modules/.bin 目录创建软连接

package.json会默认读取到bin下的命令 ,所以下面script中会执行
```
{
  "name": "webpackDemo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build":"webpack",
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^5.38.1",
    "webpack-cli": "^4.7.2"
  }
}

```
