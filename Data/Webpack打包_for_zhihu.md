# Webpack

## 背景

- ES Modules存在环境兼容问题

- 模块文件过多，网络请求频繁

- 所有的前端资源都需要模块化

由此产生了如下需求：

## 需求

- 新特性代码编译，新特性编译到可兼容

- 模块化JavaScript打包

- 支持不同类型的资源模块

## 模块打包器（Webpack）

- 模块加载器（Loader）

- 代码拆分（Code Splitting）

- 资源模块（Asset Module）

## Webpack

### 快速上手

-  yarn init --yes
- yarn add webpack webpack-cli --dev
- yarn webpack --version
- 打包 yarn webpack

	- 修改package.json

		- "scripts": {
    "build": "webpack"
    }

### 配置文件

- 默认配置 `src/index.js` -> `dist/main.js`
- 增加自定义文件webpack.config.js
	- const path = require('path')

```javascript
module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.join(__dirname, 'output')
  }
}
```

### 工作模式/mode

- 命令行 yarn webpack --mode development
- webpack.config.js配置

	- production

		- 生产模式下，Webpack 会自动优化打包结果

	- development

		- 开发模式下，Webpack 会自动优化打包速度，添加一些调试过程中的辅助

	- none

		- None 模式下，Webpack 就是运行最原始的打包，不做任何额外处理

### 打包结果运行原理

- 立即执行函数

	- 接收一个参数
	- 传入一个数组

		- 数组中的元素都是对应的一个模块

- 将所有模块打包到一起，并且提供一些基础代码，让模块和模块之间的依赖正确

### Webpack 资源模块加载

- 安装css-loader以及style-loader

- use是从后往前执行

 - code

    ```javascript
    rules: [
      {
        test: /.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
       }
    ]
    ```

- Loader是Webpack的核心特性

- 借助于Loader就可以加载任何类型的资源

### Webpack 导入资源模块

- 打包入口 -> 运行入口
- JavaScript 驱动整个前端应用的业务

	- 让js作为打包的入口，将css作为资源文件导入
	- 根据代码的需要动态导入资源

- 优势

	- 逻辑合理，JS确实需要这些资源文件
	- 确保上线资源不缺失，都是必要的

### Webpack 加载器

- 文件资源加载器

	- 安装file-loader

- URL加载器

	- 安装url-loader
	
- 也需要安装file-loader
	
- 小文件使用Data URLs，减少请求次数
  大文件单独提取存放，提高加载速度

  ```javascript
  {
     test: /.png$/,
     use: {
       loader: 'url-loader',
       options: {
         limit: 10 * 1024 // 10 KB
       }
     }
   }
  ```

- 常用加载器分类

	- 编译转换类

		- 比如css-loader，就是将css代码转化为javascript模块

	- 文件操作类

		- 比如file-loader，将文件拷贝到输出目录，然后将文件路径进行导出

	- 代码检查类

		- 比如eslint-loader，统一代码风格，提高代码质量。不会修改生产环境的代码

### 处理 ES2015

- webpack 不会编译es6代码，因为模块需要，所以处理import和export

- 安装babel-loader

	- 还需要安装插件@babel/core以及@babel/preset-env

- code

     ```javascript
     {
        test: /.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
     ```

- webpack只是打包工具，加载器可以用来编译代码

### 模块加载方式

- 遵循ES Modules 标准的import声明
- 遵循 CommonJS 标准的require函数
- 遵循AMD 标准的define函数和require函数
- Loader加载的非JavaScript也会触发资源加载
- 样式代码中的@import指令和url函数
- HTML代码中图片标签的src属性

	- 安装 html-loader
	- code

	  ```javascript
      {
      test: /.html$/,
      use: {
        loader: 'html-loader',
        options: {
          attrs: ['img:src', 'a:href']
        }
      }
        }
      ```

### 核心工作原理

- Loader机制是webpack的核心
- Loader的工作原理

	- 自定义markdown-loader
	- 安装marked
	- Loader负责资源文件从输入到输出的转换
	- 对于同一个资源可以依次使用多个Loader
	- css-loader -> style-loader
	- 工作管道

### 插件机制介绍

