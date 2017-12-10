---
title: Linux Commands in Common Use
updated: 2017-01-03 10:38
tags: [linux, cmd]
category: Linux
---

## 硬件交互

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

## 文件系统、文本处理

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

## 系统环境

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
