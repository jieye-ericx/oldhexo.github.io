---
toc: true
title: Webpack 笔记
categories: 
 - 笔记
tags:
 - 大前端
 - webpack
---
## webpack简介
### webpack是什么

webpack 是一种前端资源构建工具，一个静态模块打包器(module bundler)。
在 webpack 看来, 前端的所有资源文件(js/json/css/img/less/...)都会作为模块处理。
它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源(bundle)。
<!-- more -->
![image-20200331004145708](/images/webpack.assets/image-20200331004145708.png)

### webpack 五个核心概念

#### Entry 

入口(Entry)指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

#### Output

输出(Output)指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名。

#### Loader

Loader 让 webpack 能 够 去 处 理 那 些 非 JavaScript 文 件 (webpack 自 身 只 理 解 JavaScript)

#### Plugins

插件(Plugins)可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩， 一直到重新定义环境中的变量等。

#### Mode

模式(Mode)指示 webpack 使用相应模式的配置。 

| 选项        | 描述                                                         | 特点                        |
| ----------- | ------------------------------------------------------------ | --------------------------- |
| development | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 | 能让代码本地调试 运行的环境 |
| production  | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。 | 能让代码优化上线 运行的环境 |

### 基本编译打包命令

1. 创建文件 

2.  运行指令 

   + 开发环境指令：

     `webpack src/js/index.js -o build/js/built.js --mode=development `

     功能：webpack 能够编译打包 js 和 json 文件，并且能将 es6 的模块化语法转换成 浏览器能识别的语法。

   + 生产环境指令：`webpack src/js/index.js -o build/js/built.js --mode=production` 

     功能：在开发配置功能上多一个功能，压缩代码。

3. 结论:webpack 能够编译打包 js 和 json 文件。 能将 es6 的模块化语法转换成浏览器能识别的语法。能压缩代码。

4. 问题 不能编译打包 css、img 等文件。 不能将 js 的 es6 基本语法转化为 es5 以下语法。

这就引出了webpack的自定义配置。

### webpack 开发环境的基本配置

#### 创建配置文件

1. 创建文件 webpack.config.js 

2. 配置内容如下 

   ```javascript
   const { resolve } = require('path'); // node 内置核心模块，用来处理路径问题。
   module.exports = {
   entry: './src/js/index.js', // 入口文件
   output: { // 输出配置
   filename: './built.js', // 输出文件名
   path: resolve(__dirname, 'build/js') // 输出文件路径配置
   },
   mode: 'development' //开发环境
   };
   ```

3. 运行指令: `webpack`(与上节相比，少了入口出口文件以及参数)

4. 结论: 此时功能与上节一致。

#### 打包样式资源

1. 下载安装 相关样式的loader 包

   `npm i css-loader style-loader less-loader less -D`

2.  修改配置文件（主要新增module对象，制定rules，每一个rule会用正则匹配到对应的一类文件，然后用指定的loader加载）

   ```javascript
   // resolve 用来拼接绝对路径的方法
   const {resolve} = require('path');
   module.exports = {
   // webpack 配置
   // 入口起点
   	entry: './src/index.js',
   // 输出
   	output: {
   // 输出文件名
   		filename: 'built.js',
   // 输出路径
   // __dirname nodejs 的变量，代表当前文件的目录绝对路径
   		path: resolve(__dirname, 'build')
   	},
   // loader 的配置
   	module: {
   		rules: [
   // 详细 loader 配置
   // 不同文件必须配置不同 loader 处理
   			{
   // 匹配哪些文件
   				test: /\.css$/,
   // 使用哪些 loader 进行处理
   				use: [
   // use 数组中 loader 执行顺序：从右到左，从下到上 依次执行
   // 创建 style 标签，将 js 中的样式资源插入进行，添加到 head 中生效
   					'style-loader',
   // 将 css 文件变成 commonjs 模块加载 js 中，里面内容是样式字符串
   					'css-loader'
   				]
   			},
   			{
   				test: /\.less$/,
   				use: [
   					'style-loader',
   					'css-loader',
   // 将 less 文件编译成 css 文件
   // 需要下载 less-loader 和 less
   					'less-loader'
   				]
   			}
   		]
   	},
   // plugins 的配置
   	plugins: [
   // 详细 plugins 的配置
   	],
   // 模式
   	mode: 'development', // 开发模式
   // mode: 'production'
   }
   ```

3. 运行指令: webpack

如果遇到其他样式文件如sass，可以参考自行配置。

#### 打包 HTML 资源

1. 创建相关html文件

2. 下载安装 plugin 包

   `npm install --save-dev html-webpack-plugin`

