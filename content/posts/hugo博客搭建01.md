---
author: "YF"
date: 2019-12-12
linktitle: "Hugo博客搭建01"
description: "Hugo博客搭建01"
title: Hugo博客搭建01
categories: [ "Development", "hugo" ]
tags: ["hugo", "theme", "html", "css"]
weight: 10
---

最终还是放弃了自建博客的想法，虽然的vue＋gin博客已经基本完工。但是考虑到

   * 1.服务器要花钱  
   * 2.别人写的主题的确比我好看 (..•˘_˘•..)

最终还是决定用hugo搭建一个免费博客。真香～～

## 搭建流程

    hugo new site hugo-blog
    cd hugo-blog
    hugo new about.md　//创建文章
    cd themes //下载主题
    git clone https://github.com/htr3n/hyde-hyde.git
    cd ..
    hugo server --theme=hyde-hyde  --buildDrafts

## 部署
####  创建git仓库名字叫做yf-gooo.github.io
    hugo --theme=hyde-hyde --baseUrl="https://yf-gooo.github.io/" --buildDrafts
    cd public
    git init
    git add .
    git commit -m ""
    git remote add origin https://github.com/YF-Gooo/yf-gooo.github.io.git
    git push -u origin master

## 改进
    考虑采用gitlab/cicd自动更新博客，写好markdown，提交自动触发网站更新流程(已完成)