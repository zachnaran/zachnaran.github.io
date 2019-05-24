---
layout: post
title: "启用Windows 10内置Linux子系统以及xshell连接"
tags: [Windows,linux,ubuntu]
comments: true
---

查看方法：
windows键 + R 打开运行 输入 dxdiag 回车
在操作系统那行可以看到 版本17763  google 查询后 Windows 10 1809版本
各版本的区别自行 google 此教程仅适用于 Windows 10 1809版本

---

## 1.首先查看电脑系统版本
```
查看方法：
windows键 + R 打开运行 输入 dxdiag 回车
在操作系统那行可以看到 版本17763  google 查询后 Windows 10 1809版本
各版本的区别自行 google 此教程仅适用于 Windows 10 1809版本
```
![20190523164101169_24319](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190523164157032_10741.png)

## 2. 打开开发人员模式
```
如图所示
```
![控制面板 -E 程序 -E 程序和功能 -E 启用或关闭Windows功能 -E 适用于Linux的Windows子系统”的选项](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190523164553218_10746.png)

## 3.开启Linux子系统功能
```
如图所示
```
![=840x](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190523164938736_8518.png) 

## 4.重启电脑
```
开启开发人员模式后系统会提示您要重新启动电脑的
```
## 5.安装Linux
```
cmd启动命令行，键入 “ bash ”，系统可能会提醒你 该子系统没有已安装的分发版 ，这时候你可以去 Microsoft Store 来安装分发版。

```
![安装linux](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190523165200870_1216.png)

```
打开应用商店 搜索 ubuntu 点获取，安装即可
```
![打开应用商店 搜索 ubuntu 安装即可](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524111233544_30037.png)

## 6.初始化Linux
```
第一次运行时，会打开一个控制台窗口，并提示时等待一段时间完成安装
安装完成后，提示您需要创建新的账户和密码
```
![安装完成后，提示您需要创建新的账户和密码](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524111514899_8217.png)

## 7.在 xshell 下连接 ubuntu
```
首先需要先修改一下 一些ssh 配置

按 Windows + X 打开 Windows PowerShell（管理员）
输入 wsl 回车 进入 ubuntu

```
### 1.卸载 ssh server
```
sudo apt-get remove openssh-server
```
### 2.安装 ssh server
```
sudo apt-get install openssh-server
```
### 3.修改 ssh server 配置
```
sudo vim /etc/ssh/sshd_config

```
#### 需要修改以下几项：
```
Port 2222  #默认的是22，但是windows有自己的ssh服务，也是监听的22端口，所以这里要改一下
UsePrivilegeSeparation no
PasswordAuthentication yes
AllowUsers youusername # 这里改成你登陆WSL用的

```
### 4.启动 ssh server
```
sudo service ssh --full-restart

```
### 5.让配置一劳永逸生效
```
touch service.sh
sudo chmod  777 service.sh
vi service.sh
文件写入 sudo service ssh --full-restart  然后 :wq 保存并退出
每次开机后 只需在cmd下执行一次 sh service.sh 即可使用 xshell 进行连接
```
### 6.连接 ubuntu
```
配置完成，使用 xshell 进行连接
输入 ifconfig 查看IP地址
```
![wsl](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524113300844_26443.png)

```
执行 sh service.sh 回车
```
```
打开 xshell 
新建 - 输入名称 和主机ip 如上图：127.0.0.1

```
![微信截图_20190524113503](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524114839879_9107.png)
![输入ubuntu设置的用户名和密码](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524114935239_6696.png)

```
输入ubuntu设置的用户名和密码 点确定
选择 127.0.0.1 进行连接
```
![选择 127](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524115415111_16204.png)
![连接](https://raw.githubusercontent.com/zachnaran/zachnaran.github.io/master/images/2019-05-24-windows10-linux-ubuntu/20190524115433856_19602.png)
```
连接成功！
```
## 8.更新并升级
```
sudo apt update && sudo apt upgrade
```