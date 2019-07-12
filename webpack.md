webpack学习  
===

核心概念
---

### entry 入口

```javascript 
// 单入口
const config = {
  entry: './src/index.js'
}
```

```javascript {line-numbers}
// 多入口(使用对象语法)
const config = {
  entry: {
    index: './src/index.js',
    admin: './src/admin.js'
  }
}
```

### output 输出
控制 webpack 如何向硬盘写入编译文件

```javascript {line-numbers}
// 单入口
const config = {
  entry: './src/index.js',
  output: {
    filename: '/dist/index.js',
  };
```

```javascript {line-numbers}
// 多入口（用占位符“[ ]”确保文件名称的唯一性）
const config = {
  entry: {
    index: './src/index.js',
    admin: './src/admin.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```

### mode

|选项|描述|
|---|---|
| development | 设置 process.env.NODE_ENV 的值设为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 |
| production(默认值) | 设置 process.env.NODE_ENV 的值设为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 UglifyJsPlugin。 |
|none|不开启任何优化项。|

### loader
用于对模块的源代码进行转换，可以在 import 或"加载"模块时预处理文件

示例（es6转换）

```javascript {line-numbers}
// 安装loader
$ npm install --save-dev babel-loader
```

```javascript {line-numbers}
// test 指定匹配规则
// use 指定使⽤用的 loader 名称
const config = {
  module: {
      rules: [
          {
              test: /\.js$/,
              use: 'babel-loader'
          }
      ]
  }
}
```
### plugins 插件
