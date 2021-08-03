---
layout: post
title: 记一次git撤销删除操作
categories: [git, GitHub]
description: 记一次git撤销删除操作
keywords: git, GitHub
---

今天想把自己前几学期写的代码整理到GitHub上，没想到还没上传，代码自己先被删没了，吓得心脏骤停，好在后面抢救回来了。
<!-- ======= -->

一开始建仓库，然后 `git remote add origin` 连接到远程仓库，`push` 到仓库里，结果发生了莫名错误，尝试了几次都是这样，于是想 `git clone` 后手动复制到文件夹中再上传。

项目文件夹都拖进去了，这时控制台给报了个错： `Failed to connect to github.com port 443: Timed out` ，然后自动创建的文件夹连带着我拖进去的所有文件就被删除了，被删除了，删除了，除了。。。

我去看Windows的粘贴板，发现自己的没开，按 `win + v` 也没用，而且想起来我是直接拖进去的，也不是复制的啊，心里一阵懵逼。

打开回收站，当然也没有，这类系统删除后应该不会进回收站。

然而就在这时，我看到了回收站里还有之前删除的 `.git` 文件夹，那是远程连接上传失败后我直接删除的，我知道 `.git` 文件夹内必然还有项目文件信息，于是将其还原，打开文件夹没有找到项目文件，用 `git` 打开后用命令 `git status` 查看一下，输出的内容是文件删除信息，这告诉我有戏！

删除后如何撤销删除？我去网上找了找，发现用 `git checkout` 命令就能解决，于是输入 `git checkout .` 将所有删除的文件还原。

重新上传文件成功，这次终于是有惊无险。

主要原因是连接 `git` 时总是会出现如 `timeout` 等一系列问题，远不如以前好用了，可能是国内的网络原因嘛，大家都懂。另外我发现，`VSCode` 内置的 `git` 命令行成功率也不如 `git bash` ，无论是 `git clone` 还是 `git push` ，所以以后还是多用 `git bash` 吧。

---

我错了，无论是 `VSCode` 还是 `git bash` ，它们的情况都差的离谱，`push` 一次都要尝试好多次

以下是各类报错：
- Empty reply from server
- Failed to connect to github.com port 443: Timed out
- OpenSSL SSL_read: Connection was reset, errno 10054