3. 修改配置文件（html比较特殊，使用plugins 配置）

   ```javascript
   const { resolve } = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   module.exports = {
   entry: './src/index.js',
   output: {
   filename: 'built.js',
   path: resolve(__dirname, 'build')
   },
   module: {
   rules: [
   // loader 的配置
   ]
   },
   plugins: [
   // plugins 的配置
   // html-webpack-plugin
   // 功能：默认会创建一个空的 HTML，自动引入打包输出的所有资源（JS/CSS）
   // 需求：需要有结构的 HTML 文件
   new HtmlWebpackPlugin({
   // 复制 './src/index.html' 文件，并自动引入打包输出的所有资源（JS/CSS）
   template: './src/index.html'
   })
   ],
   mode: 'development'
   };
   ```

4. 运行指令: webpack

#### 打包图片资源

1. 下载安装 loader 包 `npm install --save-dev html-loader url-loader file-loader`

2. 配置对应文件（同上增加rule）

   ```javascript
   const {resolve} = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   module.exports = {
   	entry: './src/index.js',
   	output: {
   		filename: 'built.js',
   		path: resolve(__dirname, 'build')
   	},
   	module: {
   		rules: [
   			{
   // 问题：默认处理不了 html 中 img 图片
   // 处理图片资源
   				test: /\.(jpg|png|gif)$/,
   // 使用一个 loader
   // 下载 url-loader file-loader
   				loader: 'url-loader',
   				options: {
   // 图片大小小于 8kb，就会被 base64 处理
   // 优点: 减少请求数量（减轻服务器压力）
   // 缺点：图片体积会更大（文件请求速度更慢）
   					limit: 8 * 1024,
   // 问题：因为 url-loader 默认使用 es6 模块化解析，而 html-loader 引入图片是 commonjs
   // 解析时会出问题：[object Module]
   // 解决：关闭 url-loader 的 es6 模块化，使用 commonjs 解析
   					esModule: false,
   // 给图片进行重命名
   // [hash:10]取图片的 hash 的前 10 位
   // [ext]取文件原来扩展名
   					name: '[hash:10].[ext]'
   				}
   			},
   			{
   				test: /\.html$/,
   // 处理 html 文件的 img 图片（负责引入 img，从而能被 url-loader 进行处理）
   				loader: 'html-loader'
   			}
   		]
   	},
   	plugins: [
   		new HtmlWebpackPlugin({
   			template: './src/index.html'
   		})
   	],
   	mode: 'development'
   };
   ```
   
3. 运行指令: webpack

#### 打包其他资源

对于除了以上的文件外，还有其他各种资源文件需要处理，如.ttf .svg .eot等，这里直接使用`file-loader`处理。

增加对应rule即可:

```javascript
// 打包其他资源(除了 html/js/css 资源以外的资源)
{
// 排除 css/js/html 资源
	exclude: /\.(css|js|html|less)$/,
	loader: 'file-loader',
	options: {
		name: '[hash:10].[ext]'
	}
}
```

#### devserver 搭建

对于前端工程化开发，一定离不开一个能够热更新的开发服务器。

可以在webpack.config.js中如下配置：

```javascript
module.exports = {
	...
    mode: 'development',
	devServer: {
		// 项目构建后路径
		contentBase: resolve(__dirname, 'build'),
		// 启动 gzip 压缩
		compress: true,
		// 端口号
		port: 3000,
		// 自动打开浏览器
		open: true
	}
    ...
};
```

下载`webpack-dev-server`后直接运行即可。

#### 打包后对应文件放在对应文件夹

以img为例，在对应的rule下设置`outputPath`

```javascript
{
	// 处理图片资源
	test: /\.(jpg|png|gif)$/,
	loader: 'url-loader',
	options: {
		limit: 8 * 1024,
		name: '[hash:10].[ext]',
		// 关闭 es6 模块化
		esModule: false,
		outputPath: 'imgs'
	}
}
```

#### 开发config模板

```javascript

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build'),
  },
  module: {
    rules: [
      // loader 的配置
      {
        // 处理 less 资源
        test: /\.less$/,
        use: ['style-loader', 'css-loader', 'less-loader'],
      },
      {
        // 处理 css 资源
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        // 处理图片资源
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          // 关闭 es6 模块化
          esModule: false,
          outputPath: 'imgs',
        },
      },
      {
        // 处理 html 中 img 资源
        test: /\.html$/,
        loader: 'html-loader',
      },
      {
        // 处理其他资源
        exclude: /\.(html|js|css|less|jpg|png|gif)/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          outputPath: 'media',
        },
      },
    ],
  },
  plugins: [
    // plugins 的配置
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ],
  mode: 'development',
  
}
```

### webpack 生产环境的基本配置

#### 提取 css 成单独文件

假设如下文件目录

![image-20200408154347927](/images/webpack.assets/image-20200408154347927.png)

下载插件 `npm install --save-dev mini-css-extract-plugin`

处理css时:

