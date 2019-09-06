---
title: Run a cpp Example on Win10 Docker with VS Build Tools
date: 2017-05-15 16:40:11
categories: 
   - DevOps
tags: 
   - Docker
thumbnailImage: architecture-buildings-business-2385210.jpg
thumbnailImagePosition: left
autoThumbnailImage: yes
coverImage: galaxy.jpg
coverCaption: "A beautiful galaxy"
comments: true
---

Sometimes when you need to use a cpp runtime environments on Windows system for your special applications, you can just install a Visuo Studio xxx to do it. But if you want to dilivery it to cloud environment, maybe you need make it into a Docker just like me. 
<!--more-->
<!--toc-->
### Install Docker into Win10

1. Install [Docker CE](https://docs.docker.com/docker-for-windows/install/) in one computer with Windows 10 system, which its Virtualization must to be turn on firstly in your BIOS.

2. Note that, you need to log out once to complete installation when the installation succeeded.

3. After it, when you start Docker, it will check your computer setting of Hyper-V. 

   ![Windows Feature](http://blog.kuberfly.me/2017/05/15/Run-a-cpp-Example-on-Win10-Docker-with-VS-Build-Tools/windows_feature.png)
   Hyper-v must be turn on status when you run Docker, which means your Virtual Box can not be used in the meantime. maybe there will have one computer restart here, don't panic, this is the last restart if you only use Docker instead of virtual machines.

4. After Docker started, you can find a penguin log in your taskbar. here has some tips to help you test Docker or improve Docker.

   1. Switch Registry: 

     You can switch your registry to [China Official Registry](https://registry.docker-cn.com/) by edit the Daemon which by right click your Docker to choose Setting. Or you can change registry to a private registry like your aliyun's speeder address.
     ![Switch Regostry](http://images.5bug.wang//2018/03/201803016053_9660.png)

   2. Switch to Windows Container Mode: 

      In order to use windows images in Docker, you must change mode from Linux Container to Windows Container. after that, you can check it through `docker version` command.

      ![switch](https://yqfile.alicdn.com/b7e02a92cc7689b498dc430e3a6b264c332c9c1f.png)

   3. First example of windows container: execute `docker version` in your cmd.exe, then the client and server information for Docker will be shown. After that,  you can execute `docker run microsoft/sample-dotnet` in your cmd.exe or Windows PowerShell.exe (more popular and more convenient) . if there has a logo output, your installation would be correct.

### Load a Windows Image with VS Build Tools

1. Basic Commands: 

   After your Docker installation succeeded, you can always use it in your PowerShell through some usual commands to get some basic status like the bellowed.

   ```
   #list running containers
   docker ps
   #list all containers(include running/exited/created)
   docker ps -a
   #list images
   docker images
   #check basic info of your Docker
   docker info
   ```

2. **Load Images**: 

   So, if you want to run a container with cpp files in it, you need to have a windows image with VS build tools environment first. I already packed a ready image, this image is built by [Dockerfile](https://github.com/kubertalk/DockerImage-Winserver/blob/master/Dockerfile?1556451298000), which is from a source image `microsoft/windowsservercore` with Visual Studio Build Tools in it. You just need to load my image into a new folder on your system through ***`docker load -i \\cd-srv04\pxa\Projects\simulation\docker-images\winservercore-vstools.tar`***. 

   PS: if you want to what's the difference of `windowsservercore` and `nanoserver`, please visit [here](https://docs.microsoft.com/zh-cn/virtualization/windowscontainers/deploy-containers/deploy-containers-on-server).

   if you want to know how to build your own windows image, please visit [here](https://github.com/docker/labs/blob/master/windows/windows-containers/WindowsContainers.md?1556446332153).

   and if you want to know the how to build and save the image we are using, and the please see [here](https://github.com/kubertalk/DockerImage-Winserver). 
   
3. Check Images:

   Use **`docker images`** to check if this image already in your system when it load finished.
   
   ![Check Images](http://blog.kuberfly.me/2017/05/15/Run-a-cpp-Example-on-Win10-Docker-with-VS-Build-Tools/check_images.png)

### Start a Container with a Interactive Cmdlet

1. After the image load successfully, you can run a container with this image, or you can build other images using this image.

2. Start a Container:

   Use ***`docker run -it -v $host-code-addr:$container-code-addr win-vs`*** to start a container with a interactive cmdlet, just like the below picture. Then you can do everything you like in this container.

   ![Start Container](http://blog.kuberfly.me/2017/05/15/Run-a-cpp-Example-on-Win10-Docker-with-VS-Build-Tools/start_container.png)

3. About the above command, just give you some tips for it:
   ```
   $host-code-addr = c:/xxx/code   #code address in host computer, means container outer
   $container-code-addr = c:/code  #code address in container inner
   
   docker run -it -v $host-code-addr:$container-code-addr win-vs
   # -i,--Interactive: keep STDIN open
   # -t,--tty : allocate a pseudo-TTY
   # -v,--volume list: Bind mount a volume
   ```
   
4. **Compile and Run a Hello World**:

   ```
   cd code
   #compile your files
   cl .\hello.cpp /EHsc
   #run your application
   .\hello
   ```

   ![Run Hello-World](http://blog.kuberfly.me/2017/05/15/Run-a-cpp-Example-on-Win10-Docker-with-VS-Build-Tools/hello_world.png)

5. Move your whole application of vc++ code to your shared volume on your host computer, and make it in your container. just have a try !

6. After test or debug on this pseudo cmdlet, you can use `exit` or `ctl+P+Q` to quit this window. Then you can use `docker start/restart -i $containerID`  to go back this container's cmdlet.

### QA and Issues

**Have a fun and if you have any issues or problem, please don't hesitate to contact me with kubertqiu@hotmail.com**.