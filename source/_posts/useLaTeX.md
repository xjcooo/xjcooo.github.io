---
title: hexo系列(四)使用LaTeX写公式
Typora-root-url: ..
date: 2019-07-25 14:45:31
categories: hexo笔记
tags:
- hexo系列
- blog
mathjax: true
---

# 前言

测试Typora对LaTeX的支持, markdown中默认支持LaTeX,支持行间公式(display方式,语法:\$\$...\$\$),但是行内公式(inline方式,语法:\$...\$)需要在设置中配置(位置:文件->偏好设置),内联公式的配置选项选择打钩,具体如下:

![typoraSetting_md](/images/typoraSetting_md.png)

**备注 :**由于`hexo`默认的渲染引擎`hexo-renderer-marked`不支持`mathjax`,所以需要变更`Hexo`的markdown渲染引擎, 其中一种是`hexo-renderer-kramed`. 详细配置见文章 :

## inline方式

```text
sum = $\sum_{b}^{a}x^2$
```

效果 :

sum = $\sum_{b}^{a}x^2$

## display方式

```text
$$
sum = 	\sum_{b}^{a}x^2\\
\pi\\
\alpha\\
\sqrt\pi\\
$$
```

效果 :
$$
sum = 	\sum_{b}^{a}x^2\\
\pi\\
\alpha\\
\sqrt\pi\\
$$

# LaTeX介绍