```javascript
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
....
rules: [
  {
    test: /\.css$/,
    use: [
      // 创建 style 标签，将样式放入
      // 'style-loader',
      // 这个 loader 取代 style-loader。作用：提取 js 中的 css 成单独文件
      MiniCssExtractPlugin.loader,
      // 将 css 文件整合到 js 文件中
      'css-loader',
    ],
  },
],
plugins: [
	new MiniCssExtractPlugin({
		// 对输出的 css 文件进行重命名
		filename: 'css/built.css'
	})
]
```

#### css 兼容性处理

![image-20200408154656853](/images/webpack.assets/image-20200408154656853.png)

`npm install --save-dev postcss-loader postcss-preset-env`

如下，配置`postcss-loader`后，再在`package.json`中配置`browserslist`(详细配置可以谷歌)，再配置`process.env.NODE_ENV`后即可，执行`webpack`后，可以发现CSS自动添加了不同浏览器的适配代码。

```javascript
const { resolve } = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

// 设置nodejs环境变量
// process.env.NODE_ENV = 'development';

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build'),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          'css-loader',
          /*
            css兼容性处理：postcss --> postcss-loader postcss-preset-env

            帮postcss找到package.json中browserslist里面的配置，通过配置加载指定的css兼容性样式

            "browserslist": {
              // 开发环境 --> 设置node环境变量：process.env.NODE_ENV = development
              "development": [
                "last 1 chrome version",
                "last 1 firefox version",
                "last 1 safari version"
              ],
              // 生产环境：默认是看生产环境
              "production": [
                ">0.2%",
                "not dead",
                "not op_mini all"
              ]
            }
          */
          // 使用loader的默认配置
          // 'postcss-loader',
          // 修改loader的配置
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              plugins: () => [
                // postcss的插件
                require('postcss-preset-env')(),
              ],
            },
          },
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
    new MiniCssExtractPlugin({
      filename: 'css/built.css',
    }),
  ],
  mode: 'development',
}
```

#### 压缩 css

使用`npm install --save-dev optimize-css-assets-webpack-plugin`

```javascript
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')
plugins: [
	// 压缩 css
	new OptimizeCssAssetsWebpackPlugin()
]
```

只需加入以上配置即可！

####  js 语法检查(eslint)

使用`npm install --save-dev eslint-loader eslint eslint-config-airbnb-base eslint-plugin-import`

语法检查： eslint-loader eslint
注意：只检查自己写的源代码，第三方的库是不用检查的
设置检查规则：
	package.json中eslintConfig中设置：

```json
"eslintConfig": {
	"extends": "airbnb-base"
}
```

依赖airbnb --> eslint-config-airbnb-base eslint-plugin-import eslint

配置rule:

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,//记得排除
  loader: 'eslint-loader',
  options: {
    // 自动修复eslint的错误
    fix: true
  }
}
```

如果不想检查某一句：

```javascript
// 下一行eslint所有规则都失效（下一行不进行eslint检查）
// eslint-disable-next-line
console.log(add(2, 5));
```

#### js 兼容性处理(babel)

安装`npm install --save-dev babel-loader @babel/core @babel/preset-env @babel/polyfill core-js`

js兼容性处理：babel-loader @babel/core 
  1. 基本js兼容性处理 --> @babel/preset-env

    问题：只能转换基本语法，如promise高级语法不能转换
  2. 全部js兼容性处理 --> @babel/polyfill  

    问题：我只要解决部分兼容性问题，但是将所有兼容性代码全部引入，体积太大了~

  3. 需要做兼容性处理的就做：按需加载  --> core-js

配置

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,
  loader: 'babel-loader',
  options: {
    // 预设：指示babel做怎么样的兼容性处理
    presets: [
      [
        '@babel/preset-env',
        {
          // 按需加载
          useBuiltIns: 'usage',
          // 指定core-js版本
          corejs: {
            version: 3
          },
          // 指定兼容性做到哪个版本浏览器
          targets: {
            chrome: '60',
            firefox: '60',
            ie: '9',
            safari: '10',
            edge: '17'
          }
        }
      ]
    ]
  }
}
```

#### js 压缩

生产环境下会自动压缩js代码

```javascript
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build')
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  // 生产环境下会自动压缩js代码
  mode: 'production'
};
```

#### HTML 压缩

直接使用之前安装的`HtmlWebpackPlugin`

```javascript
plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html',
    // 压缩html代码
    minify: {
      // 移除空格
      collapseWhitespace: true,
      // 移除注释
      removeComments: true
    }
  })
]
```

### webpack 优化配置

1. webpack性能优化

   + 开发环境性能优化

   + 生产环境性能优化
2. 开发环境性能优化
   + 优化打包构建速度
   +  HMR
   + 优化代码调试
   + source-map
3. 生产环境性能优化
  + 优化打包构建速度
    + oneOf
    + babel缓存
    + 多进程打包
    + externals
    + dll
  + 优化代码运行的性能
    + 缓存(hash-chunkhash-contenthash)
    + tree shaking
    + code split
    + 懒加载/预加载
    + pwa

