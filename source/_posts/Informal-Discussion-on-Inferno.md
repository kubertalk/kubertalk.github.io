---
title: Informal Discussion on Inferno
date: 2017-03-18 12:43:59
categories: 
   - 研究僧日常
   - 操作系统
tags: 
   - 操作系统
thumbnailImage: alphabets-cubes-love-1148572.jpg
thumbnailImagePosition: left
autoThumbnailImage: yes
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
comments: yes
---
研究生操作系统课程的作业--每人调研分享一个操作系统
<!--more-->
<!--toc-->
### **漫谈Inferno**

#### **Inferno History**

- [IEEE Internet Computing](https://zh.wikipedia.org/w/index.php?title=IEEE_Internet_Computing&action=edit&redlink=1)在1997年3-4月的刊物中有Inferno网络软件的广告。该广告宣称可利用多种设备在包含互联网、电信网络，以及局域网等之"任何网络"上进行通讯。图中甚至摆放了[PlayStation](https://zh.wikipedia.org/wiki/PlayStation)之类的照片，如果电玩可以跟电脑沟通、手机可以访问电子邮件、电视可以访问语音邮件。
- [朗讯科技](https://zh.wikipedia.org/wiki/朗訊科技)至少有两个内部项目有采用Inferno：Lucent VPN Firewall Brick以及Lucent Pathstar phone switch。这也打开了贩卖Inferno源代码授权的尝试，不过反应并不热烈。朗讯科技没特别做营销又忽略了Inferno与互联网的重要关连，而[Java语言](https://zh.wikipedia.org/wiki/Java)跟Inferno目标市场相似、采用类似的技术、可以在[网页浏览器](https://zh.wikipedia.org/wiki/網頁瀏覽器)中运行，也同时满足了当时对于[面向对象语言](https://zh.wikipedia.org/wiki/OOP)的流行。当[SUN](https://zh.wikipedia.org/wiki/昇陽電腦)大力营销自家的Java时，朗讯科技自太阳微系统获取Java的授权，宣称所有Inferno的设备皆能运行Java。Java比特码与Dis比特码的转译器就是为了达成这个功能所产生的。然而Inferno还是卖不出去。
- Inferno商业部门过了三年就关闭且被卖给[Vitanuova](https://zh.wikipedia.org/w/index.php?title=Vita_Nuova_Holdings&action=edit&redlink=1)在1999年。Vitanuova持有相关的权利后，便继续研发并对整个系统采用商业化授权的方式，随后提供免费下载以及对核心以及虚拟机以外的整个系统采用非[GPL](https://zh.wikipedia.org/wiki/GPL)兼容授权的方式。Vitanuova将软件继续移植到新的硬件以及专注在分散式应用软件上，最终将源代码采用[GPLv2](https://zh.wikipedia.org/wiki/GPL)授权发布，而Inferno操作系统现今也变成是一个[开放源代码](https://zh.wikipedia.org/wiki/開放原始碼)的项目。

#### **Licence Terms**

这里稍微提一句这个授权的问题，Inferno第四版于2004年以[自由软件](https://zh.wikipedia.org/wiki/自由軟體)的授权发布。具体来说，Inferno采用了[双授权](https://zh.wikipedia.org/wiki/多許可)的方式采用了两种授权供用户选择。用户可选择在[自由软件授权](https://zh.wikipedia.org/w/index.php?title=自由軟體授權&action=edit&redlink=1)或传统商业化授权的情况下获取Inferno。根据自由软件授权的规范，系统中各个部分可以采用不同的授权方式，这些授权方式包括[GPL](https://zh.wikipedia.org/wiki/GPL)、[LGPL](https://zh.wikipedia.org/wiki/LGPL)、[MIT License](https://zh.wikipedia.org/wiki/MIT_License)，以及[Lucent Public License](https://zh.wikipedia.org/w/index.php?title=Lucent_Public_License&action=edit&redlink=1)。这种就是根据不同的协议，在你做出修改后想要再传播时，需不需要公开修改的那部分代码。如果你不想公开任何一部分干脆使用商业版本，一开始付一点费用就可以。后来Vita Nuova让Inferno可以在[GPLv2](https://zh.wikipedia.org/wiki/GPL)的授权下获取除了字体（采用[Bigelow and Holmes](https://zh.wikipedia.org/w/index.php?title=Bigelow_and_Holmes&action=edit&redlink=1)授权）以外的整个系统。现在总共有三种授权方式可供选择。

这里面有一个[Lucent Public License](https://zh.wikipedia.org/w/index.php?title=Lucent_Public_License&action=edit&redlink=1)，顾名思义就是朗讯自己搞的一个协议，有1和1.02两版。Plan 9第四版用的就是LPL1.02版，这个协议虽然被FSF和OSI都接受，但和GPL并不相容，因为其中竟然说被老美的某州法和知识产权法所统治。

#### Inferno's Features

##### **Cross-Platform Portability**

Inferno可以直接在原生硬件中运行，也能在其他平台以应用程序的方式提供虚拟操作系统。应用程序无须经过修改或重编译即可在所有的Inferno平台开发并运行, 大多数流行的操作系统和处理器架构都支持：详细请看课堂展示[PPT]()图。

##### **Limbo**

- Easy to Learn -- 它的语法和C很相似，设计的很容易读和理解。

- Absolutely Portable -- 绝对便携的。源代码被编译成结构独立的小部分，能够很好的完全的跑在所有Inferno平台上。

- Advanced Concurrency -- 高并行性

- Dynamic Modules -- 动态的模块。除非在运行和被需要时候，载入的模块绝不会复制到内存里

- Safe -- 编译一关，运行时字符类型检查一关，运行时数组溢出检查又一关

- Automatic Garbage Collection -- 自动释放不用的内存

##### Principles

**Inferno is a distributed system based on the application of three basic principles.**

- Resources as files：把所有的资源都列在层次结构式文件系统中以文件表示。
- Namespaces：从应用程序的观点来看，网络是种单一且清楚的命名空间，能展现层次结构式文件系统，也能代表近端或远程实体分离的资源。
- Standard communication protocol：采用名为Styx的标准协议，用来访问近端或远程的所有资源。

分开介绍三条原则：

1. **Resources as files**
所有资源，无论在本地还是远程的，都被看做用层次化文件系统来表示的一个动态文件集。无论是存储设备，进程，服务，网络和网络连接，全都用文件来表示。一个应用程序可以访问每个资源，通过标准文件操作来进行。

用文件作为中心概念的好处有：

- 访问文件的接口基本由定义好的操作集组成，简单且易懂。例如open/read/write
- 文件系统减少了接口的代码量，使系统小巧灵活移植性好。
- 命名习惯规整且易懂。
- 访问权限和访问许可变得简单，还可以确保多级安全。

每个请求和每个用户发出的访问请求时间点不同，返回的文件名可能都不同。

2. **Namespaces：**
这第二个关键的原则就是可计算的命名空间，是一个应用程序建立独特的私有的视图来调用资源和服务时需要访问的东西。每一个资源集被描绘成文件层级，且用标准文件操作可以访问。各种不同的资源和服务被一个进程使用，都是要连接到某一个单个的root层级的文件名，这叫做命名空间。在一个个人的命名空间里，这些可访问的资源可以被定位到一个单个的客户或者多台服务器上通过网络。

命名空间一个最大的优点就是应用程序可以完全的显然的使用资源。一旦某个资源被挂载到你的应用程序的命名空间下，你就可以使用它但不需要知道这个资源是在本地还是远程的。

举个例子，A机器想要使用B机器的CPU，只需要将B机器的CPU挂载到A机器的CPU的文件中，就能完成这个需求，但在上层来看完全是透明的，像在本地操作一样。当然两个机器之间来进行文件挂载表示的是有一个协议叫9P。最早只是在Plan 9中用到，后来派生的一个简单版叫styx。但现在的styx和9P2000基本是一模一样的。只是有的时候还改不过口叫styx。

Namespace Example
```
This example describes a home network that consists of the following devices:

t Windows PC t Digital Camera t Video Recorder t PDA (Compaq iPAQ, for example)

The physical connection between the devices may be as follows:

t Wireless 802.11b network connecting （无线连接）

the iPAQ and the Windows PC

t Ethernet network connecting the PC to the Video Recorder（以太网连接）

t USB connection from the PC to the Digital Camera（usb连接）

Step One - mount the different name spaces（挂载不同的命名空间到/n/remote下）

Step Two - bind the namespaces together（bind所有命名空间到/homenetwork下）

Step Three - run the application from the PC（在PC端跑应用程序）

Step four - run the application from the IPAQ（在ipad端跑应用程序）

Step 5 - run on the iPAQ store on PC

```
![img](http://note.youdao.com/yws/res/4297/69A6530F06AB4B29939050ABA9F342FC)

3. **Standard protocol to access resources：**
Inferno使用单个的协议，由内核和应用程序来实现，为了表示或访问资源的。因为所有的资源包括网络网络连接都被看做文件，自然需要一个协议来通信和对于资源提供访问。这个协议就是就是styx或者9P了。这个方法通过使用已有的技术来连接远程的文件系统即建立起一个分布式系统。使用一个标准的通信协议还可以使得更集中注意在安全上。Inferno提供几个机制在安全通信上：

- 基于用户身份验证的授权
- 消息加密

因为这是底层的一部分，对于所有的应用程序是透明的。

那么styx到底是什么呢？根据官方文档的其中一句话：

The Styx protocol is the specification of the messages that are exchanged.（styx是用来交换的信息的一个规范）大概是在OSI七层的第5层会话层。

At one level, Styx consists of messages of 13 types for（styx包括13个类型的消息，基本分为）

- Starting communication (attaching to a file system);（启动，连接上文件系统）
- Navigating the file system (that is, specifying and gaining a handle for a named file);（导航指引你找到你需要的那个文件）
- Reading and writing a file; and（读，写）
- Performing file status inquiries and changes（显示文件的状态）

Table Summary of Styx messages
![img](http://note.youdao.com/yws/res/4297/8E4A3F614B3A4627A77C64E6340040A0)
For example:
```
open("/usr/rob/.profile", O_READ);
——具体执行起来就是初始化
——发送attach消息——
返回FID即所寻文件系统的root目录
——然后执行open消息
——先复制一个FID
——用新的FID通过walk消息导航找到所需文件
——然后检查用户有没有读写权限
——然后进行读和写
——然后close释放FID
__结束tcp/ip协议。
```

#### 题外话
**Styx, as a file system protocol, is merely a component in a more encompassing approach to system design: the presentation of resources as files.(styx作为一个文件系统协议，只是系统设计大的这盘棋中一部分，但是很重要的一部分，即一起皆是文件。尽管unix最先提出的这个思想，linux也有继承，但styx确实更为彻底的，极具革新性的)**

举几个例子来看一下：

- **Example 1:debugging**
我们都知道/proc是unix里动态的控制获取内核信息包括进程等一些来用的。看这个例子，假设1、2就是系统正在运行的一些进程ID。

```
/proc/

    1/
        status
        ctl
        fd
        text
        mem
        ...
    2/
        status
        ctl
        ...
...
```
这里的status显示的是进程状态的信息。Ctl显示的是一些控制的信息比如pausing暂停，restarting重启，killing关闭等；fd是一个整数，在文件open时产生。表示该文件在进程中打开；text代表程序源码；mem代表数据。

这里就是说前面提到过的类似的，当你想用远程的一台机器来调试你机器上的程序，只需要将本地的/proc文件方便的挂载到远程的机器上来调试。机器与机器是相互独立的，即使两台机器CPU架构都不相同。所以大多数与状态和控制相关的与机器无关的程序，是极其便携的。

- **Example 2：Networking**
```
/net/
    dns/
    tcp/
        clone
        stats
        0/
            ctl
            status
            data
            listen
        1/
            ...     
        ...
    ether0/
        0/
            ctl
            status
            ...
        1/
            ...
    ...
```

假设一个应用程序想建立一个TCP/IP的连接到百度，第一步肯定是先由DNS服务器解析域名，返回一个IP地址，然后访问这个IP地址的网关，向这个服务器发送请求访问页面。

而styx则是，直接往/dev/dns/里写百度的域名，然后再读取同一个文件，就会返回一串IP地址了。IP地址收到后，进入/net/tcp/clone可以看到给出的一个同级的目录比如/net/tcp/0, 0是什么呢，正式一个新的独特的TCP/IP通道。而、net/tcp/0/ctl里则会存放控制信息，比如connect 202.108.22.5, 然后读写/net/tcp/0/data连接即为建立。

这里面很多是在服务器上完成的，都不一定是在本地的，但在用户看来，跟本地是一样的。这也是前面说过的命名空间的特点。

这里将接口间的通信从二进制信息变成了可读的字符串信息，所以再次验证了它是完全不依赖于CPU架构或者具体语言的。

**某种程度上讲，Inferno/Plan9这个分布式操作系统，它能把网络上一切的资源当作文件来进行使用，这其实就是云的概念了。**

#### **总结**
那么到底为什么inferno或者说Plan 9没有火，而linux火了呢。总结一下就是，你想在大哥的基础上做改进特别好，但是你要是自立门户，几乎不跟大哥来往，那你一定活得不好。说到这里不是说Plan 9 或者inferno完全死了，他现在还是在vitanuova公司下运营，提供免费下载也卖商业版本。也不得不承认，他还是带来很多影响的。早期的freeBSD派和现在linux都从中有借鉴的地方。

那么Plan 9/inferno到底留下了什么呢？

我总结3点就是
- Change daily
- Remain original
- Adopted by others

作为ken thomsen 的生平第三大作品，虽然没有成功，但从此留下功与过，回响在历史长河中。

 
PS: 在研究生第一学期操作系统课唯一一点留下来值得我思考的是：
网络的目的：构造分布式的计算环境。则需要同步和互斥。
软件的同步和互斥：信道语义（接收和发送）
更重要的硬件的同步和互斥：原子操作（测试指令xx）

 