---
layout: post  
title: "Visdom Serve在Autodl服务器上的配置"  
date: 2024-05-22 00:10 +0800  
last_modified_at: 2024-05-22 00:10 +0800  
tags: [configuration]  
math: true  
toc: true  
excerpt: "解决Visdom Serve在远程服务器上的配置问题"
---

## 初始状态

```bash
root@autodl-container-5b9f4695b5-ba3439ed:~/autodl-tmp/StainGAN# python -m visdom.server
Checking for scripts.
Downloading scripts, this may take a little while
It's Alive!
INFO:root:Application Started
INFO:root:Working directory: /root/.visdom
You can navigate to http://localhost:8097

```
