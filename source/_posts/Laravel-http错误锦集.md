---
title: Laravel-http错误锦集
date: 2018-05-31 15:56:06
tags: [laravel]
categories: web
---
### 一、Laravel 419错误 -ajax请求 （CSRF验证）
1.在页面上添加
`<meta name="csrf-token" content="{{ csrf_token() }}">`
2.然后在页面的script标签中添加
`$.ajaxSetup({headers: {'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')}});`