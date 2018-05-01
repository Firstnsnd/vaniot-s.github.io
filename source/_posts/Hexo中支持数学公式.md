---
title: Hexo中支持数学公式
date: 2018-02-27 10:56:22
tags: Hexo
categories: web
---
### 1.安装插件
```
npm install hexo-math --save
```
安装完插件后就可以显示数学公式
### 2.修改D:\hexo\下的 _config.yml在最后
```
math:
    engine: 'mathjax'
    mathjax:
        src: "//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
        config:
            tex2jax:
                inlineMath: [ ['$','$'], ["\\(","\\)"] ]
```
<!--more-->
但这个js 还是会调用cdn.mathjax.org里的一些配置js文件，最好在主题目录下
\themes\主题文件夹\layout\_partial\ 的head.ejs 文件`<meta charset="utf-8">`
后面加上
```
<link rel="dns-prefetch" href="//cdn.bootcss.com" />
<link rel="dns-prefetch" href="//cdn.mathjax.org" />
```
### 3.Hexo下mathjax 的转义问题
Markdown 本身的特殊符号与Latex 中的符号会出现冲突的情况

- \_的转义，在Markdown 中\_是斜体，但在latex 中，却有下标的意思；
- \\\的转义，在Markdown 中\\\会被转义为\，这样也会影响mathjax对公式中的\\\进行渲染。
解决办法：
```
只需要有冲突的公式放在{% math %} 和{% endmath %}之间即可。
```
关于Markdown中的更多的Math语法[这里](https://khan.github.io/KaTeX/function-support.html)