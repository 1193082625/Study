## webpack

#### 什么是Webpack

WebPack可以看做是**模块打包机**：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

#### WebPack和Grunt以及Gulp相比有什么特性

其实Webpack和另外两个并没有太多的可比性，Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack可以替代Gulp/Grunt类的工具。

Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，这个工具之后可以自动替你完成这些任务。

Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个浏览器可识别的JavaScript文件。

**如果实在要把二者进行比较，Webpack的处理速度更快更直接，能打包更多不同类型的文件**。

1、初始化项目，生成package.json文件： npm init -y

2、在项目中安装Webpack：npm install --save-dev webpack

3、通过终端使用webpack

_webpack可以在终端中使用其最基础的命令是_

```
webpack {entry file/入口文件} {destination for bundled file/存放bundle.js的地方}

//例如：webpack非全局安装的情况（入口文件为main.js，生成bundle.js放在public文件夹下）：
node_modules/.bin/webpack app/main.js public/bundle.js
```

4、通过配置文件使用webpack

```
// __dirname是node.js中的一个全局变量，它指向当前执行脚本所在的目录
module.exports = {
  entry: __dirname + "/app.main.js", // 唯一的入口文件
  output: {
    path: __dirname + "/public", // 打包后的文件存放的地方
    filename: "[name].js" // 打包后输出文件的文件名
  }
}
```

5、在package.json中对npm的脚本部分进行相关设置

```
"scripts": {
  "test": ...,
  "start": "webpack", // npm的start是一个特殊的脚本名称，它的特殊性表现在，在命令行中使用npm start就可以执行相关命令，如果对应的此脚本名称不是start，想要在命令行中运行时，需要用 npm run {script name}如npm run build
}
```

