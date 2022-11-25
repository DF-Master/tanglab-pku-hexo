---
title: 基于Hexo框架的GitHub个人主页搭建教程（后篇）
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

（限于篇幅，笔者将搭建教程拆分为两篇。嗯，还记得之前做了什么工作？[回去看看吧](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial/)）

<!--more-->

### Part IV. 做一次大装修，让个人主页更美观

虽然我们给个人主页套上了色彩鲜艳的Material主题，但细细品味会发觉，我们的主角——博客文章，还没有登场！而博客文章的“好兄弟”——分类、标签、404页面……也不见踪影。另一方面，个人主页的边边角角，也需要我们亲自打理一番，这当中就包括背景和题图。下面，就随着笔者的思路，看一看怎么给个人主页，做一次全方位的大装修吧！

#### 为GitHub个人主页添加文章

充实GitHub主页的第一步，自然是添加文章。按照惯例，我们首先到本地的个人主页根目录，右键弹出菜单，点选“Git bash”将其打开，然后输入如下命令，创建一篇新文章：

```bash
hexo new post "(new_post_title)"
```

此处的`(new_post_title)`指文章的标题，例如创建一篇标题叫“Are you OK?”的文章，就在Git bash输入`hexo new post "Are you OK?"`，并按下回车。

这时我们进入source文件夹，会发现生成了一个名为“_posts”的文件夹，点进去一看，里面生成了一个Markdown文件——“Are-you-OK.md”，用于撰写博客文章。如果开启了附件文件夹功能，还会连带生成一个叫“Are-you-OK”的文件夹，用来存放文章插图等附件。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_1.png" style="zoom: 60%;" />
</center>
<center><strong>图4.1</strong>	在_post文件夹中新生成的博客文章和附件文件夹（仅用于示例，之后删除）</center>

打开“Are-you-OK.md”，可以看到文件中写有如下内容：

```markdown
---
title: Are you OK?
date: 2022-11-22 09:47:59
tags:
---
```

这是每一个通过`hexo new`命令创建的Markdown文件，都会自动生成的内容，它包含着标题、创建日期、标签等信息，并可以通过修改scaffold文件夹下的post.md等模板文件，规定每一次创建Markdown文件时写入的内容与格式。<strong>为保证`hexo g`生成网页代码时，不会因为缺失信息而出错，请务必不要删除文件开头的补充信息，尤其是title和date两行！如果要撰写文章，请务必在补充信息后另起一行，再开始写作！</strong>

