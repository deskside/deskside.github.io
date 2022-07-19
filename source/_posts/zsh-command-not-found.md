---
title: 已解决 zsh command not found (macOS)
date: 2022-07-08
tags: [编程,挑战,困难]
cover: /img/developer.png
feature: false
categories:
  - 开发笔记
---

因为需要制作个人网站和文档，我电脑中需要安装 node.js（包含 npm）和 git。

顺利安装后，运行如下指令，使用 npm 安装 hexo 和 docsify 都没有问题，十分顺利。

```
npm install hexo-cli -g
npm i docsify-cli -g
```

## 遇到问题

但是，在安装完成后，准备开始使用 hexo 和 docsify 的时候，所有相关指令都无法使用，终端会给出如下错误。（请注意我使用的是 macOS 系统）

```
# 假如我运行 docsify 相关指令
docsify init
zsh command not found:docsify

# 假如我运行 hexo 相关指令
hexo s
zsh command not found:hexo
```

我在网上查了好久，但那是相关的解决措施都很「高级」……作为一个编程小白，我真的很茫然，完全看不懂。

所幸在一番折腾后，我终于解决了这个问题。接下来我将解决方案和过程分享给你，希望能发挥效果。

## 理解Terminal、Shell

首先，我们需要理解zsh是什么，以及有什么用。（没错，我连这个都不知道是什么！）

zsh全名为Z-shell，本质即是Shell。听起来很复杂，别着急，请继续看。

我们日常在使用电脑的时候，比如聊微信、打游戏、看网页，实际上是通过开发者设计好的用户界面（User Interface, UI）在与电脑交互，是一种间接沟通方式，但胜在图文并举、直观易懂。

但如果我们想要更直接地与电脑交互，我们便需要另一个工具，这便是终端（Terminal）。终端可以直接与计算机的内核（Kernel）沟通，是一种直接沟通方式，但往往需要具备一定的计算机知识，否则看到纯文字的界面会很茫然。

终端想要与计算机沟通时，不能直接与内核沟通，否则一旦用户错误操作，整个计算机就会崩溃。因此，我们还需要一个中介，用来接受用户输入的命令，并由其与内核沟通，这便是Shell（壳层）。

Shell有很多种，比如bash、csh、tcsh等，你会发现它们都以shell的缩写「sh」结尾。

因此，前述终端中报错，实际上是zsh在告诉我们：「虽然你用npm安装了相关程序，但是我找不到呀，你得告诉我文件路径在哪里哦！」

## 解决问题

既然理解了问题，那我们就可以着手开始解决问题。

首先，我们需要找到zsh的配置文件，在终端中输入以下指令应该能打开：

```
open ~/.zshrc
```

如果终端反馈找不到该文件，或者有错，请你使用Finder打开用户文件夹（我的文件夹为joeysimac），并按下`command+shift+.`，显示隐藏文件夹。如果你不知道你的用户文件夹路径是多少，请重启终端并输入以下指令，终端会告诉你当前路径。

```
pwd
# 我的程序返回了如下指令
/Users/joeysimac
```

打开文件后，应该会有一行大致如下的文字：

```
export PATH=$HOME/bin:/usr/local/bin:$PATH
```

其中以`:`断开的便是不同的`/bin路径`，请记住这几个不同的路径，接下来可能会用到。

因为 macOS 从 Catalina 版开始，其默认 shell 从 bash 改为 zsh。因此，你需要找到`# User configuration`，在其下方加入一行代码，告知zsh「之前bash的相关配置你可以用哦」。最终效果如下：

```
# User configuration
source ~/.bash_profile
```



其次，我们需要知道刚刚通过npm全局安装好的程序（以hexo为例），它现在具体在什么地方。

因为我是使用npm安装的，所以通常而言，安装好的程序应该会在`/Users/joeysimac/.npm-global/bin`这个文件夹中。

因此，我们需要将hexo现在的位置`/Users/joeysimac/.npm-global/bin/hexo`告诉zsh。请将下列路径替换为你自己的路径，然后在终端中输入。

```
sudo ln -s 你的程序路径 /bin路径
# 示例：sudo ln -s /Users/joeysimac/.npm-global/bin/hexo /usr/local/bin
```

如果发生错误，请尝试将`/bin路径`更换为前述的其他路径。

最后，为了使配置文件立刻生效，我们需要在终端中输入：

```
source ~/.zshrc
```

到此为止，你可以试着重启终端，试着运行相关程序。依旧以hexo为例，逐行敲入下列代码，看看终端是否能正常运行。

```
hexo init blog
cd blog
hexo s
```

## 复盘

这是我第一次撰写技术向文章，因为我本身并非计算机专业，也未系统学习过编程，单纯只是感兴趣，算是纯小白了。无论学习哪种语言，学习编程的道路都不会一帆风顺。

遇到问题卡壳了，那就尝试转换心情去完成别的任务吧，很多难题往往在散步的过程中灵感迸发就解决了，或者睡一觉之后脑子就想通了。

从积极的角度而言，遇到问题说明我们有了收获新知识的可能性，这难道不可喜可贺吗！



---

参考文献：
+ [zsh: command not found from CSDN](https://blog.csdn.net/u010954988/article/details/80404329#:~:text=%22%20zsh%3A%20command%20not%20found%3A%20%22%E8%BF%99%E4%B8%AA%E9%94%99%E8%AF%AF%E7%9B%B8%E4%BF%A1%E5%A4%A7%E5%AE%B6%E9%83%BD%E4%B8%8D%E9%99%8C%E7%94%9F%EF%BC%8C%E4%BB%A5%E5%89%8D%E6%AF%8F%E6%AC%A1%E9%81%87%E5%88%B0%E8%BF%99%E4%B8%AA%E9%97%AE%E9%A2%98%E9%83%BD%E6%98%AFGoogle%E4%B8%80%E4%B8%8B%EF%BC%8C%E7%84%B6%E5%90%8E%E5%91%8A%E8%AF%89%E4%BD%A0%E5%9C%A8,xxx%20%E6%96%87%E4%BB%B6%E6%B7%BB%E5%8A%A0%20xxx%20%E6%96%87%E5%AD%97%EF%BC%8C%E6%88%96%E8%80%85%E5%9C%A8Terminal%E8%BF%90%E8%A1%8C%20xxx%20%E5%91%BD%E4%BB%A4%E5%8D%B3%E5%8F%AF%EF%BC%8C%E6%9C%89%E4%BA%9Bwork%EF%BC%8C%E6%9C%89%E4%BA%9B%E4%B8%8D%E8%A1%8C%E3%80%82)
+ [Mac ln命令报错：Operation not permitted](https://blog.csdn.net/qq_33833327/article/details/78086973)
+ [shell有哪些？Zsh和Bash的区别是什么？](https://www.jianshu.com/p/a891af6f87e0)
+ [玩转 Terminal 终端：入门指南及进阶技巧](https://sspai.com/post/45534)
+ [命令行界面 (CLI)、终端 (Terminal)、Shell、TTY的区别 - stardsd - 博客园 (cnblogs.com)](https://www.cnblogs.com/sddai/p/9769086.html)
