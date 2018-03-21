# 使用webpack去编译各种文件

## 编译 ES6/7

### Bable  Bable-loader  

### Bable-presets

> 规范的一个总结：

* es2015/ es2016 /es2017
* env (开发中最常用到，包括2015-2017,以及最近已经发布的规范)
* babel-preset-react (业内人士自定义的版本)
* babel-preset-stage 0-3 指的是已经到了审批阶段，但还未正式发布的规范， 0-3指的就是具体已经审批到了那一步了；

> babel-presets里面可以指定一个target参数，


```js
module.exports = {
  entry: {
    app: 'app.js'
  },
  
  output: {
    filename: '[name].[hash:8].js'
  },
  
  module: {
    rules: [
      {
        test: /\.js$/,
        // 此处当我们虽然是配置了babel-loader，但我们不知道我们是使用的哪一种规范进行打包的，此时我们就需要用到我们的babel-presets
        // 通过preset 我们可以指定babel-loader 将我们的文件打包成什么样子
        // preset 实际就是我们这个loade的一个参数，我们可以通过如下的方式，去指定该参数;
        // 原理就是use除了可以单独的指定一个字符串之外，其也可以指定一个对象，对象属性loader指定我们要使用的loader, 通过对象属性options，我们可以向loader内部去传递参数，
        // options也可以指定一个对象，作为参数对象
        //use: 'babel-loader',
        use: {
          loader: 'babel-loader',
          options: {
            // 为我们的loader指定一个其所依循的规范
            presets: ['@bable/preset-env']
          }
        }
        exclude: '/node_modules/'
      }
    ]
  }
}

```

### Bable-plugin