#### HMR

![image-20200408201733744](/images/webpack.assets/image-20200408201733744.png)

HMR: hot module replacement 热模块替换 / 模块热替换
作用：一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块） ，极大提升构建速度。

样式文件：可以使用HMR功能：因为style-loader内部实现了~

 js文件：默认不能使用HMR功能 --> 需要修改js代码，添加支持HMR功能的代码。

> 注意：HMR功能对js的处理，只能处理非入口js文件的其他文件。
>
> ```javascript
> if (module.hot) {
>   // 一旦 module.hot 为true，说明开启了HMR功能。 --> 让HMR功能代码生效
>   module.hot.accept('./print.js', function() {
>     // 方法会监听 print.js 文件的变化，一旦发生变化，其他模块不会重新打包构建。
>     // 会执行后面的回调函数
>     print();
>   });
> }
> ```

html文件: 默认不能使用HMR功能.同时会导致问题：html文件不能热更新了~ （不用做HMR功能）

> 解决：修改entry入口，将html文件引入->entry: ['./src/js/index.js', './src/index.html'],

devserver中加入hot即可：

```javascript
devServer: {
  contentBase: resolve(__dirname, 'build'),
  compress: true,
  port: 3000,
  open: true,
  // 开启HMR功能
  // 当修改了webpack配置，新配置要想生效，必须重新webpack服务
  hot: true
}
```

#### source-map

source-map: 一种 提供源代码到构建后代码映射 技术 （如果构建后代码出错了，通过映射可以追踪源代码错误）

在module.exports中加入`devtool: source-map`

上为最基本写法，可以增加功能`[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map`

！ 内联 和 外部的区别：1. 外部生成了文件，内联没有 2. 内联构建速度更快

+ source-map：外部
    错误代码准确信息 和 源代码的错误位置
+ inline-source-map：内联
    只生成一个内联source-map
    错误代码准确信息 和 源代码的错误位置
+ hidden-source-map：外部
    错误代码错误原因，但是没有错误位置
    不能追踪源代码错误，只能提示到构建后代码的错误位置
+ eval-source-map：内联
    每一个文件都生成对应的source-map，都在eval
    错误代码准确信息 和 源代码的错误位置
+ nosources-source-map：外部
    错误代码准确信息, 但是没有任何源代码信息
+ cheap-source-map：外部
    错误代码准确信息 和 源代码的错误位置 
    只能精确的行
+ cheap-module-source-map：外部
    错误代码准确信息 和 源代码的错误位置 
    module会将loader的source map加入

种类这么多，至于如何使用：

+ 开发环境：速度快，调试更友好
    速度快(eval>inline>cheap>...)
      eval-cheap-souce-map
      eval-source-map
    调试更友好  
      souce-map
      cheap-module-souce-map
      cheap-souce-map
    --> eval-source-map  / eval-cheap-module-souce-map
+ 生产环境：源代码要不要隐藏? 调试要不要更友好
    内联会让代码体积变大，所以在生产环境不用内联
    nosources-source-map 全部隐藏
    hidden-source-map 只隐藏源代码，会提示构建后代码错误信息
    --> source-map / cheap-module-souce-map

#### oneOf

rules中的loader在执行时所有文件都会被匹配一次，使用`oneof`来使一个文件只被匹配一次：

注意：不能 两个配置处理同一种类型文件，所以可以把一些一定执行的放在oneof外部。

```javascript
rules: [
  {
    // 在package.json中eslintConfig --> airbnb
    test: /\.js$/,
    exclude: /node_modules/,
    // 优先执行
    enforce: 'pre',
    loader: 'eslint-loader',
    options: {
      fix: true
    }
  },
  {
    // 以下loader只会匹配一个
    // 注意：不能 两个配置处理同一种类型文件
    oneOf: [
      {
        test: /\.css$/,
        use: [...commonCssLoader]
      },
      {
        test: /\.less$/,
        use: [...commonCssLoader, 'less-loader']
      },
      /*
        正常来讲，一个文件只能被一个loader处理。
        当一个文件要被多个loader处理，那么一定要指定loader执行的先后顺序：
          先执行eslint 在执行babel
      */
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/preset-env',
              {
                useBuiltIns: 'usage',
                corejs: {version: 3},
                targets: {
                  chrome: '60',
                  firefox: '50'
                }
              }
            ]
          ]
        }
      },
      {
        test: /\.(jpg|png|gif)/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          outputPath: 'imgs',
          esModule: false
        }
      },
      {
        test: /\.html$/,
        loader: 'html-loader'
      },
      {
        exclude: /\.(js|css|less|html|jpg|png|gif)/,
        loader: 'file-loader',
        options: {
          outputPath: 'media'
        }
      }
    ]
  }
]
```

