---
title: hexo系列(五)变更渲染器支持LaTex语法
Typora-root-url: ../
date: 2019-07-25 16:17:58
categories: hexo笔记
tags: 
- hexo系列
- blog
---

`hexo`的默认渲染引擎`marked`不支持`mathjax`, 所以需要更换`hexo`的`markdown`渲染引擎为`Kramed`,该引擎支持`mathjax`.

# 组件依赖变更

## 变更渲染引擎

引擎变更,其中对应的依赖变更不做赘述

```shell
// 卸载`marked`
npm uninstall hexo-renderer-marked --save
// 安装`kramed`
npm install hexo-renderer-kramed --save
```

## 变更语法解析器

语法解析器变更,其中对应的依赖变更不做赘述

```shell
// 卸载`hexo-math`
npm uninstall hexo-math --save
// 安装`hexo-renderer-mathjax`
npm install hexo-renderer-mathjax --save
```

# 配置修改

为解决

## 引擎`kramed`配置变更

1. 修改文件`/node_modules/hexo-renderer-kramed/lib/renderer.js`

```js
// Change inline math rule
function formatText(text) {
    // Fit kramed's rule: $$ + \1 + $$
    return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
}

/* 修改为直接返回text,即 */

// Change inline math rule
function formatText(text) {
    // Fit kramed's rule: $$ + \1 + $$
    // return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
    return text;
}
```

2. 修改文件`node_modules\kramed\lib\rules\inline.js``

   ​	`` LaTex`与`markdown`语法上有语义冲突, 所以`hexo`默认的转义规则会将一些字符进行转义，所以我们需要对默认的规则进行修改.

```js
escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
//修改为
escape: /^\\([`*\[\]()# +\-.!_>])/,
```

```js
em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
// 修改为
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```



## 语法解析器`Mathjax`配置变更

1. 修改文件`node_modules/hexo-renderer-mathjax/mathjax.html`

```html
<!-- 注释掉代码 -->
<script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
<!-- 添加依赖 -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

最终效果:
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
        tex2jax: {
            inlineMath: [ ["$","$"], ["\\(","\\)"] ],
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code'],
            processEscapes: true
        }
    });
    MathJax.Hub.Queue(function() {
        var all = MathJax.Hub.getAllJax();
        for (var i = 0; i < all.length; ++i)
            all[i].SourceElement().parentNode.className += ' has-jax';
    });
</script>
<!--<script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>-->
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
```

2. 开启`mathjax`配置

   修改主题目录下`_config.yml`配置文件

```yaml
# 不同的主题配置方法略微有区别, 故而使用如下配置:

mathjax:
    enable: true

# 或者

mathjax: true
```

​		由于我使用的主题是`Next`, 所以修改`themes/next/_config.yml`, 配置为

```yaml
mathjax:
    enable: true
```

3. 文章中开启`mathjax`支持

   3.1 `hexo`单文章头部添加`mathjax: true`配置, 效果如下 :

   ```yml
   title: hexo系列(四)使用LaTeX写公式
   Typora-root-url: ..
   date: 2019-07-25 14:45:31
   categories: hexo笔记
   tags:
   - hexo系列
   - blog
   mathjax: true
   ```

   3.2 `hexo`所有新建文章添加`mathjax: true`配置

   修改`scaffolds/post.md`,在头部体中添加`mathjax: true`, 效果如下 :

   ```yaml
   title: {{ title }}
   date: {{ date }}
   categories: 
   tags: 
   Typora-root-url: ../
   mathjax: true
   ```

   

