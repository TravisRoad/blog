---
title: dpkg
tags:
  - dpkg
  - Linux
  - Ubuntu
categories:
  - Linux
date: 2022-03-24 22:35:56
---

## dpkg 误删除 info 目录

最近开始用 `Linux` 了， 比较怂所以采用了对新手比较友好的 `ubuntu`。

在安装 `wine` 的时候手抖把 `info` 目录删除了，这个目录是用来存储 `dpkg` 的安装信息的。之后的安装都会弹出 `warning`， 显示一堆软件找不到。

应用了以下两个命令之后可以解决大部分的警告。说的简单一点就是把软件重新装了一遍。剩下的警告可以手动解决。[^1]

```shell
sudo apt-get --reinstall install `dpkg --get-selections | grep '[[:space:]]install' | cut -f1`
```

```shell
for package in $(apt-get upgrade 2>&1 | grep "warning: files list file for package '" | grep -Po "[^'\n ]+'" | grep -Po "[^']+"); do
    apt-get install --reinstall "$package";
done
```

[^1]: https://blog.wingszeng.top/recover-dpkg-info/
