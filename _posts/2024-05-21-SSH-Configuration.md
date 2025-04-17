---
layout: post  
title: "【教程】VScode中下载了插件但是无法找到SSH Target连接服务器的解决方法"  
date: 2024-05-20 00:10 +0800  
last_modified_at: 2024-05-20 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "VScode中下载了插件但是无法找到SSH Target连接服务器的解决方法（CANNOT find SSH Target in remote explorer）"
---
## 版本信息

+ VSCode版本vscode version：(version 1.82)
+ 已下载扩展installed extensions：

  Remote - SSH v0.106.4
  Remote - SSH: Editing Configuration Files v0.86.0
  Remote Development v0.24.0 WSL v0.81.3

## 问题简述

几天前我从pycharm转战vscode，在使用SSH连接服务器时遇到了一些问题。根据一些较为古早的教程，应当下载上述的几个扩展插件，然后在Remote Explorer当中选择SSH target以连接服务器。但是在目前版本下下载上述扩展插件后Remote Explorer里没有SSH target选项，我选择了SSH tunnel，，显示通道不存在 `the tunnel isn't existed`。未能成功连接服务器。

当时原问题如下：

I want to connect to the Linux server by SSH. But after I installed the extensions above, SSH target were still not found in the remote explorer. There were only Dev Containers and WSL Target. I have successfully connected the server by SSH in pycharm. How to solve this problem?

I searched online and found some advices about changing the version of extension, but useless. I also found that maybe I should install Remote Containers, but I can't find it in the extensions store.

## 解决方法

使用SSH tunnel是正确的，解决通道不存在问题的方法是在cmd中使用 `icacls "key address" /grant:r 11094:"(R)"`，其中 `11094`需要替换成你的admin名称。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/sshVScode.png?raw=true" width="100%">
