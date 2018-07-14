---
title: Hexo+GitHub搭建个人博客
author: HongChangCui
data: 2017-01-30 20:30
categories: 开源项目
tags:
 - Hexo
 - GitHub
cover_picture: hexo.jpg
---
Hexo是要给博客框架，使用MarkDown继续文章，生产静态网页。

GitHub Pages是一个静态网站托管服务。

<!--more-->

# 创建个人仓库
#### 注册GitHub账户，新建项目：账户名.github.io
# 安装软件
# Git  检查成功：git --version
 - 配置：
     `git config --global user.name "你的GitHub用户名"`
     `git config --global user.email "你的GitHub注册邮箱"`
 - ssh密钥：
     `ssh-kegen -t rsa -C "你的GitHub注册邮箱"`
       将生成的.ssh文件夹中的id.rsa.pub密钥复制到GitHub==>setting=>SSH Key
       `ssh git@github.com 验证是否成功`

以上配置的目的：GitHub要求每次推送都是合法的用户，所以每次推送都需要输入用户名密码验证，为了省去这一步骤，采用ssh，当你推送的时候，git就会匹配你的私钥跟GitHub上的公钥，如果匹配就推送。
#### Node.js 检查成功：node -v  npm -v

安装node.js，是为了利用它的包管理工具npm，一边安装hexo及其相关插件。

#### hexo： 检查成功：hexo -v

- npm install hexo -g   
- npm init
- npm install
- hexo g
- hexo s

可实现本地浏览hexo博客

# 推送-GitHub
#### 修改_config.xml :配置推送

deploy: 

- type: git
- repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
- branch: master

#### 安装插件
   `npm install hexo-deployer-git --save`

# 通过GitHub Page预览

- hexo clear

- hexo g
- hexo d

浏览器中输入： http://用户名.github.io 

# 绑定域名:通过域名预览
#### 域名添加解析：
- 记录类型：CNAME  主机记录：WWW 记录值：用户名.github.io
- 记录类型：A      主机记录：@   记录值：ping 用户名.github.io 得到的IP
#### 博客配置CNAME:
进入本地博客目录/source中，添加文件CNAME（没有后缀），输入内容：你的域名

#### 重新发布
- hexo clean

- hexo g
- hexo d

在浏览器中输入你的域名即可访问博客

# 博客主题
在博客目录中有一个themes文件夹，里面存放博客的主题，默认安装的是landscape。

hexo 主题：

https://github.com/hexojs/hexo/wiki/Themes

https://hexo.io/themes/ 


你可以下载任意主题，放到该文件夹下，配置博客目录下_config.xml中的theme节点,值为主题的名字，即可更换。

# 配置主题

在主题目录下也有一个配置主题的文件_config.xml，可以对某个主题进行配置。




