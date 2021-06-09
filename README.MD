webpack知识点总结

## 1.安装环境 

下载并安装nodejs，如果需要nodejs版本管理则需要安装nvm进行控制
安装检查

```
node -v  查看node安装版本
npm -v   查看npm安装版本(安装node自带)
nvm -list 查看安装的各种版本nodejs
```
### 项目初始化

```
npm init -y --一步到位配置
```

安装 webpack

```
npm install webpack webpack-cli --save-dev

```

本项目中查看安装版本

```
./node_modules/.bin/webpack -v
```

## 2. 手写一个 webpack 配置 webpack.config.js

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

## 3. 通过 npm script 运行 webpack

原理：模块局部安装会在 node_modules/.bin 目录创建软连接

package.json 会默认读取到 bin 下的命令 ,所以下面 script 中会执行

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

## 4. webpack 基础用法

1.  了解 entry、output 的单文件和多文件用法

2.  核心概念 loaders
    webpack 开箱即用只支持 js 和 json 两种文件，通过 loaders 来支持其他文件类型的模块转化
    loaders 本身是一个函数，接收源文件作为参数，返回转化的结果

    ```
    module:{
      rules:[
      {test:/\.txt$/,use:'xxx-loader'}
    ]}
    ```

常见的 Loaders 有哪些?

```
  babel-loader转换ES6、ES7等JS新特性语法

  css-loader 支持.css 文件的加载和解析

  less-loader 将 less 文件转换成 css

  ts-loader 将 TS 转换成 JS

  file-loader 进行图片、字体等的打包

  raw-loader 将文件以字符串的形式导入

  thread-loader 多进程打包 JS 和 cSs

```

3.  核心概念 plugins --增强 webpack 功能
    用于 bundle 打包文件的优化，资源管理和环境变量注入，作用于整个构建过程

```
plugins:[
    new HtmlWebpackPlugin(....)
]
```

常见的 Plugins 有哪些?

```
  CommonsChunkPlugin    将 chunks 相同的模块代码提取成公共 js

  CleanWebpackPlugin    清理构建目录

  ExtractTextWebpackPlugin 将css从 bunlde 文件里提取成一个个个独立的css文件

  CopyWebpackPlugin    将文件或者文件夹拷贝到构建的输出目录

  HtmlWebpackPlugin    创建 html 文件去承载输出的 bundle

  uglifyjsWebpackPlugin    压缩 JS

  zipWebpackPlugin    将打包出的资源生成一个 zip 包
```

4.  核心概念 mode --指定当前构建环境 production、development、none 默认 pro

内置函数功能

development 设置 process.env . NODE_ENV 的值为 development .开启 NamedchunksPlugin 和 NamedModulesPlugin

production 设置 process.env.NODE_ENV 的值为 production .开启 FlagDependencyUsagePlugin , FlagIncludedChunksPlugin ,ModuleconcatenationPlugin , NoEmitOnErrorsPlugin ,occurrenceOrderPlugin , sideEffectsFlagPlugin 和 TerserPlugin .

none 不开启任何优化选项

## 5. 资源解析

### 1. 解析 es6 使用 babel-loader babel 的配置文件是 .babelrc

.babelrc 代码说明如下:

```
    // presets 是一系列babel plugins的配置集合  一个plugins对应一个功能
    {
    "presets": [
        '@babel/preset-env'  --增加es6的babel preset配置
    ],
    "plugins":[
        "@babel/proposal-class-properties"
    ]
    }

```

那么我们解析 es6 只安装配置 @babel/preset-env 就可以了 ,代码如下

```
npm i @babel/core @babel/preset-env' babel-loader -D
```

webpack.config.js 中添加 js 规则解析配置
```
  module:{
    rules:[
        {test:/.js$/,use:'babel-loader'}
    ]
  }

```

### 2. 解析其他资源 比如 vue

```

{
  presets: [
  '@vue/app',
  '@vue/cli-plugin-babel/preset' --这个是脚手架最新的
  ]
}
```

### 3. 解析样式 css、less

1. 安装 css-loader、style-loader

```
npm i style-loader css-loader -D
```

css-loader 用于加载.css 文件，并转化成 commonjs 对象
style-loader 将样式通过 <style> 标签 插入到 head 中

webpack.config.js 中添加 css 规则解析配置

```
module:{
    rules:[
        {test:/\.css$/,use:['style-loader','css-loader']}
    ]
  }
```

2. 安装 less-loader 将 less 解析成 css

```
npm i less less-loader -D
```

webpack.config.js 中添加 less 规则解析配置

```
module:{
    rules:[
        {test:/\.less$/,use:['style-loader','css-loader','less-loader']}
    ]
  }
```

### 4. 解析图片、字体

1.安装 file-loader 来解析文件

```
  npm i file-loader -D
```

webpack.config.js 中添加 图片 规则解析配置

```
module:{
    rules:[
        {test:/\.(png|svg|jpg|gif)$/,use:'file-loader'}
    ]
  }
```
也可以解析字体
```
module:{
    rules:[
        {test:/\.(woff|woff2|eot|ttf|otf)$/,use:'file-loader'}
    ]
  }
```