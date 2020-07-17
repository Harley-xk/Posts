---
title: Vapor - 开始一个 Vapor 项目
abstract: Vapor 是一个使用 Swift 语言开发的服务端框架。本文是 Vapor 系列的第一篇，简单介绍了如何在 macOS 中创建一个 Vapor 项目。
date: 2020-07-17 18:12
tags:
 - Swift
 - Vapor
---

## 序 —— Vapor 现状

[Vapor](https://vapor.codes/) 是一个使用 [Swift](https://swift.org/) 语言开发的服务端框架，从 2016 年发布至今，在 [Github](https://github.com/vapor/vapor) 上已经获得了将近 20K 的 star。作为 Swift 跨平台愿景的重要一环，Vapor 同时也为广大 iOS 开发者提供了新的职业路线。

### 文档

Vapor 开发团队精力有限，文档更新相对有些滞后，特别是大版本更新时，尝鲜的开发者经常需要自己阅读源代码来摸索一些 api 的使用方式。这也是很多开发者一直在观望而没有入坑甚至弃坑的原因之一。并且一开始并没有中文文档，这也阻挡了一部分对英文存在恐惧心理，但是又想尝试 Vapor 的开发者。

不过目前这个状况正在慢慢变好，文档正在逐步完善；另外，以[晋先森](https://github.com/Jinxiansen)为代表的一众 Swift 爱好者正在进行文档的[中文翻译](https://cn.docs.vapor.codes/4.0/)工作，相信这项工作能让国内更多的 iOS 开发者加入到 Vapor 的社区中来。

### 生态

我所在的一个 Vapor 交流群中经常会讨论， Vapor 相比于目前流行的解决方案，比如 Node、Go、Java 等，并没有什么非常大的优势，所以为什么要选 Vapor 呢？

确实，后端开发并不仅仅是简单的接口调用和增删改查，目前就软件生态来讲，Vapor 与它的同行还有很大的距离；不过好在依托于 Swift，有大量原来 iOS 生态资源的补充，相信这种差距不会一直存在下去；另外根据我近两年自己踩坑的感受，随着越来越多的人进入到这个社区，整个 Swift 服务端的生态是越来越完善的。

### 语言

Swift 从 2014年发布，已经到了第六年，版本号也已经更新到了 5.3，虽然国内目前依然是 OC 为主，但是在国外早已是 iOS 开发的主流。作为一个 10 年经验的 iOS 开发者，我自己也从 2016 年开始，就在所有业余和公司项目中使用 Swift 作为主力开发语言。在这期间也学习和接触了一些其他的技术栈，比如 Laravel(PHP)，Vue(JS)、.net(C#)。

就从语言层面来说，Swift 是我所有接触过的语言中编码体验最好的(没有之一)。

### 开发者

目前尝鲜 Vapor 的一众开发者大多都是 iOS 的从业人员，以兴趣爱好和业余摸索为主。我个人的观点是：不管 Vapor 的前景如何，对于一个客户端的开发者来说，学习和了解后端开发的工作模式是有益无害的。就算最后没有转到后端开发，也能在开发客户端的时候加深对网络通信和后台交互的理解。而对于本身就熟悉 Swift 的 iOS 开发者来说，Vapor 无疑是一个很好的切入点。

所以，如果你想尝试 Vapor，不要问有没有前景，熟悉了后端开发的基本概念和运作方式，必要的时候如果转换到其他平台，学期成本也会大大降低。

## 环境准备

### Xcode

目前为止，编写和调试 Swift 代码最好的环境依然是 Xcode，所以推荐依然使用 Xcode 来开发 Vapor 项目，对于 iOS 开发者来说也没有学习成本。

Vapor 4 最低支持 Swift 5.2，因此需要安装 Xcode 11.4 及以上版本。

![Xcode](/post-images/vapor-start/xcode.png)

### Vapor 工具箱

Vapor Toolbox 是官方提供用来管理和编译 Vapor 项目的一个命令行工具。这个工具不是必须的，它最终还是调用的 swift 的命令行工具，如果你很了解 swift 和 spm，理论上可以直接使用。

安装 Toolbox 很简单，使用 homebrew:

```bash
brew install vapor
```

安装完成后，在命令行输入`vapor --help`可以看到如下图所示，Toolbox 可以帮助创建项目、更新依赖、编译、部署等。

![Vapor Help](/post-images/vapor-start/vapor-help.png)

## 使用 Vapor toolbox 创建项目

Toolbox 使用起来还是很方便的，创建项目依然只需要一行命令：

```bash
vapor new Hello
```

Toolbox 会自动去 github 下载最新的项目模板，鉴于 github 在国内的访问情况，有条件的建议架梯子后在执行上面的操作。

创建之前 Toolbox 会询问你是否需要使用 [Fluent](https://github.com/vapor/fluent)，Fluent 是 Vapor 的 ORM 框架，如果你需要跟数据库打交道的话是绕不开的，这里建议选择 yes。

接着 Toolbox 会询问你需要使用哪个数据库，Vapor 目前支持 Postgres、MySQL、MongoDB、SQLite 等，官方推荐使用 Postgres。当然，如果你的项目没有很高的数据并发读写的要求，SQLite 也能满足需求。

> 如果你不需要数据库，也可以直接在新建项目时加上 `-n` 命令，即使用 `vapor new Hello -n`，这个命令会自动对所有后续的问题回答 no。

![Vapor New](/post-images/vapor-start/vapor-new.png)

> 如果你是初学者，一开始建议先选择 SQLite，这样可以省略掉复杂的数据库配置过程，先把项目跑起来。

## 使用 Swift Package Manager

Vapor 工程是一个标准的 spm 项目，所以理论上不需要 Vapor Toolbox 也可以正常运转。随着 spm 的不断完善，现在只需要直接打开一个 Package.swift 文件，Xcode 就可以自动根据 spm 的配置自动拉取依赖和管理项目了。

进入上面 Vapor 生成的项目，直接使用 Xcode 打开 Package.swift 文件，如下图，可以看到 Xcode 已经自动在拉取依赖了：

![Xcode Fetching](/post-images/vapor-start/xcode-fetching.png)

完成之后 Xcode 会自动配置好 schme 和 target，接下来就可以跟调试 iOS 程序一样，点击 “Run” 按钮开始调试。

服务器运行起来后，可以在 Xcode 控制台看到输出的状态：

```bash
[ INFO ] Server starting on http://127.0.0.1:8080
```

到此，一个 Vapor 服务端项目就运行起来了。

## 后记

Vapor 系列文章其实已经酝酿了很久了，一直没有动笔写，后来 Vapor 又更新了 4.x 大版本，改动还是挺大的，所以又花了一些时间踩坑。因为我自己也只是在业余项目中使用 Vapor，理解深度有限，所以并不打算把这个系列写成 Vapor 教程。目前的想法是根据我自己的实践和体验，针对一些技术细节分享一下我自己的实践过程。如果你想深入学习 Vapor，还是需要去看 Vapor 的官方文档或者相关书籍。

### 书籍

Vapor 的学习资料目前不多，除了官方文档之外，还有一本 Vapor 作者参与编写的 [《Server Side Swift with Vapor》](https://store.raywenderlich.com/products/server-side-swift-with-vapor)。

这本书是针对 Vapor 3 写的，虽然 Vapor 4 在 API 层面有一些改动，但是整体的架构并没有大的变化；并且这本书对整个 Vapor 的使用写的非常详细，目前依然值得一读。
