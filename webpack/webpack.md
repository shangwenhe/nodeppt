title: test
speaker: speaker
url: https://github.com/ksky521/nodeppt
transition: move
files: /fe/main.css

[slide  data-transition="slide1"]

# 前端工程构建工具
## 演讲者：shangwenhe

[slide  data-transition="slide"]

# 前端工程构建 {:&.flexbox.vleft}
### 配置,注册插件
### 计算依赖
### 编译,文件转换
### 产出文件

[slide style="background-image:url('/fe/gulp/gulp-white-text.svg');background-color:#cf4646;"]

# gulp {:&.flexbox.vleft}
### 用自动化构建工具增强你的工作流程！ 

<br/>


[slide style="background-image:url('/fe/gulp/gulp-white-text.svg');background-color:#cf4646;"]

# API

|API      | 说明     |
| :--- | :--- |
| gulp.src | 加载给定的资源      |      
| pipe() | 为资源添加任务节点,处理步骤     |      
| gulp.dest| 文件发布     |      
| gulp.task | 创建任务     |      
| gulp.watch | 监听文件动态     |      

[slide style="background-image:url('/fe/gulp/gulp-white-text.svg');background-color:#cf4646;"]

# 定义一个任务 {:&.flexbox.vleft}

```javascript
gulp.task('basetask', function(cb) {
	gulp.src('client/templates/*.jade') // 获取文件
		.pipe(jade())    // 对文件进行jade编译
		.pipe(minify())  // 对编译后的文件进行压缩
		.pipe(gulp.dest('build/minified_templates'));  // 产出到给定的目录
});
```

## 依赖其他模块
```javascript
gulp.task('mytask', ['basetask'], function() {
  // 做一些事
});
```

## 监听文件动态
```javascript
gulp.watch('file', function(event){
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```



[slide data-transition="slide"]
# webpack
[slide style="background:#2B3A42 no-repeat 50% 50% ;background-image:  url('/fe/webpack/webpack.svg') , url('/fe/webpack/modules.svg');background-size:20%, contain;"]

[slide]
# webpack是一个javascript应用的bundler生成器 {:&.flexbox.vleft}
## webpack is a module bundler for modern JavaScript applications 

[slide]
# entry和chunk[块]概念? {:&.flexbox.vleft}
### entry是webpack打包的入口
### chunk是webpack的最终产物,chunk是由多个模块组成的
### chunkname entry name
<br/>
```javascript
// 单chunk配置
const config = {
  entry: './path/to/my/entry/file.js'
};
module.exports = config;
// 多chunk配置
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
    vendors: './src/vendors.js',
  }
};
```
[slide]
# output {:&.flexbox.vleft}

