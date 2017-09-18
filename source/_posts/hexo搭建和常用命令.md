---
title: hexo搭建和常用命令
date: 2017-09-18 18:23:28
tags: [hexo,方法论]
categories: 方法论
---

## hexo 还在不断迭代,各版本之间差别较大,本文基于hexo3.3.8

## [hexo官方文档](https://hexo.io/docs/)
### 安装 
```npm
npm install -g hexo
```
目前版本的hexo将server和deployer与主程序分开了,所以还需要另外安装
```npm
npm install hexo-server --save
```
```npm
npm install hexo-deployer-git --save
```
### 常用命令
```hexo new "postName" #新建文章
  hexo new page "pageName" #新建页面
  hexo generate #生成静态页面至public目录
  hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
  hexo deploy #部署到GitHub
  hexo help  # 查看帮助
  hexo version  #查看Hexo的版本
```
### 参考资料: [使用hexo+github搭建免费个人博客详细教程](http://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html)

### 在博客中插入图片

修改_config.yml配置文件post_asset_folder项为true。

修改后,执行new命令后,会在_post文件夹下生成一个与博客md文件同名的文件夹,将图片放入此文件夹

引用方法

```
{% asset_img 这是一个新的博客的图片.jpg 这是一个新的博客的图片的说明 %}
```
