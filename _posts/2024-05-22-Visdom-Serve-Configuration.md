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

**下载安装**

```bash
pip install visdom
```

**启动**

```bash
python -m visdom.server
```

此时输出应为

```bash
Checking for scripts.
Downloading scripts, this may take a little while
It's Alive!
INFO:root:Application Started
INFO:root:Working directory: /root/.visdom
You can navigate to http://localhost:8097

```

**使用SSH链接映射到本地**

此处参考了[服务器visdom的本地显示](https://blog.csdn.net/weixin_43702653/article/details/127273564)，但是与该博客不同的是，本地打开"http://localhost:8097/"网址才成功映射。

```bash
ssh -L 8080:localhost:8097  -p 44789 root@connect.westc.gpuhub.com
```