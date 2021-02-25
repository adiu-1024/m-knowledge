#### NPM介绍
* 安装及验证
  ```cmd
  npm -v
  ```
  说明：随着 NodeJS 的安装就已经存在，可通过以上命令查看版本号

* 使用场景

  编写命令行执行程序

  从 NPM 服务器拉取第三方包到本地使用

  将自己编写的包上传到 NPM 服务器供别人使用

#### NPM常用指令
* 初始化配置文件
  ```cmd
  npm init -y
  ```
  说明：在当前目录下生成一个严格的 JSON 文件，用来记录包的相关信息

* 安装依赖包
  ```cmd
  npm i m-downloads --save  或  npm i -S m-downloads
  npm i nodemon --save-dev  或  npm i -D nodemon
  npm i 模块名@latest -g   // 安装到最新版本
  ```
  作用：前者用于安装项目运行所依赖的模块，后者用于安装项目开发所依赖的模块

* 移除依赖包
  ```cmd
  npm uninstall [package]
  ```

* 更新依赖包
  ```cmd
  npm update [package]
  npm update [package][@版本号]
  ```

* 查看node_modules的位置
  ```cmd
  npm root  查看当前项目的安装位置
  npm root -g  查看全局的安装位置
  ```

* 查看可以升级的依赖包
  ```cmd
  npm outdated
  ```
  说明：通过 npm update 升级所有红色表示的符合语义化版本规范的依赖包


#### NPM源地址
* 查看源地址
  ```cmd
  npm config ls
  ```
  输出结果
  ```config
  registry = "https://registry.npmjs.org/"
  ```
  说明：默认使用 npm 的官方镜像仓库

* 更改源地址
  ```cmd
  npm config set registry https://registry.npm.taobao.org
  ```
  作用：使用淘宝镜像仓库，提升包的下载速度

#### NPM包的路径更改
* 下载路径
  ```cmd
  npm config set prefix "D:\npm\download"
  ```

* 缓存路径
  ```cmd
  npm config set cache "D:\npm\cache"
  ```
  作用：缓解 C 盘压力，避免电脑卡顿

#### 创建并发布NPM包
* 注册账户
  ```URL
  https://www.npmjs.com
  ```

* 规范的包构成

      包的基本信息 package.json

      入口文件 index.js

      包的说明文档 README.md

* 发布操作

      检验包名是否已存在 npm search [package]

      通过 npm adduser 输入用户名和密码

      使用 npm publish [package] 进行发布

* 打开当前包的 README.md 文档 和 bug 反馈页面
  ```cmd
  npm docs [package]
  npm bugs [package]
  ```
  说明：打开指定包的相关信息则在后边添加包的名称即可

* 包的命名使用作用域
  ```JSON
  {
    "name": "@adiu/downloads"
  }
  ```
  ```cmd
  npm publish --access=public
  ```
  注意：被划了作用域的包，默认是私有的，发布时需要通过 --access=public 把它变为公有的包

* 注意事项

      发布的仓库必须是 npm 的官方仓库或公司内部的私有库

      package.json 文件中一定要指定对应的入口文件

#### 文件package.json配置
* 必须的配置
  ```JSON
  {
    "name": "项目名称",
    "version": "大版本.次版本.小版本",
    "main": "index.js"
  }
  ```

* 描述相关配置
  ```JSON
  {
    "description": "模块的描述信息，便于使用者查找和了解模块的功能",
    "keywords": ["ES10", "nodejs", "关键字"]
  }
  ```
  作用：类似于项目的 SEO 优化，有利于模块获得更精准的匹配和曝光

* 开发人员相关配置
  ```JSON
  {
    /* 一个 author 对应一个人 */
    "author": {
      "name": "模块的主要作者",
      "email": "adiu_1024@163.com",
      "url": "https://github.com/adiu-1024"
    },
    /* 模块的贡献者信息 */
    "contributors": [
      { "name": "张三", "email": "", "url": "" },
      { "name": "李四", "email": "", "url": "" }
    ]
  }
  ```
  说明：人员的信息描述除以上结构外还可以是一个字符串

* 地址相关配置
  ```JSON
  {
    /* 模块的展示主页 */
    "homepage": "https://www.project.com",
    /* 指定一个地址或者一个邮箱，便于使用者反馈问题 */
    "bugs": {
      "url": "https://github.com/adiu-1024/m-downloads/issues",
      "email": ""
    },
    /* 模块的代码仓库地址 */
    "registry": {
      "type": "git",
      "url": "https://github.com/adiu-1024/m-downloads"
    }
  }
  ```

* 脚本命令配置
  ```JSON
  {
    "scripts": {
      "start": "node ./bin/www",
      "test": ""
    }
  }
  ```
  说明：以上命令通过 npm 直接执行，其它自定义命令需要使用 npm run 来执行

* 依赖相关配置
  ```JSON
  {
    /* 项目运行所依赖的模块 */
    "dependencies": {
      "m-downloads": "^1.3.9"
    },
    /* 项目开发所依赖的模块 */
    "devDependencies": {
      "nodemon": "^2.0.2"
    }
  }
  ```

* 指定项目运行的版本范围
  ```JSON
  "engines": {
    "node": ">= 14.15.4",
    "npm": ">= 6.14.10"
  }
  ```

* 发布模块的相关配置
  ```JSON
  {
    "publishConfig": {
      "registry": "如果发布到公司内部搭建的私有 npm 仓库，则需要配置地址"
    },
    /* 允许推送到 npm 服务器的文件列表，也可指定文件夹 */
    "files": [ "index.js", "package.json", "README.md" ]
  }
  ```
  作用：防止垃圾文件被推送到 npm  服务器

* 静态配置
  ```JSON
  {
    "name": "app",
    /* Node环境读取属性 process.env.npm_package_config_port */
    "config": {
      "port": "8080"
    }
  }
  ```
  说明：用来配置一些不怎么发生变化的属性，比如端口号
