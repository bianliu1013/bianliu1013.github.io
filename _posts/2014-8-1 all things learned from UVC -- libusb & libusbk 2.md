---
layout: post
title: all things learned from UVC
date:  2014-08-01 11:23:32
categories: uvc c++
---

<br>
**于UVC协议的QA，欢迎扩充，添加comments**。
<br>
 

###usb 协议概述###
http://99tiantian.blog.163.com/blog/static/220056512011101710514812/
http://99tiantian.blog.163.com/blog/static/220056512011101511265343/

 

###usb和endpoint关系###
端点（Endpoint）：是 USB 设备中的可以进行数据收发的最小单元，支持单向或者双向的数据传输。设备支持端点的数量是有限制的，除默认端点外低速设备最多支持 2 组端点（2 个输入，2 个输出），高速和全速设备最多支持 15 组端点。

总起来说就是一个设备（device）会拥有一个或者多个endpoint,
endpoint 0: 所有的USB 设备必须具有端点0，它可以作为in 端点，也可以作为out 端点，USB 系统软件利用它来实现缺省的控制管道，从而控制设备。设备插入windows后，系统用来查询设备信息。

<br>
###Endpoint的类型###
USB endpoint 有四种类型，也就分别对应了四种不同的数据传输方式。它们是控制传输（Control Transfers），中断传输（Interrupt Data Transfers），批量传输(Bulk DataTransfers)，等时传输(Isochronous Data Transfers)。控制传输用来控制对USB 设备不同部分的访问，通常用于配置设备，获取设备信息，发送命令到设备，或者获取设备的状态报告。总之就是用来传送控制信息的，每个USB 设备都会有一个endpoint 0 的控制端点，内核里的USB core 使用它在设备插入时进行设备的配置

 
<br>
###如何拿到工作（我们需要的）的endpoint###
正在研究中：是固定的address，还是启动是获取？
`update` ：现在看来，可以在init的时候，通过libusb 获取，根据类型，大小参数区分，是否是自己需要的EP。

<br>
###如何与usb设备进行通信###
用usblib库， 简单用例：
http://www.dreamincode.net/forums/topic/148707-introduction-to-using-libusb-10/page__p__885389__hl__USB__fromsearch__1#entry885389
http://libusb.sourceforge.net/api-1.0/index.html

<br>
###是否有windows自带api与usb设备通讯###
WinUSB library.  Do some work  like libusb, but provided by MS.

 
<br>
###我们需要和usb直接打交道吗？换句话说，我们需要在WinUSB 或者 libuse上面封装，发送数据，控制我们的camera###
暂时看来，不需要，因为UVC是在USB协议上层的子协议（是否可以这么理解？）而我们有这样的库，帮我们做UVC的事情。
`update`：说法没有错误，只是现在看来，不够。

 
<br>
###接问题7，那是神马东东###
DirectShow
优势：有demo, 封装好。
劣势：跨平台？（有多少项目能真正做到这点？）
More info: http://msdn.microsoft.com/en-us/library/windows/hardware/ff568658(v=vs.85).aspx
`update`：DirectShow不够，痛苦的摸索后，最后还是转到libusb上。

 
<br>
###与usb设备进行通讯时，同步传输？ 可否异步，多线程###
DirectShow 关心。（不确定）
update: Direct Show不够，direct show 排除，问题无意义。

 
<br>
###提供一个最简单的demo，譬如从摄像头里拿参数信息###
`update`: future。