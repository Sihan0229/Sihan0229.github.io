---
layout: post  
title: "【教程】在新服务器上配置Anaconda和Connectome Workbench"  
date: 2025-09-02 00:10 +0800  
last_modified_at: 2025-09-12 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "快速配置需要的环境变量"
---


# Anaconda

1. 根据设备配置[下载Anaconda安装程序](https://repo.anaconda.com/archive/)并上传到服务器上。

2. 更改安装程序权限，`Anaconda3-2022.10-Linux-x86_64.sh`更改为下载的安装程序名称。 

```bash
chmod +x Anaconda3-2022.10-Linux-x86_64.sh
```

3. 运行安装文件，`Anaconda3-2022.10-Linux-x86_64.sh`更改为下载的安装程序名称。 

```bash
./Anaconda3-2022.10-Linux-x86_64.sh
```
4. 回车，阅读完后输入`yes`

5. 配置环境变量，`/home/gaosihan/`更改为你的默认安装路径

```bash
echo 'export PATH=/home/gaosihan/anaconda3/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
6. 激活anaconda。如果显示不正常的话，可以关掉这个shell重新打开。
```bash
source activate
conda deactivate
conda activate
```
# Connectome Workbench

1. 下载`workbench-linux64-v2.0.1.zip`并上传到服务器上。
2. `unzip`解压缩，打开文件夹，可以找到`/bin_linux64`子文件夹，复制其路径
3. 将`/bin_linux64`的路径添加到环境变量
```bash
echo 'export PATH=/home/gaosihan/software/workbench/bin_linux64:$PATH' >> ~/.bashrc
```
4. 检查配置，出现标红的`/bin_linux64`路径即可
```bash
echo $PATH | grep '/home/gaosihan/software/workbench/bin_linux64'
```