- 增强 Webpack 自动化能力
- Loader专注实现资源模块加载
- Plugin 解决其他自动化工作

 - 自动清除输出目录，清除dist目录

    - clean-webpack-plugin

     - code

         ```javascript
         const { CleanWebpackPlugin } = require('clean-webpack-plugin')
         ...
         plugins: [
           new CleanWebpackPlugin()
         ]
         ```

    - 自动生成使用bundle.js的HTML

    	- 通过webpack输出HTML文件
    	- HTML文件模版
    	
    	- 同时输出多个页面文件
    	
    - html-webpack-plugin
    	
    - code

      ```javascript
      const HtmlWebpackPlugin = require('html-webpack-plugin')
        ...
        plugins: [
            new CleanWebpackPlugin(),
          // 用于生成 index.html
          new HtmlWebpackPlugin({
          title: 'Webpack Plugin Sample',
          meta: {
        viewport: 'width=device-width'
            },
          template: './src/index.html'
          }),
        // 用于生成 about.html
    	    new HtmlWebpackPlugin({
    	      filename: 'about.html'
    	    })
    	  ]
    	  ```
    	
    - 拷贝静态文件至输出目录

    	- copy-webpack-plugin
    	- code

    	  ```javascript
      const CopyWebpackPlugin = require('copy-webpack-plugin')
        new CopyWebpackPlugin([
          // 'public/**'
        'public'
      ])
    	  ```
    	
    - 压缩输出代码

- 实现大多数前端工程化工作
- 相比于Loader，Plugin拥有更宽的能力范围

	- Plugin通过钩子机制实现

### 自动编译

- 增强开发体验，实现自动编译
- watch工作模式

	- 方法一

		- 1. 监听文件变化，自动重新打包

			- yarn webpack --watch

		- 2. 自动刷新浏览器

			- browser-sync dist --files "**/*"

		- 操作麻烦，并且存在频繁的磁盘操作

	- 方法二

		- Webpack Dev Server

			- 提供用于开发的Http Server
			- 集成「自动编译」和「自动刷新浏览器」等功能

		- 安装 webpack-dev-server --dev
		- 执行 yarn webpack-dev-server --open
		- 将打包文件暂时存在内存中，并不在磁盘中

### Webpack Dev Server

- 静态资源访问

	- devServer默认只会serve打包输出文件
	- 只要是Webpack输出的文件，都可以直接被访问
	- 其他静态资源文件也需要serve
	- contentBase 额外为开发服务器制定查找资源目录

- 代理 API

 - 问题：开发阶段接口跨域问题

    - 跨域资源共享（CORS）

    	- 使用CORS的前提是API必须支持
    	- 并不是任何情况下API都必须支持
    	- 同源部署
    	- // // 开发阶段最好不要使用这个插件
    // new CopyWebpackPlugin(['public'])

    - 支持配置代理

    - 目标：将github api代理到本地开发服务器当中

     - Endpoint可以理解为接口端点/入口

         ```javascript
      devServer: {
        contentBase: './public',
        proxy: {
          '/api': {
            // http://localhost:8080/api/users -> https://api.github.com/api/users
            target: 'https://api.github.com',
            // http://localhost:8080/api/users -> https://api.github.com/users
            pathRewrite: {
              '^/api': ''
            },
            // 不能使用 localhost:8080 作为请求 GitHub 的主机名
            changeOrigin: true
          }
        }
      }
         ```

      - 主机名是HTTP协议中的相关概念

### Source Map

- 介绍

	- 如果需要调试应用，错误信息无法定位，调试和运行都是基于运行代码
	- SourceMap 源代码地图，映射得到源代码
	- SourceMap 解决了源代码与运行代码不一致所产生的问题
	- //# sourceMappingURL=jquery-3.4.1.min.map

- Webpack 配置 Source Map

	- Webpack支持12种方式

		- 'eval',
