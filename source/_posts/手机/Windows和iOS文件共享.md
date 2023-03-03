---
title: Windows和iOS（一）：文件共享
date: 2021-9-24 09:01:00
tags: 
 - ios
categories: 
 - 手机
---

以前一直在苹果的生态中使用各种苹果的服务，例如AirDrop, iCloud文件, iCloud照片等，但是，后来由于MacBook Air出现电池故障以后，就更换成了一台Windows PC。于是，使用过程中，出现了各种不方便，归根结底，就是数据无法在手机和PC之间方便地同步和共享。

起初，我曾经尝试下载并安装了Windows版本的iCloud。虽然软件的版本已经很久没有更新，但是我只需要使用文件和照片的同步功能，想着应该问题不大，但是在实际使用过程中，遇到的以下一系列问题让我果断卸载了该软件：

- 程序在后台运行导致CPU占用率很高，风扇会持续转动并发出噪音

- Windows上的照片删除后，被删除的照片仍然会保留在手机上

- iPhone上的部分照片无法成功下载到Windows上

- 使用一段时间后，程序会挂起，导致无法同步

  <!-- more -->

经过一番比较和尝试，我决定使用微软的网盘工具[OneDrive](https://www.microsoft.com/zh-cn/microsoft-365/onedrive/download)来解决文件和照片的同步问题。在使用一段时间以后，OneDrive完全能满足我所有的需求。手机和电脑上的文件和照片不仅能实现双向同步，并且，Windows自带的照片管理软件能很方便的查看和编辑所有的照片。

需要注意的是，有些应用，例如在微信的聊天窗口中，如果要发送文件给对方，只能通过iCloud选择需要发送的文件。为了能在iCloud的操作窗口中，访问OneDrive里面的文件，需要在**文件**应用中，**打开OneDrive的选项**。

{% asset_img icloud-onedrive.jpg 600 %}

除了iCloud以外，苹果还有一项非常好用的功能，即AirDrop。这项功能可以让Mac和iPhone只需要通过局域网就可以互传文件，不需要再依赖于互联网。在Windows系统上，我想到了用**Windows的文件共享（CIFS/SMB）**功能来实现类似的效果。在Windows文件夹的属性中，有一个“共享”选项，打开共享功能后，就可以在iPhone上，通过**文件**应用，访问该共享目录。

{% asset_img fileshare01.jpg 600 %}

打开iPhone上的**文件**应用，打开**连接服务器**选项，输入Windows的主机名，例如*smb://nuc*，其中nuc就是Windows的主机名。

{% asset_img icloud-connect-server.jpg 600 %}
连接服务器成功以后，就可以在iPhone上访问Windows共享文件夹里面的数据了。通过这种方式，就可以在局域网中，让手机和电脑之间互传文件了。

{% asset_img icloud-server-share.jpg 600 %}