---
title: 基于Hexo框架的GitHub个人主页搭建教程（前篇）
tags:
  - github
  - hexo
  - nodejs
  - rd
categories:
  - web
  - rd
date: 2022-11-24 22:00:00
---

一年前的暑假，笔者曾经对照着网上的教程，试着弄一个专属于自己的GitHub主页，但由于笔者对具体的技术细节不太熟悉，加之时间不太充裕，写作灵感又极其匮乏，于是套了个Material主题后就不了了之。

最近适逢疫情严峻，笔者无法顺利开展实验，于是决定重拾计算机的“老本行”，钻研一下如何搭建自己的GitHub主页。经过这一两天的努力，笔者终于完成了GitHub个人主页的搭建，接下来只需要对博客进行小修小补，并搞清楚版本管理与部署即可。

也许有人会问，按你的说法，用Hexo搭建GitHub个人主页，只需要一天搭框架，一天修故障，不是易如反掌的事？但换个角度想，笔者毕竟没有按部就班照着一份系统的教程来建主页，而是把网上的资料东拼西凑，才勉强搭了一个能用的，其中踩过的坑和绕过的远路，只有自己知晓。为了避免今后再费周折，笔者决定写下这篇教程，既是给自己准备一份建站的参考资料，也可以用来帮助其他初涉Hexo，希望用它搭建博客的爱好者。

既然这样，那……就开始了？

<!--more-->

### Part 0. Git是啥？GitHub又是啥？为啥要用GitHub做个人主页？

Git是一款基于快照（snapshot-based）的分布式版本控制（distributed version control）工具。“基于快照”意味着，当一个用户用Git更新文件（如一段代码）时，Git不会为每个文件保存一个版本，而是先计算文件的哈希码，并与之前的版本比较，如果哈希码相同，Git便会记下一个链接，这个链接指向上一个版本储存的文件，如果哈希码不同，Git则会创建一个快照，并保存这个快照的索引，如图0.1所示。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig0_1.png" style="zoom: 60%;" />
</center>
<center><strong>图0.1</strong>	Git的“基于快照”的版本控制思路</center>

而“分布式”意味着，任何一个开发者，都可以在自己的电脑上，存储自己编辑的当前版本的文件，以及这些文件从创建，到每一次修改，最后变成现在的本地存储版本的过程。这样，即使开发者与（存储主干版本的）公共服务器断开连接，开发者仍然能用自己的电脑，对文件进行增删与修改，并且在本地提交版本更新，等到与公共服务器重连后，再提交pull request，然后与主干版本合并，从而大大提高了开发效率（见图0.2）。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig0_2.png" style="zoom: 60%;" />
</center>
<center><strong>图0.2</strong>	Git的“分布式”存储思路</center>

虽然Git是分布式版本控制工具，但正如前面提到的那样，为了保证有一个稳定的主干版本，供不同的开发者下载与修改，必须要设立一台公共服务器，用以存储主干版本的文件。这台公共服务器，既可以由开发者亲自组装搭建，也可以来自于具有代码托管能力的商业公司（即Git server as a service），而GitHub便是其中历史悠久（2008年创立），规模最大（超过8千万的用户，超过2亿的代码仓库，其中开放仓库近3千万），也是最富盛名的Git服务平台，其简洁易用的界面、丰富的代码资源、友好的社区氛围，为搭建个人网站提供了极大的便利。

也许稍微懂行一点的朋友会问：为什么你这么推荐GitHub，而不推荐GitLab或Gitee呢？毕竟，如果不更换DNS，也不用校园网，GitHub很容易因为网络问题，无法拉取或上传代码，导致个人网页难以持续更新，而用GitLab或Gitee，则大概率可以避免这些问题。不错，GitHub确实有网络不畅的痛点（这个在之后的[疑难解答](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial_faq/)中也会提及），然而把GitLab或Gitee当GitHub的代餐，遇到棘手问题的概率更大，找不到解决方案的概率也更大，因为互联网环境下，讨论GitHub使用方法的帖子和文章，远比讨论GitLab或Gitee的要多。

