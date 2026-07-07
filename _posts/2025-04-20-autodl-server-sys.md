---
layout: post  
title: "【教程】Autodl服务器系统盘清理"  
date: 2025-04-18 00:10 +0800  
last_modified_at: 2025-04-20 00:10 +0800  
tags: [configuration]  
math: true  r
toc: true  
excerpt: "Autodl服务器系统盘爆满，只配过三个项目的环境，针对Conda环境问题对根目录进行清理，从96%降到21%。"
---
正好有一个实例快满了也没什么用，就试一下清理系统盘的方法，方便以后需要的时候用。目前系统盘已经96.11%快满了，但是数据盘才用了16.79%。这个服务器主要是一个项目**配环境**比较麻烦，没配完就满了，所以主要是针对这个问题来删系统盘。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/autodl961.png?raw=true" width="100%">

只开了无卡模式来删文件，尽管已经96%+了还是能连上服务器的。
<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/autodl97.png?raw=true" width="60%">

首先删两个不影响系统运行的目录，这两步和[AutoDL帮助文档](https://www.autodl.com/docs/qa1/)里的一样，用完降到了76.79%。
```bash
du -sh /root/miniconda3/pkgs/ && rm -rf /root/miniconda3/pkgs/*      # conda的历史包
du -sh /root/.local/share/Trash && rm -rf /root/.local/share/Trash   # jupyterlab的回收站
```
<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/autodl7679.png?raw=true" width="100%">

也按照[AutoDL帮助文档](https://www.autodl.com/docs/qa1/)教程检查了/tmp/和/root/.cache，/tmp/所占空间不多，/root/.cache也只有5.0G，所以还要排查其他的。
```bash
du -sh /tmp/
#64K     /tmp/
du -sh /root/.cache
#5.0G    /root/.cache
```
目前已知只有`/root/autodl-tmp`，`/root/autodl-nas`，`/root/autodl-pub`，`/root/autodl-fs`几个文件夹不占用系统盘，所以直接退回到`/`路径排查所有的文件夹。使用的命令是[AutoDL帮助文档](https://www.autodl.com/docs/qa1/)当中提到的
```bash
du -sh xxx
```
排查结果为只有这四个文件比较大，`/usr/`最好不要动，`/root/`里面是`/miniconda/`占的空间为主，可以清理，`/opt/`里只有`/nvidia/`也不能删，`/CONDA_PKGS/`需要处理。

|文件夹名|所占空间|是否能清理|
|--|--|--|
| `/usr/` | 9.2G |❌|
| `/root/` | 26G |✅`/miniconda/envs/`里不需要的旧环境可以删|
| `/opt/` | 1.4G |❌用 GPU 的话别删|
| `/CONDA_PKGS/` | 11G |✅出现在`/`目录下很不合理，可能是配环境的时候设置错了|


<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/sys_check1.png?raw=true" width="80%">

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/sys_check2.png?raw=true" width="80%">

首先看了miniconda3里的虚拟环境路径下有两个已经不需要了的虚拟环境，进行删除。
<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/sys_envs_delete.png?raw=true" width="100%">

使用的语法为
```bash
conda env remove -n nmed2024
conda env remove -n bnt
conda env remove -n xxxxxxx #环境名
```
这一波删完，系统盘内存已经降到了57%

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/autodl5704.png?raw=true" width="100%">


然后移动一下`/CONDA_PKGS/`，这个文件夹里的东西不应该出现在`/`目录下，可能是直接运行开源代码的时候没有改原作者的路径，导致出现在了`/`目录下，如果还需要的话，进行这样的移动：
查看自己服务器的CONDA_pkgs路径
```bash
 conda config --show pkgs_dirs
```
会显示例如

```bash
pkgs_dirs:
  - /root/miniconda3/pkgs
  - /root/.conda/pkgs
```
移动`/CONDA_PKGS/`到正确的位置，并删除原始的`/CONDA_PKGS/`文件夹。

```bash
mv /CONDA_PKGS/* /root/miniconda3/pkgs
```
ChatGPT建议我写一个只读的文件夹避免再次意外写入，可供参考

```bash
mkdir /CONDA_PKGS
chmod -w /CONDA_PKGS
```
最后清理一下缓存

```bash
conda clean -a -y
```
<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/sys_conda_cache_delete.png?raw=true" width="100%">


清理完的结果为，系统盘降到了21.25%，任务完成。

<img src="https://github.com/Sihan0229/Sihan0229.github.io/blob/master/assets/autodl2125.png?raw=true" width="100%">


当然，如果已经确认不需要这些包，可以进行删除操作
```bash
rm -rf /CONDA_PKGS
```

