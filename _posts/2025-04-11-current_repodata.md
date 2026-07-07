---
layout: post  
title: "【教程】解决conda创建环境报错 failed with repodata from current_repodata.json"  
date: 2025-04-12 00:10 +0800  
last_modified_at: 2025-04-12 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "使用anaconda创建新环境报错failed with repodata from current_repodata.json，在原始环境创建命令后添加 --repodata-fn=repodata.json -y"
---
conda创建环境报错
```bash
conda create --name cortexode python=3.8
```
```bash
Collecting package metadata (current_repodata.json): done
Solving environment: failed with repodata from current_repodata.json, will retry with next repodata source.
```
尝试了如下解决方案均无效
```bash
# 1：更新 Conda 
conda update -n base -c defaults conda -y
# 2：切换 Conda 镜像源
conda create --name cortexode python=3.8 -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main -y
```

查看bug日志
```bash
conda create --name cortexode python=3.8 -y --debug
```
```bash
DEBUG conda.core.subdir_data:_load(390): 304 NOT MODIFIED for 'https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/noarch/repodata.json'. Updating mtime and loading from disk
DEBUG conda.core.subdir_data:_read_pickled(455): found pickle file /root/miniconda3/pkgs/cache/0240b227.q
DEBUG conda.core.subdir_data:_load(321): No local cache found for https://repo.anaconda.com/pkgs/main/linux-64/repodata.json at /root/miniconda3/pkgs/cache/47929eba.json
DEBUG urllib3.connectionpool:_new_conn(971): Starting new HTTPS connection (1): repo.anaconda.com:443
DEBUG urllib3.connectionpool:_make_request(452): https://repo.anaconda.com:443 "GET /pkgs/main/linux-64/repodata.json HTTP/1.1" 200 None
```
因此
```bash
#1. 清除 Conda 缓存
conda clean --all -y
#2. 关闭 Conda 的 repodata.json 处理优化
conda create --name cortexode python=3.8 --repodata-fn=repodata.json -y
```
成功解决