此外，像Gitee之类的国内Git服务平台，还存在两个头疼的问题：其一，这些平台更多面向国内用户，国际市场开拓不大，若决定常驻这些平台，便会错过许多来自国外开发者的优质代码，以及与这些国外开发者沟通交流，切磋技艺的机会；其二，向Gitee等国内平台提交代码后，由于平台的自我审核机制，这些代码基本上不能第一时间上传并展示在平台网站上，这就导致若是提交软件源码，其他用户并不能立即享受新版本软件的便利，若是提交个人博客的文章，其他用户也不能第一时间关注。鉴于这两点，笔者建议，<strong>如果GitHub能正常登陆，且能正常拉取/提交代码，仍应将GitHub作为代码仓库与个人网页的首选。</strong>

### Part I. 前期准备：GitHub账号的申请，GitHub主页仓库的创建，以及本地Git环境的配置

#### 注册个人GitHub账号，在其中创建GitHub主页对应的代码仓库

为了建立GitHub主页，首先我们要创建一个GitHub账号，并在这个账号中创建一个代码仓库，用于存储发布在GitHub主页的文章，以及运行GitHub主页所需要的代码。为此，我们在浏览器中访问[GitHub.com](GitHub.com)，这将弹出图1.1所示的界面，点击右上角的“Sign up”按钮，GitHub将会自动跳转到注册页面。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_1.png" style="zoom: 40%;" />
</center>
<center><strong>图1.1</strong>	GitHub首页界面，注册按钮用红框标出</center>

注册页面会提示输入邮箱，一般用自己经常使用的邮箱（学校邮箱、网易邮箱、QQ邮箱皆可）。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_2.png" style="zoom: 40%;" />
</center>
<center><strong>图1.2</strong>	GitHub注册界面，提示你输入邮箱</center>

之后依次输入用户名和账号密码，点击确认，GitHub会发送认证邮件，我们只需打开邮箱，进入认证邮件，点击认证链接，即可完成注册。

注册后，我们登录GitHub账号，进入用户界面，点击右上角的加号（在你的缩略头像左侧），会弹出一个下拉菜单，点击“New repository”，就会转到代码仓库的创建页面，如图1.3所示。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_3.png" style="zoom: 40%;" />
</center>
<center><strong>图1.3</strong>	从GitHub用户界面创建代码仓库（<del>对不起我是二次元</del>）</center>

我们给即将创建的代码仓库，命名为`(your_username).github.io`，此处的`(your_username)`应与当前登录的GitHub账号名称（不是昵称！）相同，此外，还要保证这个代码仓库勾选的是“Public”而非“Private”，这样其他人才能访问你的界面。确保选项无误后，点击下方的绿色按钮“Create repository”，GitHub就会生成GitHub主页对应的代码仓库。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_4.png" style="zoom: 75%;" />
</center>
<center><strong>图1.4</strong>	把即将创建的代码仓库命名为<code>(your_username).github.io</code>，注意到命名区为红色，表明同名仓库已被创建</center>

#### 在本地环境安装Git，并以SSH方式关联本地Git与GitHub

根据Part 0的描述可知，仅仅在GitHub申请账号和建立代码仓库，而没有建立本地的Git环境，或是建立了Git环境，却没有将其与GitHub关联，仍然不能把博客页面上传到GitHub部署。因此，我们要做的下一步工作，就是在本地计算机上安装Git，然后在Git bash中登陆自己的GitHub账号，最后配置SSH公钥，便于之后拉取或上传代码时，以SSH方式关联本地Git与GitHub。

