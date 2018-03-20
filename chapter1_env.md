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