```javascript
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
    publicPath: 'http://cdn.vipkid.com.cn/' // 添加CDN 
  }
}
// output: ./dist/app.js ./dist/search.js
```
[ filename内置的字段\[name, hash, id, query, chunkhash\]](https://webpack.js.org/configuration/output/#output-filename)



[slide]
# resolve {:&.flexbox.vleft}
```javascript
resolve:{
  alias: { // 创建 import 或 require 的别名
    'vue': 'src/static/vue/vue.min.js', // 补充修复vue2与webpack2集成时的bug
    '@': path.resolve(__dirname, '../src') // 配置绝对地址 basePath
  },
  // 告诉 webpack 解析模块时应该搜索的目录。
  modules: [distth.resolve(__dirname, "./src"), "node_modules"]
}
```


[slide]
# module {:&.flexbox.vleft}
```javascript
module:{
  noParse: /jquery|lodash/,
  rules:[{
    //条件 test, include, exclude 和 resource 
    test: /\.vue$/,
    include: [ path.resolve(__dirname, './src') ],
    exclude: [ path.resolve(__dirname, './src/vendor') ],

    //结果 loader, options, use . loader是use的别名
    loader: 'vue-loader',
    options:{
      modules: true
    }
  }]
}
```
[slide]
# webpack.conf {:&.flexbox.vleft}
```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
    publicPath: 'http://cdn.vipkid.com.cn/' // 添加CDN 
  },
	resolve:{
		alias: { // 创建 import 或 require 的别名
			'vue': 'src/static/vue/vue.min.js', // 补充修复vue2与webpack2集成时的bug
			'@': path.resolve(__dirname, '../src') // 配置绝对地址 basePath
		},
		// 告诉 webpack 解析模块时应该搜索的目录。
		modules: [distth.resolve(__dirname, "./src"), "node_modules"]
	},
	module:{
		noParse: /jquery|lodash/,
		rules:[{
			//条件 test, include, exclude 和 resource 
			test: /\.vue$/,
			include: [ path.resolve(__dirname, './src') ],
			exclude: [ path.resolve(__dirname, './src/vendor') ],

			//结果 loader, options, use . loader是use的别名
			loader: 'vue-loader',
			options:{
				modules: true
			}
		}]
	},
	plugins: []
}
```

[slide]
# plugins {:&.flexbox.vleft}
```
插件目的在于解决 loader 无法实现的其他事。
插件与核心代码几乎是同时执行的。核心代码中有好多事件hook用于插件的执行的。
```

```javascript
 compiler.plugin('run', function(compiler, callback) {
    // console.log(compiler)
    console.log("webpack 构建过程开始！！！");
    callback();
  });
 
  compiler.plugin("compile", function(params) {
    console.log("编译器开始编译...");
  });

  compiler.plugin("compilation", function(compilation) {
    console.log("开始编译一个区块...");

    compilation.plugin("optimize", function() {
      console.log("文件优化...");
    });
  });
  
  compiler.plugin("after-compile", function(params, callback) {
    console.log(" after编译器开始编译...");
    callback()
  });

  compiler.plugin('make', function(compiler, callback) {
    console.log("webpack make 构建过程开始！！！");
    callback();
  });
 
  compiler.plugin('emit', function(compiler, callback) {
    // console.log(compiler)
    console.log("webpack 编译完成！！！");
    callback();
  });
  
  compiler.plugin('after-emit', function(compiler, callback) {
    // console.log(compiler)
    console.log("webpack after编译完成！！！");
    callback();
  });
  
```
[slide data-transition="slide"]
# webpack构建流程 {:&.flexbox.vleft}
### 从启动webpack构建到输出结果经历了一系列过程，它们是：

	1、生成配置

	2、注册插件

	3、解析文件依赖

	4、载入loader

	5、生成chunk, 输出chunk


[slide]

# 1、生成配置 {:&.flexbox.vleft}
解析webpack配置参数，合并从shell传入和webpack.config.js文件里配置的参数，生产最后的配置结果。

```javascript
// 添加环境变量，并使用配置文件
webpack --env.platform=web  --config example.config.js 

// 生成配置文件
lib/WebpackOptionsDefaulter.js
lib/node/NodeEnvironmentPlugin.js
lib/WebpackOptionsApply.js
```

[slide]
# 2、注册插件 {:&.flexbox.vleft}
注册所有配置的插件，好让插件监听webpack构建生命周期的事件节点，以做出对应的反应。
```javascript
// 如果plugins 存在且为数组将插件加载到编译器中
lib/webpack.js 

if(options.plugins && Array.isArray(options.plugins)) {
	compiler.apply.apply(compiler, options.plugins);
}
```

[slide]
# 3、解析文件依赖 {:&.flexbox.vleft}
从配置的entry入口文件开始解析文件构建AST语法树，找出每个文件所依赖的文件，递归下去。
<br/>计算依赖与计算bundle是同时的
```javascript
// entry 开始计算依赖
compiler.plugin("make", (compilation, callback) => {
	const dep = SingleEntryPlugin.createDependency(this.entry, this.name);
	compilation.addEntry(this.context, dep, this.name, callback);
});

// chunk组成bundle
addChunk(name, module, loc) {
	// ... ... 省略
	const chunk = new Chunk(name, module, loc);
	this.chunks.push(chunk);
	if(name) {
		this.namedChunks[name] = chunk;
	}
	return chunk;
}

// 计算文件依赖
processDependenciesBlockForChunk(module, chunk) {
	// ... ... 省略
	const iteratorBlock = b => {
		// ... ... 省略
		c = this.addChunk(b.chunkName, b.module, b.loc);
		let deps = chunkDependencies.get(chunk);
		if(!deps) chunkDependencies.set(chunk, deps = []);
		deps.push({ chunk: c, module });
		queue.push({ block: b, module: null, chunk: c });
	};
}
```

[slide]

# 4、载入loader {:&.flexbox.vleft}
在解析文件,递归的过程中根据test文件类型和module配置找出合适的loader用来对文件进行转换。
```javascript

	const normalModuleFactory = new NormalModuleFactory(this.options.context,
			this.resolvers, this.options.module || {});
	// 定义上下文件环境
	this.applyPlugins("normal-module-factory", normalModuleFactory);

	const params = this.newCompilationParams();
	this.applyPluginsAsync("before-compile", params, err => {
		// ... ... 略
		// 编译结果
		const compilation = this.newCompilation(params);
		this.applyPluginsParallel("make", compilation, err => {
		// ... ... 略
			compilation.seal(err => {
				this.applyPluginsAsync("after-compile", compilation, err => {
					return callback(null, compilation);
				});
			});
		});
	});
```

[slide]
# 5、生成chunk，输出chunk {:&.flexbox.vleft}
从entry开始递归完后得到每个文件的最终结果，并生成代码块chunk。
```javascript
	// 将编译后的文件写入到文件中
 const emitFiles = (err) => {
      require("async").forEach(Object.keys(compilation.assets), (file, callback) => {

        let targetFile = file;
        const queryStringIdx = targetFile.indexOf("?");
        if(queryStringIdx >= 0) {
          targetFile = targetFile.substr(0, queryStringIdx);
        }

        const writeOut = (err) => {
          if(err) return callback(err);
          const targetPath = this.outputFileSystem.join(outputPath, targetFile);
          let content = compilation.assets[file].source();
          // ... ... 略
          this.outputFileSystem.writeFile(targetPath, content, callback);
        };
        // 如果包含目录则创建目录
        if(targetFile.match(/\/|\\/)) {
          const dir = path.dirname(targetFile);
          this.outputFileSystem.mkdirp(this.outputFileSystem.join(outputPath, dir), writeOut);
        } else writeOut();

      }, err => {
        if(err) return callback(err);
        afterEmit.call(this);
      });
	}
	// 由emit 触发
	this.applyPluginsAsync("emit", compilation, err => {
		if(err) return callback(err);
		outputPath = compilation.getPath(this.outputPath);
		this.outputFileSystem.mkdirp(outputPath, emitFiles);
	});
```	


[slide]
# 深入了解产出

[slide]
# 原码深入 {:&.flexbox.vleft}

### 定义模块

```javascript

// 定义模块
var modules = [(function (module, exports, __webpack_require__){
  module.exports = { index:'test0' }
})];

```

### 定义加载器 解决模块依赖关系

```javascript
// 模块加载器
function __webpack_require__(moduleId){
    // ... 
  	// 统一管理所有模块
    var module ={ exports: {} };
  	modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
  	return module.exports;
}
```

### 入口文件
```javascript
// 入口文件
function entry(){
    // 加载模块
    let test = __webpack_require__(0);
  	console.log(test.index);
}
entry();
```

[slide]

# 完整示例	{:&.flexbox.vleft}
```javascript

(function(modules){
   var installedModules = {};
  
  	// webpack 的迭代器
    function __webpack_require__(moduleId){
 		if(installedModules[moduleId]) {
          return installedModules[moduleId].exports;
        }
      	// console.log(installedModules);
 		var module = installedModules[moduleId] = {
 				exports: {}
			};
      	// console.log(moduleId,module,installedModules);
  	 	modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
     	return module.exports;
    }
  
  return __webpack_require__(0)
  
})([
  	(function(module, exports, __webpack_require__){
      	console.log(0); 
        let m1 = __webpack_require__(1)
        let m2 = __webpack_require__(2)
      	module.exports = { index:'test0', m1: m1, m2:m2}
    }), 
  	(function(module, exports, __webpack_require__){
     console.log(1); 
      let module2 = __webpack_require__(2);
       console.log(module2)
      module.exports =  { index:'test1'}
    }), 
  	(function(module, exports, __webpack_require__){
        console.log(2);
       let module3 = __webpack_require__(3);
       console.log(module3)
       module.exports =  { index:'test2'}
    }), 
    (function(module, exports, __webpack_require__){
        console.log(3)
       module.exports =  { index:'test3'}
    })
])

```
https://codepen.io/shangwenhe/pen/gGPvMz

[slide]

# Q&A
