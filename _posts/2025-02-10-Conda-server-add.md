---
layout: post  
title: "在无法联网的服务器上配置Anaconda虚拟环境后再添加新需要的python包"  
date: 2025-02-10 00:10 +0800  
last_modified_at: 2025-02-11 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "在无法联网的服务器上安装python包"
---
在根据[在无法联网的服务器上配置Anaconda虚拟环境
](https://sihan0229.github.io/2025/02/09/Conda-server.html)打包传输虚拟环境之后，可能还需要补充或者改动一些python包，具体改动的方法如下：

## 1. 手动下载需要的Python包

进入网站[Python Package Index (PyPI)](https://pypi.org/)，搜索需要的Python包，以nibabel为例。

在此页面选择`Download files`

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/pypi_download.png?raw=true" width="100%">

下载`nibabel-5.3.2.tar.gz`

## 2. 将下载好的Python包上传到服务器上

将下载好`nibabel-5.3.2.tar.gz`的上传到服务器Anaconda目录的`anaconda3/pkgs/`子目录下，无需进行解压操作。


```bash
# conda environments:
#
base                     /n04dat/shgao/software/anaconda3
dhcp_env              *  /n04dat/shgao/software/anaconda3/envs/dhcp_env
```