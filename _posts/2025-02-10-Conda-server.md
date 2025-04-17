---
layout: post  
title: "【教程】在无法联网的服务器上配置Anaconda虚拟环境"  
date: 2025-02-10 00:10 +0800  
last_modified_at: 2025-02-11 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "实验室的服务器无法联网，但是需要配置虚拟环境，在另一个可以联网的linux设备上配置好虚拟环境再传输到服务器上"
---
操作流程：

# 1. 在另一个可以联网的linux设备上配置好虚拟环境

在 `base`环境下操作，假设已经配置好的环境为 `dhcp`

（如果没有激活base环境，参考[Conda activate激活base环境出错解决方法](https://sihan0229.github.io/2025/02/03/Conda-activate.html)）

```bash
conda package -n dhcp
```

若运行正常，将会输出类似

```bash
WARNING: The conda.install module is deprecated and will be removed in a future release.
# prefix: /root/miniconda3/envs/dhcp
# files: 51836
# success
unknown-0.0-py31_0.tar.bz2
```

若希望指定文件名称，则可以使用

```bash
conda package -n dhcp -o dhcp_env.tar.gz
```

或者

```bash
conda package -n dhcp -o dhcp_env.tar.bz2
```

# 2. 传输到无法联网的服务器上

找到anaconda/envs路径，例如 `/share/home/shgao/.conda/`。如果不知道路径可以进行查找

进入该路径，新建目录用于解压环境

```bash
mkdir -p dhcp_env
```

如果是 `.tar.gz`文件，使用解压命令

```bash
tar -xzf dhcp_env.tar.gz -C dhcp_env
```

如果是 `.tar.bz2`文件，则使用解压命令

```bash
tar -xjf dhcp_env.tar.bz2 -C dhcp_env
```

若成功，无输出

# 3. 激活环境

```bash
conda activate dhcp_env
```

# 4. 验证环境是否可用

```bash
conda info --envs
```

显示如下内容即为完成

```bash
# conda environments:
#
base                     /n04dat/shgao/software/anaconda3
dhcp_env              *  /n04dat/shgao/software/anaconda3/envs/dhcp_env
```
