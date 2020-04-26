---
title: 201705 交大黑客松（Hacker Marathon)纪要
date: 2017-05-20 09:40:55
categories: 
	- 研究僧日常
tags: 
	- Jetson TX1
	- CUDA
	- 智能小车
thumbnailImage: mushroom.jpg
thumbnailImagePosition: left
autoThumbnailImage: true
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
---

201705 交大黑客松（Hacker Marathon)纪要
<!--more-->
<!--toc-->

### 201705 交大黑客松（Hacker Marathon)纪要

#### 例子1--NVIDIA小车

**详情描述**

本项目拼装了一台能够智能识别图像的小车，该小车利用摄像头捕捉物体并分类，如猫、瓶子、人等等，进而根据分类结果执行相关动作。

1.控制：在ROS环境下，利用twist结构控制小车执行相关动作，并发布topic，然后在ARDUINO中订阅该主题。

2.视觉识别：

**解决的问题**

1.小组成员在36个小时内，完成小车驱动软件的编制，并实现远程控制。

2.小车能够根据不同的图片做出不同动作，遇到障碍时会停止动作，并且操控者可以在任何时候终止小车运动。

3.具体动作有：前进、后退、原地旋转等组合动作。

**项目使用指南**

用于控制行进动作的图片可以打印在A4纸上，由操控者手举着图片让小车识别后分别执行不同的行进动作。

**未来发展规划**

1.机场加油车或拖车：

利用视觉识别功能捕捉飞机，捕捉到之后靠近目标并执行相关任务。

2.道路清扫车：

若识别到瓶子等垃圾，自动清扫；若识别到汽车或行人时，停止前进。

#### 例子2--图片识别

**SDK使用情况说明**

我们利用了IBM Waston服务来做图片识别，之后将其转换为对应的表情包（目前是Emoji）。

服务部署在青云上，依赖于其提供的QingStor来存储上传的图片，并使用负载均衡/VPC等服务来提供高可靠的服务。

![青云API](/Users/kuber/Documents/ 青灯黄卷2/面试书籍/青云API.tiff)

#### 例子三--NVIDIA小车

在本次比赛中，我们将深度学习中的目标识别问题与智能小车的运动控制问题相结合，顺利完成了整个比赛项目。整个项目可以分解为如下几个部分：首先，利用图像处理工具Opencv对小车摄像头接受到的图像信息进行处理，将接收图像的RGB通道信息进行处理。之后，利用深度学习领域的SSD算法进行目标识别方面的处理，利用分类识别效果很好的深度网络模型对接受到的图片信息进行识别，进而输出图像的类别信息。最后，针对接收图像的不同类别，控制智能小车做出前行、后退、原地360度左转/右转等不同动作、除此之外，还加入了利用超声波进行障碍物躲避、小车的即时停止等功能。充分利用了相关企业提供的硬件、软件资源。

![小车套件](/Users/kuber/Documents/ 青灯黄卷2/面试书籍/小车套件.tiff)

#### 例子四--NVIDIA小车

**项目使用指南**

使用远程桌面连接小车的TK板，输入启动指令后，小车在接收到可识别的图片信号才会启动，并在遇到障碍物（距离障碍物一定距离）时停止移动。

小车的移动规律为：

识别出猫，向左转。

识别出人，向右转。

识别出车，向前进。

识别出椅子，向后退。

识别出飞机，向左转并直行。

识别出房屋，向右转并直行。

**核心技术说明**

小车本身搭载了摄像头，TK板内连接图像处理系统，通过caffeine module识别出图像内的物体，根据分析出的问题做出相应的举动。

小车内的TK板用ROS连接ardurino板，ardurino上烧录的程序根据ROS发送的信息进行相应的操作。

**未来发展规划**

搭载人脸情绪或手势识别的小型机器人。可以根据情绪信息分析进行相应操作。比如在人情绪低落时会拥抱的机器人。

#### 例子五--NVIDIA小车

**详情描述**

本作品主要由视频采集模块、信号处理模块和控制系统组成。上位机部分由具有超强计算能力的Jetson TX1嵌入式开发板和USB相机组成，相机采集的实时数据交由TX1处理，结合Caffe深度学习框架，可以实时的识别视频中的各类物体。与此同时，向下位机发送控制指令，驱动小车做出相应的动作。并且可以通过远程控制，实现对小车的即时控制。作品流程如下图所示。

![小车项目架构](/Users/kuber/Documents/ 青灯黄卷2/面试书籍/小车项目架构.tiff)

