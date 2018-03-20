##  版本是3.8.1 大版本是3.8


### 核心的概念 

* Entry
* Output
* Loaders
* Plugin 

#### Entry

+ 代码的入口 可能会存在有多个入口的情况，一般只会发生在以下两种情形：
+ 一个是可能是多页面的应用程序；
+ 另外一种情况就是单页面中 逻辑代码是分割开的，应用的代码放到一个entry里面，剩下的框架的代码放到另外一个entry里面；注意此时的逻辑的是 代码打包的入口，并不一定代表的是代码执行的入口；
+ entry 的值可以有三种形式 ： string | [string] | object { <key>: string | [string] } 建议采用的是第三种key: value的形式；
  
```bash
 entry: {
  home: "./home.js",
  about: "./about.js",
  contact: "./contact.js"
 }
 # entry 采用 object形式的好处：
 # 一、我们知道每一个entry其对应的key（chunk）到底是什么
 # 二、如果我们想要继续添加一个新的入口，我们只需要重新指定一个key: value即可
```

#### Output 是对打包生成文件的描述

```bash
module.exports = {
  entry : {
    index: 'index.js',
    vendor: 'vendor.js'
  },
  output: {
    # 当我们的入口是多个的时候，对应的我们的output应该是多个，此时我们就要用到自定义规则的方式；
    # [name] 中的name 指的就是 每一个入口文件的key； 
    # [hash:5] 既是webpack 在打包过程中出现的 独一无二的标识码，这会让我们打包之后的文件 拥有一个版本号；
    # 这就是我们多个output的一个自定义的配置；
    filename: '[name].min.[hash:5].js'
  }
}
```

#### Loaders 在webpack中去处理除js之外的所有文件，都需要去用到loaders;

* 处理文件
* 转化为模块 ：其主要的工作就是将我们所说的文件，转化为模块(js可以处理的module)

```bash
module.exports = {
  #　之所以此处我们暴露的是module , 是因为loaders会将我们指定的文件，都转化为js可以识别的module;
  # 在js中使用css是一件令人匪夷所思的事情，但是我们可以通过css loader,将css变成js的一个模块，然后在js中当作依赖 将其引入进来；
  module: {
    rules : [
      {
        # test的含义就是 ： 当满足后面的正则条件，我们就去使用css-loader 
        test: /\.css$/,
        use: 'css-loader'
      }
    ]
  }
}
```
> 常用的loaders:

* 编译相关： babel-loader、 ts-loader
* 样式相关： style-loader、 css-loader、less-loader、postcss-loader
* 文件相关： file-loader、 url-loader

#### Plugins

可以参与打包的整个过程， 打包优化和压缩， 配置编译时的变量 是极其灵活
 
```bash
const webpack = require('webpack');

module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin();
  ]
}

```

> 常用的Plugins

* 优化相关：
   + CommonsChunkPlugin: 用来提取不同chunk之间 相同的代码
   + UglifyWebpackPlugin:  用来压缩js, 并生成相应的sourcemap
   
* 功能相关：
   + ExtractTextWebpackPlugin: 用来提取css 作为一个单独的文件
   + HtmlWebpackPlugin: 
   + HotModuleReplacementPlugin: 模块热更新
   + CopyWebpackPlugin: 帮助copy文件，打包的时候会引用第三方的资源，将我们引用到的静态资源 直接移到输出目录当中；


#### 名称讲解

* chunk:  在webpack中指的是代码块，在webpack中其会将代码默认的分成一个一个的块 
* bundle:  已经被打包好的，一捆 代码；





