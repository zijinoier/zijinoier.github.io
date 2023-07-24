---
layout: post
title: 开发环境代码化:VSCode配置远程Docker容器作为开发环境
date: 2020-12-20 16:12:32
tags:
- Docker
- VS code
- Cloud
categories: 教程
---

> 快速设置VSCode使用远程服务器(通常是Linux)上的Docker容器来作为开发环境

## 优点

- 本地桌面干净： 只有docker客户端~
- 开发环境代码化：Dockerfile
- 切换环境只需切换容器



## 远程主机配置

### 安装 Docker Engine

参考[这里](https://docs.docker.com/engine/install/)官方文档

### Docker开启远程端口

在远程主机上：

1. 创建文件 `daemon.json` 到目录 `/etc/docker`:

   ```
   {"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}
   ```

2. 创建文件 `/etc/systemd/system/docker.service.d/override.conf`:

   ```
   [Service]
   ExecStart=
   ExecStart=/usr/bin/dockerd
   ```

3. 重启docker：

   ```
   systemctl daemon-reload
   systemctl restart docker.service
   ```
<!--MORE-->
## 本地桌面配置

### 安装 Open SSH 客户端

#### Linux/Mac

直接用包管理器安装open-ssh

#### Win7

1. 下载编译好的open ssh二进制包：[这里](https://github.com/PowerShell/Win32-OpenSSH/releases)

2. 解压到 C:OpenSSH

3. 把 C:OpenSSH 加到系统环境变量Path上（需要注销再登录才能生效）

4. 打开cmd，运行

   ```
   C:> ssh-agent
   ```

   确保能找到

#### Win10

巨硬厂从Win10开始终于向社区靠拢，系统自带open ssh了，参考这个 [教程](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)

### 启用ssh-agent

#### Linux/Mac

安装了open-ssh以后默认自动运行，如果没有:

```
eval "$(ssh-agent -s)"
```

#### Win7

`PowerShell` 窗口里面运行 （administrator 模式）:

```
Set-Service ssh-agent -StartupType Automatic
```

然后 [Windows Task Manger] —> [Services] 找到ssh-agent，启动它

#### Win10

添加了openssh以后，应该也是自动运行的

### 生成 ssh 秘钥对

运行 `ssh-keygen`

```
ssh-keygen
```

一路回车，默认生成两个文件:

- `id_rsa`
- `id_rsa.pub`

把这个`id_rsa.pub` 传送到远程主机，复制成文件： `~/.ssh/authorized_keys`
如果远程主机上这个`authorized_keys`已经存在，就添加到后面

```
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

本地运行如下命令加上私钥

```
ssh-add id_rsa
```

测试一下，无需密码直接ssh连接到远程主机:

```
ssh <user>@<host>
```

### 本地配置 Docker 客户端

#### 安装Docker

##### Linux/Mac

直接包管理器安装，就这么简单。。。

##### Windows

下面二选一

- 方法一：安装[docker desktop](https://hub.docker.com/editions/community/docker-ce-desktop-windows), — **其实不需要这么大而全**
- 方法二：下载 [docker.exe](https://github.com/StefanScherer/docker-cli-builder/releases/) 放到Path包含的路径下就行了， 比如c:windows

#### 配置本地使用远程 Docker 服务

创建一个context：

```
docker context create <context name> --docker "host=ssh://<user>@<host>"
```

切换到上面这个context：

```
docker context use <context name>
```

测试一下：

```
docker info
```

这里会输出和在远程主机上运行 `docker info` 一样的结果， 实际上这里docker本地只是一个客户端，连接到远程主机上的docker服务。

### Visual Studio Code

#### 安装 VSCode

这里 [VSCode](https://code.visualstudio.com/)
打开 VSCode， 安装插件：

- Extensions -> Search Remote -> `Remote Development`
- Extensions -> Search Docker -> `Docker`

## 开始一个项目!

> 这里有两个方式，个人觉得第一个更（省）好（事）

### 方式一： Attach Remote Host Container

#### 本地 VSCode 访问远程容器

#### 切换 Docker context

打开VSCode，按下 `ctrl+shift+p` 运行 `docker contexts use` , 选择上面创建的docker context.

#### 连接容器

按下 `ctrl+shift+p` 运行 `Remote-Containers:Attach to Running Container...`, 选择上面创建的容器名字。

连接成功后，按下 `ctrl+k, ctr+o`, 你会发现VSCode弹出的不是本地目录，而是容器内部的目录！现在VSCode只是一个客户端，一切操作都在容器中了！

尝试一下。选择上面创建的 /workspaces/newproj，新建一个main.py，保存。再去主机上看，~/newproj/main.py就躺在那。

现在，可以愉快地在容（工）器（地）里面编（搬）码（砖）了。

##### 方式二 - 微软官方推荐的[办（麻）法（烦）](https://code.visualstudio.com/docs/remote/containers-advanced)，略。