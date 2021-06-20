---
title: "Hugo博客搭建02"
date: 2019-12-13T02:00:32+08:00
linktitle: "Hugo博客搭建02"
description: "Hugo博客搭建02"
categories: [ "Development", "hugo" ]
tags: ["hugo", "theme", "html", "css"]
weight: 10
---

## Gitlab CI/CD流程
#### 直接上gitlab-ci.yml，runner用的式shell方式，因为懒得build hugo的docker镜像啦～
    stages:
        - deploy

    job1:
        stage: deploy
        tags:
            - basic
        script:
            - hugo --theme=hyde-hyde --baseUrl="https://yf-gooo.github.io/" --buildDrafts
            - git clone "https://$CI_GITHUB_TOKEN@github.com/YF-Gooo/yf-gooo.github.io.git"
            - cd yf-gooo.github.io
            - rm -rf posts about portfolio img
            - cp -r ../public/* ./
            - git config user.name "YF-Gooo"
            - git config user.email "ucakyj1@gmail.com"
            - git add .
            - git commit -m "update site"
            - git push --set-upstream "https://$CI_GITHUB_TOKEN@github.com/YF-Gooo/yf-gooo.github.io.git"
        only:
            - master

### 以后只需要生成public文件，就会自动同步到我的博客上，灰常方便好用
