### vue-cli（vue脚手架）超详细教程
vue2是对新手很友好的MVVM框架，有完善的官方中文文档，阅读起来也非常容易理解，由浅入深，示例完整。同时官方也提供了一个开箱即用的 vue-cli 帮我们生成一个完整的项目框架。

vue.js 著名的全家桶系列, 包含了， vue-router, vuex， axios，再加上 vue-cli 就是一个从 路由，数据流管理，http请求都有的核心项目，vue 社区的丰富资源，也是满足了我们日常开发中的需求了。

vue-cli 这个构建工具大大降低了 webpack 的使用难度，开箱即用的特性，大大降低了我们的学习成本，加快了我们的开发速度。

在使用vue-cli之前，请确认你的电脑已经安装了 node，建议版本在 8.0.0 以上

 

安装 vue-cli (这里我们确认已安装过node)

使用 npm（不知道npm的同学，可以自行百度，或者看我上面 nvm的安装教程 ） 全局安装 webpack，打开命令行工具输入：

```
npm install webpack -g
```

安装完成之后输入：

```
webpack -v
```

如果出现相应的版本号，则说明安装成功。

注意：webpack 4.X 开始，需要安装 webpack-cli 依赖 ,所以使用这条命令：

```
npm install webpack webpack-cli -g

  PS D:\Notes> webpack -v
  4.16.2
```
全局安装 vue-cli

在命令行中输入：

```
npm install -g vue-cli
```

安装完成后，检验是否安装成功，输入 ：

vue-V (注意：这里是大写的 “V”)

如果出现版本号，则说明安装成功。

```
PS D:\Notes> vue -V
3.0.5
```

打开 node 的安装目录，也可以看到我们全局安装的 vue-cli

用 vue-cli 来构建项目。

首先，在D盘新建一个文件夹作为项目的存放地，然后cd到目录下，输入命令，创建项目

vue init webpack vue-demo

等待模板下载成功后，会有一个交互式的选项让你选择：

```
? Project name vue-demo # 项目名称，直接回车，按照括号中默认名字（注意这里的名字不能有大写字母，如果有会报错Sorry, name can no longer contain capital letters）
? Project description A Vue.js project # 项目描述,随便写
? Author # 作者名称
? Vue build standalone # 我选择的运行加编译时
    Runtime + Compiler: recommended for most users
? Install vue-router? Yes # 是否需要 vue-router，路由肯定要的
? Use ESLint to lint your code? Yes # 是否使用 ESLint 作为代码规范.
? Pick an ESLint preset Standard # 一样的ESlint 相关
? Set up unit tests Yes # 是否安装单元测试
? Pick a test runner 按需选择 # 测试模块
? Setup e2e tests with Nightwatch? 安装选择 # e2e 测试
? Should we run `npm install` for you after the project has been created? (recommended) npm # 包管理器，我选的NPM
```


安装完成后，安装提示，cd 到项目目录, 执行命令 npm install 进行初始化。

### Project initialization finished!
### ========================

```
To get started:

  cd vue-demo
  npm run dev
```

如果安装速度太慢。可以把 npm 的安装源指向 淘宝镜像，这里就不赘述了。

启动项目

```
 npm run dev

 DONE  Compiled successfully in 4014ms                                                   

 I  Your application is running here: http://localhost:8080
```

如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，需要修改一下配置文件 config里的index.js 的 post

```
module.exports = {
  dev: {

    Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {}, // 这里可以配置跨域

     Various Dev Server settings
    host: 'localhost', // can be overwritten by process.env.HOST
    port: 8080, // 端口 就是改这里
    autoOpenBrowser: false, // 这里是ture的话，就会自动打开浏览器

```

我的端口没有被占用，所以直接启动成功，打开 http://localhost:8080 就能看到欢迎页面。

vue-cli 的 webpack 配置分析

从 package.json 可以看到 开发 和 生产 环境的入口。
```

"scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "test": "npm run unit",
    "lint": "eslint --ext .js,.vue src test/unit",
    "build": "node build/build.js"
  },

```
dev 就是开发环境的启动命令
build 是生产打包环境的命令
lint 是ESLint的检查命令 在 --ext 前加一个 --fix 可以自动修复不符合规范的代码
打包上线

运行 npm run build 命令，就可以进行打包工作了，打包完成后，会生成 dist 目录，项目上线时，把dist 目录下的文件放到服务器就可以了。

调试工具 vue-tool

在谷歌商店，搜索这个插件，安装到浏览器，调试项目很好用。
另：

  1.npm 开启了npm run dev以后怎么退出或关闭？
    ctrl+c
  2.--save-dev
    自动把模块和版本号添加到模块配置文件package.json中的依赖里devdependencies部分
  3. --save-dev 与 --save 的区别
    --save     安装包信息将加入到dependencies（生产阶段的依赖）
    --save-dev 安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它