'cheap-eval-source-map',
'cheap-module-eval-source-map',
'eval-source-map',
'cheap-source-map',
'cheap-module-source-map',
'inline-cheap-source-map',
'inline-cheap-module-source-map',
'source-map',
'inline-source-map',
'hidden-source-map',
'nosources-source-map'

	- 每种方式的效率和效果各不相同
	- 不同devtool之间的差异

		- eval

			- 只能定位文件，不知道源代码行类信息，构建速度最快，

		- eval-source-map

			- 可以定位到行和列的错误信息，因为生成了sourceMap

		- cheap-source-map

			- 阉割版的source-map，只有行没有列，生成速度会变快

		- cheap-module-eval-source-map

			- 只能定位到行，和编写源代码一样

		- cheap-source-map

			- 没有用eval处理代码，没有module是loader处理之后的代码

		- inline-source-map

			- 和source-map效果上是一样的，source-map是以物理文件的形式存在的，inline-source-map是以dataurl方式

		- hidden-source-map

			- 开发第三方包比较有用，生成了sourcemap文件但是不能在开发工具中找到

		- nosources-source-map

			- 可以提供行列信息，但是在开发工具中看不到代码，为了在生产环境中暴露源代码

	- 总结

		- eval - 是否使用eval执行模块代码
cheap - Source Map 是否包含行信息
module - 是否能够得到Loader处理之前的源代码

	- 选择合适的Source Map

		- 开发模式

			- cheap-module-eval-source-map

				- 代码没行不会超过80个字符
				- 代码经过Loader转换过后的差异较大
				- 首次打包速度慢，重写打包相对较快

		- 生产模式

			- none

				- Source Map 会暴露源代码
				- 调试是开发阶段的事情

			- nosources-source-map

				- 在源代码中可以找到source-map但是不会在开发者工具中暴露

		- 理解不同模式的差异，适配不同的环境

### Webpack 自动刷新的问题

- 提供对开发者友好的开发服务器
- 方法1:代码中写死编辑器的内容
- 方法2:额外代码实现刷新前保存，刷新后读取
- 问题核心：自动刷新导致的页面状态丢失
- 最好的解决方式：页面不刷新的前提下，模块也可以及时更新

### HMR体验

	- 全称：Hot Module Replacement
	- 模块热更新
	- 热拔插

		- 在一个正在运行的机器上随时插拔设备

	- 模块热替换

		- 在应用运行过程中实时替换某个模块
		- 应用运行状态不受影响
		- 自动刷新导致页面状态丢失

			- 热替换只将修改的模块实时替换至应用中

	- HMR是Webpack中最强大的功能之一

		- 极大程度的提高了开发者的工作效率

- 开启HMR

	- 集成在webpack-dev-server中
	- webpack-dev-server --hot
	
- 也可以通过配置文件开启
	
  ```javascript
      const webpack = require('webpack')
        devServer: {
        hot: true
      // hotOnly: true // 只使用 HMR，不会 fallback 到 live 
    reloading
          },
        plugins: [
        new HtmlWebpackPlugin({
          title: 'Webpack Tutorial',
          template: './src/index.html'
    }),
    new webpack.HotModuleReplacementPlugin()
	  ]
	  ```
	
- HMR 的疑问

	- Webpack 中的 HMR 并不可以开箱即用
	- Webpack中的HMR需要手动处理模块热替换逻辑
	- Q1.为什么样式文件的热更新开箱即用？

		- 是经过loader处理的

	- Q2.凭什么样式文件可以自动处理？

		- 当样式文件改变时可以及时替换掉文件，就可以覆盖样式了。javascript逻辑是毫无规律的。

	- Q3.我的项目没有手动处理，JS照样可以热替换

		- 使用了框架
		- 框架下的开发，每种文件都是有规律的
		- 通过脚手架创建的项目内部集成了HMR方案

- HMR APIs

 - 处理JS模块热替换

    ```javascript
    module.hot.accept('xxx', () => {
    // TODO
    })
    ```

- HMR 注意事项

	- 处理HMR的代码报错会导致自动刷新

		- hotOnly: true

	- 没启用HMR的情况下，HMR API报错

		- ```javascript
      if (module.hot) {
// 判断是否存在
}
	    ```
	    
	- 当devServer中HMR的配置去掉时，代码打包之后不会出现相关代码

### 生产环境优化

- 生产环境和开发环境存在差异
- 生产环境注重运行效率
- 开发环境注重开发效率
- mode 模式

	- 为不同的工作环境创建不同的配置

