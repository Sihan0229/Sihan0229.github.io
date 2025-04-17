---
layout: post  
title: "【教程】Autodl服务器更换镜像后无法连接VScode"  
date: 2024-05-28 00:10 +0800  
last_modified_at: 2024-05-28 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "修改~/.ssh文件夹下的config与known_hosts"
---
在 `C:\Users\[your username]\.ssh\`路径下有 `config`和 `known_hosts`两个文件，在文件中完整删除与该服务器相关的行即可

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/ssh_config.png?raw=true" width="80%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/ssh_known_hosts.png?raw=true" width="80%">
