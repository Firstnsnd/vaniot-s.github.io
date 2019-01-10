---
title: Markdown语法笔记
date: 2018-01-30 22:37:07
tags: [Markdown,Hexo]
categories: tool
---

Markdown发展至今衍生出各种版本,因而并没有统一的标准,本篇博文仅用于在hexo中的配置及使用,记录了Markdown中的一些方法。
## 一.符号
1.符号转义

描述中需要用到 Markdown 的符号：`_ # * `在这些符号前加反斜杠

2.显示&lt;&gt;
```markdown
输入&lt;和&gt;
```
<!--more--> 
## 二、表格
 makrdown中的表格和并只能由html代码实现
## 三、数学
在`hexo`中支持数学(latex)公式,需要依赖于[hexo-math](https://github.com/hexojs/hexo-math),在站点中的配置文件`_config.yml`可加入如下配置:
```markdown
math:
    engine: 'mathjax'
    mathjax:
        src: "//cdn.bootcss.com/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
        config:
            tex2jax:
                inlineMath: [ ['$','$'], ["\\(","\\)"] ]
```
 但这个js 还是会调用cdn.mathjax.org里的一些配置js文件，最好在主题目录下`\themes\主题文件夹\layout\ _partial\head.ejs`文件中`<meta charset="utf-8">`后面引入:
```html
<link rel="dns-prefetch" href="//cdn.bootcss.com" />
<link rel="dns-prefetch" href="//cdn.mathjax.org" />
```
Markdown 本身的特殊符号与Latex 中的符号会出现冲突的情况

- `_`的转义，在Markdown 中`_`是斜体，但在latex 中，却有下标的意思；
- `\\`的转义，在Markdown 中`\\`会被转义为`\`，这样也会影响mathjax对公式中的`\\`进行渲染。

>解决办法

只需要有冲突的公式放在`{% math %} `和`{% endmath %}`之间即可。

### 1.行内公式
当要在一行内书写数学公式时,使用`$表达式$`来书写如:
```markdown
$\sum_{i=0}^{n}i^2$
```
输出:$\sum_{i=0}^{n}i^2$
### 2.独立公式
使用独立的公式,输出的公式独占一行,使用`$$$表达式$`展现内容:
```markdown
$$\sum_{i=0}^{n}i^2$$
```
输出内容如下: $$\sum_{i=0}^{n}i^2$$

3.当标记为小数时，将小数写在{ }中，如:
```markdown
$2^{1.2}+a_{5.6}$
```
输出为:$2^{1.2}+a_{5.6}$
> 关于Markdown中的更多的Math语法[这里](https://khan.github.io/KaTeX/function-support.html)
## 四、画图
### 1.时序图
在hexo中流程图的实现,依赖于[sequence](https://github.com/bubkoo/hexo-filter-sequence
),在站点中的配置文件`_config.yml`可加入如下配置:
```yml
#sequence
sequence:
    webfont: "//cdnjs.cloudflare.com/ajax/libs/webfont/1.6.27/webfontloader.js"
    snap: "//cdnjs.cloudflare.com/ajax/libs/snap.svg/0.4.1/snap.svg-min.js"
    underscore: "//cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js"
    sequence: "//cdnjs.cloudflare.com/ajax/libs/js-sequence-diagrams/1.0.6/sequence-diagram-min.js"
    style: ""
    options: 
       theme: simple
       css_class: ""

```
使用如下的语法规则,`sequence`为闭合的代码块,可画出时序图:
   
   
    Title: Here is a title
    A->B: Normal line
    B-->C: Dashed line
    C->>D: Open arrow
    D-->>A: Dashed open arrow
生成的时序图:
```sequence
Title: Here is a title
A->B: Normal line
B-->C: Dashed line
C->>D: Open arrow
D-->>A: Dashed open arrow
```
### 2.流程图
在hexo中流程图的实现,依赖于[flowchart](https://github.com/bubkoo/hexo-filter-flowchart),在站点中的配置文件`_config.yml`可加入如下配置:
```yml
#flowchart:
flowchart:
    raphael: "//cdnjs.cloudflare.com/ajax/libs/raphael/2.2.7/raphael.min.js"
    flowchart: "//cdnjs.cloudflare.com/ajax/libs/flowchart/1.6.5/flowchart.min.js"
    options: 
       scale: 1,
       line-width: 2
       line-length: 50
       text-margin: 10
       font-size: 12
```
使用如下的语法规则,`flow`为闭合的代码块,可画出流程图:
   
    st=>start: 开始
    e=>end: 结束
    op=>operation: 我的操作
    cond=>condition: 确认？

    st->op->cond
    cond(yes)->e
    cond(no)->op
生成的流程图:
```flow
st=>start: 开始
e=>end: 结束
op=>operation: 我的操作
cond=>condition: 确认？

st->op->cond
cond(yes)->e
cond(no)->op
```