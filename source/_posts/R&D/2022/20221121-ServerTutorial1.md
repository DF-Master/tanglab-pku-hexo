---
title: Introduction：从零开始的轻松运维实践
date: 2022-11-21
tags:
- rd
categories:
- rd
---


---

> 筌者所以在鱼，得鱼而忘筌；言者所以在意，得意而忘言
> 

---

<!--more-->

# 序

从2020因为疫情居家开始，笔者第一次因为“学生免费试用服务器”接触到了云服务器，再之后学习linux，接触Rasberry Pi，到成为Tanglab的主力运维，也算是将近3年。不得不说运维的过程便是不断学习的过程，时代太快了，熟悉的工具、流程和技术转眼就跟不上需求，但体会到自己的“雕虫小技”被人夸赞的时候，还是能获得些许的满足感。

对笔者这种业余人士来说，不需要学习底层原理，运维只是学习工具的过程。掌握数倍于人的工具，就能获得无法取代的价值。

正所谓“得鱼而忘荃，得意而忘言”，摸索新事物本身就是运维的乐趣所在。因此，继承上一篇[Tutorial 面向科研人员的Markdown应用教程](http://tanglab.cn/2022/11/04/R&D/2022/Tanglab%20Tutorial%20%E9%9D%A2%E5%90%91%E7%A7%91%E7%A0%94%E4%BA%BA%E5%91%98%E7%9A%84Markdown%E5%BA%94%E7%94%A8%E6%95%99%E7%A8%8B/) ，本篇笔者将就如何以最低的门槛、最有趣的程度入门运维进行一个介绍。

# 寻找一个动机

笔者身边的很多朋友，无论背景，多少都有过一些运维的经历。事实上这也是大多数人的情况，看到别人的操作，不禁提出“我也想试试”。所以大多数人都不缺少相关的技术，只是缺少一个动机罢了。

这边，笔者给出了一些了解到的、常见的动机：

- **想建立一个自己的网站**。建立BLOG比维护公众号容易100倍且强大100倍。
- **搭一个游戏服务器**。笔者朋友们建立过Minecraft、饥荒、泰拉瑞亚、FVTT的服务器，因为是私服，所以可以做很多insane的改动。
- **流量代理**。但是这个往往都是自建自用，很少会拿来分享。
- 建立**一些WEB工具**。尤其是对于计算机专业同学来说，这无疑是很好的实践机会。
- **搭建云盘**。最有名的就是群晖NAS，对小体量的云端存储的话树莓派也能胜任，很多时候会直接做成“云主机”。
- **搭建云计算工具**。对非专业人士来说用的不多，最多的应该是NovelAI炼丹炉了。
- **路由器**。属于笔者的知识盲区，但定制化路由器完全是可以做的。

根据动机的不同，方案也随之改变。本文主要是面向有意愿学习相关技术、从事实验性学科的读者的，所以笔者根据“轻松”的原则，以腾讯云为例，以下顺序进行实践简介，相信通过1-2天的实践，读者就能迅速入门，体会到维护一个私有服务器的乐趣。

# 简介

## **Connect My Server**: 连上我的服务器

首先，笔者先假设读者已经拥有了一个[腾讯云的服务器](https://cloud.tencent.com/act/pro/2022double11?fromSource=gwzcw.6984642.6984642.6984642&utm_medium=cpc&utm_id=gwzcw.6984642.6984642.6984642&gclid=EAIaIQobChMIi7Oy0fub-wIV0lBgCh0AegadEAAYASAAEgIKWvD_BwE) ，当然读者把它替换成局域网下的Server、阿里云、华为云、亚马逊云都是一样的。腾讯云服务器学生免费试用，而且新人第一年2核轻量应用服务器只要50元（2022.11.07），笔者认为对新人很友好，下面以此为例说明使用方法。作为服务器，ubuntu22.04 Server 64bit 是目前比较新且主流的环境。

- 如非必要，笔者不建议购买大陆的服务器，大陆服务器的网站服务需要管局审核、公安备案，过程复杂冗长，对于初学者，购买境外服务器在下载速度和内容管理上都更便利。

初学者最好的学习途径就是阅读图文并茂的[官方文档](https://cloud.tencent.com/document/product/1207/44642) ，其详细程度让人感概这也许不是面向高中生、而是以小学生都能读懂并学会的标准进行的写作。学习用WEBUI操作的同时，也可以一并把防火墙的端口打开、默认的登陆密码改掉，方便后续的操作。

不过，考虑到微信很难用，用微信登陆腾讯云用WEBUI的体验并不好，所以本地化的登陆方案也应该被考虑。你需要的，就是记住账号、密码、服务器IP，然后打开他们：

- 使用系统原生的远程登陆软件，比如windows的Powershell,win11用户甚至用默认的windows terminal体验更佳
- 一些专用的远程软件，比如mobaXterm，因为自带X11服务，所以在需要UI的远程工作上很实用
- 常用的ssh工具，比如winscp, xshell, 这个完全看个人喜好

## **Order My Server**: 如身使手,如手使指

如果你对Linux有足够多的使用经验，完全可以跳过这一节。不过，为了便利初心者，这边有一份最基础的[linux入门教程](https://nuaa-pa.gitee.io/gitbook/others/linux-manual.html)，笔者第一次读时也学到了不少新内容。

但是，掌握这些命令只是学习了使用的基础，却不足以让服务器变得“好用”。就像Everything之于windows一样，我们需要一些微小的工作来让工作环境更整洁、易用：

- 使用一个更美观、方便的shell: [zsh](https://wiki.archlinux.org/title/Zsh_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)) 或者 [fish](https://www.ruanyifeng.com/blog/2017/05/fish_shell.html)
- 安装一些更易用的Linux工具：[Modern-Unix](https://github.com/ibraheemdev/modern-unix)
- 换源：国内网站下载国外资源往往较慢，需要使用镜像，不过腾讯云一般默认已换源，就不需要这一步了

最后，一般还需要更新环境：关于这些内容，可以阅读笔者的其他文章[Tutorial:从零开始的Linux使用【1】](https://www.notion.so/Tutorial-Linux-1-105c4409942b413bb1e0ad769b81e62a) [Tutorial:从零开始的Linux使用【2】](https://www.notion.so/Tutorial-Linux-2-f2f48a18a4b043d4898185f114616adb) 。相信在阅读之后，你对linux的使用已经有了初步的理解。

## **域名**：一个好名字能让朋友更容易记住你

服务器最重要的一点就是需要一个固定IP，而云服务器一般都绑定了一个固定的公网IPV4地址，这无疑是极其方便的。如果对IPV4、DNS这些互联网的基本概念还不理解，可以参考少数派上的[小白也能看懂的网络基础系列](https://sspai.com/post/65470) 。

一个好域名不但可以让你的朋友更容易记住你，也可以用作一个服务器不同功能的区分：比如我的.tech域名就指向我的互联网工具，而.space则指向我的游戏服务器。此外，当服务器迁移时，只需要更改域名解析的地址，就可以维持web服务的稳定。因此，一个好域名是必不可少的。

当然，在国内的大厂商[购买域名](https://console.cloud.tencent.com/domain/all-domain)肯定是需要审核和备案的，不过海外域名笔者不熟，不作介绍。

## Github：同步率100%

关于github，网上的教程实在太多，这边就不狗尾续貂了。学习Github最直接的方法当然是阅读[官方文档](https://docs.github.com/cn/get-started) 。如果想进一步学习git的命令行使用（虽然使用网页版的UI界面或者[Github-Destop](https://desktop.github.com/)也能实现），那也有对应的[中文教程](https://www.runoob.com/w3cnote/git-guide.html) 。不过在实际生产中命令行的使用也不频繁，所以笔者更推荐将[Github-Destop](https://desktop.github.com/) 作为生产力工具。

笔者主要想讲的，是将Github应用于科研生产之中。在[Github开源项目排行榜](https://www.githubs.cn/top) 上可以看到许多广受好评的项目，比如：

- [免费编程电子书籍](https://github.com/EbookFoundation/free-programming-books/blob/main/books/free-programming-books-zh.md)
- [代码编辑器Vscode](https://github.com/microsoft/vscode)
- [Linux内核源码](https://github.com/torvalds/linux)
- [Git源码](https://github.com/git/git)
- [内网穿透代理应用FRP](https://github.com/fatedier/frp)
- [命令行纠错的小工具thefuck](https://github.com/nvbn/thefuck)

当然，除了上述有名的大项目和各类实用的工具，github上也不乏一些有意思的仓库:

- [996.ICU收集996工作讨论的仓库](https://github.com/996icu/996.ICU)
- ~~赛博炼丹~~AI绘图 [SDWebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui)
- 一款好看的中文字体[霞鹜文楷](https://github.com/lxgw/LxgwWenKai-Lite)
- [免费多平台音乐播放器Listen1](https://github.com/listen1/listen1_desktop)

总之Github上好玩的东西固然很多，在下载以外，github上还提供了GithubPages,可用于搭建网站,[用法](https://sspai.com/post/54608)也非常简单。

此外，笔者推荐也创建一个和名字一样的库，在其中写一份README.md。这种同名库不但可以作为个人主页/Github封面使用，还可以创造一些有意思的特效。[参考](https://juejin.cn/post/6854573219660660743)

## Homepage: 写点东西

在所有网页服务中，最基本的就是个人主页。一个好的个人主页可以作为简历、资料站、发布页……今天搭建个人主页的方式日趋多样，既可以部署到Github、Netflify等网站；也可以在个人主机或者Server上发布，利用内网穿透、DDNS或者公网IP进行访问。此外，互联网平台也为个人主页的搭建提供了十分详尽的教程。

目前主流的个人主页都是基于[HEXO](https://hexo.io/zh-cn/docs/index.html)或者[HUGO](https://www.gohugo.org/)进行搭建，笔者只对前者有所了解。HEXO作为国内比较主流的博客框架有较好的教程支持，比如[EASY-HEXO](https://easyhexo.com/) 就做到了小鲑鱼也能看懂。此外，腾讯云官方文档也对部署HEXO有着详细的[教程](https://cloud.tencent.com/document/product/1210/43365)。

需要强调的是，**部署hexo不需要任何代码基础**，而且完成部署后就会对git，markdown, 云服务，网页框架有一个基础的认识，可谓是性价比极高的学习体验，强烈推荐大家试一试！下面是Tanglab成员的一些个人主页：

- [http://jiangyida.top/](http://jiangyida.top/)
- [https://chemlzh.github.io/](https://chemlzh.github.io/)

sudo npm config set registry [https://registry.npmmirror.com](https://registry.npmmirror.com/)

# 应用示例

## 例1：在腾讯云20.04ubuntu服务器上建立基本的运行环境

换源

https://github.com/SuperManito/LinuxMirrors

```bash
sudo su root
bash <(curl -sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh)
```

### 更新包管理器

apt,python

```bash
sudo apt update
sudo apt-get update
sudo apt install python3-pip -y
sudo apt-get install python3-setuptools -y
sudo pip install --upgrade setuptools
sudo python3 -m pip install --upgrade pip
# python 软链接到 python3 
sudo ln -s /usr/bin/python3 /usr/bin/python
python -V

```

nodejs

https://github.com/nodesource/distributions

```bash
sudo apt -y update
# 下面请自行修改版本 
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
npm -V
node -v
# npm换源 https://github.com/0fengzi0/Blog/issues/27

# 可以安装n作为nodejs版本管理器。n更简单，但是nvm对全局多版本兼容性支持更好。
npm install -g n
n latest
```

### 常用linux工具

```bash
sudo apt-get install fish -y
#更改默认shell: sudo chsh -s /usr/bin/fish

```

### modern-unix

[https://github.com/ibraheemdev/modern-unix](https://github.com/ibraheemdev/modern-unix)

```python

sudo npm install -g tldr
# snap 也是一个linux包管理器
# snap有一点不好，会装在用户目录下/home/jiangyd,但是snap不限制发行版而且不安装依赖，只是稍微大一些
sudo apt install snapd
sudo snap install bottom
sudo  npm install gtop -g
sudo apt install fzf
sudo snap install procs

```

## 例2：搭建hexo博客(ayer主题)

### hexo install（theme ayer）

```bash
# sudo npm config set registry http://registry.npmmirror.com
sudo npm install -g hexo-cli
hexo -V
mkdir <your_blog_name>
hexo init <your_blog_name>
cd blogtest/
ls -al
hexo s
# 打开IP:4000 已经可以看到网页
```

ayer

```bash
npm i hexo-theme-ayer -S
# Modify theme setting in _config.yml to ayer
# theme: ayer
hexo s
# 打开IP:4000 已经可以看到网页的主题更改
# 根据下列网址自行修改需要的配置
# https://easyhexo.com/1-Hexo-install-and-config/1-3-config-hexo.html#%E9%85%8D%E7%BD%AE-hexo-2
# https://github.com/shen-yu/hexo-theme-ayer

# 将网页部署到github及其他平台（git使用有困难的，可以跳过或者去学习下git）
# https://easyhexo.com/1-Hexo-install-and-config/1-4-deploy-hexo.html#%E9%83%A8%E7%BD%B2%E5%88%B0-github
# 进一步地，可以把整个文件包都打包成Github仓库并上传，这样每次下载后只需要npm install就可以完成异地部署了
```

## 例3：使用frp进行内网穿透

这部分主要对frp进行介绍。

frp 是一个专注于内网穿透的高性能的反向代理应用，支持 TCP、UDP、HTTP、HTTPS 等多种协议。可以将内网服务以安全、便捷的方式通过具有公网 IP 节点的中转暴露到公网。frp具有配套[中文文档](https://frps.cn/11.html)。下面是代码示例，比起参考示例，更推荐阅读文档。

[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

### FRPS

```python

#登陆服务器(root账号)
wget https://github.com/fatedier/frp/releases/download/v0.44.0/frp_0.44.0_linux_386.tar.gz
tar -zxvf frp_0.44.0_linux_386.tar.gz
vim frp_0.44.0_linux_386/frps.ini
```

```python
[common]
bind_port = 7000
vhost_http_port = 7860
dashboard_addr = 0.0.0.0
dashboard_port = 7001
# 7860是为了适应StableDiffusionWebui

[ssh6001]
type = tcp
listen_port = 6001

[ssh6002]
type = tcp
listen_port = 6002

```

```python
sudo vim /lib/systemd/system/frps.service
# 改版本号
sudo systemctl daemon-reload
sudo systemctl restart frps.service
sudo systemctl status frps.service

# http://tanglab.cn:7001/ 可以看运行状况
```

```bash
[Unit]
Description=frps Server
Documentation=https://github.com/fatedier/frp
After=network-online.target

[Service]
ExecStart=/home/ubuntu/frp_0.44.0_linux_386/frps -c /home/ubuntu/frp_0.44.0_linux_386/frps.ini
Type=simple
User=root
Group=nogroup
WorkingDirectory=/tmp
Restart=on-failure
RestartSec=60s

[Install]
WantedBy=multi-user.target
elias=rc-local.service
```

### frpc

```python
cd /smb/open-apps/
wget https://github.com/fatedier/frp/releases/download/v0.44.0/frp_0.44.0_linux_386.tar.gz
tar -zxvf frp_0.44.0_linux_386.tar.gz
chmod 777 -R  /smb/open-apps/frp_0.44.0_linux_386/
vim /smb/open-apps/frp_0.44.0_linux_386/frpc.ini
```

```python
[common]
server_addr = <frps domain>
server_port = 7000
log_file = /var/log/frpc.log
log_level = info
log_max_days = 3
login_fail_exit = true

[sshb205]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6001

[web7860]
type = http
local_port = 7860     // 本地运行端口
custom_domains = <frps domain>
```

```python
sudo vim /lib/systemd/system/frpc.service

```

```python
[Unit]
Description=frpc cilent
Documentation=https://github.com/fatedier/frp
After=network-online.target

[Service]
ExecStart=/smb/open-apps/frp_0.44.0_linux_386/frpc -c /smb/open-apps/frp_0.44.0_linux_386/frpc.ini
Type=simple
User=root
Group=nogroup
WorkingDirectory=/tmp
Restart=on-failure
RestartSec=60s

[Install]
WantedBy=multi-user.target
elias=rc-local.service
```

```python
sudo systemctl daemon-reload
sudo systemctl restart frpc.service
sudo systemctl enable frpc.service
sudo systemctl status frpc.service
# https://github.com/xiaoming2028/FreePAC/wiki/2022%E5%B9%B4%E6%9C%80%E6%96%B0VPS%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC%E6%90%AD%E5%BB%BAV2ray%E8%AF%A6%E7%BB%86%E5%9B%BE%E6%96%87%E6%95%99%E7%A8%8B
```

# 写在最后

总结笔者不长的运维经历，经验如下：

- 认真阅读文档，就和阅读仪器说明书一样必要仔细
- 不吝提出问题，github上可以是issue，甚至很多工程师会耐心地一对一联系，可以从他们身上学很多东西
- 尝试不同的方案，并做好记录，尤其是从过去的“单打独斗”到实验室中的“走向合作”，一份记录无论是成功或失败，都具有相当的价值。