**创意来源**

现今深度学习如此火热，对于各个领域，与其结合必将有更新更广的发展空间。而酷炫的自动驾驶技术也让我们心驰神往，路上行驶的车辆，如何有效地避让行人和车辆，如何在路口减速停车，如何有效地做路径规划等，让我们存有疑惑也充满激情。而且此次NVIDIA提供了基于嵌入式平台的智能车相关硬件和软件，队友一拍即合准备尝试NVIDIA的项目。对于智能车系统的开发，将结合NVIDIA的Jetson TX1平台和深度学习实现类似真实路况的前进，后退，停止等。

**解决的问题**

1. 对于基于视觉的自动驾驶技术，与真实的复杂路况相比，我们尝试先从相应的目标识别展开。当车辆在路上行驶时，必将对周围的路况和物体做识别判断，从而控制车辆的行进状态。所以如何有效地识别观察的物体是必不可少的环节。


2. 对于前者，有效识别物体是非常重要的，而识别后对控制系统的改变也是重中之重，平稳的行驶，稳定的转弯以及及时的刹车等都是需要好好研究解决的问题。


3. 对于远程控制，需要考虑传输速率的实时性问题。

由英伟达公司的嵌入式GPU Jetson TX1作为主控芯片的智能小车。该设备使用开放的Linux + ROS系统，可以通过摄像头实时识别19类物体，并能根据摄像头看到六类物体做出六个不同的规定行进动作。

**核心技术说明**

1. Caffe框架及训练的模型。


2. Arduino控制程序

**SDK使用情况说明**

1. NVIDIA  英伟达嵌入式开发平台


2. Arduino


3. Opencv


4. Caffe


5. ROS

**未来发展规划**

1. 基于嵌入式平台自动驾驶技术

Jetson平台具有能耗低，体积小，计算能力强等优点，能够较好地应对自动驾驶要遇到的问题。其强有力的计算能力，能够实时处理接收的数据，较多的接口，具有很强的可扩展性。

2. 车载人脸识别防盗技术

现在汽车数量迅速增加，人们也有较强的防盗意识，但是现有的防盗技术，还是有很多缺点，例如钥匙被盗或者被复制的情况。而基于人脸识别的技术，能够有效的起到防盗作用，对于恶意的盗窃或者无关人员的随意借车能够有效避免。

3. 车辆（地面移动平台）智能跟随系统

无人机的智能跟随系统已经很火热，而对于车辆（地面移动平台）的智能跟随系统，却少有人做。随着智能硬件（NVIDIA嵌入式开发板）计算能力的提升，对于处理地面复杂场景将有更好的解决方法和更好的愿景。

![小车套件2](/Users/kuber/Documents/ 青灯黄卷2/面试书籍/小车套件2.tiff)

![小车项目架构2](/Users/kuber/Documents/ 青灯黄卷2/面试书籍/小车项目架构2.tiff)

#### 例子六NVIDIA小车

**详情描述**

此款小车结合多种技术，比如SenseTime的计算机视觉识别技术，以及NVIDIA的Jetson Platform TK1作为主控芯片，使用开放的Ubuntu+ROS系统，耦合度低，扩展性强，可以搭载各类传感器，对于输入的信号进行识别。用Caffe的深度学习框架进行训练，可以识别各类物体，并做出相应的动作。此外，我们使用远程桌面控制，使得小车能够自由移动。

此款小车的创新意义在于NVIDIA GPU Jetson TX1提供了在传统机器人系统中使用的CPU无法提供的人工智能计算能力，使机器人有了一颗“智能”的大脑。

**创意来源**

无人驾驶，人机交互为当今的热点技术。在无人驾驶的热潮中，如何有效识别路况，判别障碍物为一重要的课题。比如：在无人驾驶汽车在遇到障碍物时，会根据障碍种类做出不同运动反应。如果视频输入端检测到飞鸟可以直行通过，而判断行人走路则立即停止。如何实现这类功能？我们通过把课题做在小车上，通过摄像头读取并判定前方物体信息，从而做出不同的反馈动作，验证想法的可行性。本小车作为无人驾驶技术的验证雏形，具有灵活，便捷和开放的特点。

**解决的问题**

无人驾驶概念逐步融入生活。对于无人驾驶技术，如需验证视觉算法的可行性，则需要耗费很大的空间，时间和精力：比如，汽车的安装，场地的限制和计算机视觉算法的调试。而我们的智能小车平台，则可以作为一个集成而浓缩的模型，为无人驾驶技术的验证与发展如虎添翼。