#### 缓存

1. ##### babel缓存
     
     cacheDirectory: true
     --> 让第二次打包构建速度更快

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,
  loader: 'babel-loader',
  options: {
    presets: [
      [
        '@babel/preset-env',
        {
          useBuiltIns: 'usage',
          corejs: { version: 3 },
          targets: {
            chrome: '60',
            firefox: '50'
          }
        }
      ]
    ],
    // 开启babel缓存
    // 第二次构建时，会读取之前的缓存
    cacheDirectory: true
  }
}
```

2. ##### 文件资源缓存

+ hash: 每次wepack构建时会生成一个唯一的hash值。
  问题: 因为js和css同时使用一个hash值。
  如果重新打包，会导致所有缓存失效。（可能我却只改动一个文件）

  ```javascript
  {
    test: /\.(jpg|png|gif)/,
    loader: 'url-loader',
    options: {
      limit: 8 * 1024,
      name: '[hash:10].[ext]',
      outputPath: 'imgs',
      esModule: false
    }
  }
  new MiniCssExtractPlugin({
    filename: 'css/built.[hash:10].css'
  })
  ```

+ chunkhash：根据chunk生成的hash值。如果打包来源于同一个chunk，那么hash值就一样。
  问题: js和css的hash值还是一样的
  因为css是在js中被引入的，所以同属于一个chunk。

  chunk是指所有跟着index引入的文件最后会变成一个文件，一个chunk。

+ contenthash: 根据文件的内容生成hash值。不同文件hash值一定不一样--> 让代码上线运行缓存更好使用

  ```javascript
  new MiniCssExtractPlugin({
    filename: 'css/built.[contenthash:10].css'
  })
  ```

![image-20200408205255192](/images/webpack.assets/image-20200408205255192.png)

可以发现，如果修改css不修改js，则css缓存失效，js不失效。

#### tree shaking

tree shaking：去除无用代码
前提：
1. 必须使用ES6模块化  
2. 开启production环境 

作用: 减少代码体积

*在package.json中配置* ：

`"sideEffects": false`:所有代码都没有副作用（都可以进行tree shaking)

问题：可能会把css / @babel/polyfill （副作用）文件干掉,可以`"sideEffects": ["\*.css", "\*.less"]`解决。

#### code split

##### 方案一

假如我有两个如下名字的文件，直接在`index.js`中引入`test.js`的话，最后两个文件被打包成一个，可以使用如下方法:

```javascript
// 单入口
// entry: './src/js/index.js',
entry: {
  // 多入口：有一个入口，最终输出就有一个bundle
  index: './src/js/index.js',
  test: './src/js/test.js'
},
output: {
  // [name]：取文件名
  filename: 'js/[name].[contenthash:10].js',
  path: resolve(__dirname, 'build')
}
```

##### 方案二

```javascript
module.exports = {
  /*
    1. 可以将node_modules中代码单独打包一个chunk最终输出
    2. 自动分析多入口chunk中，有没有公共的文件。如果有会打包成单独一个chunk
  */
  optimization: {
    splitChunks: {
      chunks: 'all'
    }
  },
  mode: 'production'
};

```

##### 方案三

```javascript
/*
  通过js代码，让某个文件被单独打包成一个chunk
  import动态导入语法：能将某个文件单独打包
  默认使用chunkid命名，webpackChunkName指定打包后名字
*/
import(/* webpackChunkName: 'test' */'./test')
  .then(({ mul, count }) => {
    // 文件加载成功~
    // eslint-disable-next-line
    console.log(mul(2, 5));
  })
  .catch(() => {
    // eslint-disable-next-line
    console.log('文件加载失败~');
  });
```

#### lazy loading

如下，不使用第一行引入方式而是第八行（也会像上面一样单独打包成一个chunk）

```javascript
// import { mul } from './test';

