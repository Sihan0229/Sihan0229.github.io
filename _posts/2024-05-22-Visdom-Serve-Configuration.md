---
layout: post  
title: "【教程】Visdom Server在Autodl服务器上的配置（填坑）"  
date: 2024-05-22 00:10 +0800  
last_modified_at: 2024-05-22 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "解决Visdom Server在远程服务器上的配置问题"
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

此处参考了[服务器visdom的本地显示](https://blog.csdn.net/weixin_43702653/article/details/127273564)

例如Autodl登陆指令为 `ssh -p 44789 root@connect.westc.gpuhub.com`，则只需要在 `ssh`和 `-p`之间添加 `-L 8080:localhost:8097`即可

```bash
ssh -L 8080:localhost:8097  -p 44789 root@connect.westc.gpuhub.com
```

现在已经可以成功进入Visdom界面了，显示内容为

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/visdom_1.png?raw=true" width="100%">

然后新建一个bash终端来运行程序。
