---
title: 搭建GitHub主页的常见问题
tags:
  - GitHub
  - Hexo
  - Node.js
  - Material主题
categories:
  - 网络技术
date: 2022-11-24 22:00:00
---

笔者在亲手搭建GitHub主页时，时常遇到一些奇怪的故障，这些故障或是由插件或网页主题的代码错误引起，或是由于不可言说的网络问题所致，现将目前为止遇到的较为棘手的故障，以及对应的解决方案，一并汇总在这篇文章中，供以后采用Hexo搭建博客时参考。

### GitHub部分

#### 1. hexo d部署时一直出现“Connection reset”或“Connection redirected(?)”，之后弹出“Spawn failed”错误，导致生成的网页一直部署不上去

**问题描述**

在git bash界面中（当前路径在博客根目录），输入`hexo d`并运行，弹出类似于下面的错误信息：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig1_1.png" style="zoom: 60%;" />
</center>
<center><strong>图1.1</strong>	运行<code>hexo d</code>命令后，会弹出“Spawn failed”错误</center>

尝试了网上的解决方案，没有效果，怀疑是网络连接问题。为验证部署错误是否由网络连接引起，在git bash界面中输入`ssh -T git@github.com`并运行，弹出下列错误提示：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig1_2.png" style="zoom: 60%;" />
</center>
<center><strong>图1.2</strong>	试图以SSH方式登录GitHub，会弹出“Connection reset”错误</center>

考虑到图1.2是在实验室有线网下测试得到的，笔者又在校园网环境测试，结果如下：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig1_3.png" style="zoom: 60%;" />
</center>
<center><strong>图1.3</strong>	切换为校园网后，能以SSH方式成功登录GitHub</center>

据此可以认为是本地Git服务与GitHub仓库之间的连接出现了问题。

**解决方案**

考虑到国内设立的“网络叹息之墙”，想要在国内直接连接GitHub可谓难于登天。目前尚能使用的解决方案如下：

**(1) 换用能连通GitHub的DNS**