document.getElementById('btn').onclick = function() {
  // 懒加载~：当文件需要使用时才加载~
  // 预加载 prefetch：会在使用之前，提前加载js文件 
  // 正常加载可以认为是并行加载（同一时间加载多个文件）  
  // 预加载 prefetch：等其他资源加载完毕，浏览器空闲了，再偷偷加载资源
  import(/* webpackChunkName: 'test', webpackPrefetch: true */'./test').then(({ mul }) => {
    console.log(mul(4, 5));
  });
};
```

PWA

PWA: 渐进式网络开发应用程序(离线可访问)
workbox --> workbox-webpack-plugin

安装:`npm install --save-dev workbox-webpack-plugin`

配置：

```javascript
const WorkboxWebpackPlugin = require('workbox-webpack-plugin');
new WorkboxWebpackPlugin.GenerateSW({
  /*
    1. 帮助serviceworker快速启动
    2. 删除旧的 serviceworker
    生成一个 serviceworker 配置文件~
  */
  clientsClaim: true,
  skipWaiting: true
})
```

在index.js注册

```javascript
// 注册serviceWorker
// 处理兼容性问题
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker
      .register('/service-worker.js')
      .then(() => {
        console.log('sw注册成功了~');
      })
      .catch(() => {
        console.log('sw注册失败了~');
      });
  });
}
```

问题：

1. eslint不认识 window、navigator全局变量
    解决：需要修改package.json中eslintConfig配置
    "env": {
      "browser": true // 支持浏览器端全局变量
    }
    
 2. sw代码必须运行在服务器上
    --> nodejs
    -->
      `npm i serve -g`
    
    serve -s build 启动服务器，将build目录下所有资源作为静态资源暴露出去

#### 多进程打包

`npm install --save-dev thread-loader`

```javascript
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: [
    /* 
      开启多进程打包。 
      进程启动大概为600ms，进程通信也有开销。
      只有工作消耗时间比较长，才需要多进程打包
    */
    {
      loader: 'thread-loader',
      options: {
        workers: 2 // 进程2个
      }
    },
    {
      loader: 'babel-loader',
      options: {
        presets: [
          [
            '@babel/preset-env',
            {
              useBuiltIns: 'usage',
              corejs: { version: 3 },
              targets: {
                chrome: '60',
                firefox: '50'
              }
            }
          ]
        ],
        // 开启babel缓存
        // 第二次构建时，会读取之前的缓存
        cacheDirectory: true
      }
    }
  ]
}
```

#### externals

忽略需要打包的内容，此时需要cdn引入。

```javascript
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build')
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  mode: 'production',
  externals: {
    // 拒绝jQuery被打包进来
    jquery: 'jQuery'
  }
};
```

#### dll

webpack.dll.js：

```javascript
/*
  使用dll技术，对某些库（第三方库：jquery、react、vue...）进行单独打包
    当你运行 webpack 时，默认查找 webpack.config.js 配置文件
    需求：需要运行 webpack.dll.js 文件
      --> webpack --config webpack.dll.js
*/

const { resolve } = require('path');
const webpack = require('webpack');

