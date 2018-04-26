---
title: sublime配置
date: 2017-12-30 10:47:31
tags: sublime
categories: tool
---
#### 1.安装package control
按住ctrl+\`在sublime的console中输入如下命令：
```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
<!--more-->
#### 2.常用的插件
1.Emmet
输入标签简写形式，然后按Tab键 
[官方文档](https://docs.emmet.io/) [速查表](https://docs.emmet.io/)
2.JsFormat
在JS文件中通过鼠标右键->JsFormat或者Ctrl+Alt+F
3.CSS Format
CSS 格式化插件
4.function name display
这个插件可以在状态栏显示出当前光标处于哪个函数中
5.DocBlockr
主要是用来注释编写说明
### 3.常用设置
```
{
	"bold_folder_labels": true,
	"color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
	"highlight_line": true,
	"ignored_packages":
	[
		"Vintage"
	],
	"previewonclick": false,
	"rulers":
	[
		80
	],
	"save_on_focus_lost": true,
	"show_full_path": true
}
```