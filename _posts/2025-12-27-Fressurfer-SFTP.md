---
layout: post  
title: "【环境配置】解决下载FreeSurfer后SFTP无法登录服务器的问题"  
date: 2025-12-27 00:10 +0800  
last_modified_at: 2025-12-28 00:10 +0800  
tags: [Configuration]  
math: true  r
toc: true  
excerpt: "FreeSurfer的终端输出污染 SFTP 协议通道"
---
## 问题描述

在Linux服务器上下载[FreeSurfer](https://zhida.zhihu.com/search?content_id=274242324&content_type=Article&match_order=1&q=FreeSurfer&zhida_source=entity)并在 `~/.bashrc`中配置后，原本可以正常登录的[SFTP](https://zhida.zhihu.com/search?content_id=274242324&content_type=Article&match_order=1&q=SFTP&zhida_source=entity)突然无法登录了，换别的SFTP软件（比如[Termius](https://zhida.zhihu.com/search?content_id=274242324&content_type=Article&match_order=1&q=Termius&zhida_source=entity)、Termcc的SFTP功能）也一样无法登录。 这可能是因为安装FreeSurfer后在 `~/.bashrc`中直接配置了

```bash
export FREESURFER_HOME=/home/soft/freesurfer8.1.0/8.1.0-1
export FS_LICENSE=/home/software/FreeSurfer/license.txt
source $FREESURFER_HOME/SetUpFreeSurfer.sh
```

会导致每次SSH登录时会在终端输出

```bash
-------- freesurfer-linux-rocky8_x86_64-8.1.0-20250718 --------
Setting up environment for FreeSurfer/FS-FAST (and FSL)
FREESURFER_HOME   /home/soft/freesurfer8.1.0/8.1.0-1
FSFAST_HOME       /home/soft/freesurfer8.1.0/8.1.0-1/fsfast
FSF_OUTPUT_FORMAT nii.gz
SUBJECTS_DIR      /home/soft/freesurfer8.1.0/8.1.0-1/subjects
MNI_DIR           /home/soft/freesurfer8.1.0/8.1.0-1/mni
```

而这些输出会污染 SFTP 协议通道，导致SSH登录正常，而SFTP无法登录。

## 解决方案

修改刚刚在 `~/.bashrc`中直接配置的FreeSurfer内容，限定只在交互式shell中运行。其中 `[[ $- == *i* ]]`中的 `$-` 是当前 shell 的选项标志，当 shell 是交互式时（正常 SSH 登录），`$-` 会包含字母 `i`，当 shell 是非交互式时（SFTP/SCP 执行命令），`$-` 不包含 `i`，使得正常 SSH 登录会显示所有欢迎信息、FreeSurfer 输出，而SFTP 连接时，这些会输出信息的命令直接被跳过，SFTP 通道保持干净。

```bash
if [[ $- == *i* ]] && [[ -z "$SSH_ORIGINAL_COMMAND" ]]; then
    # FreeSurfer 8.1.0 配置
    export FREESURFER_HOME=/home/soft/freesurfer8.1.0/8.1.0-1
    export FS_LICENSE=/home/gaosihan/software/FreeSurfer/license.txt
    source $FREESURFER_HOME/SetUpFreeSurfer.sh 2>/dev/null  # 抑制错误输出
fi
```

修复后使得新配置生效

```bash
source ~/.bashrc
```

如果不确定当前是否为该原因导致的问题，可以尝试在命令行终端里尝试使用标准 SFTP 命令连接，并使用 `-v` 参数输出详细的连接过程。

```bash
sftp -v username@server.ip
```

如果输出包含

```bash
Received message too long 757935405
Ensure the remote shell produces no output for non-interactive sessions.
```

则说明SFTP 客户端向服务器请求启动 SFTP 子系统后，从服务器收到了一个长度异常的数据包（757935405 字节），而不是正常的 SFTP 协议数据，客户端无法理解，报出 Received message too long 并断开连接。这一般是 shell 的提示符、欢迎信息、或者 `.bashrc`里某个脚本的输出导致的，可以用类似的方法排查。
