
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