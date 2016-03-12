# HujiangTechnology.github.io
####这个files分支为整个博客发布环境，下载下来之后直接可以使用

### 为了方便多人管理博客，特写此说明博客发布的过程

## 下载环境

克隆本仓库files分支中的所有文件，会得到文件夹``HujiangTechnology.github.io-files``
可以把这个文件夹重新命名，名字随意,比如``hujiangios``。  
  
  下载的文件夹有一个``source``，这个文件夹下``_posts``文件夹中存放我们写的文章``.md``格式的,也就是markdown格式的文件。  
  另外还有一个文件夹``public``	,这个文件夹存放我们转换好的``.html``文件.  
  
 **因为博客文章是基于静态页面的，hexo会把``source``文件夹里所有的``.md``文件转换为``.html``文件存放到``public``文件夹，也就是每一篇文章都会被转换成网页格式,然后把``public``文件夹里的内容上传到github,这样网站就会显示文章。所以每次更新一篇文章就会先把所有的``.md``文件重新转换为``HTML``格式，故一定要保证所有发布的文章都在``source``文件夹下**  
 

 **目前没有添加.gitignore，应该添加.gitignore忽略node_modules文件， 使用 cnpm install 来安装依赖**

## 写文章
用命令行进入``hujiangios``文件夹内

```
cd hujiangios
hexo new "filename" //建立新文章
```

这时候会看到``hujiangios/source/_posts``文件夹下会生成一个``.md``文件和一个同名的文件夹
打开``.md``文件就可以编辑文件了，同名的文件夹中放文章中所需要的图片

写文章的格式  
```
title: 标题
date: 2015-11-23 16:26:13
categories:
tags: 
---
```  

写完一段后添加``<--more-->``会出现主页的MORE

## 文章发布
文章编写好就需要发布测试了

```
hexo g  
hexo d
```
执行完这两句命令，文章就被发布了

## 上传git
**如果确认文章没什么问题了以后，就需要把本地环境的files分支上传github的files分支，供其他人使用。保证每个人都能拿到最新的所有文章**

**注意：**  
**本仓库的master分支和环境的master不是一个。本仓库的master分支为博客页面，也就是public文件夹里的内容，files分支则是整个大环境**




