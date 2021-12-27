---
title: windows 3000 4200 端口神奇的消失事件
date: 2021-12-28 00:20:25
tags:
  - windows
  - port
  - bug
  - error
categories:
  - windows
  - [code, error]
---

# :disappointed_relieved:

今天在写 react 玩具的时候发现 3000 端口始终被占用，但是在我运行`netstat -ano | find 3000`后并没有结果，也就是说此时 3000 端口并没有被占用，上网搜索之后很多答案都是指示 3000 被占用，使用`npx kill-port 3000`这样的命令`kill`掉就好了，但是很显然这样并不能解决问题。

最后终于看到了有用的帖子，这是链接:[Windows 10 無法 LISTEN Port 4200 與 Port 3000 的靈異事件整理](https://blog.miniasp.com/post/2019/03/31/Ports-blocked-by-Windows-10-for-unknown-reason)。

最后采用了如下方法解决

```powershell
netsh int ipv4 show excludedportrange protocol=tcp
```

列出了所有的保留端口，发现 3000 被保留了

那么这个时候我们要做的事情就很简单了，把 3000 的端口设为**不保留**的即可。

1. 首先关闭`wsl`，重启`winnet`服务，释放保留的*port range*

   ```powershell
   # 1. 停用 WSL 服務
    wsl --shutdown
    wsl -l -v

    # 2. 停用 Docker 服務

    # 3. 重啟 WinNAT 服務
    net stop winnat
    net start winnat

    # 4. 啟動 Docker 服務
   ```

2. 设定自己的保留部分

   ```powershell
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=3000
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=3001
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=4200
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=5000
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=5001
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=8080
   netsh int ipv4 add excludedportrange protocol=tcp numberofports=1 startport=8888
   ```

   如果想要删除也很容易

   ```powershell
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=3000
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=3001
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=4200
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=5000
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=5001
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=8080
   netsh int ipv4 delete excludedportrange protocol=tcp numberofports=1 startport=8888
   ```
