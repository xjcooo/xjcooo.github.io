---
title: 自建博客之hexo+github+Typora
date: 2019-03-22 22:54:23
tags: hexo
Typora-root-url: ../
---

本文意在构建免费自用博客，使用GitHub管理博客内容，使用GitHub Pages功能发布博客，访问的地址形如：

https://xxxx.github.io

# 1. 软件安装

## 1.1 hexo的安装

依赖于node.js和npm，环境搭建好后，使用如下安装命令安装hexo：

```shell
npm install -g hexo-cli
```

使用如下命令安装hexo自动发布到GitHub的插件：

```shell
npm install hexo-deployer-git --save
```

## 1.2 GitHub依赖安装

不同平台安装方式请自行搜索不做赘述

## 1.3 Typora的安装

[下载地址](https://www.typora.io/)

请根据自己系统下载，开箱即用

# 2. 使用说明

## 2.1 hexo

简易使用见本博客的文章《hello-world》

## 2.2 GitHub

1. 首先登陆GitHub，
2. 新建一个仓库，项目名称形如xxx.github.io，其中xxx以为任意字母组合

![image-20190322232130511](/images/image-20190322232130511.png)

​	GitHub Pages默认可以使用`Jekyll`的方式来管理博客，由于本文使用的是hexo，所以发布以静态页面的方式来发布博客，配置页面如下：

![image-20190322233527293](/images/image-20190322233527293.png)

3. hexo自动上传博客使用的是hexo-deployer-git插件，需要修改_config.yml配置：

```yaml
deploy:
	type: git
	repo: <repository url>
	branch: [branch]
	message: [message]
```

| 参数    | 描述                                                         |
| ------- | :----------------------------------------------------------- |
| repo    | Git仓库地址                                                  |
| branch  | 分支名称，请使用master分支，以保证Git会自动检索到博客，来发布博客 |
| message | 自定义提交信息（默认为`Site update:{{("YYYY-MM-DD HH:mm:ss")}}`) |

![image-20190322234438231](/images/image-20190322234438231.png)

4. 使用`hexo g && hexo deploy`将内容推送到GitHub上，此时即可使用https://xxx.github.io进行访问了，也就是上面Git仓库配置图中`Your site is ready to be published at`后面的地址