- 不同环境下的配置

 - 1. 配置文件根据环境不同导出不同配置

     - ```javascript
      module.exports = (env, argv) => {
       if (env === 'production') {
        config.mode = 'production'
        config.devtool = false
        config.plugins = [
         ...config.plugins,
         new CleanWebpackPlugin(),
         new CopyWebpackPlugin(['public'])
          ]
         }
       return config
      }
      ```

   - 2. 一个环境对应一个配置文件

   	- 不同环境对应不同配置文件
   	- 增加webpack.prod.js以及webpack.dev.js
   	- 安装webpack-merge合并配置

- DefinePlugin

 - 为代码注入全局成员

    - process.env.NODE_ENV
    	- 根据这个成员判断此时的环境
    	- ```javascript
    plugins: [
          new webpack.DefinePlugin({
          // 值要求的是一个代码片段
      API_BASE_URL: JSON.stringify('https://api.example.com')
          })
        ]
        ```

- Tree-shaking

	- 去掉代码中未引用部分，dead-code
	- 在生产模式下自动开启
	
	- 一组功能搭配使用后的优化效果

	- 使用
    
    	-   ```javascript
    optimization: {
      // 模块只导出被使用的成员
      usedExports: true, // 代表枯树叶
      // 尽可能合并每一个模块到一个函数中
    concatenateModules: true,
	  // 压缩输出结果
    // minimize: true // 将树叶摇掉
		}
	```
		
	- 合并模块 concatenateModules
		
			- 尽可能的将所有模块合并输出到一个函数中
			- 既提升了运行效率，又减少了代码的体积
			- 称之为 Scope Hoisting
	
- Tree-shaking & Babel

	- Tree Shaking前提是 ES Modules
	- 由Webpack 打包的代码必须使用 ESM
	- 为了转换代码中的ESMAScript新特性

		- babel-loader
		- ES Modules -> CommonJS

	- 按道理说，在babel时就无法生效。但是现在的babel-loader不会导致失效，是因为禁止了commonJs的转换

- sideEffects 副作用

 - 允许通过配置的方式去标识代码是否有副作用，从而为tree-shaking提供更大的压缩空间
  - 副作用：模块执行时除了导出成员之外所做的事情

  - sideEffects一般用于npm包标记是否有副作用

  - 使用

  	- package.json中
   
  	- ```json
   "sideEffects": [
    "./src/extend.js",
    "*.css"
    ]
  	```

  	- webpack配置中
   
  	- ```javascript
   optimization: {
    sideEffects: true
  }
    ```
  	
  - 注意确保代码没有副作用

- 代码分割

 - 打包缺点

    - 所有代码最终都被打包到一起
    	- bundle体积过大

    - 分析

    	- 并不是每个模块在启动时都是必要的
    	- 分包，按需加载
    	- 每次请求都会有一定的延迟
    	- 请求的Header浪费带宽流量
    	- 模块打包是必要的

    - Code Splitting 代码分包/代码分割

    	- 多入口打包

    		- 多页应用程序
    	 - 一个页面对应一个打包入口

    	     - ```javascript
           plugins: [
             new CleanWebpackPlugin(),
               new HtmlWebpackPlugin({
                 title: 'Multi Entry',
                 template: './src/index.html',
                 filename: 'index.html',
               chunks: ['index']
             }),
               new HtmlWebpackPlugin({
                 title: 'Multi Entry',
                 template: './src/album.html',
                 filename: 'album.html',
               chunks: ['album']
               })
           ]
    	       ```

    	     - 公共部分单独提取

           	-   ```javascript
           optimization: {
             splitChunks: {
                 // 自动提取所有公共模块到单独 bundle
             chunks: 'all'
    	         }
           },
    	       ```
    	
    	- 动态导入
    	
    		- 动态导入的模块会被自动分包
    		
    		- 魔法注释

- MiniCssExtractPlugin

	- 提取CSS到单个文件

- OptimizeCssAssetsWebpackPlugin

	- 压缩css文件

- 输出文件名 Hash

	- 生产模式下，文件名使用Hash
	- hash种类

		- filename: '[name]-[hash].bundle.js'
		- filename: '[name]-[chunkhash].bundle.js'
		- filename: '[name]-[contenthash:8].bundle.js'

			- 控制缓存比较好
