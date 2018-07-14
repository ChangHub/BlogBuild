---
title: NPM使用
author: HongChangCui
date: 2017-05-04 22:43:24
categories: 包管理
tags: 
  - NPM
cover_picture: npm.jpg
---
npm全称：Node Package Manager,Node.js的包管理工具，是全球最大开源生态系统。可以方便JS开发者分享、重用和更新代码。
<!--more-->
## npm安装
1. 下载
  npm和Node.js捆绑在一起，官网(https://nodejs.org/en/)下载Node.js安装即可。

2. 检查

``` coffeescript
node -v  npm -v
```
![NPM安装检查][1]
npm是从Node.js中独立出来的一个项目，npm比Node更新更频繁，很可能仍需要更新npm版本。
``` coffeescript
npm install npm@latest -g
```
更新可能出现如下出错：
![NPM更新出错][2]
可执行如下命令：
``` coffeescript
npm config set proxy null  //设置代理为空 
```

由于软件经常更新改变，Node.js有自己的版本管理工具，如：
 NVM、 NodeList 

## npm权限
当安装全局包时，可能会报错EACCES error，表示你没有权限写入目录（包安装的目录）。处理方式有三种：
- 修改默认目录的权限
- 修改默认目录的位置
- 使用包管理工具维护

## 安装本地包
如果为自己的模块引入包，可以使用本地包。
``` coffeescript
npm install <package_name>
```
默认位置：C:\Users\Administrator\node_modules
使用dir node_modules可以查看是否正常安装，node_modules目录是否存在。
在目录中，如果没有package.json，则会安装最新的版本。反之，根据package.json安装指定的版本。
使用安装包：在目录的同级目录下新建index.js文件，
``` javascript
// index.js 
var lodash = require('lodash');
 
var output = lodash.without([1, 2, 3], 1);
console.log(output);
```
运行： node index.js 输出[2, 3]。

## 使用package.json
管理本地包最好的方式就是创建package.json文件。
### package.json
必备字段：name、version
例如：
``` json
{
  "name": "my-awesome-package",
  "version": "1.0.0"
}
```
### npm init
通过一个命令行式问卷调查，输入各种字段值，完成package.json的初始化。
npm init --yes或者npm init --y
从当前目录提取信息，生成一个默认的package.json文件。
例如：
``` json
{
  "name": "Administrator",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
## 更新本地包
``` coffeescript
npm udpate <package_name>
```
查看哪些包需要更新：npm outdated
## 卸载本地包
卸载安装目录中的安装包：
``` coffeescript
npm uninstall <package_name>
```
移除package.json中的依赖项：
``` coffeescript
npm uninstall --save <package_name>
```
## 安装全局包
如果想像grunt CLI一样，作为命令行工具使用，可以安装全局包
``` coffeescript
npm install -g <package_name>
```
默认位置：C:\Users\Administrator\AppData\Roaming\npm\node_modules
## 更新全局包
``` coffeescript
npm update -g <package_name>
```
查看哪些包需要更新：npm outdated -g --depth=0
## 卸载全局包
``` coffeescript
npm uninstall -g <pacakge_name>
```
## 发布包
1.npm网站注册：https://www.npmjs.com/signup
``` coffeescript
npm login
```
提示输入用户名、密码、邮箱，这些是注册时填写的
2.创建一个文件夹FoldXXX，cd进入该文件夹，生成基础配置文件
``` coffeescript
npm init
```
提示配置包的相关信息
3.写包内代码index.js
``` coffeescript
module.exports=.....
```
4.发布到npm上
``` coffeescript
npm publish FoldXXX
```
5.验证下载
``` coffeescript
npm install FoldXXX
```
6.使用包
``` coffeescript
require('index.js')
```
7.撤销发布
``` coffeescript
npm --force unpublish FoldXXX
```
force强制删除
## 淘宝NPM镜像
国内直接使用NPM官方镜像很慢，推荐使用淘宝NPM镜像。
安装：
``` coffeescript
npm install -g cnmp --registry=https://registry.npm.taobao.org
```
这样可以使用cnmp代替nmp 命令。


[1]: https://www.github.com/ChangHub/BlogImages/raw/master/npm%E5%AE%89%E8%A3%85%E6%A3%80%E6%9F%A5.jpg "npm安装检查"
[2]: https://www.github.com/ChangHub/BlogImages/raw/master/npm%E6%9B%B4%E6%96%B0%E5%87%BA%E9%94%99.jpg "npm更新出错"