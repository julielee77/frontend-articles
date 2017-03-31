# webpack构建开发环境
## 安装webpack及相关组件
1. 在设备上安装全局的webpack 

	```
	npm install webpack -g
	```
2. 创建项目文件夹并进入，然后运行

	```
	npm init #会创建一个package.json文件，并进入编辑
	```
	package.json是项目的依赖模块，也可在编辑器中编辑
	以下示例为整理的项目中大概会用的depencies

	```
	//package.json
	{
	  "name": "demo",
	  "version": "1.0.0",
	  "author": {
	    "name": "XXX"
	  },
	  "description": "",
	  "license": "XXX",
	  "dependencies": {
	  	"coffee-loader": "~0.7.1",
    	"coffee-script": "^1.10.0",
    	"css-loader": "~0.15.0",
    	"jade": "^1.11.0",
    	"jade-loader": "~0.7.0",
    	"js-beautify": "^1.5.10",
    	"json-loader": "~0.5.1",
    	"less": "^2.5.1",
    	"less-loader": "^2.0.0",
    	"script-loader": "~0.6.0",
    	"style-loader": "~0.12.0",
    	"url-loader": "~0.5.0",
    	"image-loader": "",
    	"stylus": "",
    	"stylus-loader": "",
    	"babel-core": "",
    	"babel-loader": "",
    	"jsx-loader":""
  		}
	}
	```
	[package.json参数参考](http://www.mujiang.info/translation/npmjs/files/package.json.html)
	
	注：对于一些转换的loader需要安装转换工具（未配置会提示并自动安	装），如less-loader和less。

3. 运行npm安装

	```
	npm install webpack --save-dev
	```
	会自动寻找路径的下的package.json，安装对应的dependencies和	webpack（里面包含了webpack模块的plugin，其它plugin需安	装），均存放在新建的node_modules文件夹中。后续如果要新增	loader可运行
	
	```
	npm install XXX-loader --save-dev
	```
	安装完后会自动写入package.json

## webpack.config 配置
webpack.config.js用于指出entry入口文件、编译的loaders、output输出信息、plugin方法的引入等。也可以用其它命名，但运行的时候需指出。

```
/*webpack.config.js示例*/
var path=require('path');//自带的处理或转化文件路径的模块
var webpack = require('webpack');
var ExtractTextPlugin=require('extract-text-webpack-plugin');
var extractCss=new ExtractTextPlugin('../css/[name].css');//定义输出为css格式的样式文件，并住的指定名字和路径

var ROOT_PATH=path.resolve(__dirname);
var APP_PATH=path.resolve(ROOT_PATH,'app');
var BUILD_PATH=path.resolve(ROOT_PATH,'build');
module.exports = {
  //context:ROOT_PATH,//处理entry选项的及目录（绝对路径）
  //页面入口配置文件,每个entry属性对应一个文件  字符串、数组或对象
  entry: {
    app:path.resolve(APP_PATH,'index.js')
  },
  output: {
    //publicPath用于调试或CDN之类的域名
    path: BUILD_PATH,
    filename: '[name].bundle.js'
  },
  devtool:'eval-source-map',
  //出错时会采用source-map的形式直接显示出错的代码位置
  module: {
    //-loader可以省略，多个loader间用！连接
    // 使用loader前要先安装，比如url-loader  npm install url-loader --save-dev（或--save）
    loaders: [
      {
        test: /\.jade$/,
        loader:'jade-html'
      },
      {
        test:/\.scss$/,
        loader:extractCss.extract('css!sass')
      },
      {
        test:/\.less$/,
        loader:'css!less'
      },
      {
        test:/\.styl$/,
        loader:extractCss.extract('css!stylus')
      },
      {
        test:/\.jsx$/,
        loader:'jsx?harmony'
      },
      {
        test:/\.coffee$/,
        loader:'coffee'
      },
      {
        test:/\.ts$/,
        loader:'typescript'
      },
      {
        test:/\.js$/,
        loader:'babel',
        //或者在项目配置一个.babelrc
        query:{
          presets:['es2015']
        }
      },
      {
        test: /\.(png|jpe?g|gif)$/i,
        loader: [
          'image?{bypassOnDebug: true, progressive:true, \
              optimizationLevel: 3, pngquant:{quality: "65-80"}}',
          'url?limit=10000&name=img/[hash:8].[name].[ext]'
        ] //超过8kb才使用url-loader来映射到文件，都则转为data url形式
        //Base64是网络上最常见的用于传输8Bit字节代码的编码方式之一
      },
      {
        test: /\.(woff|eot|ttf)$/i,
        loader: 'url?limit=10000&name=fonts/[hash:8].[name].[ext]'
      }
    ]
  },
  //其它解决方案配置
  resolve: { //设置文件自动扩展后缀名 require可以省略后缀名（不推荐）
    root: '...',//绝对路径
    extensions: [
      '',
      '.js',
      '.json',
      '.css'
    ],
    alias: { //模块别名定义，如后续直接用AppStore即可

    }
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin('common.js'),
    extractCss
    ]
};
```

[配置项参考](http://webpack.github.io/docs/configuration.html)

# 采用webpack编译各类文件
需要预先安装对应的loader，并在webpack.config.js中声明loader
如上面的packge.json文件中所使用的loader

- less文件  less-loader（并安装less依赖）
- stylus文件 stylus-loader（并安装stylus依赖）
- ES6 babel-loader和babel-preset-es2015（并安装babel-core依赖）
- coffeeScript coffee-loader（并安装coffee-script依赖）
- JSX  jsx-loader 将react的jsx转换成js代码

注：编译时，不会生成中间文件，如将Less作为style嵌入到html中，则不会生成对应的css文件。
[loaders参考](http://webpack.github.io/docs/list-of-loaders.html)

# 插件的使用
使用webpack模块的插件（webpack.XXXPlugin）需先在当前路径安装webpack

```
npm install webpack --save-dev #同上可在安装package.json时一并安装
```
这样，webpack模块的plugin就都安装了
非webpack模块的插件需要在当前项目安装，如：

```
npm install extract-text-webpack-plugin
```

配置示例见webpack.config.js
常用插件

- webpack.optimize.Commons.chunkPlugin  合并js中的相同部分，见上示例

- webpack.optimize.MinChunkSizePlugin 压缩js


- ExtractTextPlugin 单独打包css（webpack默认需要style-loader将当作style插入html）


- UglifyJsPlugin 代码丑化，建议开发过程中不开启


# webpack模块引入的使用
在html中直接引入webpack最终生成的js脚本即可。各脚本模块、样式文件、图片可以直接使用CommonJS来书写，并可以直接引入未编译的less/coffeeScript等。如：

```
require('./style.less');
document.write('It works');
document.write(require('./content.js'));
```
注：require时可以指明loader（若未在webpack.config.js中配置），如

```
require('!style!css!less!./style.less')
```


# 运行webpack

```
webpack #使用默认的webpack.config.js编译
```


```
webpack --display-error-detail #可输出错误信息
```


```
webpack --config XXX.js #使用另外的配置文件编译
```

```
webpack --progress --colors #可显示进度
```

```
webpack --progress --colors --watch #监听模式（没有变化的模块不会重复编译）
```

```
webpack -p 压缩混淆代码（相当于--optimize-minimize --optimize-occurence-order）
```

```
webpack -d 生成map映射文件（即模块最终打包到了哪里）
```
其它常用cli

```
webpack <entry> <output> --module-bind '后缀名=loaders列表' 
```

# Development server
以监听模式自动运行webpack。

可与react的react-hot-loader（不刷新同步显示修改的页面）配合使用，参见<http://www.jianshu.com/p/8adf4c2bfa51>。

# 实例


[multiple entry points](https://github.com/webpack/webpack/tree/master/examples/multiple-entry-points)
# 链接补充
webpack入门  [segmentFault传送门](http://segmentfault.com/a/1190000002551952)

webpack使用优化 [gitHub issue传送门](https://github.com/lcxfs1991/blog/issues/2)

[知乎专栏](http://zhuanlan.zhihu.com/FrontendMagazine/20367175) 包括文中提到的前两篇，可暂且忽略react相关
