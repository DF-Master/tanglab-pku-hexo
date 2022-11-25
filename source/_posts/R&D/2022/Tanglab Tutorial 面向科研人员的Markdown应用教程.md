---
title: Tutorial:面向科研人员的Markdown应用教程
date: 2022-11-04
categories:
- rd
tags:
- rd
- markdown
- tutorial
---




> **"Markdown" is two things:**
> 
> 
> **(1) a plain text formatting syntax;**
> 
> **(2) a software tool, that converts the plain text formatting to others.**
> 

## 为什么要学习Markdown？

虽然你不一定能够感觉到，但是markdown已经存在于你的身边：

1. 它可以记录笔记，比如本文就是用markdown写作的
2. 每天阅读的BLOG，文档，下载包中的README.md，乃至微信公众号——他们可能都是基于markdown写作的
3. MARKDOWN可以大大减少重复性、对个人提升毫无帮助的工作——让你离开一遍又一遍的统一格式、版本管理、手动存储和笔记备注，把更多的精力放在创造性的工作之上——**如果说互联网是当代生产力的基础，那markdown就是当代互联网的基础。**

<!--more-->

## Markdown很难吗？

谈到“代码”，很多人便陡然变色——人类对于未知的事物本就存在犹疑和恐惧，但如果换成类似的形式，便顿时容易接受了。例如，下面的代码是HTML形式的内容，类似的格式大家在chrome中按一下F12就可以看到：

```bash
<p>Text attributes <em>italic</em>, <strong>bold</strong>, <code>monospace</code>.</p>

```

下面是这段文字在浏览器中呈现的效果：

---

Text attributes *italic*, **bold**, `monospace`

---

但是，你可以用另一种更可读、简单的方法来写它，这就是markdown:

```bash

Text attributes _italic_, **bold**, `monospace`.
```
> 
> 以markdown作为学习互联网生产力工具的第一步，没有任何门槛
> 

所以你看到了，markdown并不难——甚至有现成的工具，比如notion，typora, 让你连“‘**’代表加粗”这种事都不需记忆，而可以用快捷键和UI来完成markdown的写作。

## markdown能做什么？

现在，就可以说明markdown的价值了:

- 作为标记语言，输入纯文本,在输出时具有格式：并非word的所见即所得，它需要后期渲染，但工具人人都可以有（比如你的浏览器）
- 人类可读，具有html代码通用性
- 高兼容性：不限制软件、平台，linux-vim和windows-文本编辑器都可以读写（如果你愿意的话）
- 高拓展性：它只是一段含有标注的代码，你可以把它转成word，ppt,网页；你也可以用Git等工具轻松管理它的版本

### 一些可能在科研中会出现的场景

> 2022年了还在用传统office?Markdown更适合Scripts的制作
> 
- [做一个支持即时修改、版本管理、基于网页的PPT](https://demo.sli.dev/composable-vue/1) ，现在你可以离开U盘，随时随地展示数据和idea了
- [用web代替word分享你的protocol](http://tanglab.pku.edu.cn/2022/11/02/R&D/2022/%E5%9F%BA%E4%BA%8ENMRPipe%E7%9A%84RDC%E6%95%B0%E6%8D%AE%E5%A4%84%E7%90%86%E6%95%99%E7%A8%8B%202a112f74ab744859baeea5a26ca18cb8/)
- [用markdown结合excel处理数据](https://tableconvert.com/zh-cn/markdown-to-excel),我的实验数据习惯用Python处理完后以markdown形式输出，在大量数据预览上Markdown大有优势
- [美化Onenote,Onenote高速排版](https://onemark.neux.studio/)
- [在浏览器中安装插件，使用markdown为邮件排版](https://markdown-here.com/)
- [美化你的网页工具](https://github.com/whitphx/streamlit-webrtc),例如streamlit就支持markdown的文本格式

相信，上述功能中，应该有一二戳中痛点。再不济，可以以markdown为抓手为tanglab.pku.edu.cn供稿，必可大大减少运维排版时间。

### 对一些观点的批判

- 科研任务如此繁重，我现在这样就已经很累了，不想学新东西。
    - 逆水行舟，不进则退。
- 我很少接触代码
    - markdown工具对生产力的提高相当本质，而且本身就是面向普通人的。
- 我缺少学习相关技术的渠道
    - 学习技术，以前是读说明书咨询工程师，当代事读文档写issue
    - **markdown有[配套中文文档](https://www.markdown.xyz/),推荐阅读学习！**

## 立刻开始！

> 工欲善其事，必先利其器。
> 

一个好的工具对于新手来说能够事半功倍。在此，我给出一些个人意见

- Notion和Onenote仍然是目前笔记界的大象。OneMark可以让Onenote支持Markdown，但notion给出了更加强大的、易用的体系——用“/”键几乎可以完成一切的工作。学习Notion的使用，可以参考这篇教程[Notion使用笔记](https://www.notion.so/Notion-c03630aaa2a64421ac4268420e2b0ba6)

- 追求免费、好用的话，Vscode+Markdown插件具有最强的泛用性，绑定Github账户还能实现配置同步，是最好的免费选择。
- 付费软件中，typora是有过较多好评的，价格也不贵，可以考虑。Obsedian和notion整体差不多。
- 当然，你也可以使用网页版本，比如 [https://dillinger.io/](https://dillinger.io/)

至于具体的语法，前人之述备矣，参考[配套中文文档](https://www.markdown.xyz/)即可。

## 应用

学会markdown后，你就可以尝试在[https://blot.im/](https://blot.im/) 或者 [github.com](http://github.com) 上建立你自己的网站主页了。来看看最基础的[Github HomePage](https://github.com/DF-Master)吧！只需要建立一个和Github Name一样的仓库，放上自我介绍文件,就可以实现这种效果。

当然，只是一个Homepage未必能满足你学会新技术的自豪感。如果blot.im满足不了你的话，你可以试试[在Github上建立一个基于Hexo的个人网站](https://www.notion.so/9153624b700d486190e49c32cc2b8bf4)，虽然我没尝试过，但基于hugo的网站应该也不错。

既然提到了github，那你还可以下载一个github-desktop,用git来管理你的github，实现多版本管理。

很好！如果你完成了上述步骤，那你应该已经初步了解了Github，现在你已经踏入互联网生产力技术的大门了，相信接下来只要你自主学习，就能完成本文提到的全部应用了。

## 写在最后

最近在化院和老师同学交流的时候，会发现很多我觉得是“雕虫小技”“常识”的东西，大家却觉得“身边并不见有什么人会”。学校的象牙塔可能让大家忽略了世界的发展和变化，世界变得如此之快，我们也要跟上才行。

希望能给大家一个接触的契机，我已经倾囊相授了（悲），都不是很难的东西。