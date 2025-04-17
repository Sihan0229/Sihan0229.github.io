---
layout: post  
title: "【教程】通过MobaXterm SSH连接Autodl服务器"  
date: 2024-05-23 01:10 +0800  
last_modified_at: 2024-05-23 01:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "MobaXterm SSH连接Autodl服务器流程"
---
打开MobaXterm界面，点击左上角的 `Sessions`建立新的连接

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm1.png?raw=true" width="100%">

选择SSH连接

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm2.png?raw=true" width="100%">

这里要输入 `主机host`、`用户名username`和 `端口Port`，打开Autodl网站，复制登陆指令。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm4.png?raw=true" width="100%">

这个时候复制到的登陆指令的格式一般类似于 `ssh -p 44789 root@connect.westc.gpuhub.com`，其中 `connect.westc.gpuhub.com`对应 `host`，`root`对应 `username`，`44789`对应 `post`。MobaXterm的post好像不支持和Xshell一样粘贴，只能手动输入端口号。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm3.png?raw=true" width="100%">

连接到主机后，会进入命令行界面，显示输入密码的提示，复制刚刚Autodl网站的密码，使用 `Shift+Ins`粘贴后 `Enter`（此处不会显示输入的密码）

```bash
root@connect.westc.gpuhub.com's password:
```

成功输入密码后会弹出这个密码安全界面，自由输入自己的密码即可。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm6.png?raw=true" width="100%">

显示以下内容即成功连接Autodl服务器。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm8.png?raw=true" width="100%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/MobaXterm9.png?raw=true" width="100%">
