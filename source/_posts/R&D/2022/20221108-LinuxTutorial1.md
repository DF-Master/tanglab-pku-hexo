---
title: Tutorial:从零开始的Linux使用【1】
date: 2022-11-08
categories:
- rd
tags:
- rd
---



---

> 本文面向Tanglab及类似的需要使用Linux、但缺乏相关基础的科研人士
> 

> 本文基于[**《TLCL 快乐的 Linux 命令行》**](http://www.sysumeg.com/index.php/project/tlcl/) ，计划每周给实验室同学普及一些linux常识。如果你希望深入学习，应当阅读原文。
> 

> 如果你拥有丰富的经验，基础内容对你的帮助可能比较有限，但扩展内容应该能给你一些有益的启发。
> 

---
<!--more-->
# 序

![Linus Torvalds](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/2022/202211//linus.png)

Linus Torvalds 在1991年写了 Linux 内核的第一个版本。一提到 Linux，许多人都会说到“自由”——“自由”是指一台没有任何秘密的计算机，你可以从它那里了解一切。基于命令行界面（CLI）的Linux可以完成很多复杂的任务，它更原始，也更强大、自由。学习 Linux 命令行会让你受益匪浅，给你极大的回报。不像其他一些计算机技能， 一段时间之后可能就被淘汰了，命令行知识却不会落伍，你今天所学到的，在十年以后 都会有用处。命令行通过了时间的考验。

# Shell - 1：Shell与终端

> 基础内容：从终端认识shell(了解)；扩展内容：后台运行shell（实用），其他shell
> 

shell 就是一个程序，它接受从键盘输入的命令， 然后把命令传递给操作系统去执行。几乎所有的 Linux 发行版(Ubuntu, CentOS, etc)都提供一个名为 bash 的 来自 GNU 项目的 shell 程序。“bash” 是 “Bourne Again SHell” 的首字母缩写， 是最初在 Unix 上由 Steve Bourne 写成 shell 程序 sh 的增强版。

当使用图形用户界面时，我们需要另一个和 shell 交互的叫做终端仿真器的程序。 如果我们浏览一下桌面菜单，可能会找到一个。虽然在菜单里它可能都被简单地称为 “terminal”。在windows 11下，有windows terminal.

好，开始吧。启动终端仿真器！一旦它运行起来，我们应该看到一行像这样的文字：

```bash
(base) [jiangyd@localhost ~]$
```

这叫做 shell 提示符，当 shell 准备好了去接受输入时，它就会出现。然而， 它可能会以各种各样的面孔显示，这则取决于不同的 Linux 发行版， 它通常包括你的用户名@主机名，紧接着当前工作目录（稍后会有更多介绍）和一个美元符号。

我这边前面还有一个（base），这是因为我使用了anaconda的默认环境，这也会在之后讲。

如果提示符的最后一个字符是“#”, 而不是“$”, 那么这个终端会话就有超级用户权限。 这意味着，我们要么是以 root 用户的身份登录，要么是我们选择的终端仿真器提供超级用户（管理员）权限。

```bash
[root@localhost jiangyd]#
```

*在多人维护的服务器中，尽量不要使用root账户！使用root权限应当写文档告知其他人。

假定到目前为止，所有事情都进展顺利，那我们试着键入字符吧。在提示符下敲入 一些像下面一样的乱七八糟的字符,因为这个命令没有任何意义，所以 shell 会提示错误信息：

```bash
(base) [jiangyd@localhost ~]$ asdasd
bash: asdasd: command not found...
```

## ****命令历史****

如果按下上箭头按键，我们会看到刚才输入的命令“asdasd”重新出现在提示符之后。 这就叫做命令历史。许多 Linux 发行版默认保存最后输入的500个命令。 按下下箭头按键，先前输入的命令就消失了。

* 在fish中，你还可以用上键来检索特定的命令，之后会讲到

## 复制

虽然，shell 是和键盘打交道的，但你也可以在终端仿真器里使用鼠标。X 窗口系统 （使 GUI 工作的底层引擎）内建了一种机制，支持快速拷贝和粘贴技巧。 如果你按下鼠标左键，沿着文本拖动鼠标（或者双击一个单词）高亮了一些文本， 那么这些高亮的文本就被拷贝到了一个由 X 管理的缓冲区里面。然后按下鼠标中键， 这些文本就被粘贴到光标所在的位置。试试看。

*但是，在windows terminal中，使用的是直接的ctrl+C ctrl+V

## 一些简单命令

```bash
jiangyd@localhost ~> date
Mon Nov  7 20:48:49 CST 2022
jiangyd@localhost ~> cal
    November 2022
Su Mo Tu We Th Fr Sa
       1  2  3  4  5
 6  7  8  9 10 11 12
13 14 15 16 17 18 19
20 21 22 23 24 25 26
27 28 29 30
```

```bash
jiangyd@localhost ~> df
Filesystem            1K-blocks       Used   Available Use% Mounted on
devtmpfs              131726824          0   131726824   0% /dev
tmpfs                 131746180          4   131746176   1% /dev/shm
tmpfs                 131746180     609468   131136712   1% /run
tmpfs                 131746180          0   131746180   0% /sys/fs/cgroup
/dev/mapper/cl-root    52403200   27196932    25206268  52% /
/dev/mapper/cl-home   176021532   93666116    82355416  54% /home
/dev/sda2                999320     285280      645228  31% /boot
/dev/sda1                613184       7416      605768   2% /boot/efi
/dev/mapper/mpatha1 23313366256 4564139156 17578832220  21% /smb
tmpfs                  26349236          0    26349236   0% /run/user/1003
tmpfs                  26349236          0    26349236   0% /run/user/1009
tmpfs                  26349236          0    26349236   0% /run/user/1004
tmpfs                  26349236          0    26349236   0% /run/user/1005
tmpfs                  26349236          0    26349236   0% /run/user/1017
tmpfs                  26349236          0    26349236   0% /run/user/1010
tmpfs                  26349236          0    26349236   0% /run/user/1002
tmpfs                  26349236          0    26349236   0% /run/user/1000
tmpfs                  26349236          0    26349236   0% /run/user/1008

jiangyd@localhost ~> free
              total        used        free      shared  buff/cache   available
Mem:      263492364    25730312     7261180      622992   230500872   235255688
Swap:       4194300     4194300           0
```

* 安装了[modern-unix](https://github.com/ibraheemdev/modern-unix)系列工具后，可以使用更美观的命令gtop，btm

## 结束终端

在 shell 提示符下输入 exit 命令来终止一个终端会话，或者直接关闭窗口

## 扩展内容：后台运行

很多时候，我们会遇到断网/断电/死机等情况导致终端意外登出，正在运行的进程突然暴死，或者想在一个任务正在运行的情况下另开一个任务……这就需要后台运行与进程守护功能。

不过，如果只是想并行运行任务的话，除了使用终端的TAB功能，还有什么办法呢？在windows terminal中，有一个取巧的解决方案：你可以Alt + shift + =/- 来实现上下或者左右分屏，多余的内容用ctrl + L 清除，然后ctrl + D 也可关掉不需要的屏幕。试试看吧！

### nohup

那么，为了防止终端暴死，该用什么办法呢？最简单的办法就是使用Nohup + “&”，例子如下：

```bash
nohup <your code> &
```

```bash
nohup g16 /smb/jiangyd/g16_mission_file/SDA-close-core-opt-TDPBE0.gjk &
```

* 这个还有扩展格式, 比如nohup ~/user/test.sh>output.log 2>&1 &，此处不讲

### 后台守护进程systemd

systemd可以设置进程在失败后自动重启、开机后自动打开，往往用于需要常开的功能，但是需要管理员权限，所以这边只是简单给出[教程](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html) 。但是，对于需要建立永久服务（如：BLOG）的人来说，这会很重要。

当然，npm的forever也可以实现进程守护的功能，受众太少这里不介绍了。

### Screen

第二种办法就是另开一个不会随着终端结束的terminal，这就是screen的用处了，它的好处就是我们可以随时打开另一个会话，当前终端的关闭也不会影响screen内的会话：

```bash
# screen不是linux自带的，需要安装
# 显示当前的screen
jiangyd@localhost ~> screen -ls
There are screens on:
3457181.alphafold       (Detached)
2329155.8080    (Detached)
1756412.clash   (Detached)
# 随时进入会话并查看会话内容
jiangyd@localhost ~> screen -r clash
INFO[0000] Start initial compatible provider Auto-Fast
INFO[0000] Start initial compatible provider NETFLIX
INFO[0000] Start initial compatible provider Auto-Failover
INFO[0000] Start initial compatible provider Auto-Edge
INFO[0000] Start initial compatible provider Auto
INFO[0000] Start initial compatible provider Express
INFO[0000] Start initial compatible provider Proxy
INFO[0000] Start initial compatible provider Geo
INFO[0000] Start initial compatible provider Video
INFO[0000] HTTP proxy listening at: 127.0.0.1:7890
INFO[0000] SOCKS proxy listening at: 127.0.0.1:7891
INFO[0000] RESTful API listening at: 127.0.0.1:9090
INFO[0190] [TCP] 127.0.0.1:38930 --> github.com:443 match DomainKeyword(github) using Proxy[GLaDOS-N2-02]
INFO[16316] [TCP] 127.0.0.1:43728 --> github.com:443 match DomainKeyword(github) using Proxy[GLaDOS-N2-02]
INFO[273716] [TCP] 127.0.0.1:59694 --> github.com:443 match DomainKeyword(github) using Proxy[GLaDOS-N2-02]
INFO[1031488] [TCP] 127.0.0.1:35140 --> github.com:443 match DomainKeyword(github) using Proxy[GLaDOS-N2-07]
# ctrl+A ctrl +D 离开会话
# ctrl+C ctrl +D 关闭会话

```

### tmux

某种意义上，tmux可以说是screen+,而byobu又是tmux+。可是，实验室中真的会有这么多后台窗口的需要吗……

虽然因为习惯原因还是继续用screen，但tmux和byobu还是非常值得推荐的，参见[教程](http://www.ruanyifeng.com/blog/2019/10/tmux.html)。

## 扩展内容：其他shell

虽然bash shell是默认的、主流的shell，但主流不代表“好用”，下面将对两款“好用”的shell进行介绍。

### [Fish: the **f**riendly **i**nteractive **sh**ell](https://www.ruanyifeng.com/blog/2017/05/fish_shell.html)

> 点击超链接阅读原文
> 

Fish 是"the **f**riendly **i**nteractive **sh**ell"的简称，最大特点就是方便易用。很多其他 Shell 需要配置才有的功能，Fish 默认提供，不需要任何配置。

如果你想拥有一个方便好用的 Shell，又不想学习一大堆语法，或者花费很多时间配置，那么你一定要尝试一下 Fish。安装请参考[官方网站](https://fishshell.com/#platform_tabs) 。

Fish 的语法与 Bash 有很大差异，Bash 脚本一般不兼容。你可以用下面的代码把它设置成默认shell，也可以每次输入“fish”手动启动它。

```bash
jiangyd@localhost ~ [3]> which fish # 查询fish位置
/usr/bin/fish
jiangyd@localhost ~> chsh -s /usr/bin/fish # 更换默认shell
```

fish有几大特点：

- 默认彩色显示
- 有效路径会有下划线
- 自动建议
    - 如果采纳建议，可以按下`→`或`Control + F`
    - 如果只采纳一部分，可以按下`Alt + →`
- 自动补全
- fish的语法和bash不同，请查询文档。配置文件是`~/.config/fish/config.fish`
- 自定义主题和安装插件：[oh-my-fish](https://github.com/oh-my-fish/oh-my-fish)

### [Zsh: a shell designed for interactive use](https://wiki.archlinux.org/title/Zsh_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

相比fish,zsh需要更多的自定义，在语法上也更传统（更接近bash），但远远没有做到开箱即用。但是，对有bash使用需求的用户来说，zsh是相当优秀的平替。

因为本文面向的是初学者，笔者的经验也有限，所以不会对zsh进行过多的介绍，感兴趣的读者可以阅读官方文档和[oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)

# Shell - 2: ****文件操作****

> 基础内容：pwd,cd,ls,file,less,cp,mv,mkdir,rm, ln; 扩展内容：用vscode实现文档传输、文本编辑
> 

## pwd,ls

linux文件系统像一个倒立的大树，它的结构往往很复杂：

```bash
⋊> jiangyd@localhost ⋊> /s/o/t/source on master ◦ tree                                         12:51:54
.
└── _posts
    ├── 2021-09-08-Publications.md
    ├── 2021-09-09-Contact-Information.md
    ├── member
    │   ├── former-members.md
    │   ├── phd-students.md
    │   ├── post-PhD.md
    │   ├── Staff.md
    │   ├── tangc.md
    │   └── Undergrad.md
    ├── news
    │   ├── 2021-09-10-Tanglab庆祝2021教师节.md
    │   ├── 2021-10-16-刘聪研究员报告：α-Synuclein病理性聚集与帕金森病.md
    │   ├── 2022-01-21-TangLab新年活动--保龄球初体验.md
    │   ├── 220427踏青.md
    │   ├── 220730羽毛球.md
    │   ├── 兴大报告学术笔记—Ichio-Shimada-教授.md
    │   └── 《蛋白质折叠和结合以及染色体动力学的能量景观》：汪劲教授受邀来CCME做报告.md
    ├── R&D
    │   ├── 2022
    │   │   ├── Tanglab Tutorial 面向科研人员的Markdown应用教程.md
    │   │   └── 基于NMRPipe的RDC数据处理教程 2a112f74ab744859baeea5a26ca18cb8.md
    │   ├── biomolecular_dynamics_study.md
    │   ├── PRE.md
    │   ├── RNA.md
    │   └── UB.md
    ├── sportsmeeting.md
    └── Tuturial for Alphafold on Tanglab pku edu cn 9512ed4c670b401094acbb8deaecffc9.md

5 directories, 23 files
```

因此，了解这个大树中我们所处的位置，是很有必要的：

```bash
⋊> jiangyd@localhost ⋊> /s/o/t/source on master ◦ pwd                                          12:51:58
/smb/openapps/tanglab-pku-hexo/source
```

对一般用户，默认的工作目录就是/home/yourname，在tanglab中，存储盘/smb/yourname也是常用的工作空间。

查看当前工作目录下的文件，可以使用ls（先前的例子使用了tree，可以展示进一步的子目录）。

```bash
⋊> jiangyd@localhost ⋊> /s/o/t/source on master ◦ ls                                           12:55:49
_posts/
# 展示特定目录
⋊> jiangyd@localhost ⋊> ~ ls /home/jiangyd/                                                    12:58:39
authorized_keys  Country.mmdb  Downloads/   Music/     Public/                  test/
cache.db         Desktop/      glados.yaml  nohup.out  stable-diffusion-webui/  Videos/
clash/           Documents/    gromacs/     Pictures/  Templates/
# 展示细节
⋊> jiangyd@localhost ⋊> ~ ls /home/jiangyd/ -l                                                 13:00:01
total 5600
-rw-rw-r--   1 jiangyd jiangyd       0 May  5  2022 authorized_keys
-rw-r--r--   1 jiangyd Tanglab   16384 Oct 17 15:05 cache.db
drwxr-xr-x   3 jiangyd Tanglab      36 Oct  5 19:55 clash/
-rw-r--r--   1 jiangyd Tanglab 5703521 Oct 17 15:05 Country.mmdb
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Desktop/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Documents/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Downloads/
-rw-r--r--   1 jiangyd Tanglab      16 Oct 17 15:05 glados.yaml
drwxrwxr-x   3 jiangyd jiangyd      21 May 10 15:37 gromacs/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Music/
-rw-------   1 jiangyd Tanglab    2226 Nov  7 21:05 nohup.out
drwxr-xr-x.  2 jiangyd jiangyd     100 Jun 22  2018 Pictures/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Public/
drwxr-xr-x  15 jiangyd Tanglab    4096 Oct 20 19:09 stable-diffusion-webui/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Templates/
drwxr-xr-x. 12 jiangyd jiangyd     249 Oct 31 14:28 test/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Videos/
# 展示隐藏文件
⋊> jiangyd@localhost ⋊> ~ ls /home/jiangyd/ -al                                                13:00:36
total 5692
drwxr-xr-x. 27 jiangyd jiangyd    4096 Nov  8 12:39 ./
drwxr-xr-x. 19 root    root        230 Oct  8 19:33 ../
-rw-rw-r--   1 jiangyd jiangyd       0 May  5  2022 authorized_keys
-rw-------   1 jiangyd Tanglab   15385 Nov  8 12:38 .bash_history
-rwxr-xr-x.  1 jiangyd jiangyd      18 May 11  2019 .bash_logout*
-rwxr-xr-x.  1 jiangyd jiangyd     141 May 11  2019 .bash_profile*
-rwxr-xr-x.  1 jiangyd jiangyd     436 Jun  2 19:21 .bashrc*
drwxr-xr-x. 22 jiangyd jiangyd    4096 Nov  8 12:19 .cache/
-rw-r--r--   1 jiangyd Tanglab   16384 Oct 17 15:05 cache.db
drwxr-xr-x   3 jiangyd Tanglab      36 Oct  5 19:55 clash/
drwxr-sr-x.  2 jiangyd jiangyd      30 Apr 24  2022 .conda/
-rwxr-xr-x.  1 jiangyd jiangyd       3 Apr 24  2022 .condarc*
drwxr-xr-x. 18 jiangyd jiangyd    4096 Nov  8 12:19 .config/
-rw-r--r--   1 jiangyd Tanglab 5703521 Oct 17 15:05 Country.mmdb
-rwxr-xr-x.  1 jiangyd jiangyd     553 Apr 26  2022 .cshrc*
drwxr-xr-x.  3 jiangyd jiangyd      25 Apr 25  2022 .dbus/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Desktop/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Documents/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Downloads/
-rwxr-xr-x.  1 jiangyd jiangyd      16 Jun 22  2018 .esd_auth*
drwxr-xr-x   2 jiangyd Tanglab       6 Jul 20 19:43 .gaussian/
-rw-r--r--   1 jiangyd Tanglab      63 Oct 15 13:04 .gitconfig
-rw-r--r--   1 jiangyd Tanglab      16 Oct 17 15:05 glados.yaml
drwxrwxr-x   3 jiangyd jiangyd      21 May 10 15:37 gromacs/
-rwxr-xr-x   1 jiangyd Tanglab     431 Jul 20 14:45 .history*
-rwxr-xr-x.  1 jiangyd jiangyd       0 Apr 25  2022 .history.7254921*
-rwxr-xr-x.  1 jiangyd jiangyd    2178 Jun 22  2018 .ICEauthority*
drwxr-xr-x.  2 jiangyd jiangyd      24 Apr 24  2022 .keras/
drwxr-xr-x.  5 jiangyd jiangyd      41 Nov  8 12:40 .local/
drwxr-xr-x.  6 jiangyd jiangyd      81 Jun 22  2018 .mozilla/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Music/
-rw-------   1 jiangyd Tanglab    2226 Nov  7 21:05 nohup.out
drwxrwxr-x.  5 jiangyd jiangyd      84 Apr 26  2022 .npm/
-rw-------   1 jiangyd Tanglab      29 Nov  8 12:39 .npmrc
drwx------   3 jiangyd jiangyd      26 Jun 11 15:59 .nv/
drwxr-xr-x.  2 jiangyd jiangyd     100 Jun 22  2018 Pictures/
drwxr-xr-x.  3 jiangyd jiangyd      19 Jun 22  2018 .pki/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Public/
-rw-------   1 jiangyd Tanglab     418 Oct 15 16:12 .python_history
drwx------.  2 jiangyd jiangyd      98 Jul 30 16:19 .ssh/
drwxr-xr-x  15 jiangyd Tanglab    4096 Oct 20 19:09 stable-diffusion-webui/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Templates/
drwxr-xr-x. 12 jiangyd jiangyd     249 Oct 31 14:28 test/
drwxr-xr-x.  2 jiangyd jiangyd       6 Jun 22  2018 Videos/
-rw-------   1 jiangyd Tanglab    8955 Oct 20 19:08 .viminfo
drwxr-xr-x   5 jiangyd Tanglab     208 Oct 17 19:39 .vscode-server/
-rw-rw-r--   1 jiangyd jiangyd     400 Oct 17 19:39 .wget-hsts
-rw-------   1 jiangyd Tanglab     201 Oct 26 13:32 .Xauthority
```

ls还有很多常用选项，例如：

| Option | Long Option      | Description                                                                                                                                                                                                                  |
| ------ | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -a     | --all            | List all files, even those with names that begin with a period, which are normally not listed(i.e.,hidden).                                                                                                                  |
| -d     | --directory      | Ordinaryly,if a directory is specified, ls will list the contents of the directory, not the directory itself. Use this option in conjunction with the -l option to see details about the directory rather than its contents. |
| -F     | --classify       | This option will append an indicator character to the end of each listed name. For example, a '/' if the name is a directory.                                                                                                |
| -h     | --human-readable | In long format listings, display file sizes in human readable format rather than in bytes.                                                                                                                                   |
| -l     |                  | Display results in long format.                                                                                                                                                                                              |
| -r     | --reverse        | Display the results in reverse order. Normally, ls display its results in ascending alphabetical order.                                                                                                                      |
| -S     |                  | Sort results by file size.                                                                                                                                                                                                   |
| -t     |                  | Sort by modification time.                                                                                                                                                                                                   |

了解具体该使用什么选项，可以使用以下命令来查询：

```bash
⋊> jiangyd@localhost ⋊> ~ man ls
⋊> jiangyd@localhost ⋊> ~ ls --help
# modern-unix介绍了tldr,一种更人类可读的help文档
⋊> jiangyd@localhost ⋊> ~ tldr ls                                                              13:07:14

  ls

  List directory contents.
  More information: https://www.gnu.org/software/coreutils/ls.

  - List files one per line:
    ls -1

  - List all files, including hidden files:
    ls -a

  - List all files, with trailing `/` added to directory names:
    ls -F

  - Long format list (permissions, ownership, size, and modification date) of all files:
    ls -la

  - Long format list with size displayed using human-readable units (KiB, MiB, GiB):
    ls -lh

  - Long format list sorted by size (descending):
    ls -lS

  - Long format list of all files, sorted by modification date (oldest first):
    ls -ltr

  - Only list directories:
    ls -d */
```

## cd

看清目录后，切换目录使用cd命令,cd 后不加任何参数就是回到主目录

```bash
⋊> jiangyd@localhost ⋊> /s/o/t/source on master ◦ pwd                                          12:58:32
/smb/openapps/tanglab-pku-hexo/source
⋊> jiangyd@localhost ⋊> /s/o/t/source on master ◦ cd                                           12:58:34
⋊> jiangyd@localhost ⋊> ~ pwd                                                                  12:58:36
/home/jiangyd
```

cd 后可以加 **相对路径** 和 **绝对路径**。绝对路径开始于根目录，以“/”作为开头；相对路径开始于当前目录，以“./”或者不加任何描述作为开头。此外，特殊的，“~/”代表以用户起始目录，对一般用户代表“/home/yourname”。因此，下面的命令完全一致：

```bash
⋊> jiangyd@localhost ⋊> ~ cd                                                                   13:07:21
⋊> jiangyd@localhost ⋊> ~ cd /home/jiangyd/test/                                               13:13:59
⋊> jiangyd@localhost ⋊> ~/test cd                                                              13:14:21
⋊> jiangyd@localhost ⋊> ~ cd test/                                                             13:14:23
⋊> jiangyd@localhost ⋊> ~/test cd                                                              13:14:35
⋊> jiangyd@localhost ⋊> ~ cd ./test/
⋊> jiangyd@localhost ⋊> ~/test cd                                                              13:14:30
⋊> jiangyd@localhost ⋊> ~ cd ~/test/                                                           13:14:31
```

* 最后，请注意linux是大小写敏感的！

此外，cd 还提供一些快捷方式，试试看吧:

```bash
# 回退
cd - 
# 上级目录
cd .. 
# 用户根目录
cd ~<user_name>
```

## file, less

file 命令可以用来确定文件的类型

```bash
⋊> jiangyd@localhost ⋊> / file cache.db                                                        13:31:19
cache.db: data
⋊> jiangyd@localhost ⋊> / file sbin/                                                           13:31:41
sbin/: directory
⋊> jiangyd@localhost ⋊> ~ file nohup.out                                                       13:33:19
nohup.out: ASCII text
```

其中，文本文件是重要的文件类型。在计算机中，信息与数字的转换十分重要。毕竟，计算机只能理解数字。其中，最早也是最简单的一种表达法，叫做 ASCII 文本。ASCII（发音是”As-Key”）是美国信息交换标准码的简称。这是一个简单的编码方法，它首先 被用在电传打字机上，用来实现键盘字符到数字的映射。

文本是简单的字符与数字之间的一对一映射。它非常紧凑。五十个字符的文本翻译成五十个字节的数据。文本只是包含简单的字符到数字的映射，理解这点很重要。它和一些常见办公文档编辑软件，比如说由微软 Office 或 OpenOffice.org，创建的文字内容不同。和简单的 ASCII 文件形成鲜明对比，这些文档内容中包含许多非文本元素，来描述它的结构和格式。纯 ASCII 文件只包含字符本身，和一些基本的控制符，像制表符、回车符及换行符。纵观 Linux 系统，许多文件以文本格式存储，也有许多 Linux 工具来处理文本文件。甚至 Windows 也承认这种文件格式的重要性。著名的 NOTEPAD.EXE 程序就是一个纯 ASCII 文本文件编辑器。

*markdown就是一种纯文本文件，可以参考我的[Tutorial 面向科研人员的Markdown应用教程](http://tanglab.cn/2022/11/04/R&D/2022/Tanglab%20Tutorial%20%E9%9D%A2%E5%90%91%E7%A7%91%E7%A0%94%E4%BA%BA%E5%91%98%E7%9A%84Markdown%E5%BA%94%E7%94%A8%E6%95%99%E7%A8%8B/)

既然文本文件很重要，在linux中查看和编写文本文件就很重要了。less命令提供了一种阅读文本文件的方式（当然你还可以使用cat, vim，etc）. less的命令很简单：

```bash
less <file_name>
# less 程序是早期 Unix 程序 more 的改进版。
# “less” 这个名字，套用习语 “less is more” ， 这个习语是现代主义建筑师和设计者的座右铭。
```

常用的快捷键如下：

| Page UP or b       | 向上翻滚一页                                             |
| ------------------ | -------------------------------------------------------- |
| Page Down or space | 向下翻滚一页                                             |
| UP Arrow           | 向上翻滚一行                                             |
| Down Arrow         | 向下翻滚一行                                             |
| G                  | 移动到最后一行                                           |
| 1G or g            | 移动到开头一行                                           |
| /charaters         | 向前查找指定的字符串                                     |
| n                  | 向前查找下一个出现的字符串，这个字符串是之前所指定查找的 |
| h                  | 显示帮助屏幕                                             |
| q                  | 退出 less 程序                                           |

## Start Travel

现在，你已经基本掌握了文件跳转的办法，来试试看参观下面的目录吧。虽然有些内容只能由管理员操作，但只是参观目录的话，一般没有问题：

| 目录  | 评论                                                                        |
| ----- | --------------------------------------------------------------------------- |
| /     | 根目录，万物起源。                                                          |
| /bin  | 包含系统启动和运行所必须的二进制程序。                                      |
| /boot | 包含 Linux 内核、初始 RAM 磁盘映像（用于启动时所需的驱动）和 启动加载程序。 |
有趣的文件：
• /boot/grub/grub.conf or menu.lst， 被用来配置启动加载程序。
• /boot/vmlinuz，Linux 内核。 |
| /dev | 这是一个包含设备结点的特殊目录。“一切都是文件”，也适用于设备。 在这个目录里，内核维护着所有设备的列表。 |
| /etc | 这个目录包含所有系统层面的配置文件。它也包含一系列的 shell 脚本， 在系统启动时，这些脚本会开启每个系统服务。这个目录中的任何文件应该是可读的文本文件。
有趣的文件：虽然/etc 目录中的任何文件都有趣，但这里只列出了一些我一直喜欢的文件：
• /etc/crontab， 定义自动运行的任务。
• /etc/fstab，包含存储设备的列表，以及与他们相关的挂载点。
• /etc/passwd，包含用户帐号列表。 |
| /home | 在通常的配置环境下，系统会在 /home 下，给每个用户分配一个目录。普通用户只能 在自己的目录下写文件。这个限制保护系统免受错误的用户活动破坏。 |
| /lib | 包含核心系统程序所使用的共享库文件。这些文件与 Windows 中的动态链接库相似。 |
| /lost+found | 每个使用 Linux 文件系统的格式化分区或设备，例如 ext3文件系统， 都会有这个目录。当部分恢复一个损坏的文件系统时，会用到这个目录。这个目录应该是空的，除非文件系统 真正的损坏了。 |
| /media | 在现在的 Linux 系统中，/media 目录会包含可移动介质的挂载点， 例如 USB 驱动器，CD-ROMs 等等。这些介质连接到计算机之后，会自动地挂载到这个目录结点下。 |
| /mnt | 在早些的 Linux 系统中，/mnt 目录包含可移动介质的挂载点。 |
| /opt | 这个/opt 目录被用来安装“可选的”软件。这个主要用来存储可能 安装在系统中的商业软件产品。 |
| /proc | 这个/proc 目录很特殊。从存储在硬盘上的文件的意义上说，它不是真正的文件系统。 相反，它是一个由 Linux 内核维护的虚拟文件系统。它所包含的文件是内核的窥视孔。这些文件是可读的， 它们会告诉你内核是怎样监管计算机的。 |
| /root | root 帐户的家目录。 |
| /sbin | 这个目录包含“系统”二进制文件。它们是完成重大系统任务的程序，通常为超级用户保留。 |
| /tmp | 这个/tmp 目录，是用来存储由各种程序创建的临时文件的地方。系统每次 重新启动时，都会清空这个目录。 |
| /usr | 在 Linux 系统中，/usr 目录可能是最大的一个。它包含普通用户所需要的所有程序和文件。 |
| /usr/bin | /usr/bin 目录包含系统安装的可执行程序。通常，这个目录会包含许多程序。 |
| /usr/lib | 包含由/usr/bin 目录中的程序所用的共享库。 |
| /usr/local | 这个/usr/local 目录，是非系统发行版自带程序的安装目录。 通常，由源码编译的程序会安装在/usr/local/bin 目录下。新安装的 Linux 系统中会存在这个目录， 并且在管理员安装程序之前，这个目录是空的。 |
| /usr/sbin | 包含许多系统管理程序。 |
| /usr/share | /usr/share 目录包含许多由 /usr/bin 目录中的程序使用的共享数据。 其中包括像默认的配置文件、图标、桌面背景、音频文件等等。 |
| /usr/share/doc | 大多数安装在系统中的软件包会包含一些文档。在/usr/share/doc 目录下， 我们可以找到按照软件包分类的文档。 |
| /var | 除了/tmp 和/home 目录之外，相对来说，目前我们看到的目录是静态的，这是说， 它们的内容不会改变。/var 目录存放的是动态文件。各种数据库，假脱机文件， 用户邮件等等，都位于在这里。 |
| /var/log | 这个/var/log 目录包含日志文件、各种系统活动的记录。这些文件非常重要，并且 应该时时监测它们。其中最重要的一个文件是 /var/log/messages。注意，为了系统安全，在一些系统中， 你必须是超级用户才能查看这些日志文件。 |

### 符号链接

在到处查看时，可能会看到一个目录，列出像这样的一条信息：

```bash
lrwxrwxrwx 1 root root 11 2007-08-11 07:34 libc.so.6 -> libc-2.6.so
```

为何这条信息第一个字符是“l”，并且有两个文件名呢？ 这是一个特殊文件，叫做符号链接（也称为软链接或者 symlink ）。 在大多数“类 Unix” 系统中， 有可能一个文件被多个文件名所指向。这个特性很有用。它的作用就像域名：既方便管理，也方便用户记忆、使用。

上面展示了一个叫做 “libc.so.6” 的符号链接，这个符号链接指向一个 叫做 “libc-2.6.so” 的共享库文件。这意味着，寻找文件 “libc.so.6” 的程序，实际上得到是文件 “libc-2.6.so”。之后，我们将学习如何建立符号链接。

## mkdir, cp, mv, rm

今天，用图形文件管理器来完成一些文件管理更加方便，但是命令行提供了更多的自定义选项，例如copy所有的html类型文件并且不进行替换：

```bash
cp -u *.html destination
```

上面，使用了命令行的一个强大功能——通配符。通配符允许你依据字符的组合模式来选择文件名。下表列出这些通配符 以及它们所选择的对象：

| 通配符        | 意义                                         |
| ------------- | -------------------------------------------- |
| *             | 匹配任意多个字符（包括零个或一个）           |
| ?             | 匹配任意一个字符（不包括零个）               |
| [characters]  | 匹配任意一个属于字符集（characters）中的字符 |
| [!characters] | 匹配任意一个不是字符集中的字符               |
| [[:class:]]   | 匹配任意一个属于指定字符类中的字符           |

| 字符类    | 意义                   |
| --------- | ---------------------- |
| [:alnum:] | 匹配任意一个字母或数字 |
| [:alpha:] | 匹配任意一个字母       |
| [:digit:] | 匹配任意一个数字       |
| [:lower:] | 匹配任意一个小写字母   |
| [:upper:] | 匹配任意一个大写字母   |

下面给出了一些例子：

| 模式                   | 匹配对象                                                  |
| ---------------------- | --------------------------------------------------------- |
| *                      | 所有文件                                                  |
| g*                     | 文件名以“g”开头的文件                                     |
| b*.txt                 | 以"b"开头，中间有零个或任意多个字符，并以".txt"结尾的文件 |
| Data???                | 以“Data”开头，其后紧接着3个字符的文件                     |
| [abc]*                 | 文件名以"a","b",或"c"开头的文件                           |
| BACKUP.[0-9][0-9][0-9] | 以"BACKUP."开头，并紧接着3个数字的文件                    |
| [[:upper:]]*           | 以大写字母开头的文件                                      |
| [![:digit:]]*          | 不以数字开头的文件                                        |
| *[[:lower:]123]        | 文件名以小写字母结尾，或以 “1”，“2”，或 “3” 结尾的文件    |

当然，通配符在脚本中往往出现得更多，了解他们对于阅读脚本是有帮助的。（这是一个笑话）以及你可以通过下面这个代码来快速完成运维生涯：

```bash
rm -rf *
# 它的意思是删除当前目录所有文件，一般这么做完就可以下班了（笑）
```

此外，一些图形文件管理器也支持通配符，比如Dolphin和GNOME，试试看吧！此外，常用的搜索引擎也有类似的语法，试试看吧！

有了通配符的概念，就能高效地使用下面的命令了。

### mkdir - ****创建目录****

在开始下面的部分前，我们先学习一下mkdir。 mkdir 命令被用来创建目录，用法非常简单：

```bash
mkdir <dir_name>
```

现在有了目录之后，我们可以以此为对象学习后面的内容。

### ****cp - 复制文件和目录****

**cp 命令，复制文件或者目录。它有两种使用方法：**

```bash
# 复制单个文件或目录”item1”到文件或目录”item2”
cp item1 item2
# 复制多个项目（文件或目录）到一个目录下。
cp item... directory
```

| 选项              | 意义                                                                                                                        |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------- |
| -a, --archive     | 复制文件和目录，以及它们的属性，包括拥有者和所有权。 通常情况下，文件拷贝具有执行拷贝操作的用户的默认属性。                 |
| -i, --interactive | 在覆盖已存在文件之前，提示用户确认。如果这个选项不指定， cp 命令会默认覆盖文件。                                            |
| -r, --recursive   | 递归地复制目录及目录中的内容。当复制目录时， 需要这个选项（或者 -a 选项）。                                                 |
| -u, --update      | 当把文件从一个目录复制到另一个目录时，仅复制 目标目录中不存在的文件，或者是文件内容新于目标目录中已经存在文件的内容的文件。 |
| -v, --verbose     | 显示翔实的命令操作信息                                                                                                      |

试试下面的例子：

| 命令                | 运行结果                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| cp file1 file2      | 复制文件 file1 内容到文件 file2。如果 file2 已经存在， file2 的内容会被 file1 的内容覆盖。如果 file2 不存在，则会创建 file2。                                                                    |
| cp -i file1 file2   | 这条命令和上面的命令一样，除了如果文件 file2 存在的话，在文件 file2 被覆盖之前， 会提示用户确认信息。                                                                                            |
| cp file1 file2 dir1 | 复制文件 file1 和文件 file2 到目录 dir1。目录 dir1 必须存在。                                                                                                                                    |
| cp dir1/* dir2      | 使用一个通配符，在目录 dir1 中的所有文件都被复制到目录 dir2 中。 dir2 必须已经存在。                                                                                                             |
| cp -r dir1 dir2     | 复制目录 dir1 中的内容到目录 dir2。如果目录 dir2 不存在， 创建目录 dir2，操作完成后，目录 dir2 中的内容和 dir1 中的一样。 如果目录 dir2 存在，则目录 dir1 (和目录中的内容)将会被复制到 dir2 中。 |

### ****mv - 移动和重命名文件****

mv的用法和cp基本一致。试试下面的例子：

| mv file1 file2      | 移动 file1 到 file2。如果 file2 存在，它的内容会被 file1 的内容覆盖。 如果 file2 不存在，则创建 file2。 这两种情况下，file1 都不再存在。                      |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mv -i file1 file2   | 除了如果 file2 存在的话，在 file2 被覆盖之前，用户会得到 提示信息外，这个和上面的选项一样。                                                                   |
| mv file1 file2 dir1 | 移动 file1 和 file2 到目录 dir1 中。dir1 必须已经存在。                                                                                                       |
| mv dir1 dir2        | 如果目录 dir2 不存在，创建目录 dir2，并且移动目录 dir1 的内容到 目录 dir2 中，同时删除目录 dir1。如果目录 dir2 存在，移动目录 dir1（及它的内容）到目录 dir2。 |

### ****rm - 删除文件和目录****

rm,删除，用法更加简单：

| 选项              | 意义                                                                                                                          |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| -i, --interactive | 在删除已存在的文件前，提示用户确认信息。 如果不指定这个选项，rm 会默默地删除文件                                              |
| -r, --recursive   | 递归地删除文件，这意味着，如果要删除一个目录，而此目录 又包含子目录，那么子目录也会被删除。要删除一个目录，必须指定这个选项。 |
| -f, --force       | 忽视不存在的文件，不显示提示信息。这选项覆盖了“--interactive”选项。                                                           |
| -v, --verbose     | 在执行 rm 命令时，显示翔实的操作                                                                                              |

试试下面的实例：

| 命令              | 运行结果                                                            |
| ----------------- | ------------------------------------------------------------------- |
| rm file1          | 默默地删除文件                                                      |
| rm -i file1       | 除了在删除文件之前，提示用户确认信息之外，和上面的命令作用一样。    |
| rm -r file1 dir1  | 删除文件 file1, 目录 dir1，及 dir1 中的内容。                       |
| rm -rf file1 dir1 | 同上，除了如果文件 file1，或目录 dir1 不存在的话，rm 仍会继续执行。 |

但总之，rm用多了会带来不幸（尤其是你误删了什么东西的时候），一方面你可以定期给服务器做备份，另一方面尽量不要在rm中使用通配符。

## ****ln — 创建链接****

ln 命令既可创建硬链接，也可以创建符号链接。可以用两者中的任意一种形式来使用它：

```bash
# 硬链接
ln file link
# 符号链接,创建符号链接，”item” 可以是一个文件或是一个目录。
ln -s item link
```

与更加现代的符号链接相比，硬链接是最初 Unix 创建链接的方式。每个文件默认会有一个硬链接， 这个硬链接给予文件名字。我们每创建一个硬链接，就为一个文件创建了一个额外的目录项。 硬链接有两个重要局限性：

- **一个硬链接不能关联它所在文件系统之外的文件。这是说一个链接不能关联 与链接本身不在同一个磁盘分区上的文件。**
- **一个硬链接不能关联一个目录。**

目前，符号链接是更常用的。符号链接生效，是通过创建一个特殊类型的文件，这个文件包含一个关联文件或目录的文本指针。在这一方面， 它们和 Windows 的快捷方式差不多，当然，符号链接早于 Windows 的快捷方式 很多年;-)

文件管理器 GNOME 和 KDE 都提供了一个简单而且自动化的方法来创建符号链接。 在 GNOME 里面，当拖动文件时，同时按下 Ctrl+Shift 按键会创建一个链接，而不是 复制（或移动）文件。在 KDE 中，无论什么时候放下一个文件，会弹出一个小菜单， 这个菜单会提供复制，移动，或创建链接文件选项。

下面给出了一个常用的实例：

```bash
sudo ln -s /usr/bin/python3 /usr/bin/python
sudo ln -s /usr/bin/pip3 /usr/bin/pip
```

这一步需要管理员权限(sudo)，我们之后会讲到。在现代的Ubuntu发行版中往往没有默认的python命令（这往往对应于python2），因为默认的python3版本的命令是Python3和pip3。这边把python映射到python3，使得使用习惯保持一致。但其实版本已经发生了迁移——这就是符号链接的作用。

对于一些路径比较复杂的文件，你也可以用类似的方法建立链接，像快捷方式一样使用ln吧！

## 扩展内容: 用vscode实现文档传输、文本编辑

到这里，你已经掌握了linux文件操作的基础，但是每次输入命令难免令人烦躁。这边，我们用免费的vscode作为例子，展示如何用UI界面实现文件目录跳转和文本编辑。这里是[官方文档](https://code.visualstudio.com/docs/remote/ssh-tutorial) 。不过更建议直接动手试试：它是开箱即用的类型。

当你用上UI界面后，你会爱上它的！试试看吧！

# Shell - 3 了解命令

> 基础：which,help,man； 扩展：alias
> 

在这之前，我们已经知道了一系列神秘的命令，每个命令都有自己奇妙的选项和参数。在这一章中，我们将试图去掉一些神秘性，了解并使用基本的命令，甚至创建我们自己的命令。

命令可以是下面四种形式之一：

- 一个可执行程序，就像我们所看到的位于目录/usr/bin 中的文件一样。 这一类程序可以是用诸如 C 和 C++ 语言写成的程序然后编译得到的二进制文件, 也可以是由诸如 shell，perl，python，ruby 等等脚本语言写成的程序。
- 一个内建于 shell 自身的命令。bash 支持若干命令，内部叫做 shell 内部命令 (builtins)。例如，cd 命令，就是一个 shell 内部命令。
- 一个 shell 函数。这些是小规模的 shell 脚本，它们混合到环境变量中。 在后续的章节里，我们将讨论配置环境变量以及书写 shell 函数。但是现在， 仅仅意识到它们的存在就可以了。
- 一个命令别名。我们可以定义自己的命令，建立在其它命令之上。

如何查询命令的种类？ Linux 提供了type命令:

```bash
type command
```

```bash
[me@linuxbox ~]$ type type
type is a shell builtins
[me@linuxbox ~]$ type ls
ls is aliased to `ls --color=tty`
# ls 命令实际上 是 ls 命令加上选项”--color=tty”的别名。现在我们知道为什么 ls 的输出结果是有颜色的！
[me@linuxbox ~]$ type cp
cp is /bin/cp
```

但是，很多时候，我们希望了解更多的命令特性，下面将进行介绍。

## ****which － 显示一个可执行程序的位置****

有时候在一个操作系统中，不只安装了可执行程序的一个版本。虽然在桌面系统中这并不普遍， 但在大型服务器中却很平常。为了确定所给定的执行程序的准确位置，使用 which 命令：

```bash
⋊> jiangyd@localhost ⋊> ~ which ls                                                             19:53:46
/usr/bin/ls
⋊> jiangyd@localhost ⋊> ~ which nvcc                                                           19:54:53
/usr/local/cuda/bin/nvcc
```

## ****help,man － 得到 shell 内建命令的帮助文档****

关于这一点，我们在前面已经有提过了。例如，想查询cd的使用，比起用Google，你还可以用以下代码：

```bash
help cd # bash shell
cd --help
man cd
```

当然，很多时候，我们并不愿意阅读那么长的说明（"Too Long; Didn't Read”）,那你可以试试[tldr](https://github.com/tldr-pages/tldr):

```bash
⋊> jiangyd@localhost ⋊> ~ tldr cd                                                              19:59:16

  cd

  Change the current working directory.
  More information: https://manned.org/cd.

  - Go to the specified directory:
    cd path/to/directory

  - Go up to the parent of the current directory:
    cd ..

  - Go to the home directory of the current user:
    cd

  - Go to the home directory of the specified user:
    cd ~username

  - Go to the previously chosen directory:
    cd -

  - Go to the root directory:
    cd /
```

如果你想了解一些基本的信息，可以使用whatis:

```bash
⋊> jiangyd@localhost ⋊> ~ whatis cd                                                            19:59:19
cd (1)               - bash built-in commands, see bash(1)
cd (1p)              - change the working directory
```

当然，很多时候，还希望了解某个特定命令的版本，一般在后面加—version就行了

```bash
⋊> jiangyd@localhost ⋊> ~ python --version                                                     20:25:03
Python 3.6.8
```

## 扩展内容：alias - ****创建你自己的命令****

现在是时候，编写自己的命令了！我们将用 alias 命令创建我们自己的命令。但在开始之前，我们需要展示一个命令行小技巧。可以把多个命令放在同一行上，命令之间 用”;”分开。它像这样工作：

```bash
command1; command2; command3...
```

例如：

```bash
⋊> jiangyd@localhost ⋊> ~ cd /usr/bin/ ; pwd                                                   20:33:11
/usr/bin
```

那么，许多复杂的命令可以用这种方式完成：

```bash
# tanglab.pku.edu.cn上使用alphafold所需之吟唱
[jiangyd@localhost bin]$ module load anaconda3/anaconda3-2021.11
[jiangyd@localhost bin]$ source activate
(base) [jiangyd@localhost bin]$ conda activate alphafold
(alphafold) [jiangyd@localhost bin]$ cd /smb/jiangyd/alphafold-multitimer/alphafold/
(alphafold) [jiangyd@localhost alphafold]$ pwd
/smb/jiangyd/alphafold-multitimer/alphafold
```

这些代码可以简化成

```bash
module load anaconda3/anaconda3-2021.11; source activate; conda activate alphafold; cd /smb/jiangyd/alphafold-multitimer/alphafold/
```

```bash
[jiangyd@localhost ~]$ module load anaconda3/anaconda3-2021.11; source activate; conda activate alphafold; cd /smb/jiangyd/alphafold-multitimer/alphafold/
(alphafold) [jiangyd@localhost alphafold]$ pwd
/smb/jiangyd/alphafold-multitimer/alphafold
```

可以看到，二者完全一致。那么，对于如此复杂的命令组合，我们能否创建一个简单的命令来代替这一整套流程呢？

首先，我们需要像一个新的名字**。** 比方说”test”。在使用”test”之前，最好先查明”test”命令名是否已经存在于系统中。 为此，可以使用 type 命令：

```bash
[jiangyd@localhost alphafold]$ type test
test is a shell builtin
```

哦！”test”名字已经被使用了。试一下“startaf2”:

```bash
[jiangyd@localhost alphafold]$ type startaf2
bash: type: startaf2: not found
```

太棒了！它没有被占用，现在，用它来命名上述的一整套命令：

```bash
# alias <new_name>="<code_string>"
# 等号前后没有空格
alias startaf2="module load anaconda3/anaconda3-2021.11; source activate; conda activate alphafold; cd /smb/jiangyd/alphafold-multitimer/alphafold/"

```

来试试吧：

```bash
[jiangyd@localhost alphafold]$ startaf2
(alphafold) [jiangyd@localhost alphafold]$ pwd
/smb/jiangyd/alphafold-multitimer/alphafold
```

太棒了！看看我们的别名吧：

```bash
(alphafold) [jiangyd@localhost alphafold]$ type startaf2
startaf2 is aliased to `module load anaconda3/anaconda3-2021.11; source activate; conda activate alphafold; cd /smb/jiangyd/alphafold-multitimer/alphafold/'
```

如果不需要了，你也可以删掉它：

```bash
unalias startaf2
```

但是，在命令行中定义别名有点个小问题。当你的 shell 会话结束时，它们会消失。因此，你可以把它放在~/.bashrc里，这样你每次启动bash shell就会自动完成定义了！其他shell也是类似的方法，试试看编写一些简单的口令吧,比如：

```bash
# 放入.bashrc 
# vim ~/.bashrc
alias la="ls -alt"
alias start_http_proxy="export http_proxy='127.0.0.1:7890';export https_proxy='127.0.0.1:7890';"
alias stop_http_proxy="export http_proxy='';export https_proxy='';"
```