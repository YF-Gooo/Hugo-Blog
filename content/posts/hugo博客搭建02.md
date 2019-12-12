---
title: "Hugo博客搭建02"
date: 2019-12-13T02:00:32+08:00
linktitle: "hugo博客搭建02"
description: "hugo博客搭建02"
categories: [ "Development", "hugo" ]
tags: ["hugo", "theme", "html", "css"]
weight: 10
---

## Gitlab CI/CD流程

    stages:
    - deploy

    job1:
        stage: deploy
        tags:
            - basic
        script:
            - git clone "https://$CI_GITHUB_TOKEN@github.com/YF-Gooo/yf-gooo.github.io.git"
            - cd yf-gooo.github.io
            - rm -rf page about portfolio
            - cp -r ../public/{page,about,portfolio} ./
            - git config user.name "YF-Gooo"
            - git config user.email "ucakyj1@gmail.com"
            - git add .
            - git commit -m "update site"
            - git push --set-upstream "https://$CI_GITHUB_TOKEN@github.com/YF-Gooo/yf-gooo.github.io.git"
        only:
            - master