module.exports = {
  entry: {
    // 最终打包生成的[name] --> jquery
    // ['jquery'] --> 要打包的库是jquery
    jquery: ['jquery'],
  },
  output: {
    filename: '[name].js',
    path: resolve(__dirname, 'dll'),
    library: '[name]_[hash]' // 打包的库里面向外暴露出去的内容叫什么名字
  },
  plugins: [
    // 打包生成一个 manifest.json --> 提供和jquery映射
    new webpack.DllPlugin({
      name: '[name]_[hash]', // 映射库的暴露的内容名称
      path: resolve(__dirname, 'dll/manifest.json') // 输出文件路径
    })
  ],
  mode: 'production'
};
```

单独打包后，上一节可以看出，其他文件正常打包后只需引入打包好的ddl即可，则`webpack.config.js`中配置：

```javascript
const webpack = require('webpack');
// 告诉webpack哪些库不参与打包，同时使用时的名称也得变~
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');
plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html'
  }),
  // 告诉webpack哪些库不参与打包，同时使用时的名称也得变~
  new webpack.DllReferencePlugin({
    manifest: resolve(__dirname, 'dll/manifest.json')
  }),
  // 将某个文件打包输出去，并在html中自动引入该资源
  new AddAssetHtmlWebpackPlugin({
    filepath: resolve(__dirname, 'dll/jquery.js')
  })
]
```

### webpack 配置详情

#### entry

entry: 入口起点
  1. string --> './src/index.js'

     单入口，打包形成一个chunk。 输出一个bundle文件。
     此时chunk的名称默认是 main。
  2. array  --> ['./src/index.js', './src/add.js']

     多入口，所有入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。

     + 只有在HMR功能中让html热更新生效~

  3. object

     多入口，有几个入口文件就形成几个chunk，输出几个bundle文件
     此时chunk的名称是 key(即下面的index、add)。

     + 特殊用法

       ```javascript
       entry:{
         // 所有入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。
         index: ['./src/index.js', './src/count.js'], 
         // 形成一个chunk，输出一个bundle文件。
         add: './src/add.js'
       }
       ```

#### output

```javascript
output: {
  // 文件名称（指定名称+目录）
  filename: 'js/[name].js',
  // 输出文件目录（将来所有资源输出的公共目录）
  path: resolve(__dirname, 'build'),
  // 所有资源引入公共路径前缀 --> 'imgs/a.jpg' --> '/imgs/a.jpg'
  publicPath: '/',
  chunkFilename: 'js/[name]_chunk.js', // 非入口chunk的名称
  // library: '[name]', // 整个库向外暴露的变量名
  // libraryTarget: 'window' // 变量名添加到哪个上 browser
  // libraryTarget: 'global' // 变量名添加到哪个上 node
  // libraryTarget: 'commonjs'
}
```

#### module

```javascript
module: {
  rules: [
    // loader的配置
    {
      test: /\.css$/,
      // 多个loader用use
      use: ['style-loader', 'css-loader']
    },
    {
      test: /\.js$/,
      // 排除node_modules下的js文件
      exclude: /node_modules/,
      // 只检查 src 下的js文件
      include: resolve(__dirname, 'src'),
      // 优先执行
      enforce: 'pre',
      // 延后执行
      // enforce: 'post',
      // 单个loader用loader
      loader: 'eslint-loader',
      options: {}
    },
    {
      // 以下配置只会生效一个
      oneOf: []
    }
  ]
},
```

#### resolve

```javascript
// 解析模块的规则
resolve: {
  // 配置解析模块路径别名: 优点简写路径 缺点路径没有提示
  alias: {
    $css: resolve(__dirname, 'src/css')
  },
  // 配置省略文件路径的后缀名
  extensions: ['.js', '.json', '.jsx', '.css'],
  // 告诉 webpack 解析模块是去找哪个目录
  modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
}
```

#### devserve

```javascript
devServer: {
  // 运行代码的目录
  contentBase: resolve(__dirname, 'build'),
  // 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload
  watchContentBase: true,
  watchOptions: {
    // 忽略文件
    ignored: /node_modules/
  },
  // 启动gzip压缩
  compress: true,
  // 端口号
  port: 5000,
  // 域名
  host: 'localhost',
  // 自动打开浏览器
  open: true,
  // 开启HMR功能
  hot: true,
  // 不要显示启动服务器日志信息
  clientLogLevel: 'none',
  // 除了一些基本启动信息以外，其他内容都不要显示
  quiet: true,
  // 如果出错了，不要全屏提示~
  overlay: false,
  // 服务器代理 --> 解决开发环境跨域问题
  proxy: {
    // 一旦devServer(5000)服务器接受到 /api/xxx 的请求，就会把请求转发到另外一个服务器(3000)
    '/api': {
      target: 'http://localhost:3000',
      // 发送请求时，请求路径重写：将 /api/xxx --> /xxx （去掉/api）
      pathRewrite: {
        '^/api': ''
      }
    }
  }
}
```

#### optimization

生产环境下才有意义。

```javascript
optimization: {
  splitChunks: {
    chunks: 'all'
    // 默认值，可以不写~
    /* minSize: 30 * 1024, // 分割的chunk最小为30kb
    maxSiza: 0, // 最大没有限制
    minChunks: 1, // 要提取的chunk最少被引用1次
    maxAsyncRequests: 5, // 按需加载时并行加载的文件的最大数量
    maxInitialRequests: 3, // 入口js文件最大并行请求数量
    automaticNameDelimiter: '~', // 名称连接符
    name: true, // 可以使用命名规则
    cacheGroups: {
      // 分割chunk的组
      // node_modules文件会被打包到 vendors 组的chunk中。--> vendors~xxx.j
      // 满足上面的公共规则，如：大小超过30kb，至少被引用一次。
      vendors: {
        test: /[\\/]node_modules[\\/]/,
        // 优先级
        priority: -10
      },
      default: {
        // 要提取的chunk最少被引用2次
        minChunks: 2,
        // 优先级
        priority: -20,
        // 如果当前要打包的模块，和之前已经被提取的模块是同一个，就会复用
        reuseExistingChunk: true
      } 
    }*/
  },
  // 将当前模块的记录其他模块的hash单独打包为一个文件 runtime
  // 解决：修改a文件导致b文件的contenthash变化
  runtimeChunk: {
    name: entrypoint => `runtime-${entrypoint.name}`
  },
  minimizer: [
    // 配置生产环境的压缩方案：js和css
      //默认使用Terser，一般不需要配置
    new TerserWebpackPlugin({
      // 开启缓存
      cache: true,
      // 开启多进程打包
      parallel: true,
      // 启动source-map
      sourceMap: true
    })
  ]
}
```

### Webpack 5

此版本重点关注以下内容:

- 通过持久缓存提高构建性能.
- 使用更好的算法和默认值来改善长期缓存.
- 通过更好的树摇和代码生成来改善捆绑包大小.
- 清除处于怪异状态的内部结构，同时在 v4 中实现功能而不引入任何重大更改.
- 通过引入重大更改来为将来的功能做准备，以使我们能够尽可能长时间地使用 v5.

下载:

`npm i webpack@next webpack-cli -D`

#### 自动删除 Node.js Polyfills

早期，webpack 的目标是允许在浏览器中运行大多数 node.js 模块，但是模块格局发生了变化，许多模块用途现在主要是为前端目的而编写的。webpack <= 4 附带了许多 node.js 核心模块的 polyfill，一旦模块使用任何核心模块（即 crypto 模块），这些模块就会自动应用。

尽管这使使用为 node.js 编写的模块变得容易，但它会将这些巨大的 polyfill 添加到包中。在许多情况下，这些 polyfill 是不必要的。

webpack 5 会自动停止填充这些核心模块，并专注于与前端兼容的模块。

迁移：

- 尽可能尝试使用与前端兼容的模块。
- 可以为 node.js 核心模块手动添加一个 polyfill。错误消息将提示如何实现该目标。

#### Chunk 和模块 ID

添加了用于长期缓存的新算法。在生产模式下默认情况下启用这些功能。

`chunkIds: "deterministic", moduleIds: "deterministic"`

#### Chunk ID

你可以不用使用 `import(/* webpackChunkName: "name" */ "module")` 在开发环境来为 chunk 命名，生产环境还是有必要的

webpack 内部有 chunk 命名规则，不再是以 id(0, 1, 2)命名了

#### Tree Shaking

1. webpack 现在能够处理对嵌套模块的 tree shaking

```js
// inner.js
export const a = 1;
export const b = 2;

