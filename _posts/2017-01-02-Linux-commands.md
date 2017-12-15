---
title: Linux 学习
updated: 2017-01-03 10:38
tags: [linux, cmd]
category: Linux
---

# Commands
### 硬件交互

```
lspci (| grep NVIDIA)
# 获取PCI连接的设备信息，如显卡、硬盘、显示器、无线网卡等外设
```

```
lsblk
# list block devices.
# 块设备即以block为单位进行数据读写的设备，
# 与character device相对
```

```
df -h (-T: show file system type)
# 显示mount到文件系统中的硬盘空间和分区的使用情况
```

### 文件系统、文本处理

```
locate /path/to/locate/*opencv*
# 在/path/to/locate/文件夹中搜索路径名含有opencv的文件
```

```
find /path/to/find -name *opencv*
# 在/path/to/find/文件夹中搜索文件名含有opencv的文件
```

```
grep -ri 'pattern' /path/to/grep
# 在/path/to/grep/文件夹中recursively寻找pattern
```

```
sshpass -p "password" scp ...
# pass pwd to scp
```

### 系统环境

```
export PATH=$PATH:<path1>:<path2>
# 在环境变量$PATH末尾加多个path
export LD_LIBRARY_PATH=<path1>:<path2>:$LD_LIBRARY_PATH
# 在环境变量$LD_LIBRARY_PATH最前加多个path
# 常见环境变量有$JAVAHOME, $PYTHONPATH
```

```
ldconfig -v (| grep cuda)
# 扫描目录并显示搜索到的动态链接库
```

# RAID

RAID (Redundant Array of Independent Disks)，即磁盘阵列，将多个硬盘组合成一个逻辑上单独的硬盘存在于操作系统之中。包括RAID0~6多重标准。
* **RAID0** 将连续的数据在物理上存放到不同的硬盘上，这样在读写时可以多个硬盘同时读写，从而提升带宽。
* **RAID1** 用两组空间相等的硬盘，一个作为主硬盘，一个作为镜像硬盘来备份。是RAID所有标准中最安全、但是空间浪费最高的。
