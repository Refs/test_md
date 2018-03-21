# 使用webpack去编译各种文件

## 编译 ES6/7

### Bable  Bable-loader  

### Bable-presets

> 规范的一个总结：

* es2015/ es2016 /es2017
* env (开发中最常用到，包括2015-2017,以及最近已经发布的规范)
* babel-preset-react (业内人士自定义的版本)
* babel-preset-stage 0-3 指的是已经到了审批阶段，但还未正式发布的规范， 0-3指的就是具体已经审批到了那一步了；

> babel-presets里面可以指定一个target参数，用来告诉babel 当我们在进行编译的时候，根据targets指定的目标来选择 ，那些语法进行编译，而那些语法不进行编译
* targets.node
* targets.browsers: "last 2 versions"
* targets.browsers: "> 1%"




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
        // 原理就是use除了可以单独的指定一个字符串之外，其也可以指定一个对象，对象属性loader指定我们要使用的loader, 通过对象属性options，我们可以向loader内部去传递参数，You can pass options to the loader by using the options property:
        // options也可以指定一个对象，作为参数对象
        //use: 'babel-loader',
        use: {
          loader: 'babel-loader',
          options: {
            // presets 指定是一个数组，数组中的每一个元素都代表一个preset, 若我们xiang为某个preset添加参数，则我们就不能去采用string(preset的名字)的形式，而是要采用数组的形式，数组的第一元素的preset是名称字符串，第二个元素指的是preset的参数对象；
            presets: [
              ['@bable/preset-env',{
                  targets: {
                    browser: ['> 1%', 'last 2 versions']
                  } 
               }]
            ]
          }
        }
        exclude: '/node_modules/'
      }
    ]
  }
}

```

### Bable-plugin

> 之前在语法规范中，我们通过babel-presets来指定打包时候的规范，但这只针对语法，而针对函数与方法的转换，如：Generator、Set、Map、Array.from、Array.prototype.includes等 babel都没有帮我们去处理， 我们需要去借助下面的两个插件；

* Babel Polyfill ： polyfill(垫片 填充器)由于浏览器之间 对于 标准的实现不一致，所以我们需要一个垫片使它们保持同样的API
  + 全局垫片： 一旦我们引入polyfill我们就可以在全局范围内，即浏览器环境内 对其里面实现的API 进行调用，如promise generator ； 即我们一如之后，其就会在全局进行一些变量的定义，相当于对全局变量污染，所以很明显 其就是为了开发`应用`而准备的；（在全局的环境下面，不存在的方法，给其加上）
  + 为应用而准备： `开发应用`对比`开发框架`, 应用是写被自己使用的代码，而框架是开发被别人使用的代码，
  + 使用： npm install babel-polyfill -save  ;  在entry文件前面引入 import "babel-polyfill"将其作为一个依赖；

* Babel Runtime Transform : 与polyfill对比，这是一个局部的垫片
  + 局部垫片
  + 为开发框架准备： 1.其不会去污染全局变量（我们在开发将要被别人引入的代码，在开发的过程中需要去使用es6的一些函数与方法，而我们又不想去污染全局的变量，此时就会用到这个垫片； 即不会在全局范围内去增加新的变量，而只会在代码局部去新增一些变量，以不影响我们代码的使用；
  + 使用： npm install babel-plugin-transform-runtime -save-dev  npm install babel-runtime -save 
  + 在项目的根目录下新建一个.babelrc文件，在文件中去配置babel的一些参数，其中就包含 此插件的一些配置；
  
 * 利用.bablerc 来配置参数 等于是将babel-loader的配置参数单拿出来，单独作为babel的一个配置文件；
 
 ```bash
 # 在根目录新建文件.babelrc 作为 babel runtime时的配置文件
 touch .babelrc
 
 ```
 
 > 若报错：  this.setDynamic is not a function, 说明 babel的版本与runtime 的版本不一致， 换一个版本 npm isntall @babel/runtime --save
 
 npm install @babel/plugin-transform-runtime --save-dev
 
```json
// .babelrc中

{
    "presets" : [
        "targets" : {
            "browsers" : ["last 2 versions"]
        }
    ],
    "plugins": ["@bable/transform-runtime"]
}

```

## 编译Typescript

### typescript-loader

* 安装： 
  + 官方： npm install typescript ts-loader --save-dev
  + 个人： npm install typescript awesome-typescript-loader --save-dev

* 配置：需要同时去配置两个文件
  + tsconfig.json
  + webpack.config.js

```js
module.exports = {
  entry: {
     app : 'app.ts'
  },
  
  output: {
      filename: '[name].bundle.js'
  },
  module: {
     rules: [
        {
            // ? 的意思是0个或者是1个，即结尾可能有两种方式ts tsx;
            test: /\.tsx?$/,
            use: {
              loader: 'ts-loader'
            }
        }
     ]
  }
}

```

```json
// tsconfig.json中
{
    
    "compileOptions": {
      "module" : "commonjs",
      "target" : "es5",
      "allowjs" : true
    },
    "include" : [
        "./src/*"
    ],
    "exclude" : [
        "./node_modules/"
    ]

}
 
```

### typescript 与 其它js library 混用

* typings
  + npm install typings -g 
  + typings install lodash

* 使用typings下载的d.ts文件

```json
// tsconfig.json中
{
    
    "compileOptions": {
      "module" : "commonjs",
      "target" : "es5",
      "allowjs" : true,
      // 通过配置路径 指向d.ts文件
      "typeRoots": [
          "./node_modules/@type",
          "./typings/modules"
      ]
    },
    "include" : [
        "./src/*"
    ],
    "exclude" : [
        "./node_modules/"
    ]

}
 
```

## 提取公用代码

> 当前前段开发的一个主流的模式就是模块化开发，就一定会有其它模块，依赖于公共模块的现象，这一部分公共模块其实就是我们提到的公用代码，我们需要单独的将这些公用的代码 提取出来；

* 减少代码冗余
* 提升代码下载的速度

### 通过CommonsChunkPlugin 来提取公用的代码

> webpack optimize 下内置的一个插件 ： webpack.optimize.CommonsChunkPlugin

* 配置语法： 

```bash
 
 








