---
title: What is X Window?
updated: 2017-01-05 10:47
tags: [linux]
category: Linux
---

## 简介
X Window System (X视窗系统) 是服务于Unix系统的GUI系统，由Xorg基金会维护。它主要由X Server和X Client两部分组成。X Server负责管理硬件，而X Client则是应用程序。在运作上，X Client应用程序将需要呈现的画面告诉X Server，最终由X Server将结果通过它管理的硬件绘制出来。整体架构如图1所示。

![spatial transformer]({{site.baseurl}}/images/x_ser_cli.gif)
**<center>图1. X Window System架构 </center>**

## X Server
X Server负责管理硬件设备，包括输入／输出设备如键盘、鼠标、手写板、显示器以及显卡（包含驱动）等等。虽然Linux系统与显卡、显示器、键盘、鼠标等外设有相应的连接和设置，但是X Window对这些设备另有一套独立的设置以进行连接和交互。所以在CentOS系统上由multi-user.target (runlevel 3)进入图形界面graphical.target (runlevel 5)时启动`startx`会自动载入X Window所需的驱动程序。

## X Client
X Server主要是管理显示界面与在屏幕上绘图，同时将输入装置的行为告知X Client（如鼠标的移动、窗口的拖动等），此时X Client就会依据这些行为生成图示的显示资料回传给X Server，X Server再将X Client传回的显示资料描绘(delineate)在荧幕上。所以X Client的主要工作就是处理绘图资料。

## X Window Manager
因为X Client仅仅负责处理绘图资料，它本身并不知道它在X Server中的位置、大小等信息以及它与其他X Client的相对位置关系，为此Window Manager作为一种特殊的X Client就负责全部X Client的控管，包括

* 提供控制元素，如工作列、桌面背景的设定；
* 管理虚拟桌面（Virtual Desktop）；
* 提供视窗控制参数，如视窗大小、重叠、移动、最大最小化。

常用的GNOME、KDE、twm、XFCE就是X Window Manager。为了方便，在X Window Manager上常常会加入一些常用的应用软件如输入法等等。

## Display Manager
Display Manager负责提供登入的环境（包括提供图形登陆界面）以及载入用户选择的Window Manager的资料。几乎所有的X Window Manager都会提供Display Manager，例如CentOS上的GNOME使用GNOME Display Manager(gdm)来提供图形界面登陆。

## Reference
[1] [http://linux.vbird.org/linux_basic/0590xwindow.php](http://linux.vbird.org/linux_basic/0590xwindow.php)
