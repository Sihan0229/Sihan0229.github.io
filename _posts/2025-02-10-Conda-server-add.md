---
layout: post  
title: "【教程】在无法联网的服务器上配置Anaconda虚拟环境后再添加新需要的python包"  
date: 2025-02-10 00:10 +0800  
last_modified_at: 2025-02-11 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "在无法联网的服务器上安装python包"
---
在根据[在无法联网的服务器上配置Anaconda虚拟环境
](https://sihan0229.github.io/2025/02/09/Conda-server.html)打包传输虚拟环境之后，可能还需要补充或者改动一些python包，具体改动的方法如下：

# 1. 手动下载需要的Python包

进入网站[Python Package Index (PyPI)](https://pypi.org/)，搜索需要的Python包，以nibabel为例。

在此页面选择 `Download files`

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/pypi_download.png?raw=true" width="100%">

下载 `nibabel-5.3.2.tar.gz`

# 2. 将下载好的Python包上传到服务器上

将下载好 `nibabel-5.3.2.tar.gz`的上传到服务器Anaconda目录的 `anaconda3/pkgs/`子目录下，无需进行解压操作。

# 3. 禁用Conda和pip的联网功能

为了避免重复出现的联网报错，可以直接禁用联网的功能。

## 3.1 删除代理配置

如果之前尝试过ssh代理来解决无法联网的问题，但是没有成功，需要先进行删除，否则会报错

```pgsql
(dhcp_env) [shgao@n04 dhcp-dl-neonatal-main]$ conda install nibabel
Collecting package metadata (current_repodata.json): failed

ProxyError: Conda cannot proceed due to an error in your proxy configuration.
Check for typos and other configuration errors in any '.netrc' file in your home directory,
any environment variables ending in '_PROXY', and any other system-wide proxy
configuration settings.
```

删除方法为编辑 `.condarc`文件，使用vim打开，`i`进入编辑模式

```bash
vi ~/.condarc
```

删除其中以下内容后，`Esc`退出，`:wq`保存

```bash
proxy_servers:
  http: http://127.0.0.1:7890 #替换成之前设置的端口
  https: http://127.0.0.1:7890
```

检查检查代理是否删除成功

```bash
conda config --show | grep proxy
```

如果输出如下，说明代理已成功删除

```pgsql
proxy_servers: {}
```

## 3.2 移除配置时安装的清华源channels

出现如下报错时，需要移除这些channels

```pgsql
PackagesNotFoundError: The following packages are not available from current channels:

  - ./nibabel-5.3.2.tar.gz

Current channels:

  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/linux-64
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/noarch
```

移除方法为，检查所有channel名称

```bash
conda config --show channels
```

将会输出类似

```pgsql
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/Paddle/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/fastai/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
```

依次删除上面给出的所有channel

```bash
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```

再次检查现有channels

```bash
conda config --show channels
```

若输出

```bpgsql
channels: []
```

则完成

# 4. 配置需要的依赖

若此时直接尝试本地安装，如

```bash
pip install ./nibabel-5.3.2.tar.gz
```

可能会出现报错：

```pgsql
Processing ./nibabel-5.3.2.tar.gz
  Installing build dependencies ... error
  error: subprocess-exited-with-error

  × pip subprocess to install build dependencies did not run successfully.
  │ exit code: 1
  ╰─> [8 lines of output]
      WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x2b90a661cfa0>: Failed to establish a new connection: [Errno -2] Name or service not known')': /simple/hatchling/
      WARNING: Retrying (Retry(total=3, connect=None, read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x2b90a663a310>: Failed to establish a new connection: [Errno -2] Name or service not known')': /simple/hatchling/
      WARNING: Retrying (Retry(total=2, connect=None, read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x2b90a663a580>: Failed to establish a new connection: [Errno -2] Name or service not known')': /simple/hatchling/
      WARNING: Retrying (Retry(total=1, connect=None, read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x2b90a663a730>: Failed to establish a new connection: [Errno -2] Name or service not known')': /simple/hatchling/
      WARNING: Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x2b90a663a8e0>: Failed to establish a new connection: [Errno -2] Name or service not known')': /simple/hatchling/
      ERROR: Could not find a version that satisfies the requirement hatchling (from versions: none)
      ERROR: No matching distribution found for hatchling
      WARNING: There was an error checking the latest version of pip.
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× pip subprocess to install build dependencies did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
WARNING: There was an error checking the latest version of pip.

```

这是因为遇到关于 **构建依赖**（build dependencies）和 **连接错误**（NewConnectionError）的问题。
关键报错语句为

```pgsql
ERROR: Could not find a version that satisfies the requirement hatchling (from versions: none)
ERROR: No matching distribution found for hatchling
```

在安装某些 Python 包时，尤其是包含 C 语言扩展或者需要从源代码构建的包，pip 需要安装一些**构建依赖项**（如 setuptools、wheel、hatchling 等）。这些构建依赖项通常会从 PyPI（Python 包索引）下载，而你的服务器无法联网，导致 pip 无法获取这些依赖项。

错误信息显示，**pip 尝试访问 PyPI 来查找并安装 hatchling 这个依赖**，但由于连接问题，它无法成功。

因此需要再次在[Python Package Index (PyPI)](https://pypi.org/)上下载**构建依赖项的安装包**，并上传到服务器 `anaconda3/pkgs/`目录下，并进行安装。此过程可能会嵌套多个依赖包，目前可行的安装顺序为：

```bash
pip install ./setuptools-75.8.0.tar.gz
pip install ./flit_core-*.tar.gz
# 注意此处开始增加了本地寻找命令
pip install ./wheel-0.45.1.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
pip install ./pathspec-0.12.1.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
pip install calver-2022.6.26.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
pip install trove_classifiers-2025.1.15.22.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
pip install ./packaging-24.2.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
pip install ./tomli-2.2.1.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
 pip install ./setuptools-75.8.0.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
 pip install ./typing_extensions-4.12.2.tar.gz --no-deps --find-links=/n04dat/shgao/software/anaconda3/pkgs/
```

太多了，总之就按照提示慢慢下载吧……
