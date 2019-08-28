---
title: Nvidia Jetson TK1 board set up and get started
date: 2017-03-26 18:47:20
tags: 
	- Jetson TK1
	- CUDA
categories: 
	- 研究僧日常
thumbnailImage: mushroom.jpg
thumbnailImagePosition: left
autoThumbnailImage: true
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
---
老师给了一块NVIDIA Terga K1的开发板，让我玩玩看。
<!--more-->
<!--toc-->

给我的时候里面是ubuntu14.04的系统，但明显兼容性太差，乱码和闪退问题太多。所以我决定自己刷个原生系统。因为要跑cuda程序，所以安装cuda6.5（cuda7.0目前还不支持TK1板子）
整个的步骤一共三步：1.下载 2.设置 3.安装cuda6.5

#### 下载
1) 在Linux下，到L4T的页面，找到Jetson TK1和sample file system这两个下载：
链接为https://developer.nvidia.com/embedded/linux-tegra
界面如下：
![Jetson Downloading](http://blog.kuberfly.me/2017/03/26/Data-Structure-7-Sorting-1/jetson_package.png)
目前最新的版本是R21.4，以这个版本为例，下载下来的是两个文件：
Tegra124_Linux_R21.4.0_armhf.tbz2
Tegra_Linux_Sample-Root-Filesystem_R21.4.0_armhf.tbz2
我把这两个文件放在Downloads文件夹下，先解压驱动包
$sudo tar --numeric-owner -jxpf Tegra124_Linux_R21.4.0_armhf.tbz2
在解压刷机需要的文件的时候，需要用--numeric-owner选项，否则就会出现权限问题。作为Linux小白，保险起见，解压还是加上了这个选项.
等待解压结束，Downloads文件下会出现一个Linux_for_Tegra文件夹。这个文件夹下有一个rootfs的文件夹，这里就是要放L4T的地方了，所以先进入rootfs文件夹：
$cd Linux_for_Tegra/rootfs
然后在该文件下解压基于Ubuntu的L4T Sample Image：
$sudo tar --numeric-owner -jxpf ../../Tegra_Linux_Sample-Root-Filesystem_R21.4.0_armhf.tbz2
然后返回上层文件夹：
$cd ..
执行安装脚本，会生成真正的system image。
$sudo ./apply_binaries.sh

#### 开始刷机
刷机之前，仔细阅读Jeston_TK1_QuickStartGuide。在网站上能找到，先把网线、HDMI线、外接usb hub都接好，在接好micro usb线（一头在TK1上，一头在host上，这里使用一台ubuntu14.04的host）。最后通电源。
刷机时，先按住板子最边缘的force recovery按钮不松开，然后按一下reset，板子会重启进入recovery mode（听见风扇声音变化），这个时候你会发现系统当前目录下（Downloads目录下）新mount上了一个16G的eMMC设备。（可看见flash.sh这些文件）
然后执行刷机命令：
$sudo ./flash.sh -S 8GiB jetson-tk1 mmcblk0p1
mmcblk0p1就是板子内部存储对应的设备名。
刷机成功后板子重启，就能进入Ubuntu界面了，原本还需要执行网络的一些配置，但我做的时候是直接可以上网了的，这里只列出来，但不用操作）
$sudo dhclient eth0
$ifconfig
$sudo apt-get update
$sudo apt-get install ubuntu-desktop
手动刷机成功。
自动刷机
事实上Jetson TK1的官网也提供了傻瓜刷机包JetPack，只要到Jetpack的网页下载JetPackTK1-1.1-cuda6.5-linux-x64.run到进行刷机操作的Linux里，然后执行
chmod +x JetPackTK1-1.1-cuda6.5-linux-x64.run
然后双击，就会弹出图形界面，按照界面一步步来就能搞定从驱动到L4T到OpenCV和CUDA的所有相关包，不过鉴于前面所说的可能出现的权限问题，我还是放弃了这个套路。

#### 安装cuda6.5（.run方式）
在https://developer.nvidia.com/embedded/linux-tegra页面下找到CUDA和OpenCV的deb包下载之。
1.Perform the pre-installation actions. （忽略了，无非是些检查）
2. Install repository meta-data 
$ sudo dpkg -i cuda-repo-l4t-r21.3-6-5-prod_6.5-42_armhf （可以下载下来拷过去）
3. Update the Apt repository cache 
$ sudo apt-get update 
 4. Install CUDA Toolkit 
$ sudo apt-get install cuda-toolkit-6-5 （是在线安装的，结果导致那天晚上回去太晚被媳妇儿批）
5. Add the user to the video group（因为要访问gpu，必须把用户添加到video组下）
$ sudo usermod -a -G video

至此就已经安装好了，cuda-toolkit和cuda-samples都好了，因为armhf那个包里面已经包含两者）
下面进入usr/local/下，可以看到cuda的真身了。
$cd usr/local/cuda/
看到Samples,进去可以看到各种例子已经在了。
下面因为这个Samples权限是只读，所以复制他出来,执行编译好的脚本就可以复制。
$./cuda-install-samples-6.5.sh /home/ubuntu/
回到home/目录下就可以看到有一个NVIDIA-6.5_Samples的文件了。
使用make编译，整体编译,或者分开编译。（这里是原生编译即使用板子上arm的cpu编译，速度相对慢一点）
然后进入编译好的文件目录下
$cd /home/ubuntu/NVIDIA-6.5_Samples/bin/armvl7/linux/release/gnueabinf
来用./xxx执行任意编译好的文件
这里要解个毒，执行./smokeparticles时候，理论上在65536的什么的情况下，可以达到30多帧的一个分辨率，应该具有很强的推动能力，但是在我的板子上跑起来，完全不是那么回事儿，动画特别慢。

至此，全部我对TK1的初期目标达成。

#### 使用cuda
$mkdir test
$sudo vim test1.cu
$粘贴代码，保存退出
$nvcc test1.cu -o test1
$./test1
其他均可以此类推。
PS: vim编辑器确实好用，下次可试emac编辑器。