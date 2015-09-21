---
layout: post
title: all things learned from UVC1
date:  2014-08-01 11:23:32
categories: uvc c++
---


<br>
Libusb and libusbK 的关系
--

>Libusbk是libusb的超集。同时支持libusb, winusb, libusbk三种驱动模式。

 
<br>
环境搭建
--

>在 http://sourceforge.net/projects/libusbk 上可以拿到最新的 libusbk 库， setup.exe 会装好运行环境，driver installer creator Wizard 等工具。

 
<br>
如何使用
--

>用 “driver installer creator Wizard” 更新自己的driver，如果是UVC设备，就要把自带的usbvideo驱动换成 libusbk的驱动。更新成功后，device manager 里会有libusbk的一栏。

 
<br>
Sample
--

>C:\libusbK-dev-kit\examples 里有自带的各种sample， 默认是vs2008的工程，如没有2008环境，自己可以改project文件。 Sample写的都很舒服， 一般只需要更把EXAMPLE_VID，EXAMPLE_VID的值改成自己设备的， 把EXAMPLES_USE_BENCHMARK_CONFIGURE 设为0 就OK。 体验体验吧，从设备里拿到第一个字节的冲动。：）

  
<br>
如何验证？
--

> 简单的验证，可以用usblyzer抓包工具 和你的sample结果进行对比，看是否一致。 Usblyzer一次抓包buffer是有限制的（默认16k？），自己摸索的改成最大。

 
<br>
为何选择libusbk
--

>因为硬件设计，每个isopackage 的头都标示frame的信息（非标准视频文件头），而我又需要这些信息，libusb不满足，详情见我的[request](
http://libusb.6.n5.nabble.com/how-to-get-the-size-of-each-packet-td5663684.html)

 
<br>
libusk有什么不足
--

>曾经见一位仁兄抱怨，是不是他是第一个用这个库的人。。现在用下来，有时候也真有同感，不过好在库的开发者都很好，support都很nice。几轮回合下来（http://libusb.6.n5.nabble.com/template/NamlServlet.jtp?macro=user_nodes&user=334628），看着最新的release note里，好几个 reported by peter， 有那么点成就感（IT行业摸爬5年，终于为开源做点贡献），也有担心—蓝屏的问题还没有solution。将来。。。 Anyway, keep moving on.

 

<br>
2005如何编译 libusbk
--

>在 http://sourceforge.net/projects/libusbk 下到最新的source， 在\src\lib下面有libusbK_lib.vcproj，修改Version="8.00"， 然后build，应该会报告两个错， error code找不到，可以google标准的windows error code，直接用数字代替。重新编译，就能得到静态库了。