目前（2022.11），国内无法访问GitHub的直接原因是DNS污染，因此，只要将GitHub及相关服务的域名，映射到尚未污染的IP地址，理论上就能访问GitHub。提供此类服务的网站很多，笔者较为推荐的是[Fetch GitHub Hosts](https://hosts.gitcdn.top/)，它不但能提供实时更新的GitHub hosts文件，还附有集自动获取IP与配置hosts文件于一体的插件，能充分解决广大程序员无法访问GitHub的问题。

**(2) 开启移动热点或登录校园网**

CSDN部分博主发现，部分地区连接移动热点时，GitHub网站能正常访问，本地Git服务也能从GitHub仓库正常获取代码，这可能与部分地区移动服务尚未被污染有关。与此同理，教育网（大学校园网）因支持IPv6，且管理较为宽松，因此在教育网环境下，GitHub没有遭受DNS污染，可以正常访问与使用。

**(3) 使用GitHub Desktop**

如果更换DNS不起作用，又没有不被DNS污染的网络环境，但还能使用代理服务器的话，GitHub Desktop是一个不错的选择，它的使用方法，可以参考[基于Hexo框架的GitHub个人主页搭建教程（后篇）](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial_sequel/)的第六部分。

**(4) 手动配置本地Git的代理选项**

当然，倘若你真的不喜欢GitHub Desktop的图形化界面，又或者对命令行的Git bash有特殊需求，那么在能确保代理服务器能正常运行的前提下，最后的救命稻草，就是手动配置本地Git的代理选项。

首先，我们要明确的一点，就是Git插件可通过两种连接协议，访问远程的Git仓库（包括GitHub），一种协议是HTTP或HTTPS协议，例如根据GitHub仓库的网页链接，从GitHub下载源码：

```bash
git clone https://github.com/chemlzh/chemlzh.github.io
```

另一种是SSH协议，例如利用GitHub的SSH服务，将本地代码上传至远程仓库：

```bash
git push git@github.com:chemlzh/chemlzh.github.io.git
```

对于HTTP/HTTPS协议，如果要设置代理服务器，则在Git bash中输入如下命令：

```bash
git config --global http.proxy http://127.0.0.1:port
git config --global https.proxy http://127.0.0.1:port
```

此处的`port`意为代理服务器使用的端口，该端口可通过“打开‘网络与Internet’设置”→“代理”→“手动设置代理”得知，见下图1.4。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig1_4.png" style="zoom: 40%;" />
</center>
<center><strong>图1.4</strong>	“代理”设置能反映出代理服务器的端口</center>

如果要取消代理设置，则在Git bash中输入如下命令：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

对于SSH协议，如果要设置代理服务器，则打开<code>C:\Users\&lpar;your_username&rpar;\&#46;ssh\config</code>（Windows用户）或<code>~/.ssh/config</code>（Linux或Mac用户），在其中添加如下内容并保存：

```bash
# For Linux or Mac OS X
Host github.com
    User git
    ProxyCommand nc -X connect -x 127.0.0.1:7890 %h %p

# For Windows
Host github.com
    User git
    ProxyCommand connect -H 127.0.0.1:7890 %h %p
```

如果要取消代理设置，则将SSH的config文件中关于GitHub的部分删除或注释，之后保存即可。

**参考资料**

[reset by 192.30.255.112 port 22；connect to host github.com port 22: Connection timed out——解决问题的过程记录](https://blog.csdn.net/QianQiYing/article/details/104999948?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-104999948-blog-103767426.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-3-104999948-blog-103767426.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=4)

[Github打不开怎么办](https://liyangweb.com/webnear/689.html)

[ssh: connect to host github.com port 22 修改端口不起效情况下的解决方法](https://blog.csdn.net/qq_37754830/article/details/122606405?spm=1001.2101.3001.6650.9&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-122606405-blog-104999948.pc_relevant_3mothn_strategy_and_data_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-9-122606405-blog-104999948.pc_relevant_3mothn_strategy_and_data_recovery&utm_relevant_index=9)

[How to configure Git proxy?](https://itsmycode.com/configure-git-proxy/)

[一文让你了解如何为 Git 设置代理](https://ericclose.github.io/git-proxy-config.html)

### Node.js与Hexo部分

#### 1. 我按照网上的教程，把Node.js源换成阿里源了，可为什么还是没法顺利下载hexo包呢

目前网上的换源教程，都会推荐使用国内的阿里源，这样可以避免由大防火墙带来的下载中断等问题。但要注意的是，自2022年5月底开始，原阿里npm源域名[http://npm.taobao.org](http://npm.taobao.org)和[registry.npm.taobao.org](registry.npm.taobao.org)停止使用，启用的新域名分别为[http://npmmirror.com](http://npmmirror.com)和[registry.npmmirror.com](registry.npmmirror.com)。而绝大部分教程（如[这篇](https://www.jianshu.com/p/09e98b1072dc)）都是在2022年5月前发布的，且发布后没有及时更新，因此提供的阿里源网址仍是老网址，使用后有可能提示无法访问或下载npm包。

**解决方案**

在命令行终端输入下列命令并运行，将npm源替换为新的阿里源

```bash
npm config set registry https://registry.npmmirror.com
```

**参考资料**

[npm install报错ERR! code ETIMEDOUT的解决办法](https://www.jianshu.com/p/09e98b1072dc)

[【公告】淘宝 npm 域名即将切换 && npmmirror 重构升级 && 微信交流群](https://zhuanlan.zhihu.com/p/465424728)

#### 2. 看新的阿里源推荐使用cnpm，这个cnpm与原来的npm有什么关系？

**太长不看版**

（1） npm即Node.js包管理器（node package manager），用于Node.js插件管理（包括安装、卸载、管理依赖等），cnpm是npm下属的一个插件包；（2） 实际使用时，cnpm等同于npm，但由于使用了国内的镜像源，因此更新Node.js插件的速度远快于npm

**详细解答**

正如“太长不看版”所述，npm即Node.js包管理器，它负责每个Node.js插件的安装、卸载与版本依赖，以及不同版本插件的配置与管理。当npm工作时，它必须从[官方插件库](https://registry.npmjs.org/)中获取待修改插件的信息与数据，然后才能进行下一步的修改。然而，由于“网络叹息之墙”的存在，国内与npm官方源的连接十分不稳定，容易出现连接超时甚至无法访问的现象。有鉴于此，npmmirror镜像（即原阿里源）团队开发了一个叫cnpm的Node.js插件，它继承了npm的所有功能，同时采用国内的镜像源下载插件，因此更新Node.js插件的速度远快于npm。

不过，考虑到cnpm默认使用的是软链接，因此npm安装了新插件后，极有可能更新了之前的cnpm包，然后之前cnpm的软链接就会……全 部 木 大！

所以，为了防止今后随时可能出现的包管理错误，还是老老实实用如下方式设置npmmirror镜像：

```bash
npm config set registry https://registry.npmmirror.com
```

**参考资料**

[npm 和 cnpm 的区别，你真的搞懂了嘛](https://www.cnblogs.com/chase-star/p/10455703.html)

[npm和cnpm混用的坑](https://juejin.cn/post/6878194344273117192)

#### 3. 为什么在安装完hexo-cli后，执行hexo init仍然报错，说找不到“hexo”命令？

**太长不看版**

有没有一种可能，是你设置了本地环境，却忘记添加对应的环境变量？

**详细解答**

有些人在安装完Node.js后，会按照[Windows 下 Node.js 的安装图文教程](https://blog.csdn.net/github_39655029/article/details/105397485)，更改Node.js包的安装路径，却没有将安装路径一并添加到全局环境变量PATH中，导致安装hexo-cli后，系统无法得到hexo对应的“正确的”运行程序，因此认为找不到“hexo”命令

**解决方案**

以Windows为例，依次打开“设置”→“系统”→“关于”，点击右侧的“高级系统设置”，弹出“系统属性”窗口，然后直接点击“环境变量”：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig2_1.png" style="zoom: 40%;" />
</center>
<center><strong>图2.1</strong>	在Windows系统中打开环境变量设置</center>

此时会打开Windows的环境变量窗口，我们到“全局变量”中找到`Path`，点击“编辑”，弹出“编辑环境变量”窗口，再点击“新建”。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/frequently_asked_questions_fig2_2.png" style="zoom: 40%;" />
</center>
<center><strong>图2.2</strong>	在环境变量设置窗口，编辑系统变量<code>Path</code></center>

在新打开的框中，填入hexo-cli的安装路径，然后单击“确定”，譬如笔者的安装路径为

```markdown
C:\Program Files\nodejs\node_global\node_modules\hexo-cli\bin
```

**参考资料**

（暂无）

### Material主题部分

#### 1. hexo g生成本地网页出错，错误信息中含有“Cannot read property 'startsWith' of null”、“dnsprefetch”等字眼，hexo clean以后错误仍然存在

根据[EasyHexo](https://easyhexo.com/)的安装指南，如果下载安装的是hexo-theme-material 1.5.2，应该不会出现这一错误。因此可以肯定的是，该错误主要出现在hexo-theme-material 1.5.5及更新版本。

从hexo-theme-material的Issue #709可知，该错误来自于dnsprefetch.ejs的第21行：

```javascript
<% } else if(theme.comment.use.startsWith("disqus")) { %>
```

这一部分原本作用是调用disqus评论服务，但在版本更迭的过程中，作者似乎忘记了theme.comment.use可能没有初始化（或等于null），因此没有加一条“是否开启（特定的）博客评论服务”的判断语句，导致布尔判断语句出错，进而导致后续的网页生成错误。

**解决方案**

a) 如果对Material主题包的版本不介意的话，安装旧版本的Material主题包即可解决问题。旧版本的Material主题包在[Material Releases](https://github.com/iblh/hexo-theme-material/releases)中，下拉找到1.5.2版，下载后按照教程安装即可。

b) 如果执意采用高于1.5.2的Material主题包，解决方案则是直接修改dnsprefetch.ejs的出错语句。假设Material主题包放在了博客的themes文件夹下，名字叫hexo-theme-material，则dnsprefetch.ejs应该位于`/(your_blog_name)/themes/hexo-theme-material/layout/_widget`。找到dnsprefetch.ejs，用记事本（或VSCode）打开，定位到第21行（查找“startsWith”也能定位到该语句），将其改为：

```javascript
<% } else if(theme.comment.use && theme.comment.use.startsWith("disqus")) { %>
```

保存后在博客根目录下启动git bash，输入hexo clean后运行，再输入hexo g重新生成，这样便能解决上述错误。

**参考资料**

[hexo g出错 hexo clean以后无效 按照hexo文档进行的操作 （Issue #709）](https://github.com/iblh/hexo-theme-material/issues/709)

#### 2. 安装hexo-render-mathjax后，有时在语句两端各加两个星号，非但不能加粗字体，反而出现了部分字符重复出现的错误，怎么解决？

**解决方案**

这应该是hexo-render-mathjax对部分转义字符，尤其是`/`字符的解析出现偏差导致的，因此，将前后的两个星号，替换为`<strong></strong>`，基本上就能解决大部分字符莫名其妙重复的问题。同理，如果原来是程序语句的部分，也出现了字符重复的渲染错误，则将原来的<code>&#96;&#96;</code>改写为`<code></code>`。

（这不就是告诫我们，多学一点HTML嘛！嗐！）

**参考资料**

（暂无）