Git的官方网站是[https://git-scm.com/](https://git-scm.com/)，进入页面后会有一个大大的“Downloads”，点进去就会跳转到下载界面。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_5.png" style="zoom: 40%;" />
</center>
<center><strong>图1.5</strong>	Git的官方网站，已标出下载入口</center>

考虑到我们是在Windows下更新GitHub个人主页，这里我们直接点“Windows”，下载适用于Windows系统的Git套件。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_6.png" style="zoom: 40%;" />
</center>
<center><strong>图1.6</strong>	Git的下载界面，Windows用户点选“Windows”即可</center>

下载完毕后，双击安装包运行，勾选“同意使用协议”，之后选项全部默认，一路点击Next，等待安装程序执行完毕后，点击“Finish”完成安装。

通常情况下，Git套件安装后便能立即使用，如果不放心，可以在命令行中输入`git --version`并执行，若弹出`git version ×.×.×.windows.×`，且与安装包对应版本一致，便说明Git组件安装成功。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_7.png" style="zoom: 120%;" />
</center>
<center><strong>图1.7</strong>	验证Git是否安装成功</center>

有了Git组件，我们就可以用Git上传我们的博客文章，或是下载博客所需的主题文件，是不是离搭建GitHub个人主页又近了一步？不过别高兴太早，在正式搭建个人主页前，我们还要做一件非常重要的事，那就是在Git bash上登录自己的GitHub账号，并且为之后的登录配置SSH密钥。

那这里可能有人要问了，我每次拉取或提交代码，就想跟GitHub对着干，我偏偏不用SSH登录，就手动输入密码，可不可以？曾经很长一段时间，笔者就是这么做的，GitHub也默许通过密码登录，在本地Git与GitHub之间建立连接的行为。然而，2021年8月，GitHub宣布，[不再支持通过GitHub账户密码验证本地Git操作](https://github.blog/changelog/2021-08-12-git-password-authentication-is-shutting-down/)，这意味着任用户一千个不同意，也得使用SSH方式登录，这或许就是大公司的威严吧……

无论如何，我们还是要打开Git bash，输入如下命令，登录你的GitHub账号，也让本地Git记住自己的GitHub用户名（对应于命令中的[your_username]）和注册邮箱（对应于命令中的[your_email]）：

```bash
git config --global user.name [your_username]
git config --global user.email [your_email]
```

要注意的是，第一次登录时，Git会提示你输入密码，但出于保密因素，除“Enter your password: ”这行系统提示消息外，<strong>命令行不会显示任何字符</strong>，也不会在你输入或删除字符时用增减星号（井号）表示，<strong>因此输入密码时，最好放慢速度</strong>，避免因打字过快而输错。

下一步是生成适用于本地计算机的SSH密钥，为此要输入以下命令并运行，其中[your_email]仍然代表你注册GitHub时使用的邮箱：

```bash
ssh-keygen -t rsa -C user.email [your_email]
```

此时会依次弹出如下提示，我们只需依次摁回车即可，不需要额外操作：

```bash
Enter file in which to save the key (/c/Users/tanglab_lzh/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

整个密钥生成过程如下图所示：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_8.png" style="zoom: 75%;" />
</center>
<center><strong>图1.8</strong>	在Git bash登录GitHub账号与生成SSH密钥</center>

ssh-keygen会将生成的私钥文件`id_rsa`与公钥文件`id_rsa.pub`放在以下目录（如果没有该文件夹，ssh-keygen会自行生成一个），故只要找到用户文件夹，将其下的.ssh文件夹打开，便能找到`id_rsa.pub`：

```bash
C:\Users\(computer_username)\.ssh
```

用记事本打开`id_rsa.pub`，按Ctrl+A全选，再按Ctrl+C复制，然后按图1.9所示步骤，找到“SSH and GPG keys”选项卡，单击“New SSH key”，进入添加SSH密钥界面。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_9.png" style="zoom: 60%;" />
</center>
<center><strong>图1.9</strong>	找到“SSH and GPG keys”选项卡，进入添加SSH密钥界面</center>

添加SSH密钥界面如图1.10所示，总计有三个框：“Title”代表密钥名称，一般用好记的名称表示；“Key type”代表密钥类型，有“验证密钥（Authentication key）”与“签名密钥（Signing key）”两种选项，此处保持“验证密钥”不动即可；“Key”为密钥内容，这是添加密钥的重点，我们把刚才从`id_rsa.pub`复制过来的内容粘贴到这个框，然后点选“Add SSH key”，生成的公钥就会添加到GitHub中。

<center class="half">
<img src="creating_GitHub_pages_fig1_10.png" style="zoom: 60%;" />
</center>
<center><strong>图1.10</strong>	添加SSH密钥界面，其中的“Key”文本框要贴上<code>id_rsa.pub</code>的全部内容</center>

最后，在命令行中输入如下命令，以SSH方式登录GitHub服务器：

```bash
ssh -T git@github.com
```

如果命令行跳出`Hi [your_username]! You've successfully authenticated, but GitHub does not provide shell access.`，说明SSH密钥配置正确，你可以利用本地Git套件拉取或上传代码到GitHub。而如果命令行频繁跳出`Connection reset by [GitHub_SSH_IP] port 22`，则有可能是国内不可言说的网络环境所致，多尝试几次，或改用移动热点连接，或将端口切换至443，有可能改善此类情形。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig1_11.png" style="zoom: 75%;" />
</center>
<center><strong>图1.11</strong>	验证本地计算机能否通过SSH访问GitHub，注意到GitHub的22端口多次拒绝访问，可能与网络环境有关，为此笔者将登录端口切换至443</center>

### Part II. Node.js环境的安装，以及Hexo框架的初步搭建

#### 在Windows系统安装Node.js，并设置国内更新源

建立好GitHub主页仓库，并在本地配置好Git环境后，我们就可以依次安装Node.js和Hexo框架了。考虑到笔者主要在Windows上撰写博客文章，故以Windows环境为例，介绍Node.js和Hexo的安装流程，其他系统（如Mac OS X和Linux）的安装流程，可以参考[Hexo中文文档](https://hexo.io/zh-cn/docs/)，这里暂不展开论述。

访问[Node.js官方网站](https://nodejs.org/en/download/)（如果无法访问，可以访问[npmmirror中国镜像站的Node.js镜像](https://npmmirror.com/mirrors/node/)），在“Windows Installer”一行，根据系统的位数点选（一般选64位下载，如图2.1所示），浏览器就会将对应位数的Node.js安装包下载到本地。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig2_1.png" style="zoom: 60%;" />
</center>
<center><strong>图2.1</strong>	Node.js官方网站的下载页面，Windows用户通常点选64位安装包即可</center>

双击安装包运行，勾选“同意使用协议”，之后选项全部默认，一路点击Next，等待安装程序执行完毕后，点击“Finish”完成安装。

为确保Node.js的所有组件均已正常安装，我们要打开命令提示符（Windows Powershell或Windows Terminal亦可），分别输入如下命令并运行：

```bash
node -v
npm -v
```

如果分别跳出安装包对应的版本号（如图2.2所示），说明Node.js成功安装，否则应卸载Node.js，重新运行安装包进行安装。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig2_2.png" style="zoom: 120%;" />
</center>
<center><strong>图2.2</strong>	成功安装Node.js后，查询node和npm的版本号，会得到与安装包一致的结果</center>

考虑到国内与Node.js软件包服务器的连接极不稳定，笔者推荐将npm源替换为npmmirror镜像源（即原阿里源，个中关系参见[疑难解答](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial_faq/)的“Node.js与Hexo部分”），这一步骤非常简单，只需在命令行输入下列命令，回车执行即可：

```bash
npm config set registry https://registry.npmmirror.com
```

当然，npmmirror还提供了另一种换源方法，就是用npmmirror定制的包管理工具cnpm代替npm，具体命令为：

```bash
npm install -g cnpm --registry=https://registry.npmmirror.com
```

不过，笔者对cnpm并不熟悉，导致在后续的hexo安装时频频报错，最后决定放弃使用cnpm，改回原生的包管理工具npm。此外，也有知乎网友指出，换用cnpm后，因为cnpm奇葩的存放位置（hexo的软连接没有放在`/usr/local/bin`，而是放在`/var/root/.npm-global/lib/node_modules/hexo-cli/bin`），导致[即使正确安装hexo-cli，也找不到hexo命令](https://www.zhihu.com/question/332969173)。<strong>综合以上两点，笔者建议直接用npm config更换npm源，而非采用cnpm来换源（毕竟安装cnpm时也要修改registry）</strong>。

#### 在本地部署Hexo框架，生成初始GitHub主页

接下来，在你的工作区中，创建一个新文件夹，用以存放发布在GitHub主页的文章，以及运行GitHub主页所需要的代码（比如笔者就将其放在`C:\Workbench\chemlzh.github.io`）。然后，运行命令行（如果文件夹在C盘上，一定要以管理员权限运行命令行！否则没有读写权限！），并跳转到刚创建的文件夹。之后，输入下列命令，安装hexo的客户端版本：

```bash
npm i hexo-cli -g
```

正常安装后，命令行通常会输出如下提示

> hexo-cli@(<i>version-number</i>)
> added ×× packages from ×× contributors in ××s

或是如下提示

> All packages installed (×× packages installed from npm registry, used ××s(network ××s), speed ××MB/s, json ××(××kB), tarball ××MB)

如果没有看到类似于上述的提示，而是弹出红色或棕黄色的提示，并含有“error”、“unsuccessful”等字样，则表明hexo-cli没有安装成功（此时要么没有换成国内源，要么在安装过程中出现了强制中断、环境漏配等问题）。

确保hexo-cli安装成功后，依次输入以下命令并运行，让npm在新文件夹中初始化hexo环境，同时安装运行的必要组件：

```bash
hexo init .
npm install
```

如果hexo的环境变量配置正确，运行上述命令后，新文件夹中会成功生成如下文件：

```bash
.
./_config.yml  # 网站配置信息
./package.json # 在后续修改中安装的npm包的信息
./package-lock.json # 初始化hexo环境时安装的npm包的信息
./scaffolds # 模板文件夹
./source # 用户文件夹，存放博客文章、图片等
./source/_draft # 存放尚未发布的草稿
./source/_posts # 存放正式发布的文章
./themes # 主题文件夹
```

最后，输入如下命令并依次运行，随后在浏览器中打开[http://localhost:4000](http://localhost:4000)，便能在本地看到自己的博客界面。

```bash
hexo g
hexo s
```

不过，由于是首次创建，且尚未添加任何主题模板，因此显示的是hexo默认风格界面，且归档文章中只有一篇“hello world”。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig2_3.webp" style="zoom: 90%;" />
</center>
<center><strong>图2.3</strong>	首次创建hexo环境时，在本地显示的博客界面（图片引自<a href="https://zhuanlan.zhihu.com/p/111614119">随风的文章</a>）</center>

<strong>备注：</strong>如果按[Windows 下 Node.js 的安装图文教程](https://blog.csdn.net/github_39655029/article/details/105397485)，修改了包存放路径，那么在运行`hexo init .`时，将会提示“hexo”命令不存在。此时，应依次打开“Windows设置”→“系统”→“关于”→“高级系统设置”→“环境变量”，将hexo-cli目录下的bin添加至PATH中（以笔者为例，便是在全局变量的PATH中，添加一条`C:\Program Files\nodejs\node_global\node_modules\hexo-cli\bin`），然后点击“确定”。随后，重新打开终端（若提示无读写权限，需以管理员身份打开），进入博客文件夹，重新运行`hexo init .`，此时该文件夹将正常生成hexo框架文件。在大部分情况下，该策略对未修改npm包存放路径，但提示“hexo”命令不存在的情形也有效（尚未测试）。

### Part III. 将Material主题应用到GitHub个人主页

#### 为什么不采用Hexo的默认主题，而采用自定义主题？

请诸位回到Part II的结尾部分，看一看Hexo的默认风格界面吧。诚然，这个界面以白色与灰色为主色调，没有东拼西贴的小广告，与10-20年前绝大多数网站相比，可谓是简洁清爽。但仔细一想，总感觉缺了点什么——或许，对某些人而言，默认界面缺的是鲜明的色彩，扁平化的界面设计；或许，对某些人而言，默认界面缺的则是黑白的对比，棱角分明的冷艳感。

除渲染风格较为乏味外，Hexo的默认风格界面，还缺了许多有意思的功能选项：例如，我想给每篇文章加一张亮眼的题图，可是默认界面的文章根本不会显示题图；我想把归档日志和最近发表折叠到边栏，可是默认界面显示的是顶栏，而且主界面中并没有把归档日志收纳起来……

显然，建站者日益增长的审美与实用需求，与默认主题较为贫瘠的生产力之间，产生了巨大的矛盾，这促使广大的开发者对Hexo主题进行个性化修改，自定义主题因此诞生。

想要色彩鲜明的扁平化设计？Material主题就很适合你：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_1.png" style="zoom: 40%;" />
</center>
<center><strong>图3.1</strong>	Material主题演示页（<a href="https://iblh.github.io/material-demo/">原网页见此</a>）</center>

想要遵循KISS原则的教诲，在黑白二色间自由翱翔？那就尝试一下NexT主题：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_2.png" style="zoom: 40%;" />
</center>
<center><strong>图3.2</strong>	NexT主题演示页（<a href="https://theme-next.org/">原网页见此</a>）</center>

甚至，看官想来点二次元？也有大手子做好了明日方舟主题：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_3.png" style="zoom: 40%;" />
</center>
<center><strong>图3.3</strong>	Arknight主题演示页（<a href="https://ark.theme.yueplus.ink/">原网页见此</a>）</center>

可以说，除了极其个性化的设计外（这得自己动手丰衣足食），任何你想到的界面风格，都可以在GitHub上找到对应的主题源码，只要正确的下载与安装，再加上适当的调参，便能将单调的Hexo初始页面，打造为炫酷的个人主页！

好了，废话少说，让我们看看怎么在初始的Hexo框架上，添加Material主题吧。至于为什么要采用Material主题，笔者在这一节的结尾给出了充分的理由，请诸位务必往下阅读哦！

#### 如何添加Material主题

Material主题目前由iblh等人开发与维护，其所有发布版本均可从[GitHub Releases](https://github.com/iblh/hexo-theme-material/releases)中获取，这里我们由两种选择：一是下载发布较早，但Bug较少的1.5.2版本，二是下载最新发布，但存在较多Bug的1.5.6版本。如果是Hexo新手，建议下载1.5.2版本，以避免之后生成页面失败的问题；如果对自己的编程纠错能力有一定把握，可以选择1.5.6尝鲜。进入[GitHub Releases](https://github.com/iblh/hexo-theme-material/releases)，点击“Source code (zip)”，就会自动下载对应版本的Material主题包，如图3.4所示

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_4.png" style="zoom: 40%;" />
</center>
<center><strong>图3.4</strong>	在Releases页面下载1.5.6版Material主题包（旧版本下拉即可找到）</center>

将下载的1.5.6版Material主题包解压缩，得到一个叫“hexo-theme-material-1.5.6”的文件夹，考虑到我们不需要标注版本号，故更名为“hexo-theme-material”。然后，打开本地个人主页的根目录，找到“themes”文件夹并进入，再将“hexo-theme-material”复制到“themes”文件夹下。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_5.png" style="zoom: 75%;" />
</center>
<center><strong>图3.5</strong>	进入博客根目录的“themes”文件夹，将更名后的Material主题文件夹复制到“themes”下</center>

刚刚解压的Material主题包，还没有添加任何格式设置，因此我们要打开`themes/hexo-theme-material`，找到_config.template.yml，并在原地拷贝一份，旧有的_config.template.yml保持原样（或改名为_config.template_duplicated.yml，以防与根目录下的_config.template.yml冲突），新拷贝的改名为_config.yml，它代表针对Material主题的设置（而非Hexo框架的设置）。

之后，我们回到个人主页的根目录，打开其下的_config.yml（代表Hexo框架的设置，注意不要与主题文件夹下的_config.yml混淆），修改图3.6红框所示的内容。其中，title代表个人主页的标题；author代表博客文章的默认作者；language代表网页界面默认使用的语言文字，中文用户一般填“zh-CN”即可；url为博客网址，对于GitHub个人主页而言，如果不额外绑定域名，一般为`https://(your_username).github.io/`。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_6.png" style="zoom: 90%;" />
</center>
<center><strong>图3.6</strong>	编辑个人主页根目录_config.yml的部分参数</center>

最后，我们下拉到`theme:`选项，填入放在themes文件夹下的Material主题包的名字（此处为“hexo-theme-material”），如图3.7所示，便可以保存_config.yml并退出了。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig3_7.png" style="zoom: 90%;" />
</center>
<center><strong>图3.6</strong>	在根目录_config.yml中启用Material主题</center>

回到个人主页的根目录，点下右键，选择菜单中的“Git bash here”，启动Git bash，依次输入以下命令并运行。

```bash
hexo clean # 清除public文件夹下的所有文件
hexo g # 将网页生成在public文件夹下
hexo s # 启动本地服务器预览
```

如果使用的是1.5.2版，网页源代码将会正常生成，此时，在浏览器中打开[http://localhost:4000](http://localhost:4000)，应该就能得到与图3.1相似的网页界面了。（如果是1.5.6版，可以参考[疑难解答](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial_faq/)的“Material主题部分”，修改`/(your_blog_name)/themes/hexo-theme-material/layout/_widget`下的dnsprefetch.ejs，然后重新生成网页）

<strong>注意：</strong>修改_config.yml时，一定要注意书写格式！首先，选项名称后的冒号一定得是英文的冒号`:`，而不是中文的冒号`：`。其次，选项名称与冒号之间没有空格，而冒号与后面的选项取值之间，则有一个半角空格。<strong>任何没有按照上述标准添加或修改的选项，其导致的无法生成网页的html代码等后果，均由修改者承担！</strong>

#### 私货：为什么选择了Material主题？

**太长不看版：**因为NexT主题太过朴素，适合于程序员总结经验，却不适合于向他人展示成果；而Material主题色彩鲜明，界面扁平化，还能给每篇文章添加自选题图。

**详细解释：**如果你搜索“常用/好看的Hexo主题”，十有八九会有人力荐NexT主题，因为它界面简洁，只有黑白两种配色，没有多余的按钮，也没有冗杂的颜色，非常适合程序员阅读。而从GitHub的Star数看，最受欢迎的Hexo主题，NexT当之无愧（新版本7.8K，作为对比，Material主题的Star数为4K）。不过，从调动阅读兴趣的角度看，黑白配色也是一把“双刃剑”——任何枯燥的技术性内容，搭配上一眼望不到头的白底黑字或黑底白字，都将是效果拔群的“催眠模因”，刚开始可能还不太在意，读了5-10分钟才发现：这篇文章怎么还没结束！太无聊了，不看不看！

相反，Material主题运用了多种饱和度较高的颜色，且色彩搭配非常和谐，这在一定程度上减轻了阅读的困倦感；扁平化的设计风格，则将杂乱无章的超链接，统一为方正而易于识别的按钮，有一种规整、轻盈的“赛博美感”。此外，Material还为每一篇文章，添加了随机选取的题图。倘若博主觉得不太满意，还能结合第三方插件，将题图更换为萌萌哒的二次元同人图，既提升了阅读效率，又能温暖一下阿宅孤单的心灵呢（笑）。

（当然，也有人认为，色彩过于鲜艳，或是添加的二次元同人图太多，时间长了反倒会有生理与精神上的双重疲劳，因此不喜欢花花绿绿的风格。这个问题仁者见仁智者见智，至少对于我而言，NexT风格的网页不太能调得动我的兴趣）

### 参考资料

**Part 0：**

[版本控制工具(CVS、SVN、GIT)简介](https://blog.csdn.net/wanglin_lin/article/details/48845343)

**Part I：**

[Git 远程仓库(Github)](https://www.runoob.com/git/git-remote-repo.html)

[Github 简明教程](https://www.runoob.com/w3cnote/git-guide.html)

[Git password authentication is shutting down](https://github.blog/changelog/2021-08-12-git-password-authentication-is-shutting-down/)

**Part II：**

[Hexo中文文档](https://hexo.io/zh-cn/docs/)

[Windows 下 Node.js 的安装图文教程](https://blog.csdn.net/github_39655029/article/details/105397485)

[安装hexo 提示 command not found，怎么解决？](https://www.zhihu.com/question/332969173)

**Part III：**

[Material Theme Docs](https://neko-dev.github.io/material-theme-docs/#/)

[hexo-theme-material | Easy Hexo](https://easyhexo.com/2-Theme-use-and-config/2-5-hexo-theme-material/)

[有哪些好看的 Hexo 主题？](https://www.zhihu.com/question/24422335)

[Hexo 好看的主题推荐](https://blog.csdn.net/zgd826237710/article/details/99671027)