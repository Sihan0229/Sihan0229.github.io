---
layout: post  
title: "一些实用的教程links"  
date: 2024-06-05 00:10 +0800  
last_modified_at: 2024-06-05 14:00 +0800  
tags: [Course Note]  
math: true  r
toc: true  
excerpt: "各种很好用的配置方法"
---

# Linux环境配置

 [Linux 创建 Python 虚拟环境](https://www.cnblogs.com/AllenMi/p/16367557.html)

 [把正在运行的程序放到后台继续](https://blog.51cto.com/lhrbest/5095909)

 [Linux服务器非root用户安装GCC](https://zhuanlan.zhihu.com/p/610194602)

 [facebookresearch pytorch3d install.md](https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md)

# git使用
[生成新的 SSH 密钥并将其添加到 ssh-agent](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

```bash
git config --global user.name 'YourGithubName'
git config --global user.email "your_email@example.com"
ssh-keygen -t ed25519 -C "your_email@example.com"
```

+ 确认路径和密码，可以不设置密码直接回车。

+ 随后进入刚刚确认的路径
+ 打开新生成的pub文件
+ 复制里面所有内容

+ 在github里点击右上角头像
+ 选择Settings
+ 选择SSH and GPG keys
+ New SSH KEY新建密钥
+ 起一个名字并且把刚刚复制的内容粘贴到Key文本框即可。

# ssh 免密登录
## Windows本地登录Linux远程
[AutoDL服务器、Pycharm、Xshell 7](https://blog.csdn.net/weixin_43793510/article/details/124369650)

首先在**本地**管理员模式打开powershell，输入
```bash
ssh-keygen
```
然后一路回车，不要改路径也不要改名称，默认id_xxxxx直接用就好，改了反而没用。

**本地**打开刚刚给的`C:\Users\xxxx/.ssh/`路径，找到`id_xxxxx.pub`这个文件，复制里面所有的内容。

**服务器**上进入`/root/.ssh/`路径，找到`authorized_keys`文件，打开，用刚刚复制的`id_xxxxx.pub`里的内容替换`authorized_keys`里面所有的内容。

重新打开VScode就可以免密连接了。

如果不止一个服务器需要免密登录，只需要复制已有的`id_xxxxx.pub`文件去替换各个服务器`authorized_keys`的就行，不需要每个单独生成一个。

## MacOS本地登录Linux远程
打开终端，进入目录 ~/.ssh，查看当前目录公私钥文件，拥有.pub结尾的文件是公钥文件，无.pub的是私钥文件。
```bash
cd ~/.ssh  
ls             
```
有公私钥文件直接进行下一步，没有的的话和Windows一样使用如下语句生成一个。默认私钥名是id_rsa，公钥名是id_rsa.pub，也可以改名字。
```bash
ssh-keygen
```
随后将公钥上传到服务器，`user@host`当中`user`是用户名，`host`是服务器地址。这一步一般要输入一次服务器密码。
```bash
ssh-copy-id -i id_rsa.pub user@host 
```
成功后会显示
```bash
Number of key(s) added:        1
Now try logging into the machine, with: "ssh -i ./id_rsa 'user@host'"
and check to make sure that only the key(s) you wanted were added.
```
随后再执行如下语句即可，如果公私钥改过名的，也需要相应更改。
```bash
ssh-add -K id_rsa
```
现在该设备可以免密登录`host`服务器了。