// module.js
import * as inner from './inner';
export { inner };

// user.js
import * as module from './module';
console.log(module.inner.a);
```

在生产环境中, inner 模块暴露的 `b` 会被删除

2. webpack 现在能够多个模块之前的关系

```js
import { something } from './something';

function usingSomething() {
  return something;
}

export function test() {
  return usingSomething();
}
```

当设置了`"sideEffects": false`时，一旦发现`test`方法没有使用，不但删除`test`，还会删除`"./something"`

3. webpack 现在能处理对 Commonjs 的 tree shaking

#### Output

webpack 4 默认只能输出 ES5 代码

webpack 5 开始新增一个属性 output.ecmaVersion, 可以生成 ES5 和 ES6 / ES2015 代码.

如：`output.ecmaVersion: 2015`

#### SplitChunk

```js
// webpack4
minSize: 30000;
```

```js
// webpack5
minSize: {
  javascript: 30000,
  style: 50000,
}
```

#### Caching

```js
// 配置缓存
cache: {
  // 磁盘存储
  type: "filesystem",
  buildDependencies: {
    // 当配置修改时，缓存失效
    config: [__filename]
  }
}
```

缓存将存储到 `node_modules/.cache/webpack`

#### 监视输出文件

之前 webpack 总是在第一次构建时输出全部文件，但是监视重新构建时会只更新修改的文件。

此次更新在第一次构建时会找到输出文件看是否有变化，从而决定要不要输出全部文件。

#### 默认值

- `entry: "./src/index.js`
- `output.path: path.resolve(__dirname, "dist")`
- `output.filename: "[name].js"`

#### 更多内容

https://github.com/webpack/changelog-v5

### 模板

加上了vue，使用时注意设置process.env.NODE_ENV。

```javascript
const { resolve } = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin')

// 定义nodejs环境变量：决定使用browserslist的哪个环境
// process.env.NODE_ENV = 'production';

// 复用loader
const commonCssLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  {
    // 还需要在package.json中定义browserslist
    loader: 'postcss-loader',
    options: {
      ident: 'postcss',
      plugins: () => [require('postcss-preset-env')()]
    }
  }
];

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/bundle.js',
    path: resolve(__dirname, 'dist')
  },
  module: {
    rules: [
        {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
      {
        test: /\.css$/,
        use: [...commonCssLoader]
      },
      {
        test: /\.less$/,
        use: [...commonCssLoader, 'less-loader']
      },
      /*
        正常来讲，一个文件只能被一个loader处理。
        当一个文件要被多个loader处理，那么一定要指定loader执行的先后顺序：
          先执行eslint 在执行babel
      */
      {
        // 在package.json中eslintConfig --> airbnb
        test: /\.js$/,
        exclude: /node_modules/,
        // 优先执行
        enforce: 'pre',
        loader: 'eslint-loader',
        options: {
          fix: true
        }
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/preset-env',
              {
                useBuiltIns: 'usage',
                corejs: {version: 3},
                targets: {
                  chrome: '60',
                  firefox: '50'
                }
              }
            ]
          ]
        }
      },
      {
        test: /\.(jpg|png|gif)/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          outputPath: 'imgs',
          esModule: false
        }
      },
      {
        test: /\.html$/,
        loader: 'html-loader'
      },
      {
        exclude: /\.(js|css|less|html|jpg|png|gif)/,
        loader: 'file-loader',
        options: {
          outputPath: 'media'
        }
      }
    ]
  },
  plugins: [
    new VueLoaderPlugin(),
    new MiniCssExtractPlugin({
      filename: 'css/built.css'
    }),
    new OptimizeCssAssetsWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html',
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    })
  ],
  mode: 'development',
  devServer: {
  	contentBase: resolve(__dirname, 'build'),
  	compress: true,
  	port: 3000,
  	open: true,
  }
};
```