剩下的部分，就是用Markdown语言撰写博客文章，事实上，除了几种较为特殊的格式外，大多数情况下，使用Markdown写作与使用Word等软件写作并无二致，并且<strong>在Typora、Notion等Markdown写作软件的帮助下，甚至不需要了解Markdown格式，只要单击右键唤出设置菜单，或是善加利用快捷键，就能完成绝大多数格式的设置。</strong>如果对Markdown语言非常感兴趣，也有[官方英文教程](https://www.markdownguide.org/)和[菜鸟编程的指南](https://www.runoob.com/markdown/md-tutorial.html)可供参考，而[Markdown 公式指导手册](https://www.cnblogs.com/RioTian/p/14111021.html)则进一步介绍如何在Markdown中添加数学公式，这对于提升Markdown水平大有裨益。

#### 为GitHub个人主页添加分类与标签

撰写个人博客越久，各类博文积累得越多，不同主题的文章交织在一起，不利于知识点的归纳总结。因此，给现有的博客文章分门别类，也就变得愈发重要。

在Hexo框架下，分门别类是依靠分类（或称类别，Categories）和标签（Tags）实现的。按照笔者的理解，分类相当于主题，一篇文章只有在大方向上围绕特定概念阐述，才能归属于对应分类；标签相当于关键词，一篇文章只要提及特定概念，就可以加上对应的标签。而要想使用Hexo的分类与标签功能，就必须分别创建分类页与标签页，并在每篇文章的页面信息部分，添加对分类与标签的描述，才能给个人主页的文章添加分类与标签。

以分类为例，在根目录下打开Git bash，输入`hexo new page "categories"`并回车，此时会创建一个分类页，其源码地址为`/source/categories/index.md`，我们打开这个`index.md`，把页面信息修改为：

```markdown
---
title: categories
date: 2022-11-17 15:55:30
type: categories
layout: categories
---
```

之后保存并退出，转到`/source/_posts`下的各篇文章，打开后按如下格式添加分类（<strong>注意：应用Material主题后，直接在categories后面空一格写分类，编译时似乎无法识别，一定要换行后空两格，写成<code>  - (your_category_name)</code>的形式才能识别</strong>）：

```markdown
---
title: ×××
categories:
  - ××××
date: 20××-××-×× ××:××:××
---
```

同理，若输入`hexo new page "tags"`并回车，此时会创建一个标签页，其源码地址为`/source/tags/index.md`，仿照前面的做法，我们把`index.md`的页面信息修改为：

```markdown
---
title: tags
date: 2022-11-17 15:56:57
type: tags
layout: tags
---
```

此时博客文章的页面信息也要相应修改为（<strong>注意：应用Material主题后，标签同样要写成<code>  - (your_category_name)</code>的形式才能识别</strong>）：

```markdown
---
title: ×××
tags:
  - ××××
  - ××××
categories:
  - ××××
date: 20××-××-×× ××:××:××
---
```

将分类与标签整合在一起，文章的分门别类就做好了，例如本篇文章的页面信息，是这样设定的：

```markdown
---
title: 基于Hexo框架的GitHub个人主页搭建教程（后篇）
tags:
  - GitHub
  - Hexo
  - Node.js
  - Material主题
categories:
  - 网络技术
date: 2022-11-20 11:59:30
thumbnail: /2022/11/20/guide-to-creating-a-GitHub-page-with-Hexo-framework-sequel/thumbnail.png
---
```

可以看到，这里的分类与标签有中文，有英文，还有标点。出于程序员的习惯，笔者不想在编译时生成中文分类与标签（否则在地址栏显示时，会变成乱码，标点符号同理），只想让这些分类与标签，在最终显示的网页界面上以中文表示。对此，Hexo框架也有自己的对策——`category_map`和`tag_map`，直译过来就是“分类映射”与“标签映射”。这两者是根目录`_config.yml`的设置选项，其格式满足：

```yaml
category_map:
  category_name1: category_address1
  category_name2: category_address2
tag_map:
  tag_name1: tag_address1
  tag_name2: tag_address2
```

上面的代码中，`category_name`（或`tag_name`）代表分类（或标签）在博客文章中的命名，而`category_address`（或`tag_address`）代表分类（或标签）在生成网页源码时采用的命名，例如本篇文章的映射是这样设计的：

```yaml
category_map:
  网络技术: networking
tag_map:
  Node.js: nodejs
  Material主题: hexo-theme-material
```

它们的作用是：把原文页面信息的“网络技术”分类，映射为“networking”，而把“Node.js”及“Material主题”这两个标签，分别映射为“nodejs”和“hexo-theme-material”。经过上述处理后，网页源代码中就不会出现乱码了。

最后，为了显示分类区与标签云，我们还要在主题文件夹的`_config.yml`，添加一段创建分类区与标签云的代码，具体而言，找到`sidebar:`对应的代码块，里面有一个选项叫`pages:`，其默认设置为：

```yaml
# Sidebar Customize
sidebar:
    ......
    categories:
        use: false
        icon: chrome_reader_mode
        divider: false
    pages:
        #About:
            #link: "/about"
            #icon: person
            #divider: false
    ......
```

将其更改为如下代码并保存：

```yaml
# Sidebar Customize
sidebar:
    ......
    categories:
        use: true
        links: "/categories"
        icon: chrome_reader_mode
        divider: false
    pages:
        #About:
            #link: "/about"
            #icon: person
            #divider: false
        tags:
            link: "/tags"
            icon: cloud_circle
            divider: false
    ......
```

我们将修改后的个人网页重新生成，得到的就是含有若干文章，且注有分类与标签的个人网页了，如图4.2与图4.3所示。值得注意的是，Material主题默认不采用分类页，而是把分类移动到侧边栏的折叠选项中，因此直接打开分类页，是看不到原来的分类的。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_2.png" style="zoom: 40%;" />
</center>
<center><strong>图4.2</strong>	Material主题的分类区（被收纳进侧边栏了）</center>

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_3.png" style="zoom: 40%;" />
</center>
<center><strong>图4.3</strong>	Material主题的标签页（原谅我壁纸色调过深，看不到标签了，悲）</center>

#### 为GitHub个人主页添加自定义404页面

> 404, can't live with them, can't live without them.
> ——反正不是我说的

如果让擅长“面向谷歌/百度编程”的程序员，提名“互联网最令人爱恨交加的东西”，404页面应该“榜上有名”。诚然，404页面弹出的那一瞬间，真的会令程序员崩溃——这意味着找到解决方案的希望再一次破灭了，但从另一角度分析，404界面也意味着，至少访问的页面（可能）曾经存在过，可以尝试用[Wayback Machine](https://archive.org/web/)追查历史快照，这何尝不是一种善意的提醒呢？而如果此时的404页面，显示的并不是简简单单的“404 Not Found”几个大字，而是幽默风趣的笑话，有意思的图片，甚至是一个小游戏，那么沮丧的心情，是不是也会感到些许轻松愉悦。

可惜的是，许多人搭建的GitHub个人主页，都没有设置自定义404页面，以至于每次我打开失效的个人主页链接，跳出来的都是这样一个玩意：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_4.png" style="zoom: 40%;" />
</center>
<center><strong>图4.4</strong>	GitHub的默认404页面（<del>噔 噔 咚</del>）</center>

这跟你们的个人主页的画风根本不搭啊！个人主页再怎么简陋，也得做一个基于本站框架的404页面吧！（路人：谁知道Hexo框架可以自定义404页面嘛？！）

当然，Hexo框架早就为大家准备了解决方案：在根目录下打开Git bash，输入`hexo new page "404"`并回车，就可以创建一个404页面，它的源码地址为`/source/404/index.md`，我们只要打开这个`index.md`，把页面信息修改为：

```markdown
title: 404 Not Found
date: 2022-11-20 13:03:13
comments: false
permalink: /404.html
```

其余部分就可以自由发挥啦！例如笔者的个人主页，使用的404页面是这样的：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_5.png" style="zoom: 40%;" />
</center>
<center><strong>图4.5</strong>	自定义404页面效果图（此处埋了个彩蛋，见<a href="https://chemlzh.github.io/404">本站404页面</a>）</center>

#### 为GitHub个人主页更换背景壁纸

不知诸位是否稍加留意过[Material主题的展示页](https://iblh.github.io/material-demo/)？细心的吃瓜群众会发现，这个展示页每篇文章的题图是色彩斑斓的，开启后的侧边栏也是色彩斑斓的，唯独背景，是一大块纯纯的灰白色！这不禁让人疑惑：为什么色彩斑斓的Material主题，宁肯用单色填充网页背景，也不愿意换上一张类似于题图风格的壁纸？其实，Material主题确实存有预设的背景壁纸，只是由于默认参数中没有启用背景壁纸，所以生成的网页自然都采用单色背景。

那么，怎样才能启用壁纸功能呢？很简单，打开主题包文件夹`themes/hexo-theme-material`下的`_config.yml`，定位到“background”附近，此时会看到如下代码：

```yaml
background:
    purecolor: "#F5F5F5"
    #bgimg: "/img/bg.png"
    bing:
        enable: false
        parameter:
```

如果要启用壁纸功能，就在`purecolor: "#F5F5F5"`前面加上一个“#”，并将`#bgimg: "/img/bg.png"`中的“#”删除，之后重新编译网页源码，就能在本地网页看到预设的背景壁纸。

等等，默认壁纸的地址为`/img/bg.png`，那这是否意味着，我们不但能启用壁纸，还能将Material预设的背景壁纸，更换为自定义壁纸？没错！在`themes/hexo-theme-material/source/img`下，确实存放着网页背景壁纸、网站logo、按钮图标等图片素材，我们只要把喜欢的背景壁纸，更名为`bg_new.png`（便于与原有的`bg.png`区别），然后拷贝到`themes/hexo-theme-material/source/img`当中，如图4.3所示：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_6.png" style="zoom: 40%;" />
</center>
<center><strong>图4.6</strong>	将自定义壁纸拷贝到默认壁纸所在的文件夹</center>

之后编辑主题文件夹的`_config.yml`，将“background”改为

```yaml
background:
    #purecolor: "#F5F5F5"
    bgimg: "/img/bg_new.png"
    bing:
        enable: false
        parameter:
```

重新编译网页源码，原来的单色背景就被替换成自己喜欢的背景壁纸了，好耶！

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_7.png" style="zoom: 40%;" />
</center>
<center><strong>图4.7</strong>	替换壁纸后的效果图（此处已更换题图，方法见下一部分）</center>

#### 为你的文章更换题图

在[上一篇教程](http://tanglab.pku.edu.cn/2022/11/24/R&D/2022/GitHub_pages_tutorial/)中，笔者曾提及，Material主题会为每一篇文章加上一张随机选取的题图。对于疏于装饰的博主们而言，Material自带的题图足以让人眼前一亮，但对于喜欢妆点主页的博主而言，Material自带的题图显然并不能满足自己的口味。这时候，启用Hexo的<strong>资源文件夹</strong>（asset folder，笔者更喜欢叫附件文件夹），就可以把随机选取的预设题图，换成萌萌哒的小姐姐，这样工作效率就能达到一万三千点呢（笑）。

**资源文件夹**的启用方法很简单，只要在根目录的`_config.yml`中，找到`post_asset_folder: `这一选项，将其更改为

```yaml
post_asset_folder: true
```

今后用`hexo new post (...)`创建新文章时，就会在存储文章的同一目录下，生成同名的资源文件夹，这是不是很方便呢？当然，已有的老文章，也可以通过在同一目录下，直接创建与其同名的空文件夹，从而达到生成资源文件夹的目的。

成功启用资源文件夹后，剩下的事情易如反掌：打开想要更换题图的文章，在其页面信息部分，添加一行与题图有关的信息，其中article_year、article_month、article_day、article_name，分别对应于文章创建的年、月、日，以及存储文章的Markdown文件名：

```markdown
thumbnail: /(article_year)/(article_month)/(article_day)/(article_name)/thumbnail.png
```

接下来，进入与文章同名的资源文件夹，把裁剪后的题图（建议高宽比为7: 22）复制到该文件夹中，并重命名为thumbnail.png（或其他图片格式），之后重新编译网页源码，就能看到替换的题图啦！

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_8.png" style="zoom: 40%;" />
</center>
<center><strong>图4.8</strong>	更换题图后，本篇文章在首页的样式</center>

事实上，Hexo的<strong>资源文件夹</strong>还有许多用途：不仅能存放题图，还能<strong>存放文章中使用的插图</strong>，极大省去额外借用或搭设图床的麻烦；不仅能存放图片，还能<strong>存放可视化图表、音乐、视频等</strong>，并能借助第三方插件进行调用，<strong>实现复杂的展示效果</strong>。对于大多数博主而言，这里最重要的功能是插入配图，而基于Markdown格式的便捷调用方式，则会在Part V中详细阐述，此处按下不表。

#### 实装代码高亮功能

原生Hexo的代码高亮插件似乎存在问题，无法识别bash等编程语言，致使这些语言的代码块全部不显示高亮。好在Material主题提供了两种代码高亮插件prettify和hanabi，它们都能正常识别大部分主流编程语言，并根据语言种类正确渲染。这里我们采用prettify，实现GitHub个人主页的代码高亮功能。

首先，为防止原生Hexo的代码高亮插件和prettify同时运行，造成行号重复出现等错误，我们要打开根目录下的`_config.yml`，找到“highlight”部分：

```yaml
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
```

将其中的“enable”和“line_number”选项均改成“false”，保存修改。之后打开主题文件夹下的`_config.yml`，查找并定位“prettify”或“hanabi”，得到下列代码段：

```yaml
# Code highlight
# You can only enable one of them to avoid issues.
# Also you need to disable highlight option in hexo's _config.yml.
#
#    Prettify
#        theme: # Available value in /source/css/prettify/[theme].min.css
prettify:
    enable: false
    theme: "github-v2"

#    Hanabi (https://github.com/egoist/hanabi)
#        line_number: [true/false] # Show line number for code block
#        includeDefaultColors: [true/false] # Use default hanabi colors
#        customColors: This value accept a string or am array to setting for hanabi colors.
#                    - If `includeDefaultColors` is true, this will append colors to the color pool
#                    - If `includeDefaultColors` is false, this will instead default color pool
hanabi:
    enable: false
    line_number: true
    includeDefaultColors: true
    customColors:
```

将“prettify”下属的“enable”选项改成“true”，保存修改，此时prettify插件已经启用。在根目录下打开Git bash，输入`hexo clean && hexo g`，重新生成本地网页，然后输入`hexo s`，就可以在本地服务器上，看到prettify渲染后的代码块。

如果对默认的“github-v2”格式不太满意，可以到`\themes\hexo-theme-material\source\css\prettify`下，查看prettify支持的代码渲染格式，它们的渲染效果参见[Color themes for Google Code Prettify](https://jmblog.github.io/color-themes-for-google-code-prettify/)。之后，回到主题文件夹下的`_config.yml`，往prettify下属选项theme后面的双引号内，填入喜欢使用的代码渲染格式，例如笔者喜欢“Tomorrow Night Bright”主题，就将theme选项那一行改为`theme: "tomorrow-night-bright"`并保存，之后生成本地网页，所有的代码块的主题就都变成“Tomorrow Night Bright”，如下图所示：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig4_9.png" style="zoom: 40%;" />
</center>
<center><strong>图4.9</strong>	prettify插件采用“Tomorrow Night Bright”主题的渲染效果</center>

### Part V. 部分实用插件的安装与配置

#### 数学公式插件：hexo-renderer-mathjax

身为一个化学搬砖工，有时也不可避免跟数学公式打交道，虽然Markdown支持以LaTeX格式书写数学公式，但是个人主页采用的Hexo框架，却没有自带渲染数学公式的插件，也就不能将LaTeX格式的数学公式，转换为便于阅读的印刷体。幸好，通过安装第三方开发的MathJax插件，原生Hexo博客上无法显示的LaTeX形式的数学公式，也能得到正确的解析与渲染，妈妈再也不用担心博客写不出公式了（笑）。

在启用第三方MathJax插件前，我们首先要把默认的文字渲染引擎`hexo-renderer-marked`，更换为加强版的`hexo-renderer-kramed`，这是因为`marked`没有对MathJax的支持。为此，我们在根目录启用Git bash，输入以下命令：

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-kramed --save
```

下一步是修改`hexo-renderer-kramed`的渲染代码，它位于`node_modules/hexo-renderer-kramed/lib/renderer.js`。找到下列代码：

```javascript
// Change inline math rule
function formatText(text) {
    // Fit kramed's rule: $$ + \1 + $$
    return text.replace(/`\$(.*?)\$`/g, '$$$$$1$$$$');
}
```

将其改为

```javascript
// Change inline math rule
function formatText(text) {
    return text;
}
```

这是因为在`kramed`下，行内公式的插入形式为<code>&#96;$(math_equation)$&#96;</code>，而Markdown行内公式的插入形式为<code>$(math_equation)$</code>，为符合Markdown的使用习惯，只能将formatText的返回值改为`text`。

第三步，才是安装第三方MathJax插件，在此之前，为避免可能发生的插件冲突，我们要彻底卸载Hexo原生的数学插件。在根目录打开Git bash，运行下列命令，卸载`hexo-math`：

```bash
npm uninstall hexo-math --save
```

然后执行下列安装命令，这里我们选用`hexo-renderer-mathjax`：

```bash
npm install hexo-renderer-mathjax --save
```

从个人主页的根目录下，找到其下的`/node_modules/hexo-renderer-mathjax/mathjax.html`，用记事本或VSCode打开，定位到最后一行（其格式为`<script src="..."></script>`），将其替换为

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>
```

到这一步，MathJax的配置大致完成，之后是针对Markdown书写格式兼容性的修改，以及MathJax的全局启用。从个人主页的根目录下，转到`node_modules/kramed/lib/rules/inline.js`，根据`var inline = {...}`定位代码块，其大致分布在第10-28行。我们将原有的escape和em选项注释掉，它们的形态大致为

```javascript
escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

并在对应行后面另起一行，填入新的escape和em选项，确认无误后保存。这样，Kramed的转义规则就不会把_和^转义为<code>&lt;em&gt;</code>。

```javascript
escape: /\\([`*\[\]()# +\-.!_>])/,
em: /\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

最后，回到根目录的`_config.yml`，在末尾添加如下代码并保存，之后重新编译网页，就能让个人网页正确渲染Markdown公式了。

```yaml
mathjax:
  enable: true
```

#### 以Markdown格式添加插图与站内超链接：hexo-asset-link

众所周知，[原生的Hexo不支持以Markdown语法格式，结合相对路径插入图片](https://hexo.io/zh-cn/docs/asset-folders)，因为这可能会导致它们在存档页或者主页上显示不正确。如果执意要以相对路径的形式，在原生Hexo博客中添加插图或超链接，就必须要编写成如下形式：

```markdown
{% asset_path /.../... This is an example path %}
{% asset_img example.jpg This is an example image %}
{% asset_link https://.../... This is an example path %}
```

而Markdown结合相对路径添加添加插图或超链接，则可以写成如下格式：

```markdown
[This is an example path](/.../...)
[This is an example image](example.jpg)
[This is an example path](https://.../...)
```

可以看到，原生Hexo格式，很难一眼分辨出哪部分是图片（或超链接地址），哪部分是文字说明，并且还要额外指定插入的是何种类型的附件；相比之下，Markdown格式把文字说明和附件地址，分别放在了中括号和小括号，并且不需要指定附件类型。两相比较，高下一目了然——自然是Markdown格式更胜一筹。

目前，许多开发者已经开发出Hexo原生格式到Markdown格式的转化插件，其中常用的有hexo-asset-link，它的安装方法很简单，只需在根目录运行如下命令即可：

```bash
npm install -s hexo-asset-link
```

安装后，确保根目录`_config.yml`中，`post_asset_folder: `选项为true，且_posts同时存有博客文章和同名资源文件夹，就能往GitHub个人主页添加插图。例如以Markdown格式插入example.jpg，就将example.jpg拷贝至资源文件夹，然后打开对应的博客文章，在指定位置写下`[This is an example image](example.jpg)`。

又如，要想插入站内链接，就往博客文章中写下

```markdown
[This is an example path](/(article_year)/(article_month)/(article_day)/(article_name))
```

之后重新编译网页，就能在博客内显示相应的站内链接。

#### 让站内搜索帮你找文章：hexo-generator-search

相信诸位都会有这样的经历：明明记得在某个网站上看到一篇非常实用的文章，结果因为忘记收藏，下次再想调取时，只能凭着记忆一篇篇翻阅，可谓是劳力又劳心。而即将介绍的站内搜索插件，便能将这样的苦恼一扫而空！

安装方法也非常简单，首先到个人主页的根目录，打开Git bash，输入如下命令，安装hexo-generator-search：

```bash
npm install hexo-generator-search --save
```

然后进入`/theme/(your_material_theme_name)`（笔者的路径为`/theme/hexo-theme-material`），用记事本（或VSCode）打开其下的_config.yml，找到第276-278行附近的位置（或者搜索“search”也能找到），会有这样一段代码：

```yaml
search:
    use: google
    swiftype_key:
```

将`use: google`改成`use: local`，其他选项不变，然后保存。

接下来回到个人主页的根目录，用记事本（或VSCode）打开此处的_config.yml，在文件末尾补上如下语句，然后保存：

```yaml
search:
  path: search.xml
  field: all
```

最后在根目录打开Git bash，输入`hexo clean && hexo g`，重新生成网页源码，然后用`hexo s`启动本地服务器，就能在[http://localhost:4000](http://localhost:4000)试用站内搜索功能啦！

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig5_1.png" style="zoom: 60%;" />
</center>
<center><strong>图5.1</strong>	站内搜索插件实装效果</center>

### Part VI. 将本地生成的个人主页部署到GitHub

经过前几部分的努力，我们终于在本地上，把简陋的初始页面打扮得焕然一新，现在，我们只要把本地生成的个人主页，上传到GitHub对应的代码仓库，就可以看到更新后的GitHub个人主页了！考虑到GitHub在国内日常崩坏的连接质量，笔者分别介绍两种部署方法，保证无论身处何地，更新永不缺席！

#### 流传于网络的部署方法：Hexo直接部署

Hexo直接部署，可谓是流传最广的部署方法，你在网络上搜索到的基于Hexo框架的GitHub个人主页搭建教程，百分之九十以上都会推荐使用Hexo自带的部署功能，实现GitHub个人主页的更新。此法无需下载额外的软件，只需打开根目录下的_config.yml，找到末尾的deploy部分：

```yaml
deploy:
  type:
```

将其改为如下内容并保存（`(your_username)`代表GitHub用户名，`(your_sitename)`代表创建GitHub主页时使用的用户名，一般等于`(your_username)`）

```yaml
deploy:
  type:
  repository: git@github.com:(your_username)/(your_sitename).github.io.git
  branch: master
```

然后，在本地个人主页的根目录打开Git bash，先运行`hexo g & hexo s`，并访问[http://localhost:4000](http://localhost:4000)，以确认生成的网页外观与内容符合自己的预期。接着，按下Cltr+C，终止本地服务器预览，随后输入`hexo d`，回车运行，本地Git便会把生成的网页源码，推送到GitHub个人主页对应的代码仓库。待部署成功后，等待几分钟，就可以访问`https://(your_sitename).github.io`，欣赏一下更新后的GitHub个人主页啦！

看起来一切都很美好，但是，实际操作中，往往会出现部署不上去的尴尬情景：

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_1.png" style="zoom: 75%;" />
</center>
<center><strong>图6.1</strong>	世界名画：动啊，Git，你为什么不动！</center>

而且用`hexo d`命令部署时，我还开着代理服务器！为什么本地的Git还是认为无法与GitHub服务器建立连接！

于是，我遍历Stackoverflow、CSDN等计算机技术论坛，又问了身边懂服务器运维的同学，结合之前更新课题组网站（那时课题组主页还是用Hugo搭建）的经验，终于鼓捣出“逃课手法”——

#### 饱受忽略的“旁门左道”：分支合并部署

这里又会有吃瓜群众不解了：你用Hexo直接部署，实际上利用了本地Git的推送功能；你为GitHub主页的代码仓库建立一个分支，然后把本地源码上传到分支仓库，最后把分支仓库合并到主干仓库，同样用到本地Git的推送功能。既然Hexo直接部署无法上传，凭什么把代码上传到分支仓库就可以呢？别急，容我推荐一款救急神器——GitHub Desktop！

首先，我们到[GitHub Desktop官网](https://desktop.github.com/)，点击中间的紫色按钮，下载64位Windows版GitHub Desktop。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_2.png" style="zoom: 40%;" />
</center>
<center><strong>图6.2</strong>	GitHub Desktop官网首页</center>

双击安装包，勾选用户协议，然后保持默认选项不变，一路点选“下一步”，完成安装。

之后启动GitHub Desktop，此时它会弹出登录界面，提示你输入GitHub账号和密码。我们按照指示，依次填写创建GitHub个人主页所使用的账号与密码，点击“Sign in”登录，此时GitHub会往注册邮箱发送含有六位验证码的邮件，并提示我们填入正确的验证码，我们只需打开邮箱，将验证邮件附带的验证码复制过来，就可以完成登录。

登录后的GitHub Desktop界面如图6.3所示，除菜单栏外，可分为如下几个部分：

（1）<strong>地址栏与状态栏</strong>，它们位于窗口正上方，其中地址栏用于显示和切换需要更改的代码仓库与分支，而状态栏则表示本地的代码仓库是否与远程的GitHub仓库同步；

（2）<strong>修改记录区</strong>，它位于窗口左侧，会标注出本地的代码仓库中哪些文件进行了修改，哪些文件被添加，哪些文件被移除；

（3）<strong>提交总结与描述区</strong>，当本地的代码仓库中有文件修改或增删，而持有者想要把上述变动应用到远程的GitHub仓库时，就要在这个区域写下提交代码的总结，以及本次提交的代码相较于前一版本的代码有何异同；

（4）<strong>工作区</strong>，它占据着窗口的大部分，会显示修改记录区中选中的文件，是在具体的哪个区域发生了变动，变动前后的内容又是什么。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_3.png" style="zoom: 40%;" />
</center>
<center><strong>图6.3</strong>	登陆后的GitHub Desktop界面</center>

我们把光标移到菜单栏中的“File”，找到“Clone repository”选项并单击，以打开爬取代码仓库的窗口。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_4.png" style="zoom: 40%;" />
</center>
<center><strong>图6.4</strong>	从菜单栏找到下载代码的按钮</center>

接着选取名字为`(your_username).github.io`的仓库（更准确说是`(your_sitename).github.io`，配图图注懒得改了），然后在“Local path”的空白框中，填上希望储存的地址，点击“Clone”，GitHub Desktop就会把远程仓库的代码，下载到指定的本地地址。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_5.png" style="zoom: 40%;" />
</center>
<center><strong>图6.5</strong>	把个人网页对应的远程GitHub仓库下载到本地</center>

<strong>之后的重头戏来了：</strong>点击地址栏中的“Current branch”，会弹出显示所有分支的下拉菜单，点击其中的“New branch”按钮，以打开创建新分支的窗口。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_6.png" style="zoom: 40%;" />
</center>
<center><strong>图6.6</strong>	从地址栏找到创建新分支的按钮</center>

然后填写新分支的名字（<strong>注意：</strong>不能与主干仓库master，以及其他已有分支仓库撞名，例如图6.7中，该仓库已经有一条名为“direct_push_branch”的分支，那么继续创建新分支时，就不能叫“direct_push_branch”），点击“Create branch”，在GitHub上生成分支仓库。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_7.png" style="zoom: 40%;" />
</center>
<center><strong>图6.7</strong>	为GitHub仓库创建新分支</center>

接下来，到本地的个人主页根目录打开Git bash，输入`hexo clean && hexo g`，重新生成源代码，然后打开根目录下的“public”文件夹，将其下的所有文件与文件夹，拷贝到刚刚下载到本地的GitHub仓库中，此时GitHub Desktop会显示添加的文件名称，以及每个文件增添的内容。我们到左下角填写提交总结与提交描述，点击“Commit to (your_branch_name)”，GitHub Desktop便会向远程的GitHub仓库发出提交申请。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_8.png" style="zoom: 40%;" />
</center>
<center><strong>图6.8</strong>	确保源码拷贝至新分支的本地仓库后，填写提交总结与提交描述，然后提交</center>

提交申请后，GitHub Desktop的工作区就会变回默认形态，只是多了一个名为“Push commits to the origin remote”的提示框，要求你把刚才提交的源代码，上传到GitHub仓库对应的分支上，我们只需按照要求，点下蓝色的“Push origin”按钮即可，GitHub Desktop会自动帮我们完成上传的工作。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_9.png" style="zoom: 40%;" />
</center>
<center><strong>图6.9</strong>	将提交的代码上传到GitHub对应的分支上</center>

上传源码后，GitHub Desktop又会弹出一个叫“Create a Pull Request from your current branch”的提示框，要求你把分支仓库的代码，全部合并到主干仓库中（分支仓库的代码依然存在，且保持不变），我们仍旧照做，点下蓝色的“Create Pull Request”按钮。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_10.png" style="zoom: 40%;" />
</center>
<center><strong>图6.10</strong>	发起分支仓库到主干仓库的合并</center>

这时GitHub Desktop不会在软件中处理合并代码的事务，而是用浏览器直接打开GitHub，并弹出类似于图6.11的Pull Request界面（<strong>所以说为什么Pull Request功能没有整合到GitHub Desktop里面？！</strong>），我们再次点下“Create pull request”，让合并工作得以进行。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_11.png" style="zoom: 40%;" />
</center>
<center><strong>图6.11</strong>	到跳转的网页版GitHub中，发起分支仓库到主干仓库的合并</center>

此时GitHub网页会发生一次跳转，如果弹出的提示框说“This branch has no conflicts with the base branch”，即分支仓库与主干仓库没有文件版本的冲突，就可以放心地点击“Merge pull request”，完成合并的最后一步。

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_12.png" style="zoom: 40%;" />
</center>
<center><strong>图6.12</strong>	检查分支仓库与主干仓库无冲突后，确认合并</center>

等到GitHub网页弹出“Pull request successfully merged and closed”的提示框，就意味着部署工作大功告成了！等待几分钟，再访问`https://(your_sitename).github.io`，你的GitHub个人主页就应该发生翻天覆地的变化啦！

<center class="half">
<img src="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202211/creating_GitHub_pages_fig6_13.png" style="zoom: 40%;" />
</center>
<center><strong>图6.13</strong>	大功告成！累死我了！</center>

根据笔者的测试，在同样挂着代理服务器的情况下，用hexo d直接部署，会一直弹出无法访问远程仓库的错误，而用GitHub Desktop结合分支仓库部署，则能正常推送源码，完成整个个人网页的更新。笔者推测，可能是因为GitHub Desktop能根据代理服务器的启用情况，自动适配网络和端口设置，而直接运行hexo d时，由于本地Git服务不会对代理服务器进行任何配置（需要手动调整），因此代理服务器并不能帮助Git与GitHub之间建立稳定的传输连接，这才是分支合并部署得以实现的主要原因。

#### 后记——关于我费尽九牛二虎之力，却发现自己绕了大远路这件事

昨天写完GitHub主页搭建教程，发给课题组懂服务器运维的同学审阅，原以为自己的教程应该没啥大问题，直到——

意达：你后半篇教程中，关于部署的讲解有点问题哈。

笔者：啥问题呢？我读了一遍下来，也没什么错误啊？

意达：你不觉得，既然你都能在GitHub Desktop创建远程分支，并且把本地网页的代码推送到远程分支中，那么，为什么不把本地网页的代码，直接推送到主干仓库呢？你这样做不是多此一举嘛？

笔者：可是我担心，如果直接把源码推送到主干仓库，万一本地源码有点闪失，岂不是容易把GitHub个人主页搞崩？传到分支仓库去，好歹合并时会检查主干与分支是否有冲突。

意达：但是你的网页只有你自己一个人编写，最后得到的主干版本，跟分支版本没有任何区别。而且，你提交源码的时候，肯定会在本地服务器上预览过至少一遍，才敢提交的吧？如果这时还有错误，那么提交到分支仓库，再合并到主干仓库中，也不会解决任何问题。

笔者：那万一我的博客需要其他人编辑，又怕其他人乱搞，该怎么办呢？开个分支出来，不是更安全嘛？

意达：我一般是直接给所有编辑者开放网站仓库的编辑权限的。

笔者：……行吧，你就当我是在练习分支仓库的使用吧。

意达：你开心就好。

### 参考资料

**Part IV：**

[hexo-theme-material | Easy Hexo](https://easyhexo.com/2-Theme-use-and-config/2-5-hexo-theme-material/)

[Markdown Guide](https://www.markdownguide.org/)

[Markdown 教程 | 菜鸟编程](https://www.runoob.com/markdown/md-tutorial.html)

[Markdown 公式指导手册](https://www.cnblogs.com/RioTian/p/14111021.html)

[hexo 下的分类和表签无法显示，怎么解决？](https://www.zhihu.com/question/29017171)

[hexo 怎么创建 404 页面？](https://www.zhihu.com/question/21650209)

[hexo 创建文章 & 文章缩略图及banner & MarkDown](https://zhangjichengcc.github.io/blog/2018/02/27/hexo-%E5%88%9B%E5%BB%BA%E6%96%87%E7%AB%A0/)

**Part V：**

[hexo&material配置修改记录](https://moonglade.top/2022/02/26/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA/hexo-material%E9%85%8D%E7%BD%AE%E4%BF%AE%E6%94%B9%E8%AE%B0%E5%BD%95/index.html)

[如何在 hexo 中支持 Mathjax？](https://www.jianshu.com/p/e8d433a2c5b7)

[请问，这个主题如何指定文章的封面图片呢和放大博客文章中的图片？ （Issue #743）](https://github.com/iblh/hexo-theme-material/issues/743)

[hexo-theme-material | Easy Hexo](https://easyhexo.com/2-Theme-use-and-config/2-5-hexo-theme-material/)

[Use the Material theme for Hexo](https://www.programmerall.com/article/95331088559/)

**Part VI：**

[超详细 Hexo + Github Pages 博客搭建教程](https://zhuanlan.zhihu.com/p/370635512)

[（一）Github + Hexo 搭建个人博客超详细教程](https://zhuanlan.zhihu.com/p/111614119)

[Getting Started with GitHub Desktop](https://docs.github.com/en/desktop/installing-and-configuring-github-desktop/overview/getting-started-with-github-desktop)