此外，智能机器人在进行人机交互的过程中，图像识别，计算机视觉为重要的一个组成部分。计算机视觉可以看作是研究如何使人工系统从图像或多维数据中“感知”的科学。计算机视觉包含如下一些分支：画面重建，事件监测，目标跟踪，目标识别，机器学习，索引建立，图像恢复等。针对“感知”，机器人可以做出不同反应，如语音反馈，动作等。从而使得人工智能上一个台阶。

本机器人是一个基于计算机视觉的人机交互平台的雏形，我们的数据集可扩大到更多不同的物品，乃至声音，自然语言等。机器人能做出的反应指令可扩大到更复杂认知，机器模型结构也可以更为复杂精密。我们的项目验证了这样一个平台的可能性，这就是我们课题所解决的问题。

**功能设计说明**

小车集多种功能于一体，摄像头模块可读取图像数据，基于OpenCV的图像识别物体目标进行识别判定；ROS平台所搭载的Arduino运动控制模块，可根据目标控制小车的运动；TX1的主板保证了基于Caffe框架的Deep Learning和的人工智能的运行；两个声纳模块的协同运作，使得小车可敏捷避障。

**核心技术说明**

SenseTime 目标-人体检测技术

NVIDIA Jetson Platform TK1 主控芯片

Caffe Setup 提供深度学习框架，接收交还的RGB数据

ROS及Arduino的集成   小车运动反应控制

Deep-CNN目标检测算法   

ROS及Arduino的集成

NVIDIA Jetson Platform TK1

Caffe Setup

Deep-CNN目标检测算法

SenseTime 目标-人体检测技术

**SDK****使用情况说明**

SDK

NVIDIA

Sensetime

Arduino

Caffe

Ros

**未来发展规划**

智能机器人在进行人机交互的过程中，图像识别，计算机视觉为重要的一个组成部分。计算机视觉可以看作是研究如何使人工系统从图像或多维数据中“感知”的科学。计算机视觉包含如下一些分支：画面重建，事件监测，目标跟踪，目标识别，机器学习，索引建立，图像恢复等。针对“感知”，机器人可以做出不同反应，如语音反馈，动作等。从而使得人工智能上一个台阶。

本机器人是一个基于计算机视觉的人机交互平台的雏形，我们的数据集可扩大到更多不同的物品，乃至声音，自然语言等。机器人能做出的反应指令可扩大到更复杂认知，机器模型结构也可以更为复杂精密。我们的项目验证了这样一个平台的可能性，这就是我们课题所解决的问题。

NVIDIA GPU Jetson TX1提供了在传统机器人系统中使用的CPU无法提供的人工智能计算能力，该小车除了视频模块外，还可以搭载语言识别和语义检测输入模块，声源定位技术，生机电模块等，有丰富的扩展空间。

由于该智能小车基于ROS平台开放性强，低耦合性，我们的智能小车可以运用于各类不同的应用场景。

首先，在家用场景中，我们的小车可以将训练图片集扩展到用户的不同手势和身体姿态，实现智能跟随功能与陪功能。同时，可以与SLAM室内定位相结合，在SLAM定位的过程中，根据定位过程中遇到的移动障碍物的不同情景作出反应，提高定位精确度。

在安防场景中，小车可以加入除了网络视频模块，加入人脸识别技术中的人脸遮挡检测，对于可疑人员进行跟踪报警，并将视频记录上传。

在地震救援场景中，该小车可通过高像素摄像头识别出被困人脸，并结合被困人的呼救声进行声源定位，声音与视频信号双重保障，使得救援小车能够更好地输送救灾物资。

**项目使用指南**

首先按电源键开启小车。

在小车行驶过程中，使用者可在摄像头前放置不同物品或物品的图片，小车即可做出不同反馈。同时，使用者也可以通过远程桌面上的按钮控制小车的前进，后退，停止，旋转。

1) 遇到小猫图片，向前直行两秒  Aasss

2) 捕捉牛图片，向后直行两秒

3) 捕捉自行车图片，原地向右旋转360度

4) 看到羊，原地向左旋转360度

5) 看到椅子，原地向右90度转弯并直行两秒

6) 看到汽车，原地向左90度转弯并直行两秒

注意：小车仅在出发地接受图片指令，动作行进过程中不接受新的图片指令输入。如需在小车行驶过程中控制小车，可在远程桌面的界面按钮停止。

小车自带的两个声纳模块，可帮助小车在行驶过程中避免障碍物。