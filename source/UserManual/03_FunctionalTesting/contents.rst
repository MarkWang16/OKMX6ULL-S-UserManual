**第三章 OKMX6ULL-S平台功能测试**
==========================

-  **注意：该产品核心板支持的功能不局限手册中的提到的功能，飞凌仅对手册中的功能项目做测试验证，手册中未说明的功能不予以保证，用户可自行测试验证。**

此章节主要说明开发板外扩接口的使用方法。

3.1 命令行功能测试
-----------------

-  命令行测试程序源码路径：用户资料/Linux/测试程序源码

本节测试所用到的测试程序在飞凌提供的Demo中已有集成，故不做文件来源说明，直接进行命令操作。

3.1.1 SDHC/MMC卡驱动测试
------------------------

-  **说明：**

-  **不支持NTFS格式的文件系统，若不清楚SD卡格式，建议使用前将其格式化为FAT32格式。**

-  **SD卡挂载目录为/run/media，支持热插拔，终端会打印关于SD卡的信息。**

-  **不同的SD卡显示信息可能会有差别，本公司采用闪迪8G的SD卡进行测试。**

-  **CPU支持的MMC数据传输模式如下图**

|image0|

SD卡插入开发板的SD卡槽后，系统会自动检查并挂载SD卡，挂载成功后，可对SD卡进行读写操作。

1、插上8G
SD卡，正常挂载后从打印信息可以看到SD卡挂载后的设备名，打印信息如下：

+-----------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# mmc0: host does not support reading read-only switch, assuming write-enable      |
|                                                                                                     |
| mmc0: Problem setting current limit!                                                                |
|                                                                                                     |
| mmc0: new ultra high speed DDR50 SDHC card at address aaaa                                          |
|                                                                                                     |
| mmcblk0: mmc0:aaaa SS08G 7.40 GiB                                                                   |
|                                                                                                     |
| mmcblk0: p1 //挂载后的文件名为mmcblk0p1                                                             |
|                                                                                                     |
| FAT-fs (mmcblk0p1): Volume was not properly unmounted. Some data may be corrupt. Please run fsck.   |
+-----------------------------------------------------------------------------------------------------+

2、/run/media为SD卡的挂载目录，查看该目录下的文件：

+-----------------------------------------------------------------+
| root@fl-imx6ull:~# ls /run/media //列出/run/media目录下的文件   |
+-----------------------------------------------------------------+

打印信息如下，mmcblk0p1为SD卡挂载后的文件名

+---------------------------+
| **mmcblk0p1** mmcblk1p1   |
+---------------------------+

3、查看SD卡中的文件，命令如下：

+------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ls -l /run/media/mmcblk0p1 //列出/run/media/mmcblk0p1目录下文件属性   |
+------------------------------------------------------------------------------------------+

打印信息如下：

+--------------------------------------------------+
| drwxr-xr-x 2 root root 4096 Jan 22 2016 bin      |
|                                                  |
| drwxr-xr-x 2 root root 4096 Feb 21 2016 system   |
+--------------------------------------------------+

4、往 SD 卡中写入文件,命令如下,写1到test.txt 文件中：

+------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# echo 1 > /run/media/mmcblk0p1/test.txt //写1到test.txt 文件中   |
|                                                                                    |
| root@fl-imx6ull:~# sync //文件同步                                                 |
+------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /run/media/mmcblk0p1/test.txt //读取 SD 卡中test.txt 文件   |
|                                                                                    |
| 1                                                                                  |
+------------------------------------------------------------------------------------+

会读到刚才我们写入的1。

5、SD卡使用完成后，在弹出SD卡前，需要使用umount卸载SD

+--------------------------------------------------+
| root@fl-imx6ull:~# umount /run/media/mmcblk0p1   |
+--------------------------------------------------+

-  **注意：退出SD卡挂载路径后再插拔SD卡**

3.1.2 USB接口测试
-----------------

**3.1.2.1 USB HOST接口存储测试**

-  \ **说明：**

-  **支持USB鼠标**\ \ **、USB键盘、U盘设备的热插拔。**

-  **使用U盘测试时，建议用格式化工具将其格式化为能被linux系统识别的FAT32格式。**

-  **目前U盘测试支持到32G，32G以上并未测试。**

-  **U盘挂载目录为/run/media**

|image1|

开发板上有三个USB
HOST接口，可选择任意一个进行测试，插入U盘同时终端会打印相关信息，由于存在很多种U盘，显示的信息可能会有差别，以实际打印信息为主。系统会自动检查并挂载U盘，挂载成功后，可对U盘进行读写操作。

1、插入U盘，显示如下的信息：

+------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# usb 1-1.3: new high-speed USB device number 5 using ci\_hdrc                |
|                                                                                                |
| usb-storage 1-1.3:1.0: USB Mass Storage device detected                                        |
|                                                                                                |
| scsi host1: usb-storage 1-1.3:1.0                                                              |
|                                                                                                |
| scsi 1:0:0:0: Direct-Access Generic MassStorageClass 1536 PQ: 0 ANSI: 6                        |
|                                                                                                |
| sd 1:0:0:0: [sda] 31116288 512-byte logical blocks: (7.94 GB/7.40 GiB)                         |
|                                                                                                |
| sd 1:0:0:0: [sda] Write Protect is off                                                         |
|                                                                                                |
| sd 1:0:0:0: [sda] Write cache: disabled, read cache: enabled, doesn't support DPO or FUA       |
|                                                                                                |
| **sda: sda1** **//挂载设备名为sda1**                                                           |
|                                                                                                |
| sd 1:0:0:0: [sda] Attached SCSI removable disk                                                 |
|                                                                                                |
| FAT-fs (sda1): Volume was not properly unmounted. Some data may be corrupt. Please run fsck.   |
+------------------------------------------------------------------------------------------------+

2、查看usb存储设备，/run/media为U盘的挂载目录，U盘挂载后的设备名为sda1，查的文件：

+------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ls -la /run/media/sda1/ //列出/run/media/sda1目录下的文件属性   |
|                                                                                    |
| drwxrwx--- 4 root disk 4096 Jan 1 1970 .                                           |
|                                                                                    |
| drwxr-xr-x 3 root root 60 May 2 13:57 ..                                           |
|                                                                                    |
| drwxrwx--- 2 root disk 4096 Mar 17 2020 sdrun                                      |
|                                                                                    |
| drwxrwx--- 2 root disk 4096 Mar 17 2020 target                                     |
+------------------------------------------------------------------------------------+

3、往U盘中写入文件,命令如下,写2到test.txt 文件中：

+-------------------------------------------------------------------------------+
| root@fl-imx6ull:~# echo 2 > /run/media/sda1/test.txt //写2到test.txt 文件中   |
|                                                                               |
| root@fl-imx6ull:~# sync //文件同步                                            |
+-------------------------------------------------------------------------------+

读取 U 盘中test.txt 文件，命令如下：

+------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /run/media/sda1/test.txt //读取 U 盘中test.txt 文件   |
|                                                                              |
| 2                                                                            |
+------------------------------------------------------------------------------+

会读到刚才我们写入的2。

4、U盘使用完成后，在拔出U盘前，需要使用umount卸载

+---------------------------------------------+
| root@fl-imx6ull:~# umount /run/media/sda1   |
+---------------------------------------------+

-  **注意：退出SD卡挂载路径后再插拔SD卡**

**3.1.2.2 OTG转HOST测试**

使用otg转host线连接到otg口，插入u盘能读取u盘内容。

|image2|

+------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ci\_hdrc ci\_hdrc.0: EHCI Host Controller                                   |
|                                                                                                |
| ci\_hdrc ci\_hdrc.0: new USB bus registered, assigned bus number 2                             |
|                                                                                                |
| ci\_hdrc ci\_hdrc.0: USB 2.0 started, EHCI 1.00                                                |
|                                                                                                |
| hub 2-0:1.0: USB hub found                                                                     |
|                                                                                                |
| hub 2-0:1.0: 1 port detected                                                                   |
|                                                                                                |
| usb 2-1: new high-speed USB device number 2 using ci\_hdrc                                     |
|                                                                                                |
| usb-storage 2-1:1.0: USB Mass Storage device detected                                          |
|                                                                                                |
| scsi host0: usb-storage 2-1:1.0                                                                |
|                                                                                                |
| usb 2-1: USB disconnect, device number 2                                                       |
|                                                                                                |
| usb 2-1: new high-speed USB device number 3 using ci\_hdrc                                     |
|                                                                                                |
| usb-storage 2-1:1.0: USB Mass Storage device detected                                          |
|                                                                                                |
| scsi host1: usb-storage 2-1:1.0                                                                |
|                                                                                                |
| scsi 1:0:0:0: Direct-Access Generic MassStorageClass 1536 PQ: 0 ANSI: 6                        |
|                                                                                                |
| sd 1:0:0:0: [sda] 31116288 512-byte logical blocks: (15.9 GB/14.8 GiB)                         |
|                                                                                                |
| sd 1:0:0:0: [sda] Write Protect is off                                                         |
|                                                                                                |
| sd 1:0:0:0: [sda] Write cache: disabled, read cache: enabled, doesn't support DPO or FUA       |
|                                                                                                |
| **sda: sda1 //挂载设备名为sda1**                                                               |
|                                                                                                |
| sd 1:0:0:0: [sda] Attached SCSI removable disk                                                 |
|                                                                                                |
| FAT-fs (sda1): Volume was not properly unmounted. Some data may be corrupt. Please run fsck.   |
+------------------------------------------------------------------------------------------------+

**3.1.2.3 USB OTG 接口g\_mass\_storage 测试**

将板载USB接口的U盘或SD卡接口的SD卡挂载到电脑上。

首先将U盘或sd卡连接到开发板（修改file=xxx，xxx为U盘或sd卡对应的设备，如file=/dev/mmcblk0p1,对应sd卡设备），然后将OTG接口连接到电脑。

加载U盘，命令如下：

+------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/fs/configfs/configfs.ko              |
|                                                                                                |
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/drivers/usb/gadget/libcomposite.ko   |
|                                                                                                |
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/drivers/usb/gadget/\\                |
|                                                                                                |
| function/usb\_f\_mass\_storage.ko                                                              |
|                                                                                                |
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/drivers/usb/gadget/legacy/\\         |
|                                                                                                |
| g\_mass\_storage.ko file=/dev/sda1 removable=1                                                 |
+------------------------------------------------------------------------------------------------+

打印信息如下：

+-----------------------------------------------------------------------------------------------+
| Mass Storage Function, version: 2009/09/11                                                    |
|                                                                                               |
| LUN: removable file: (no medium)                                                              |
|                                                                                               |
| LUN: removable file: /dev/sda1                                                                |
|                                                                                               |
| Number of LUNs=1                                                                              |
|                                                                                               |
| Number of LUNs=1                                                                              |
|                                                                                               |
| g\_mass\_storage gadget: Mass Storage Gadget, version: 2009/09/11                             |
|                                                                                               |
| g\_mass\_storage gadget: userspace failed to provide iSerialNumber                            |
|                                                                                               |
| g\_mass\_storage gadget: g\_mass\_storage ready                                               |
|                                                                                               |
| root@fl-imx6ull:~# g\_mass\_storage gadget: high-speed config #1: Linux File-Backed Storage   |
+-----------------------------------------------------------------------------------------------+

U盘（会挂载到电脑上），可以对u盘读写数据。

|image3|

卸载模块：

+---------------------------------------------+
| root@fl-imx6ull:~# rmmod g\_mass\_storage   |
+---------------------------------------------+

**3.1.2.4 USB摄像头测试**

-  **说明：本产品支持USB摄像头：**\ Webcam C270。

1、插入摄像头之前，通过lsusb指令查看USB状态和/dev下设备节点状态

+----------------------------+
| root@fl-imx6ull:~# lsusb   |
+----------------------------+

打印信息如下：

+------------------------------------+
| Bus 001 Device 003: ID 0bda:b720   |
|                                    |
| Bus 001 Device 002: ID 0424:2514   |
|                                    |
| Bus 001 Device 001: ID 1d6b:0002   |
+------------------------------------+

插入摄像头之前，查看video相关的设备节点

+--------------------------------------+
| root@fl-imx6ull:~# ls /dev/video\*   |
+--------------------------------------+

打印信息如下，

+---------------------------+
| /dev/video0 /dev/video1   |
+---------------------------+

2、插入指定的USB摄像头，再次输入命令查看USB状态，可以看到插入的USB摄像头信息。

+----------------------------+
| root@fl-imx6ull:~# lsusb   |
+----------------------------+

打印信息如下，会出现新的usb设备的vid和pid：

+----------------------------------------------------------------+
| Bus 001 Device 003: ID 0bda:b720                               |
|                                                                |
| **Bus 001 Device 004: ID 046d:0825** //该usb摄像头的vid和pid   |
|                                                                |
| Bus 001 Device 002: ID 0424:2514                               |
|                                                                |
| Bus 001 Device 001: ID 1d6b:0002                               |
+----------------------------------------------------------------+

输入命令查看USB摄像头的设备节点，

+--------------------------------------+
| root@fl-imx6ull:~# ls /dev/video\*   |
+--------------------------------------+

打印信息如下，可见新增设备节点video2即为刚才插入的usb设备：

+-------------------------------------------+
| /dev/video0 /dev/video1 **/dev/video2**   |
+-------------------------------------------+

3、输入命令查看摄像头支持的分辨率和帧速率，进入到测试程序所在目录/forlinx/cmdbin/

+------------------------------------------+
| root@fl-imx6ull:~# cd /forlinx/cmdbin/   |
+------------------------------------------+

运行测试程序luvcview，-d为对应的设备文件，-L为查询有效的图像格式

+--------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_luvcivew -d /dev/video2 -L   |
+--------------------------------------------------------------+

打印信息如下：

+-------------------------------------------------------------------+
| luvcview version v0.1                                             |
|                                                                   |
| starting process                                                  |
|                                                                   |
| video /dev/video2                                                 |
|                                                                   |
| /dev/video2 does not support read i/o                             |
|                                                                   |
| { pixelformat = 'YUYV', description = 'YUV 4:2:2 (YUYV)' }        |
|                                                                   |
| { discrete: width = 640, height = 480 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 160, height = 120 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 176, height = 144 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 320, height = 176 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 320, height = 240 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 352, height = 288 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 432, height = 240 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 544, height = 288 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 640, height = 360 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 752, height = 416 }                           |
|                                                                   |
| Time interval between frame: 1/25, 1/20, 1/15, 1/10, 1/5,         |
|                                                                   |
| { discrete: width = 800, height = 448 }                           |
|                                                                   |
| Time interval between frame: 1/25, 1/20, 1/15, 1/10, 1/5,         |
|                                                                   |
| { discrete: width = 800, height = 600 }                           |
|                                                                   |
| Time interval between frame: 1/20, 1/15, 1/10, 1/5,               |
|                                                                   |
| { discrete: width = 864, height = 480 }                           |
|                                                                   |
| Time interval between frame: 1/20, 1/15, 1/10, 1/5,               |
|                                                                   |
| { discrete: width = 960, height = 544 }                           |
|                                                                   |
| Time interval between frame: 1/15, 1/10, 1/5,                     |
|                                                                   |
| { discrete: width = 960, height = 720 }                           |
|                                                                   |
| Time interval between frame: 1/10, 1/5,                           |
|                                                                   |
| { discrete: width = 1024, height = 576 }                          |
|                                                                   |
| Time interval between frame: 1/10, 1/5,                           |
|                                                                   |
| { discrete: width = 1184, height = 656 }                          |
|                                                                   |
| Time interval between frame: 1/10, 1/5,                           |
|                                                                   |
| { discrete: width = 1280, height = 720 }                          |
|                                                                   |
| Time interval between frame: 1/10, 1/5,                           |
|                                                                   |
| { discrete: width = 1280, height = 960 }                          |
|                                                                   |
| Time interval between frame: 2/15, 1/5,                           |
|                                                                   |
| { pixelformat = 'MJPG', description = 'MJPEG' }                   |
|                                                                   |
| { discrete: width = 640, height = 480 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 160, height = 120 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 176, height = 144 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 320, height = 176 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 320, height = 240 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 352, height = 288 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 432, height = 240 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 544, height = 288 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 640, height = 360 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 752, height = 416 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 800, height = 448 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 800, height = 600 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 864, height = 480 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 960, height = 720 }                           |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 1024, height = 576 }                          |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 1184, height = 656 }                          |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 1280, height = 720 }                          |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
|                                                                   |
| { discrete: width = 1280, height = 960 }                          |
|                                                                   |
| Time interval between frame: 1/30, 1/25, 1/20, 1/15, 1/10, 1/5,   |
+-------------------------------------------------------------------+

4、输入命令进行YUV模式图像采集，可在液晶屏上预览采集的图像。

-d对应的设备文件名，-f图像模式为yuv，-s图像分辨率为800x448，-i帧速率为25fps

+--------------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_luvcivew -d /dev/video2 -f yuv -s 800x448 -i 25 //注意需要根据屏幕选择成像大小。   |
+--------------------------------------------------------------------------------------------------------------------+

打印信息如下：

+--------------------------------------------------------------------+
| luvcview version v0.1                                              |
|                                                                    |
| size width: 800 height: 448                                        |
|                                                                    |
| interval: 25 fps                                                   |
|                                                                    |
| starting process                                                   |
|                                                                    |
| video /dev/video2                                                  |
|                                                                    |
| get picture !                                                      |
|                                                                    |
| vinfo: xoffset:0 yoffset:0 bits\_per\_pixel:16 xres:800 yres:480   |
+--------------------------------------------------------------------+

5. 输入命令进行MJPEG模式图像采集，可在液晶屏上预览采集的图像，在该模式下，采集数据的同时
   也在进行录制，录制的文件名为xxx.avi，保存在执行命令的目录下，该视频文件使用常用播放器。

-d对应的设备文件名，-f图像模式为jpg，-s图像分辨率为800x448，-i帧速率为30fps

+-----------------------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_luvcivew -d /dev/video2 -f jpg -s 800x448 -i 30   |
+-----------------------------------------------------------------------------------+

打印信息如下：

+--------------------------------------------------------------------+
| luvcview version v0.1                                              |
|                                                                    |
| size width: 800 height: 448                                        |
|                                                                    |
| interval: 30 fps                                                   |
|                                                                    |
| video /dev/video2                                                  |
|                                                                    |
| format asked unavailable get width 640 height 480                  |
|                                                                    |
| vinfo: xoffset:0 yoffset:0 bits\_per\_pixel:32 xres:800 yres:480   |
|                                                                    |
| recording to video.avi                                             |
|                                                                    |
| find DRI                                                           |
|                                                                    |
| get picture !                                                      |
+--------------------------------------------------------------------+

3.1.3 有线网络测试
------------------

**3.1.3.1 IPV4基本命令测试**

-  **说明：**

**OKMX6ULL-S 有eth0、eth1两路网卡。**

**console版文件系统：开机默认eth0 IP为192.168.0.232，eth1
IP为192.168.1.232。如果修改IP请修改/etc/network/interfaces**\ 。

**qt版文件系统：开机默认开机默认eth0 IP为192.168.0.232，eth1
IP为192.168.2.232。如需修改为静态IP，可在/etc/autorun.sh文件中添加如下命令进行设置：ifconfig
eth0 192.168.10.232**

每个开发板的网络使用环境未必相同，本节测试示例中，网络环境如下。实际使用中，请按照实际网络环境自行进行配置。

+----------------+----------------+
| **底板丝印**   | **软件设备**   |
+----------------+----------------+
| NET1           | eth1           |
+----------------+----------------+
| NET2           | eth0           |
+----------------+----------------+

-  **注意：eth1与eth0不能用于同一个局域网。**

下面以eth0为例进行命令说明。

1. 在Linux系统下，使用ifconfig命令可以显示或配置网络设备，使用ethtool查询及设置网卡参数。

2. 设置IP地址 ，查看当前网卡详情：

+----------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 192.168.1.120 //设置ip              |
|                                                                      |
| root@fl-imx6ull:~# ifconfig eth0 //查看设置后网络状况                |
|                                                                      |
| eth0 Link encap:Ethernet HWaddr 3A:D9:93:8E:A8:A4                    |
|                                                                      |
| **inet addr:192.168.1.120** Bcast:192.168.1.255 Mask:255.255.255.0   |
|                                                                      |
| inet6 addr: fe80::38d9:93ff:fe8e:a8a4%2124311408/64 Scope:Link       |
|                                                                      |
| inet6 addr: fec0::38d9:93ff:fe8e:a8a4%2124311408/64 Scope:Site       |
|                                                                      |
| UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1                     |
|                                                                      |
| RX packets:28 errors:0 dropped:0 overruns:0 frame:0                  |
|                                                                      |
| TX packets:63 errors:0 dropped:0 overruns:0 carrier:0                |
|                                                                      |
| collisions:0 txqueuelen:1000                                         |
|                                                                      |
| RX bytes:11550 (11.2 KiB) TX bytes:11579 (11.3 KiB)                  |
+----------------------------------------------------------------------+

inet addr:192.168.1.120可以看出ip设置成功

1. 动态分配IP地址

如果您的开发板与路由器连接，且路由器支持DHCP自动IP地址分配可以在超级终端里面输入命令：

+-------------------------------------------------------+
| root@fl-imx6ull:~# udhcpc -i eth0                     |
|                                                       |
| udhcpc (v1.24.1) started                              |
|                                                       |
| Sending discover...                                   |
|                                                       |
| Sending select for 192.168.20.101...                  |
|                                                       |
| Lease of 192.168.20.101 obtained, lease time 86400    |
|                                                       |
| /etc/udhcpc.d/50default: Adding DNS 222.222.222.222   |
+-------------------------------------------------------+

用来动态获取IP地址，“-i”
参数用来指定网卡名称，飞凌开发板有线网络的网卡名称为eth0。

/etc/resolv.conf文件中有dns服务器信息会被自动添加。

1. 修改mac地址：

+--------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 hw ether 00:00:00:00:00:01        |
|                                                                    |
| root@fl-imx6ull:~# ifconfig eth0                                   |
|                                                                    |
| eth0 Link encap:Ethernet **HWaddr 00:00:00:00:00:01**              |
|                                                                    |
| inet addr:192.168.20.101 Bcast:192.168.20.255 Mask:255.255.255.0   |
|                                                                    |
| inet6 addr: fec0::38d9:93ff:fe8e:a8a4%2128292720/64 Scope:Site     |
|                                                                    |
| inet6 addr: fec0::200:ff:fe00:1%2128292720/64 Scope:Site           |
|                                                                    |
| UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1                   |
|                                                                    |
| RX packets:85 errors:0 dropped:0 overruns:0 frame:0                |
|                                                                    |
| TX packets:118 errors:0 dropped:0 overruns:0 carrier:0             |
|                                                                    |
| collisions:0 txqueuelen:1000                                       |
|                                                                    |
| RX bytes:22942 (22.4 KiB) TX bytes:22259 (21.7 KiB)                |
+--------------------------------------------------------------------+

1. 设置子网掩码：

+--------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 netmask 255.255.255.0 //设置eth0子网掩码为255.255.255.0   |
|                                                                                            |
| root@fl-imx6ull:~# ifconfig eth0                                                           |
|                                                                                            |
| eth0 Link encap:Ethernet HWaddr 00:00:00:00:00:01                                          |
|                                                                                            |
| inet addr:192.168.20.101 Bcast:192.168.20.255 **Mask:255.255.255.0**                       |
|                                                                                            |
| inet6 addr: fec0::38d9:93ff:fe8e:a8a4%2128915312/64 Scope:Site                             |
|                                                                                            |
| inet6 addr: fec0::200:ff:fe00:1%2128915312/64 Scope:Site                                   |
|                                                                                            |
| UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1                                           |
|                                                                                            |
| RX packets:107 errors:0 dropped:0 overruns:0 frame:0                                       |
|                                                                                            |
| TX packets:118 errors:0 dropped:0 overruns:0 carrier:0                                     |
|                                                                                            |
| collisions:0 txqueuelen:1000                                                               |
|                                                                                            |
| RX bytes:25700 (25.0 KiB) TX bytes:22259 (21.7 KiB)                                        |
+--------------------------------------------------------------------------------------------+

1. 设置广播地址

+-------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 broadcast 192.168.1.255//eth0广播地址设为192.168.1.255   |
|                                                                                           |
| root@fl-imx6ull:~# ifconfig eth0                                                          |
+-------------------------------------------------------------------------------------------+

打印信息如下：

+-----------------------------------------------------------------------+
| eth0 Link encap:Ethernet HWaddr 00:00:00:00:00:01                     |
|                                                                       |
| inet addr:192.168.20.101 **Bcast:192.168.1.255** Mask:255.255.255.0   |
|                                                                       |
| inet6 addr: fec0::38d9:93ff:fe8e:a8a4%2123332464/64 Scope:Site        |
|                                                                       |
| inet6 addr: fec0::200:ff:fe00:1%2123332464/64 Scope:Site              |
|                                                                       |
| UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1                      |
|                                                                       |
| RX packets:111 errors:0 dropped:0 overruns:0 frame:0                  |
|                                                                       |
| TX packets:132 errors:0 dropped:0 overruns:0 carrier:0                |
|                                                                       |
| collisions:0 txqueuelen:1000                                          |
|                                                                       |
| RX bytes:26130 (25.5 KiB) TX bytes:25947 (25.3 KiB)                   |
+-----------------------------------------------------------------------+

Bcast:192.168.1.255可以看出广播地址设置成功

1. 添加/删除默认网关

添加默认网关：

+--------------------------------------------------------+
| root@fl-imx6ull:~# route add default gw 192.168.20.1   |
+--------------------------------------------------------+

删除默认网关：

+--------------------------------------------------------+
| root@fl-imx6ull:~# route del default gw 192.168.20.1   |
+--------------------------------------------------------+

1. 关闭开启网卡

关闭eth0网卡：

+-----------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 down   |
+-----------------------------------------+

开启eth0网卡：

+------------------------------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 up                                                                                                |
|                                                                                                                                    |
| fec 20b4000.ethernet eth0: Freescale FEC PHY driver [Micrel KSZ8081 or KSZ8091] (mii\_bus:phy\_addr=20b4000.ethernet:01, irq=-1)   |
|                                                                                                                                    |
| root@fl-imx6ull:~# fec 20b4000.ethernet eth0: Link is Up - 100Mbps/Full - flow control rx/tx                                       |
+------------------------------------------------------------------------------------------------------------------------------------+

**3.1.3.2 IPV6测试**

1、以eth1为例设置IPV6地址

+---------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ip -6 addr add 2001:250:4000:2000::50/64 dev eth1 //设置IPV6地址   |
+---------------------------------------------------------------------------------------+

2、配置电脑 ipv6 地址

打开控制面板->网络和Internet->更改适配器选项出现如下界面：

|image4|

选择以太网，右击选择属性。

|image5|

将 ipv4 关掉，并打开
ipv6，双击“Internet协议版本6（TCP/IPV6）”，修改下图：

|image6|

将开发板和电脑用网线直连，使用ping6命令测试如下：

+---------------------------------------------------------------------------+
| root@fl-imx6ull:~# ping6 2001:250:4000:2000::49                           |
|                                                                           |
| PING 2001:250:4000:2000::49(2001:250:4000:2000::49) 56 data bytes         |
|                                                                           |
| 64 bytes from 2001:250:4000:2000::49: icmp\_seq=1 ttl=128 time=1.43 ms    |
|                                                                           |
| 64 bytes from 2001:250:4000:2000::49: icmp\_seq=2 ttl=128 time=0.399 ms   |
|                                                                           |
| 64 bytes from 2001:250:4000:2000::49: icmp\_seq=3 ttl=128 time=0.501 ms   |
|                                                                           |
| ^C                                                                        |
|                                                                           |
| --- 2001:250:4000:2000::49 ping statistics ---                            |
|                                                                           |
| 5 packets transmitted, 5 received, 0% packet loss, time 4006ms            |
|                                                                           |
| rtt min/avg/max/mdev = 0.399/0.640/1.432/0.398 ms                         |
+---------------------------------------------------------------------------+

**3.1.3.3 USB转网络测试**

|image7|

1、将USB转以太网插入USB host接口，识别信息如下：

+-----------------------------------------------------------------------------------------------------------------+
| usb 1-1.3: new high-speed USB device number 8 using ci\_hdrc                                                    |
|                                                                                                                 |
| asix 1-1.3:1.0 eth2: register 'asix' at usb-ci\_hdrc.1-1.3, ASIX AX88772B USB 2.0 Ethernet, 00:0e:c6:8f:9c:b7   |
|                                                                                                                 |
| IPv6: ADDRCONF(NETDEV\_UP): eth2: link is not ready                                                             |
+-----------------------------------------------------------------------------------------------------------------+

2、测试方法参考\ `*IPV4基本命令测试* <#OLE_LINK2>`__\ 。

3.1.4 以太网相关服务
--------------------

**3.1.4.1 FTP 服务**

-  **说明：**

-  **账户root，默认无密码**

-  **eth0网卡默认IP：192.168.0.232**

测试前需保证开发板和电脑网络连接正常，参考“\ `*网络连接测试* <#有线网络测试>`__\ ”，本章节不再赘述。

1. 设置root用户密码，这里设置为forlinx

+-------------------------------------------------------------------------+
| root@imx6ullevk:~# passwd root                                          |
|                                                                         |
| Changing password for root                                              |
|                                                                         |
| Enter the new password (minimum of 5 characters)                        |
|                                                                         |
| Please use a combination of upper and lower case letters and numbers.   |
|                                                                         |
| New password:                                                           |
|                                                                         |
| Bad password: too simple.                                               |
|                                                                         |
| Warning: weak password (enter it again to use it anyway).               |
|                                                                         |
| New password:                                                           |
|                                                                         |
| Re-enter new password:                                                  |
|                                                                         |
| **passwd: password changed.**                                           |
+-------------------------------------------------------------------------+

2、电脑使用FileZilla登录开发板

创建“新站点”，主机输入开发板IP，加密方式选择只是用明文FTP，登陆类型选择正常，用户和密码为开发板的用户和密码，点击“连接”。

|image8|

|image9|

**3.1.3.3 SSH 客户端测试**

-  **说明：**

**console版文件系统：开机默认eth0 IP为192.168.10.232，eth1
IP为192.168.1.232。**

**qt版文件系统：eth0 IP为192.168.10.232，eth1 IP为192.168.2.232。**

文件系统中移植dropbear是一个相对较小的SSH服务器和客户端，本节介绍开发板作为SSH客户端，对装有SSH服务器的Linux
主机进行访问，Linux主机搭建SSH服务的方法可以参考应用笔记中相关文件，本次使用ubuntu开发环境作为Linux主机。

配置信息：

开发板ip设置为：192.168.0.232

Linux 主机ip地址：192.168.0.149
账户名为\ **forlinx**\ ，主机名为\ **ubuntu**

由开发板访问linux主机

+---------------------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# dbclient `*forlinx@192.168.0.149* <mailto:forlinx@192.168.0.149>`__ //192.168.0.149为linux主机IP地址   |
|                                                                                                                           |
| forlinx为linux主机用户名                                                                                                  |
|                                                                                                                           |
| Host '192.168.0.149' is not in the trusted hosts file.                                                                    |
|                                                                                                                           |
| (ecdsa-sha2-nistp256 fingerprint md5 93:ff:74:8a:ed:ba:fd:21:39:d9:87:93:ad:9e:19:6f)                                     |
|                                                                                                                           |
| Do you want to continue connecting? (y/n) y                                                                               |
|                                                                                                                           |
| forlinx@192.168.0.149's password:                                                                                         |
|                                                                                                                           |
| Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-31-generic x86\_64)                                                        |
|                                                                                                                           |
| \* Documentation: https://help.ubuntu.com/                                                                                |
|                                                                                                                           |
| 504 packages can be updated.                                                                                              |
|                                                                                                                           |
| 421 updates are security updates.                                                                                         |
|                                                                                                                           |
| New release '16.04.6 LTS' available.                                                                                      |
|                                                                                                                           |
| Run 'do-release-upgrade' to upgrade to it.                                                                                |
|                                                                                                                           |
| Last login: Mon Mar 23 12:50:22 2020 from 192.168.0.232                                                                   |
+---------------------------------------------------------------------------------------------------------------------------+

3.1.5 无线网络测试
------------------

**3.1.5.1 WIFI 测试**

WiFi支持：

+--------------+--------+
| 模块         | 支持   |
+--------------+--------+
| RTL8188EUS   | WiFi   |
+--------------+--------+
| RTL8723BU    | WiFi   |
+--------------+--------+
| RTL8723DU    | WiFi   |
+--------------+--------+

\ **3.1.5.1.1 USB WIFI RTL8188eus使用**

-  **说明：USB
   WIFI无线局域网卡是选配模块，如若有需求，请联系飞凌嵌入式销售人员。**

以下对wifi模块在STA模式下，连接到无线网络的测试：

**步骤1：**\ 开发板上电，启动Linux系统。

**步骤2：**\ 连接USB WIFI到飞凌开发板的USB host接口，正确安装如下图。

|image10|

**步骤3：**\ 按照如下格式输入相应的参数：

-i表示wifi型号；

-s表示wifi热点名称；

-p表示密码，若无密码输入-p NONE；

路由器采用wpa加密方式。具体操作指令可查看wifi.sh脚本。

连接打印内容如下：

+----------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_wifi.sh -i 8188 -s forlinx -p xxxx   |
+----------------------------------------------------------------------+

打印信息如下：

+--------------------------------------------------------------------------------------------------------------------------------------------+
| wifi 8188                                                                                                                                  |
|                                                                                                                                            |
| ssid forlinx                                                                                                                               |
|                                                                                                                                            |
| pasw xxxx                                                                                                                                  |
|                                                                                                                                            |
| usbcore: deregistering interface driver rtl8723bu                                                                                          |
|                                                                                                                                            |
| RTL871X: module exit start                                                                                                                 |
|                                                                                                                                            |
| usbcore: deregistering interface driver rtl8188eu                                                                                          |
|                                                                                                                                            |
| RTL871X: rtw\_ndev\_uninit(wlan1)                                                                                                          |
|                                                                                                                                            |
| usb 1-1.3: reset high-speed USB device number 7 using ci\_hdrc                                                                             |
|                                                                                                                                            |
| RTL871X: module exit success                                                                                                               |
|                                                                                                                                            |
| RTL871X: module init start                                                                                                                 |
|                                                                                                                                            |
| RTL871X: rtl8188eu v4.3.0.9\_15178.20150907                                                                                                |
|                                                                                                                                            |
| RTL871X: build time: Mar 25 2020 02:23:46                                                                                                  |
|                                                                                                                                            |
| bFWReady == \_FALSE call reset 8051...                                                                                                     |
|                                                                                                                                            |
| RTL871X: rtw\_ndev\_init(wlan0)                                                                                                            |
|                                                                                                                                            |
| usbcore: registered new interface driver rtl8188eu                                                                                         |
|                                                                                                                                            |
| RTL871X: module init ret=0                                                                                                                 |
|                                                                                                                                            |
| ==> rtl8188e\_iol\_efuse\_patch                                                                                                            |
|                                                                                                                                            |
| IPv6: ADDRCONF(NETDEV\_UP): wlan0: link is not ready                                                                                       |
|                                                                                                                                            |
| ps: invalid option -- 'f'                                                                                                                  |
|                                                                                                                                            |
| BusyBox v1.24.1 (2019-04-27 02:24:01 CST) multi-call binary.                                                                               |
|                                                                                                                                            |
| Usage: ps                                                                                                                                  |
|                                                                                                                                            |
| Successfully initialized wpa\_supplicant                                                                                                   |
|                                                                                                                                            |
| rfkill: Cannot open RFKILL controRTL871X: set bssid:00:00:00:00:00:00                                                                      |
|                                                                                                                                            |
| l device                                                                                                                                   |
|                                                                                                                                            |
| RTL871X: set ssid [g▒isQ▒J▒)ͺ▒▒▒▒F\|▒T▒▒vZ.c3▒ɚ𞌌<▒▒▒▒] fw\_state=0x00000008                                                                |
|                                                                                                                                            |
| ioctl[SIOCSIWAP]: Operation not permitted                                                                                                  |
|                                                                                                                                            |
| ioctl[SIOCGIWSCAN]: Resource temporarily unavailable                                                                                       |
|                                                                                                                                            |
| ioctl[SIOCGIWSCAN]: Resource temporarily unavailable                                                                                       |
|                                                                                                                                            |
| RTL871X: indicate disassoc                                                                                                                 |
|                                                                                                                                            |
| wlan0: Trying to associate with 04:d7:a5:84:fa:40 (SSID='forlinx' freq=2437 MHz)                                                           |
|                                                                                                                                            |
| RTL871X: set ssid [forlinx] fw\_state=0x00000008                                                                                           |
|                                                                                                                                            |
| RTL871X: set bssid:04:d7:a5:84:fa:40                                                                                                       |
|                                                                                                                                            |
| RTL871X: start auth                                                                                                                        |
|                                                                                                                                            |
| RTL871X: auth success, start assoc                                                                                                         |
|                                                                                                                                            |
| RTL871X: assoc success                                                                                                                     |
|                                                                                                                                            |
| IPv6: ADDRCONF(NETDEV\_CHANGE): wlan0: link becomes ready                                                                                  |
|                                                                                                                                            |
| RTL871X: recv eapol packet                                                                                                                 |
|                                                                                                                                            |
| wlan0: Associated with 04:d7:a5:84:fa:40                                                                                                   |
|                                                                                                                                            |
| RTL871X: send eapol packet                                                                                                                 |
|                                                                                                                                            |
| RsvdPageNum: 8                                                                                                                             |
|                                                                                                                                            |
| udhcpc (v1.24.1) started                                                                                                                   |
|                                                                                                                                            |
| RTL871X: recv eapol packet                                                                                                                 |
|                                                                                                                                            |
| RTL871X: send eapol packet                                                                                                                 |
|                                                                                                                                            |
| RTL871X: recv eapol packet                                                                                                                 |
|                                                                                                                                            |
| RTL871X: send eapol packet                                                                                                                 |
|                                                                                                                                            |
| RTL871X: set pairwise key camid:4, addr:04:d7:a5:84:fa:40, kid:0, type:AES                                                                 |
|                                                                                                                                            |
| wlan0: WPA: Key negotiation completed with 04:d7:a5:84:fa:40 [PTKRTL871X: set group key camid:5, addr:04:d7:a5:84:fa:40, kid:2, type:AES   |
|                                                                                                                                            |
| =CCMP GTK=CCMP]                                                                                                                            |
|                                                                                                                                            |
| wlan0: CTRL-EVENT-CONNECTED - Connection to 04:d7:a5:84:fa:40 completed [id=0 id\_str=]                                                    |
|                                                                                                                                            |
| Sending discover...                                                                                                                        |
|                                                                                                                                            |
| Sending select for 192.168.4.129...                                                                                                        |
|                                                                                                                                            |
| Lease of 192.168.4.129 obtained, lease time 36000                                                                                          |
|                                                                                                                                            |
| **/etc/udhcpc.d/50default: Adding DNS 222.222.202.202**                                                                                    |
|                                                                                                                                            |
| **/etc/udhcpc.d/50default: Adding DNS 222.222.222.222**                                                                                    |
|                                                                                                                                            |
| Finshed!                                                                                                                                   |
+--------------------------------------------------------------------------------------------------------------------------------------------+

脚本运行完，能自动分配ip并添加DNS，则wifi连接成功。

**步骤5：**\ ping ip或者域名，命令如下。

+-------------------------------------------------------------+
| root@fl-imx6ull:~# ping -c 4 www.baidu.com                  |
|                                                             |
| PING www.baidu.com (220.181.38.149): 56 data bytes          |
|                                                             |
| 64 bytes from 220.181.38.149: seq=0 ttl=51 time=26.648 ms   |
|                                                             |
| 64 bytes from 220.181.38.149: seq=1 ttl=51 time=13.529 ms   |
|                                                             |
| 64 bytes from 220.181.38.149: seq=2 ttl=51 time=15.656 ms   |
|                                                             |
| 64 bytes from 220.181.38.149: seq=3 ttl=51 time=26.249 ms   |
|                                                             |
| --- www.baidu.com ping statistics ---                       |
|                                                             |
| 4 packets transmitted, 4 packets received, 0% packet loss   |
|                                                             |
| round-trip min/avg/max = 13.529/20.520/26.648 ms            |
+-------------------------------------------------------------+

**步骤6：**\ 卸载已经加入内核的模块。

+-----------------------------------+
| root@fl-imx6ull:~# rmmod 8188eu   |
+-----------------------------------+

打印信息如下：

+----------------------------------------------------------------------------------------+
| RTL871X: module exit start                                                             |
|                                                                                        |
| usbcore: deregistering interface driver rtl8188eu                                      |
|                                                                                        |
| RTL871X: indicate disassoc                                                             |
|                                                                                        |
| RTL871X: rtw\_cmd\_thread: DriverStopped(1) SurpriseRemoved(0) break at line 478       |
|                                                                                        |
| wlan0: CTRL-EVENT-DISCONNECTED bssid=04:d7:a5:84:fa:40 reason=3 locally\_generated=1   |
|                                                                                        |
| RTL871X: rtw\_ndev\_uninit(wlan0)                                                      |
|                                                                                        |
| RTL871X: rtw\_dev\_unload: driver not in IPS                                           |
|                                                                                        |
| usb 1-1.3: reset high-speed USB device number 7 using ci\_hdrc                         |
|                                                                                        |
| RTL871X: module exit success                                                           |
+----------------------------------------------------------------------------------------+

\ **3.1.5.1.2 板载WIFI的使用**

-  **说明：**

-  **wifi频率为2.4G**

-  **兼容8723bu和8723du两种wifi驱动**

-  **默认路由器采用wpa加密方式。**

如果开发板有板载的WIFI无线局域网卡，则焊接在评估板如图所示位置：

|Snipaste\_2020-06-04\_13-42-23|

**步骤1：**\ 检查开发板是否已经焊接该模块，正确焊接如上图。连接上天线。

**步骤2：**\ 开发板上电，启动Linux系统,先使用lsmod查看模块加载状态：

+-------------------------------------------------------------+
| root@fl-imx6ull:~# lsmod                                    |
|                                                             |
| Module Size Used by                                         |
|                                                             |
| mx6s\_capture 14876 0                                       |
|                                                             |
| **8723du 1313893 0** //默认wifi自动加载，8723du已加载成功   |
|                                                             |
| ov9650\_camera 12446 0                                      |
+-------------------------------------------------------------+

-  **注意：若开发板上焊接的是8723du，使用lsmod会显示8723du**

以下以8723du为例进行测试描述：

**步骤3：测试**

-  **STA模式**

该模式即作为一个站点，连接到无线网络中，操作方法如下：

-i表示wifi型号；-s表示wifi热点名称；-p表示密码，若无密码输入-p
NONE；路由器采用wpa加密方式，具体操作指令可查看wifi.sh脚本

+--------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_wifi.sh -i 8723du -s forlinx -p xxx //执行测试脚本   |
+--------------------------------------------------------------------------------------+

打印信息如下：

+-----------------------------------------------------------------------------------------------------------------------------------------------+
| wifi 8723du                                                                                                                                   |
|                                                                                                                                               |
| ssid forlinx                                                                                                                                  |
|                                                                                                                                               |
| pasw xxx                                                                                                                                      |
|                                                                                                                                               |
| usbcore: deregistering interface driver rtl8723du                                                                                             |
|                                                                                                                                               |
| usbcore: registered new interface driver rtl8723du                                                                                            |
|                                                                                                                                               |
| IPv6: ADDRCONF(NETDEV\_UP): wlan0: link is not ready                                                                                          |
|                                                                                                                                               |
| Successfully initialized wpa\_supplicant                                                                                                      |
|                                                                                                                                               |
| rfkill: Cannot open RFKILL control device                                                                                                     |
|                                                                                                                                               |
| udhcpc (v1.24.1) started                                                                                                                      |
|                                                                                                                                               |
| Sending discover...                                                                                                                           |
|                                                                                                                                               |
| wlan0: CTRL-EVENT-REGDOM-CHANGE init=BEACON\_HINT type=UNKNOWN                                                                                |
|                                                                                                                                               |
| wlan0: Trying to associate with 04:d7:a5:f9:26:1d (SSID='forlinx' freq=2427 MHz)                                                              |
|                                                                                                                                               |
| wlan0: Associated with 04:d7:a5:f9:26:1d                                                                                                      |
|                                                                                                                                               |
| IPv6: ADDRCONF(NETDEV\_CHANGE): wlan0: link becomes ready                                                                                     |
|                                                                                                                                               |
| wlan0: WPA: Key negotiation completed with 04:d7:a5:f9:26:1d [PTK=CCMP GTK=TKIP]                                                              |
|                                                                                                                                               |
| wlan0: CTRL-EVENT-CONNECTED - Connection to 04:d7:a5:f9:26:1d completed [id=0 id\_str=]                                                       |
|                                                                                                                                               |
| nf\_conntrack: automatic helper assignment is deprecated and it will be removed soon. Use the iptables CT target to attach helpers instead.   |
|                                                                                                                                               |
| Sending discover...                                                                                                                           |
|                                                                                                                                               |
| Sending select for 192.168.5.186...                                                                                                           |
|                                                                                                                                               |
| Lease of 192.168.5.186 obtained, lease time 1800                                                                                              |
|                                                                                                                                               |
| /etc/udhcpc.d/50default: Adding DNS 222.222.202.202                                                                                           |
|                                                                                                                                               |
| /etc/udhcpc.d/50default: Adding DNS 222.222.222.222                                                                                           |
|                                                                                                                                               |
| WLAN Finshed!                                                                                                                                 |
+-----------------------------------------------------------------------------------------------------------------------------------------------+

脚本运行完，能自动分配ip并生成DNS，则wifi连接成功。

ping ip或者域名，命令如下：

+----------------------------------------------+
| root@fl-imx6ull:~# ping -c 5 www.baidu.com   |
+----------------------------------------------+

打印信息如下：

+-------------------------------------------------------------+
| PING 192.168.4.1 (192.168.4.1): 56 data bytes               |
|                                                             |
| 64 bytes from 192.168.4.1: seq=0 ttl=128 time=39.783 ms     |
|                                                             |
| 64 bytes from 192.168.4.1: seq=1 ttl=128 time=81.529 ms     |
|                                                             |
| 64 bytes from 192.168.4.1: seq=2 ttl=128 time=15.236 ms     |
|                                                             |
| 64 bytes from 192.168.4.1: seq=3 ttl=128 time=12.076 ms     |
|                                                             |
| 64 bytes from 192.168.4.1: seq=4 ttl=128 time=16.300 ms     |
|                                                             |
| --- 192.168.4.1 ping statistics ---                         |
|                                                             |
| 5 packets transmitted, 5 packets received, 0% packet loss   |
|                                                             |
| round-trip min/avg/max = 12.076/32.984/81.529 ms            |
+-------------------------------------------------------------+

-  **wifi信号**

查看WiFi信号方法如下：

+------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /proc/net/wireless \| grep wlan0 \| awk '{print $3}' //获取信号强度           |
|                                                                                                      |
| 78.                                                                                                  |
|                                                                                                      |
| root@fl-imx6ull:~# cat /proc/net/wireless \| grep wlan0 \| awk '{print $4}' //获取信号质量,单位dBm   |
|                                                                                                      |
| -68.                                                                                                 |
|                                                                                                      |
| root@fl-imx6ull:~# cat /proc/net/wireless \| grep wlan0 \| awk '{print $5}' //网口背景噪声,单位dBm   |
|                                                                                                      |
| -256.                                                                                                |
+------------------------------------------------------------------------------------------------------+

-  **AP模式**

-  **说明：**

-  **本模块支持AP模式，理论最大连接用户为8个。**

-  **本例为以太网eth0连接路由器说明，配置完以太网后，需要测试eth0是否可以连接外网，如果可以连接外网（方法参考“有线网卡”章节），请继续按照操作步骤执行，如果不可以请检查以太网或者路由器连接是否正常**\ 。

工作在AP模式下，手机等设备可以直接连接模块。

设置以太网IP，配置网络防火墙：

+-----------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# udhcpc -i eth0 //自动分配IP，若以测试eth0网络正常，可不操作此步      |
|                                                                                         |
| root@fl-imx6ull:~# echo 1 > /proc/sys/net/ipv4/ip\_forward //打开 IP 转发               |
|                                                                                         |
| root@fl-imx6ull:~# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE//设置转发规则   |
+-----------------------------------------------------------------------------------------+

设置WiFi的模式与IP

确保模块8723bu已经加载。

+-------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig wlan0 up //打开WiFi                                           |
|                                                                                           |
| root@fl-imx6ull:~# ifconfig wlan0 192.168.0.10 netmask 255.255.255.0 //设置IP与子网掩码   |
|                                                                                           |
| root@fl-imx6ull:~# ifconfig wlan0 promisc //设置 wlan0 为混杂模式                         |
+-------------------------------------------------------------------------------------------+

开启AP

+--------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# udhcpd /etc/udhcpd/udhcpd.conf & //WiFi 地址、网关等配置信息            |
|                                                                                            |
| root@fl-imx6ull:~# hostapd -d /etc/hostapd/hostapd.conf & //加密方式、用户名、密码等设置   |
+--------------------------------------------------------------------------------------------+

hostapd.conf文件中：ssid为用户名，/为密码

手机等移动终端可以通过WiFi连接到开发板的AP热点，开发板默认使用以下用户名和密码：

热点名：forlinxtest密码：1234567890

**步骤4：**\ 卸载已经加入内核的模块：

+-------------------------------------------------------------------+
| root@fl-imx6ull:~# rmmod 8723du                                   |
|                                                                   |
| usbcore: deregistering interface driver rtl8723du                 |
|                                                                   |
| wlan0: CTRL-EVENT-DISCONNECTED bssid=04:d7:a5:f9:26:1d reason=0   |
+-------------------------------------------------------------------+

**3.1.5.2 4G模块实现 IE上网**

-  **说明：**

-  **目前OKMX6ULL-S核心板支持华为ME909和移远EC20的4G模块。**

-  **USB 4G扩展板是选配模块，如若有需求，请联系飞凌嵌入式销售人员。**

**3.1.5.2.1 华为ME909S4G测试**

1、 将外扩USB 4G扩展板插入USB口，固定ME909s-821 PCIE
封装4G模块，安装ipex天线座，插入SIM卡，开机上电。

2、可通过lsusb指令查看USB状态

+-----------------------------------------------------------+
| root@fl-imx6ull:~# lsusb                                  |
|                                                           |
| Bus 001 Device 004: ID 0bda:b720                          |
|                                                           |
| **Bus 001 Device 006: ID 12d1:15c1 //ME909S的VID和PID**   |
|                                                           |
| Bus 001 Device 002: ID 0424:2514                          |
|                                                           |
| Bus 001 Device 001: ID 1d6b:0002                          |
+-----------------------------------------------------------+

/dev下查看设备节点状态

+--------------------------------------------------------------------+
| root@fl-imx6ull:~# ls /dev/ttyUSB\*                                |
|                                                                    |
| /dev/ttyUSB0 /dev/ttyUSB1 /dev/ttyUSB2 /dev/ttyUSB3 /dev/ttyUSB4   |
+--------------------------------------------------------------------+

3、设备识别成功后，可进行拨号上网测试

+--------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_me909.sh   |
+--------------------------------------------+

打印信息如下：

+------------------------------------------------------------------------------------------------------------------------------------+
| eth0 Link encap:Ethernet HWaddr 7E:09:81:93:F3:0A                                                                                  |
|                                                                                                                                    |
| eth1 Link encap:Ethernet HWaddr DE:EA:3F:6C:A7:33                                                                                  |
|                                                                                                                                    |
| wlan0 Link encap:Ethernet HWaddr 7C:A7:B0:0F:FC:14                                                                                 |
|                                                                                                                                    |
| udhcpc (v1.24.1) started                                                                                                           |
|                                                                                                                                    |
| Sending discover...                                                                                                                |
|                                                                                                                                    |
| Sending select for 10.63.223.158...                                                                                                |
|                                                                                                                                    |
| Lease of 10.63.223.158 obtained, lease time 518400                                                                                 |
|                                                                                                                                    |
| /etc/udhcpc.d/50default: Adding DNS 222.222.222.222                                                                                |
|                                                                                                                                    |
| /etc/udhcpc.d/50default: Adding DNS 222.222.202.202                                                                                |
|                                                                                                                                    |
| fec 20b4000.ethernet eth0: Freescale FEC PHY driver [Micrel KSZ8081 or KSZ8091] (mii\_bus:phy\_addr=20b4000.ethernet:01, irq=-1)   |
|                                                                                                                                    |
| IPv6: ADDRCONF(NETDEV\_UP): eth0: link is not ready                                                                                |
|                                                                                                                                    |
| fec 2188000.ethernet eth1: Freescale FEC PHY driver [Micrel KSZ8081 or KSZ8091] (mii\_bus:phy\_addr=20b4000.ethernet:02, irq=-1)   |
|                                                                                                                                    |
| IPv6: ADDRCONF(NETDEV\_UP): eth1: link is not ready                                                                                |
|                                                                                                                                    |
| Finished!                                                                                                                          |
+------------------------------------------------------------------------------------------------------------------------------------+

4、连接成功之后，ping百度测试：

+-------------------------------------------------------------+
| root@fl-imx6ull:~# ping www.baidu.com                       |
|                                                             |
| PING www.baidu.com (220.181.38.150): 56 data bytes          |
|                                                             |
| 64 bytes from 220.181.38.150: seq=0 ttl=53 time=59.450 ms   |
|                                                             |
| 64 bytes from 220.181.38.150: seq=1 ttl=53 time=71.086 ms   |
|                                                             |
| 64 bytes from 220.181.38.150: seq=2 ttl=53 time=57.385 ms   |
|                                                             |
| 64 bytes from 220.181.38.150: seq=3 ttl=53 time=71.033 ms   |
|                                                             |
| --- www.baidu.com ping statistics ---                       |
|                                                             |
| 4 packets transmitted, 4 packets received, 0% packet loss   |
|                                                             |
| round-trip min/avg/max = 57.385/64.738/71.086 ms            |
+-------------------------------------------------------------+

5、断网与复位指令测试：

    断开网络连接：

+----------------------------------------------------------+
| root@fl-imx6ull:~# echo "AT^NDISDUP=1,0"> /dev/ttyUSB2   |
+----------------------------------------------------------+

    复位重启模块：

+-----------------------------------------------------+
| root@fl-imx6ull:~# echo "AT^RESET "> /dev/ttyUSB2   |
+-----------------------------------------------------+

6、更换SIM卡需要更换 fltest\_cmd\_me909.sh 中相应的APN：

中国移动：

+---------------------------------------------------+
| echo "AT^NDISDUP=1,1,\\"cmnet\\""> /dev/ttyUSB2   |
+---------------------------------------------------+

中国联通：

+---------------------------------------------------+
| echo "AT^NDISDUP=1,1,\\"3gnet\\""> /dev/ttyUSB2   |
+---------------------------------------------------+

中国电信（本文使用）：

\ **3.1.5.2.2 EC20模块测试**

|image12|

-  **说明：**

-  **使用物联网卡测试时，需确认模组固件版本，低版本固件不支持，需升级EC20固件**

-  **拨号脚本quectel-CM 在/usr/bin目录下**

-  **有些物联网卡拨号时需要设置专用账号和密码，用户需根据实际情况调整指令**

-  **使用quectel-CM --help指令查看相关参数含义**

1、连接好模块，开发板和模块上电后，可通过lsusb指令查看USB状态

+-------------------------------------------------------------+
| root@imx6ulevk:~# lsusb                                     |
|                                                             |
| Bus 001 Device 004: ID 0bda:b720                            |
|                                                             |
| **Bus 001 Device 005: ID 2c7c:0125** **//EC20的VID和PID**   |
|                                                             |
| Bus 001 Device 002: ID 0424:2514                            |
|                                                             |
| Bus 001 Device 001: ID 1d6b:0002                            |
+-------------------------------------------------------------+

2、/dev下查看设备节点状态

+-------------------------------------------------------+
| root@imx6ulevk:~# ls /dev/ttyUSB\*                    |
|                                                       |
| /dev/ttyUSB0 /dev/ttyUSB1 /dev/ttyUSB2 /dev/ttyUSB3   |
+-------------------------------------------------------+

3、EC20拨号:

+------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 down                                                                      |
|                                                                                                            |
| root@fl-imx6ull:~# ifconfig eth1 down                                                                      |
|                                                                                                            |
| root@fl-imx6ull:~# quectel-CM &                                                                            |
|                                                                                                            |
| [1] 598                                                                                                    |
|                                                                                                            |
| root@fl-imx6ull:/forlinx/cmdbin#[04-26\_19:16:06:781] WCDMA&LTE\_QConnectManager\_Linux&Android\_V1.1.34   |
|                                                                                                            |
| [04-26\_19:16:06:783] ./quectel-CM profile[1] = (null)/(null)/(null)/0, pincode = (null)                   |
|                                                                                                            |
| [04-26\_19:16:06:790] Find /sys/bus/usb/devices/1-1.1 idVendor=2c7c idProduct=0125                         |
|                                                                                                            |
| [04-26\_19:16:06:791] Find /sys/bus/usb/devices/1-1.1:1.4/net/eth2                                         |
|                                                                                                            |
| [04-26\_19:16:06:791] Find usbnet\_adapter = eth2                                                          |
|                                                                                                            |
| [04-26\_19:16:06:792] Find /sys/bus/usb/devices/1-1.1:1.4/GobiQMI/qcqmi2                                   |
|                                                                                                            |
| [04-26\_19:16:06:792] Find qmichannel = /dev/qcqmi2                                                        |
|                                                                                                            |
| [04-26\_19:16:06:851] Get clientWDS = 7                                                                    |
|                                                                                                            |
| [04-26\_19:16:06:882] Get clientDMS = 8                                                                    |
|                                                                                                            |
| [04-26\_19:16:06:914] Get clientNAS = 9                                                                    |
|                                                                                                            |
| [04-26\_19:16:06:946] Get clientUIM = 10                                                                   |
|                                                                                                            |
| [04-26\_19:16:06:978] Get clientWDA = 11                                                                   |
|                                                                                                            |
| [04-26\_19:16:07:011] requestBaseBandVersion EC20CEHCLGR06A0\ **5M1G**                                     |
|                                                                                                            |
| **//打印信息中的版本号为5Mxx支持物联网卡，如果为2Mxx则不支持**                                             |
|                                                                                                            |
| [04-26\_19:16:07:106] requestGetSIMStatus SIMStatus: SIM\_READY                                            |
|                                                                                                            |
| [04-26\_19:16:07:138] requestGetProfile[1] ctnet///0                                                       |
|                                                                                                            |
| [04-26\_19:16:07:171] requestRegistrationState2 MCC: 460, MNC: 11, PS: Attached, DataCap: LTE              |
|                                                                                                            |
| [04-26\_19:16:07:202] requestQueryDataCall IPv4ConnectionStatus: DISCONNECTED                              |
|                                                                                                            |
| [04-26\_19:16:07:266] requestRegistrationState2 MCC: 460, MNC: 11, PS: Attached, DataCap: LTE              |
|                                                                                                            |
| [04-26\_19:16:07:300] requestSetupDataCall WdsConnectionIPv4Handle: 0xe1645ec0                             |
|                                                                                                            |
| [04-26\_19:16:07:394] requestQueryDataCall IPv4ConnectionStatus: CONNECTED                                 |
|                                                                                                            |
| [04-26\_19:16:07:427] ifconfig eth2 up                                                                     |
|                                                                                                            |
| [04-26\_19:16:07:471] busybox udhcpc -f -n -q -t 5 -i eth2                                                 |
|                                                                                                            |
| [04-26\_19:16:07:506] udhcpc (v1.24.1) started                                                             |
|                                                                                                            |
| [04-26\_19:16:07:631] Sending discover...                                                                  |
|                                                                                                            |
| [04-26\_19:16:07:691] Sending select for 172.29.86.131...                                                  |
|                                                                                                            |
| [04-26\_19:16:07:751] Lease of 172.29.86.131 obtained, lease time 7200                                     |
|                                                                                                            |
| [04-26\_19:16:07:869] /etc/udhcpc.d/50default: Adding DNS 222.222.222.222                                  |
|                                                                                                            |
| [04-26\_19:16:07:869] /etc/udhcpc.d/50default: Adding DNS 222.222.202.202                                  |
+------------------------------------------------------------------------------------------------------------+

    连接成功之后，ping 百度测试：

+--------------------------------------------------------------+
| root@fl-imx6ull:~# ping www.baidu.com -I eth2 -c 3           |
|                                                              |
| PING www.baidu.com (220.181.38.150): 56 data bytes           |
|                                                              |
| 64 bytes from 220.181.38.150: seq=0 ttl=53 time=137.243 ms   |
|                                                              |
| 64 bytes from 220.181.38.150: seq=1 ttl=53 time=51.239 ms    |
|                                                              |
| 64 bytes from 220.181.38.150: seq=2 ttl=53 time=94.440 ms    |
|                                                              |
| --- www.baidu.com ping statistics ---                        |
|                                                              |
| 3 packets transmitted, 3 packets received, 0% packet loss    |
|                                                              |
| round-trip min/avg/max = 51.239/94.307/137.243 ms            |
+--------------------------------------------------------------+

**3.1.5.3 GPRS模块测试**

-  **说明：GPRS 模块与开发板之间采用串口连接，用户可以使用飞凌公司自产的
   GPRS 模块，也可以使用自己购买的串口 GPRS
   模块。目前Nand版文件系统没有添加此功能，如果使用的是Nand版开发板，请忽略此功能测试。**

|image13|

GPRS 模块与开发板之间采用串口连接，客户可以使用飞凌公司自产的 GPRS
模块，也可以使用自己购买的串口 GPRS 模块。

1）在确保模块和开发板串口UART3连接、上电 OK 情况下，
启动开发板子，进入命令行终端输入如下命令。

+-----------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# pppd call gprs /dev/ttymxc2 &                                                    |
|                                                                                                     |
| [1] 638                                                                                             |
|                                                                                                     |
| timeout set to 15 seconds                                                                           |
|                                                                                                     |
| abort on (\\nDELAYED\\r)                                                                            |
|                                                                                                     |
| abort on (\\nBUSY\\r)                                                                               |
|                                                                                                     |
| abort on (\\nERROR\\r)                                                                              |
|                                                                                                     |
| abort on (\\nNO DIALTONE\\r)                                                                        |
|                                                                                                     |
| abort on (\\nNO CARRIER\\r)                                                                         |
|                                                                                                     |
| send (^MAT^M)                                                                                       |
|                                                                                                     |
| expect (OK)                                                                                         |
|                                                                                                     |
| AT^M^M                                                                                              |
|                                                                                                     |
| OK                                                                                                  |
|                                                                                                     |
| -- got it                                                                                           |
|                                                                                                     |
| send (ATS0=0^M)                                                                                     |
|                                                                                                     |
| expect (OK)                                                                                         |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| ATS0=0^M^M                                                                                          |
|                                                                                                     |
| OK                                                                                                  |
|                                                                                                     |
| -- got it                                                                                           |
|                                                                                                     |
| send (ATE0V1^M)                                                                                     |
|                                                                                                     |
| expect (OK)                                                                                         |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| ATE0V1^M^M                                                                                          |
|                                                                                                     |
| OK                                                                                                  |
|                                                                                                     |
| -- got it                                                                                           |
|                                                                                                     |
| send (AT+CGDCONT=1,"IP","CMNET"^M)                                                                  |
|                                                                                                     |
| expect (OK)                                                                                         |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| OK                                                                                                  |
|                                                                                                     |
| -- got it                                                                                           |
|                                                                                                     |
| send (ATDT\*99\*\*\*1#^M)                                                                           |
|                                                                                                     |
| expect (CONNECT)                                                                                    |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| ^M                                                                                                  |
|                                                                                                     |
| CONNECT                                                                                             |
|                                                                                                     |
| -- got it                                                                                           |
|                                                                                                     |
| send (^M)                                                                                           |
|                                                                                                     |
| Script /usr/sbin/chat -s -v -f /etc/ppp/peers/chat-gprs-connect finished (pid 639), status = 0x0    |
|                                                                                                     |
| Serial connection established.                                                                      |
|                                                                                                     |
| using channel 1                                                                                     |
|                                                                                                     |
| Using interface ppp0                                                                                |
|                                                                                                     |
| Connect: ppp0 <--> /dev/ttymxc2                                                                     |
|                                                                                                     |
| sent [LCP ConfReq id=0x1 <asyncmap 0x0> <magic 0x23b2996e> <pcomp> <accomp>]                        |
|                                                                                                     |
| rcvd [LCP ConfReq id=0x1 <asyncmap 0xa0000> <auth pap>]                                             |
|                                                                                                     |
| No auth is possible                                                                                 |
|                                                                                                     |
| sent [LCP ConfRej id=0x1 <auth pap>]                                                                |
|                                                                                                     |
| rcvd [LCP ConfRej id=0x1 <magic 0x23b2996e> <pcomp> <accomp>]                                       |
|                                                                                                     |
| sent [LCP ConfReq id=0x2 <asyncmap 0x0>]                                                            |
|                                                                                                     |
| rcvd [LCP ConfReq id=0x2 <asyncmap 0xa0000> <auth chap MD5>]                                        |
|                                                                                                     |
| No auth is possible                                                                                 |
|                                                                                                     |
| sent [LCP ConfRej id=0x2 <auth chap MD5>]                                                           |
|                                                                                                     |
| rcvd [LCP ConfAck id=0x2 <asyncmap 0x0>]                                                            |
|                                                                                                     |
| rcvd [LCP ConfReq id=0x3 <asyncmap 0xa0000>]                                                        |
|                                                                                                     |
| sent [LCP ConfAck id=0x3 <asyncmap 0xa0000>]                                                        |
|                                                                                                     |
| sent [IPCP ConfReq id=0x1 <compress VJ 0f 01> <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]   |
|                                                                                                     |
| sent [IPCP ConfReq id=0x1 <compress VJ 0f 01> <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]   |
|                                                                                                     |
| sent [IPCP ConfReq id=0x1 <compress VJ 0f 01> <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]   |
|                                                                                                     |
| sent [IPCP ConfReq id=0x1 <compress VJ 0f 01> <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]   |
|                                                                                                     |
| rcvd [IPCP ConfReq id=0x1 <addr 192.200.1.21>]                                                      |
|                                                                                                     |
| sent [IPCP ConfAck id=0x1 <addr 192.200.1.21>]                                                      |
|                                                                                                     |
| rcvd [IPCP ConfRej id=0x1 <compress VJ 0f 01>]                                                      |
|                                                                                                     |
| sent [IPCP ConfReq id=0x2 <addr 0.0.0.0> <ms-dns1 0.0.0.0> <ms-dns2 0.0.0.0>]                       |
|                                                                                                     |
| rcvd [IPCP ConfNak id=0x2 <addr 10.131.70.33> <ms-dns1 111.11.1.3> <ms-dns2 111.11.11.3>]           |
|                                                                                                     |
| sent [IPCP ConfReq id=0x3 <addr 10.131.70.33> <ms-dns1 111.11.1.3> <ms-dns2 111.11.11.3>]           |
|                                                                                                     |
| rcvd [IPCP ConfAck id=0x3 <addr 10.131.70.33> <ms-dns1 111.11.1.3> <ms-dns2 111.11.11.3>]           |
|                                                                                                     |
| not replacing existing default route via 192.168.1.1 with metric -1                                 |
|                                                                                                     |
| local IP address 10.131.70.33                                                                       |
|                                                                                                     |
| remote IP address 192.200.1.21                                                                      |
|                                                                                                     |
| primary DNS address 111.11.1.3                                                                      |
|                                                                                                     |
| secondary DNS address 111.11.11.3                                                                   |
+-----------------------------------------------------------------------------------------------------+

2）连接成功可以尝试ping一下\ `*www.baidu.com* <http://www.baidu.com>`__

+---------------------------------------------------------------+
| root@fl-imx6ull:~# ping www.baidu.com                         |
|                                                               |
| PING www.baidu.com (39.156.66.18): 56 data bytes              |
|                                                               |
| 64 bytes from 39.156.66.18: seq=0 ttl=51 time=315.605 ms      |
|                                                               |
| 64 bytes from 39.156.66.18: seq=1 ttl=51 time=732.351 ms      |
|                                                               |
| 64 bytes from 39.156.66.18: seq=2 ttl=51 time=234.874 ms      |
|                                                               |
| 64 bytes from 39.156.66.18: seq=3 ttl=51 time=2990.813 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=4 ttl=51 time=2110.219 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=5 ttl=51 time=1151.022 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=6 ttl=51 time=450.403 ms      |
|                                                               |
| 64 bytes from 39.156.66.18: seq=7 ttl=51 time=4650.867 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=8 ttl=51 time=3747.240 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=9 ttl=51 time=2866.635 ms     |
|                                                               |
| 64 bytes from 39.156.66.18: seq=10 ttl=51 time=1986.155 ms    |
|                                                               |
| 64 bytes from 39.156.66.18: seq=11 ttl=51 time=1087.178 ms    |
|                                                               |
| 64 bytes from 39.156.66.18: seq=12 ttl=51 time=368.113 ms     |
|                                                               |
| --- www.baidu.com ping statistics ---                         |
|                                                               |
| 13 packets transmitted, 13 packets received, 0% packet loss   |
|                                                               |
| round-trip min/avg/max = 234.874/1745.498/4650.867 ms         |
+---------------------------------------------------------------+

假如 ping
命令不通，可能是之前测试以太网或无线网络接口时的一些配置的影响，此时需要先执行以下命令再测试模块：

+------------------------------------------+
| root@fl-imx6ull:~# ifconfig eth0 down    |
|                                          |
| root@fl-imx6ull:~# ifconfig wlan0 down   |
+------------------------------------------+

3.1.6 看门狗测试
----------------

看门狗是嵌入式系统中经常用到的功能。目前uboot和kernel都支持看门狗，默认出厂看门狗是关闭状态，以下分别在这两个阶段进行看门狗测试。

-  **Uboot阶段的看门狗操作**

开发板上电之后按空格键出现uboot菜单，输入0进入uboot命令行进行操作。

Uboot阶段开门狗默认关闭

+---------------------------------------------------------------------------------------+
| => setenv fl\_wdt\_en "1" //打开看门狗                                                |
|                                                                                       |
| => setenv fl\_wdt\_timeout "60" //设置复位时间为60s，用户可以自行设定时间在10s~128s   |
|                                                                                       |
| => saveenv //保存环境变量                                                             |
|                                                                                       |
| Saving Environment to NAND...                                                         |
|                                                                                       |
| Erasing NAND...                                                                       |
|                                                                                       |
| Erasing at 0x600000 -- 100% complete.                                                 |
|                                                                                       |
| Writing to NAND... OK                                                                 |
+---------------------------------------------------------------------------------------+

关闭看门狗

+------------------------------------------+
| => setenv fl\_wdt\_en "0" //关闭看门狗   |
|                                          |
| => saveenv //保存环境变量                |
|                                          |
| Saving Environment to NAND...            |
|                                          |
| Erasing NAND...                          |
|                                          |
| Erasing at 0x600000 -- 100% complete.    |
|                                          |
| Writing to NAND... OK                    |
+------------------------------------------+

-  **进入系统以后的看门狗操作**

参数说明：

+--------------+----------------------------------------+------------------------+
| **参数**     | **含义**                               | **说明**               |
+--------------+----------------------------------------+------------------------+
| settimeout   | 开启看门狗，设置复位时间，不喂狗       | 复位时间设置需大于2s   |
+--------------+----------------------------------------+------------------------+
| keepalive    | 开启看门狗，设置复位时间，定时2s喂狗   | 复位时间设置需大于2s   |
+--------------+----------------------------------------+------------------------+

-  **注意：测试时以上两个参数只能使用一个，可通过ps查看进程，保证只有一个fltest\_wdt进程。**

1. 设置60s后超时重启。

+---------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_wdt /dev/watchdog settimeout 60 &   |
|                                                                     |
| [1] 582                                                             |
+---------------------------------------------------------------------+

说明：超时时间可自行修改，如果超时时间为10s，则将命令中的60更改为10。可设置最小复位时间间隔为2s。

1. 设置如果10s内系统不喂狗，则重启系统；wdttest命令设置参数keepalive后会每2s喂一次狗，如果删除wdttest后台则会造成系统在10s内不去喂狗而造成超时重启。

+-------------------------------------------------------------------+
| root@fl-imx6ul:~# fltest\_cmd\_wdt /dev/watchdog keepalive 10 &   |
|                                                                   |
| [1] 584                                                           |
|                                                                   |
| root@fl-imx6ull:~# kill -9 584 //进程号需要根据实际情况进行修改   |
|                                                                   |
| root@fl-imx6ul:~# watchdog watchdog0: watchdog did not stop!      |
+-------------------------------------------------------------------+

说明：允许最大喂狗周期可自行修改，如果时间为40s，则将命令中的10更改为40。

3.1.7 RTC时钟驱动测试
---------------------

-  **注意：确保板子上已经安装了纽扣电池，并且电池电压正常**

RTC 测试，主要通过使用 date 和 hwclock
工具设置软、硬件时间，测试当板子断电再上电的时候，软件时钟读取 RTC
时钟是否同步。

+-------------------------------------------------------------+
| root@fl-imx6ull:~# date -u 031912002020.00 //设置软件时间   |
|                                                             |
| Thu Mar 19 12:00:00 UTC 2020                                |
+-------------------------------------------------------------+
| root@fl-imx6ull:~# hwclock -r //显示硬件时间                |
|                                                             |
| Fri May 3 17:50:51 2019 0.000000 seconds                    |
+-------------------------------------------------------------+
| root@fl-imx6ull:~# hwclock -w //将软件时间同步到硬件时间    |
|                                                             |
| Fri May 3 17:50:51 2019 0.000000 seconds                    |
+-------------------------------------------------------------+

然后给板子断电再上电，进入系统后使用命令 date
读取系统时间，可以看到时间已经同步。

3.1.8 音/视频测试
-----------------

OKMX6ULL-S 硬件上采用的WM8960音频芯片或者
NAU88C22音频芯片，软件上使用主流音频框架ALSA（Advanced Linux Sound
Architechture），为应用层提供了alsa-lib，应用程序调用提供的API就可以完成对底层的操作。用户可以使用文件系统内带的
ALSA 音频录制、播放、配置工具进行测试。

注意：底板版本为OKMX6ULx-S V1.2，请参考3.1.8.1.1 章节VM8960音频芯片测试

底板版本为OKMX6ULx-S V3.0，请参考3.1.8.1.2 章节NAU88C22音频芯片测试

**3.1.8.1 Phone/MIC 测试**

开发板上提供了两个麦克风接口，一个是板载的麦克风MIC1，另一个是3.5mm标准立体声音频接口CON25。系统默认使用板载麦克，当CON25有外部麦克风设备插入时，板载麦克MIC1自动断开，使用外部麦克风设备录音。

**3.1.8.1.1 VM8960音频芯片测试**

    |image14|

    1.设置参数，输入下图中的命令：

+---------------------------------------------------------------------------------------------+
| amixer sset Headphone 127,127                                                               |
|                                                                                             |
| amixer cset name='Playback Volume' 255,255                                                  |
|                                                                                             |
| amixer sset 'Left Output Mixer PCM' on                                                      |
|                                                                                             |
| amixer sset 'Right Output Mixer PCM' on                                                     |
|                                                                                             |
| amixer cset name='Capture Volume' 0,35                                                      |
|                                                                                             |
| amixer cset name='ADC PCM Capture Volume' 0,195                                             |
|                                                                                             |
| amixer cset name='Left Input Mixer Boost Switch' off                                        |
|                                                                                             |
| amixer cset name='Left Input Boost Mixer LINPUT2 Volume' 0                                  |
|                                                                                             |
| amixer cset name='Left Input Boost Mixer LINPUT3 Volume' 0                                  |
|                                                                                             |
| amixer cset name='Right Boost Mixer RINPUT1 Switch' on                                      |
|                                                                                             |
| amixer cset name='Right Boost Mixer RINPUT2 Switch' on                                      |
|                                                                                             |
| amixer cset name='Right Input Boost Mixer RINPUT1 Volume' 1                                 |
|                                                                                             |
| amixer cset name='Right Input Mixer Boost Switch' on                                        |
|                                                                                             |
| amixer cset name='ADC Data Output Select' 'Left Data = Right ADC; Right Data = Right ADC'   |
+---------------------------------------------------------------------------------------------+

    2.放音测试

+---------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# aplay /home/root/test.wav                                                |
|                                                                                             |
| Playing WAVE '/forlinx/audio/wo.wav' : Signed 16 bit Little Endian, Rate 22050 Hz, Stereo   |
+---------------------------------------------------------------------------------------------+

    3.录音测试

-r：采样频率；-f：声音效果格式；-c：通道设置；-d：设置录音时间；record.wav
:录音文件名称，arecord使用方法可通过arecord --help查看

+------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# arecord -r 44100 -f S16\_LE -c 2 -d 10 record.wav               |
|                                                                                    |
| Recording WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo   |
+------------------------------------------------------------------------------------+

    4.播放录音

+----------------------------------------------------------------------------------+
| root@fl-imx6ull:~# aplay record.wav                                              |
|                                                                                  |
| Playing WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo   |
+----------------------------------------------------------------------------------+

\ **3.1.8.1.2 NAU88C22音频芯片测试**

|image15|

1.设置参数，输入下图中的命令

+----------------------------------------------------------------------+
| root@fl-imx6ull:~# amixer cset name='Headphone Playback Switch' on   |
|                                                                      |
| root@fl-imx6ull:~# amixer sset Headphone 63,63                       |
|                                                                      |
| root@fl-imx6ull:~# amixer cset name='PCM Volume' 235,235             |
|                                                                      |
| root@fl-imx6ull:~# amixer cset name='DAC Limiter Volume' 12          |
|                                                                      |
| root@fl-imx6ull:~# amixer cset name='ADC Volume' 255,255             |
|                                                                      |
| root@fl-imx6ull:~# amixer cset name='Speaker Volume' 63,63           |
+----------------------------------------------------------------------+

2.放音测试

+------------------------------------------------+
| root@fl-imx6ull:~# aplay /home/root/test.wav   |
+------------------------------------------------+

3.录音测试

-r：采样频率；-f：声音效果格式；-c：通道设置；-d：设置录音时间；record.wav:录音文件名称，arecord

使用方法可通过arecord --help查看

+------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# arecord -r 44100 -f S16\_LE -c 2 -d 10 record.wav               |
|                                                                                    |
| nau8822 1-001a: master sysclk 11289600Hz, source MCLK                              |
|                                                                                    |
| nau8822 1-001a: master clock prescaler 0 for fs 44100                              |
|                                                                                    |
| Recording WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo   |
+------------------------------------------------------------------------------------+

4.播放录音

+----------------------------------------------------------------------------------+
| root@fl-imx6ull:~# aplay record.wav                                              |
|                                                                                  |
| nau8822 1-001a: master sysclk 11289600Hz, source MCLK                            |
|                                                                                  |
| nau8822 1-001a: master clock prescaler 0 for fs 44100                            |
|                                                                                  |
| Playing WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo   |
+----------------------------------------------------------------------------------+

**3.1.8.2 Speaker测试**

音频芯片WM8960内部自带的D类功放输出端由两个XH2.54-2P白色插座CON16、CON17引出，可驱动两只8Ω喇叭，最高输出功率为1W，如果需要外接更大的功放，只能从耳机插座获取信号，不能从喇叭接口获取信号。测试speaker时，不能插入耳机，使用如下命令进行测试：

|IMG\_20200602\_150817|

+--------------------------------------------------+
| root@fl-imx6ull:~# mplayer /home/root/test.mp3   |
+--------------------------------------------------+

打印信息如下：

+--------------------------------------------------+
| MPlayer 1.3.0-5.3.0 (C) 2000-2016 MPlayer Team   |
|                                                  |
| Playing /forlinx/audio/test.mp3.                 |
|                                                  |
| libavformat version 57.25.100 (internal)         |
|                                                  |
| Audio only file format detected.                 |
|                                                  |
| Load subtitles in /forlinx/audio/                |
|                                                  |
| ……                                               |
+--------------------------------------------------+

\ **3.1.8.3 播放视频测试**

由于cpu没有硬件多媒体解码器和cpu资源有限，播放的视频分辨率和帧数不高。

-fs：全屏播放；-vo fdbdev：视频驱动为Framebuffer
Device；/forlinx/video/test.mp4为播放的视频文件，mplayer更多使用方法参考mplayer
--help。

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# mplayer -fs -vo fbdev /home/root/test.mp4                                                                                                                                                        |
|                                                                                                                                                                                                                     |
| Creating config file: /home/root/.mplayer/config                                                                                                                                                                    |
|                                                                                                                                                                                                                     |
| MPlayer 1.3.0-5.3.0 (C) 2000-2016 MPlayer Team                                                                                                                                                                      |
|                                                                                                                                                                                                                     |
| Playing /home/root/test.mp4.                                                                                                                                                                                        |
|                                                                                                                                                                                                                     |
| libavformat version 57.25.100 (internal)                                                                                                                                                                            |
|                                                                                                                                                                                                                     |
| libavformat file format detected.                                                                                                                                                                                   |
|                                                                                                                                                                                                                     |
| [mov,mp4,m4a,3gp,3g2,mj2 @ 0x887550]Protocol name not provided, cannot determine if input is local or a network protocol, buffers and access patterns cannot be configured optimally without knowing the protocol   |
|                                                                                                                                                                                                                     |
| [lavf] stream 0: video (mpeg4), -vid 0                                                                                                                                                                              |
|                                                                                                                                                                                                                     |
| [lavf] stream 1: audio (aac), -aid 0, -alang und                                                                                                                                                                    |
|                                                                                                                                                                                                                     |
| VIDEO: [MP4V] 480x272 24bpp 23.976 fps 1077.9 kbps (131.6 kbyte/s)                                                                                                                                                  |
|                                                                                                                                                                                                                     |
| ==========================================================================                                                                                                                                          |
|                                                                                                                                                                                                                     |
| Opening video decoder: [ffmpeg] FFmpeg's libavcodec codec family                                                                                                                                                    |
|                                                                                                                                                                                                                     |
| libavcodec version 57.24.102 (internal)                                                                                                                                                                             |
|                                                                                                                                                                                                                     |
| Selected video codec: [ffodivx] vfm: ffmpeg (FFmpeg MPEG-4)                                                                                                                                                         |
|                                                                                                                                                                                                                     |
| ==========================================================================                                                                                                                                          |
|                                                                                                                                                                                                                     |
| Clip info:                                                                                                                                                                                                          |
|                                                                                                                                                                                                                     |
| major\_brand: isom                                                                                                                                                                                                  |
|                                                                                                                                                                                                                     |
| minor\_version: 512                                                                                                                                                                                                 |
|                                                                                                                                                                                                                     |
| compatible\_brands: isomiso2mp41                                                                                                                                                                                    |
|                                                                                                                                                                                                                     |
| encoder: Lavf55.19.104                                                                                                                                                                                              |
|                                                                                                                                                                                                                     |
| Load subtitles in /home/root/                                                                                                                                                                                       |
|                                                                                                                                                                                                                     |
| ==========================================================================                                                                                                                                          |
|                                                                                                                                                                                                                     |
| Opening audio decoder: [ffmpeg] FFmpeg/libavcodec audio decoders                                                                                                                                                    |
|                                                                                                                                                                                                                     |
| AUDIO: 44100 Hz, 2 ch, floatle, 126.2 kbit/4.47% (ratio: 15775->352800)                                                                                                                                             |
|                                                                                                                                                                                                                     |
| Selected audio codec: [ffaac] afm: ffmpeg (FFmpeg AAC (MPEG-2/MPEG-4 Audio))                                                                                                                                        |
|                                                                                                                                                                                                                     |
| ==========================================================================                                                                                                                                          |
|                                                                                                                                                                                                                     |
| [AO OSS] audio\_setup: Can't open audio device /dev/dsp: No such file or directory                                                                                                                                  |
|                                                                                                                                                                                                                     |
| AO: [alsa] 44100Hz 2ch floatle (4 bytes per sample)                                                                                                                                                                 |
|                                                                                                                                                                                                                     |
| Starting playback...                                                                                                                                                                                                |
|                                                                                                                                                                                                                     |
| Could not find matching colorspace - retrying with -vf scale...                                                                                                                                                     |
|                                                                                                                                                                                                                     |
| Opening video filter: [scale]                                                                                                                                                                                       |
|                                                                                                                                                                                                                     |
| Movie-Aspect is 1.78:1 - prescaling to correct movie aspect.                                                                                                                                                        |
|                                                                                                                                                                                                                     |
| [swscaler @ 0xab8380]bicubic scaler, from yuv420p to bgra using C                                                                                                                                                   |
|                                                                                                                                                                                                                     |
| [swscaler @ 0xab8380]No accelerated colorspace conversion found from yuv420p to bgra.                                                                                                                               |
|                                                                                                                                                                                                                     |
| [swscaler @ 0xab8380]using unscaled yuv420p -> bgra special converter                                                                                                                                               |
|                                                                                                                                                                                                                     |
| VO: [fbdev] 480x272 => 484x272 BGRA [fs]                                                                                                                                                                            |
|                                                                                                                                                                                                                     |
| Movie-Aspect is 1.78:1 - prescaling to correct movie aspect.                                                                                                                                                        |
|                                                                                                                                                                                                                     |
| [swscaler @ 0xab8380]No accelerated colorspace conversion found from yuv420p to bgra.                                                                                                                               |
|                                                                                                                                                                                                                     |
| VO: [fbdev] 480x272 => 484x272 BGRA [fs]                                                                                                                                                                            |
|                                                                                                                                                                                                                     |
| A: 0.1 V: 0.0 A-V: 0.102 ct: 0.004 0/ 0 ??% ??% ??,?% 0 0                                                                                                                                                           |
|                                                                                                                                                                                                                     |
| [VD\_FFMPEG] DRI failure.                                                                                                                                                                                           |
|                                                                                                                                                                                                                     |
| A: 20.0 V: 20.0 A-V: 0.005 ct: 0.068 0/ 0 24% 2% 5.2% 0 0                                                                                                                                                           |
|                                                                                                                                                                                                                     |
| Exiting... (End of file)                                                                                                                                                                                            |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

按Ctrl +C可以停止播放视频，或者等待Exiting... (End of file)
视频播放停止。

3.1.9 CMOS-OV9650测试
---------------------

-  **说明：**

-  **OKMX6ULL平台支持OV9650的320x240、640x480分辨率，不支持1280x1024分辨率。**

-  **支持拍照和预览；不支持自动对焦。**

-  **源码ov9650.c文件暂不开源，提供ov9650的ko模块和应用程序，参考\\Linux\\测试程序。**

CPU支持8位并行接口（DVP），底板数字摄像头（ov9650）接口经由2.0mm间距20P接口插座CON23引出,摄像头最大支持五百万像素。

    |image17|

1. 查看模块是否加载：

+-----------------------------------------------+
| root@fl-imx6ull:~# lsmod                      |
|                                               |
| Module Size Used by                           |
|                                               |
| 8723bu 1313893 0                              |
|                                               |
| mx6s\_capture 14876 0                         |
|                                               |
| **ov9650\_camera 12446 1** //已加载9650模块   |
|                                               |
| evbug 1882 0                                  |
+-----------------------------------------------+

若无\ **mx6s\_capture**\ 和\ **ov9650\_camera**\ 则需要手动加载：

+------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/drivers/media/platform/mxc\\   |
|                                                                                          |
| /subdev/mx6s\_capture.ko //加载mx6s\_capture.ko驱动模块                                  |
|                                                                                          |
| root@fl-imx6ull:~# insmod /lib/modules/$(uname -r)/kernel/drivers/media/platform/mxc\\   |
|                                                                                          |
| /subdev/ov9650\_camera.ko //加载ov9650\_camera.ko                                        |
+------------------------------------------------------------------------------------------+

1. 拍照

拍照之前请先确认video1节点，此时的name为mx6s-csi，拍照预览所用ov9650对应video1节点。

+-------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/video4linux/video1/name   |
|                                                             |
| mx6s-csi                                                    |
+-------------------------------------------------------------+

4.3寸屏采用如下命令：

+-----------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_ov9650-320x240 -c   |
+-----------------------------------------------------+

其他分辨率：5.6 7.0 8.0 10.4采用如下命令：

+----------------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_ov9650-640x480 -c                          |
|                                                                            |
| capability driver mx6s-csi card i.MX6S\_CSI businfo platform:21c4000.csi   |
|                                                                            |
| cam set width 640 height 480                                               |
|                                                                            |
| w 640 h 480                                                                |
|                                                                            |
| done                                                                       |
+----------------------------------------------------------------------------+

1. 预览

4.3寸屏采用如下命令：

+-----------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_ov9650-320x240 -p   |
+-----------------------------------------------------+

其他分辨率：5.6 7.0 8.0 10.4采用如下命令：

+----------------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_ov9650-640x480 -p                          |
|                                                                            |
| capability driver mx6s-csi card i.MX6S\_CSI businfo platform:21c4000.csi   |
|                                                                            |
| cam set width 640 height 480                                               |
+----------------------------------------------------------------------------+

3.1.10 UART串口测试
-------------------

-  **说明：**

-  **OKMX6ULL-S开发板设置3个UART口，其中UART1作为Debug使用，与板载232接口功能一样。UART2、
   UART3作为普通串口使用。**

+----------------+----------------+
| **底板丝印**   | **软件设备**   |
+----------------+----------------+
| UART2          | ttymxc1        |
+----------------+----------------+
| UART3          | ttymxc2        |
+----------------+----------------+

-  **目前测试的与电脑通讯最大波特率为256000**

-  **uart支持7和8bit数据位，1和2bit停止位**

-  **支持硬流控，使用方法见/应用笔记/硬流控的打开方法**

-  **支持奇偶校验，测试方法见/应用笔记/奇偶校验的方法**

以开发板的UART2为例，通过TTL转USB模块连接到电脑，让开发板的UART和电脑串口工具软件之间进行数据收发，来进行串口测试。

1. 开发板的UART2通过TTL转USB模块连接到电脑上，开发板上电后，在电脑设备管理器查看识别为COM3（用户以自己实际识别COM口设置参数）：

|image18|

1. 电脑端打开串口工具选择电脑识别的COM口，波特率115200，数据位8位，停止位1位，无校验，无流控，1s定时发送字符串abcdefg，设置好参数后打开串口：

|image19|

3、开发板终端打开测试程序，进行收发测试，串口参数设置与串口工具的设置要一致，测试程序会自动发送字符串abcdefgh

+-----------------------------------------------------------------------+
| root@fl-imx6ull:~# fltest\_cmd\_uart /dev/ttymxc1 115200              |
|                                                                       |
| Welcome to TTYtest! Press Ctrl + 'c' to stop.                         |
|                                                                       |
| /dev/ttymxc1,creat thread 1993839728 sucess                           |
|                                                                       |
| /dev/ttymxc1,creat thread 1985451120 sucess                           |
|                                                                       |
| sendTotal= 9 num = 1 send = abcdefgh                                  |
|                                                                       |
| **recvTotal= 8 num = 1 recv = abcdefgh //接收到串口工具发送的信息**   |
|                                                                       |
| hex:0x61 0x62 0x63 0x64 0x65 0x66 0x67 0x68                           |
|                                                                       |
| sendTotal= 18 num = 2 send = abcdefgh                                 |
|                                                                       |
| recvTotal= 16 num = 2 recv = abcdefgh                                 |
|                                                                       |
| hex:0x61 0x62 0x63 0x64 0x65 0x66 0x67 0x68                           |
+-----------------------------------------------------------------------+

从打印信息上可以UART2可接收到串口工具发送的信息\ |image20|

串口工具可接收到测试程序发送的数据。

3.1.11 USB转四串口测试
----------------------

-  **说明：**

-  **支持XR21V1414USB转串口芯片驱动**

-  **USB转四串口为选配模块，如若有需求，请联系飞凌嵌入式销售人员。**

1、开发板上电启动后，通过USB
HOST接口连接USB转四串口模块，终端上会有如下打印信息：

+----------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# usb 1-1.1: new full-speed USB device number 4 using ci\_hdrc              |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.0: This device cannot do calls on its own. It is not a modem.   |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.0: ttyXR\_USB\_SERIAL0: USB XR\_USB\_SERIAL device              |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.2: This device cannot do calls on its own. It is not a modem.   |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.2: ttyXR\_USB\_SERIAL1: USB XR\_USB\_SERIAL device              |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.4: This device cannot do calls on its own. It is not a modem.   |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.4: ttyXR\_USB\_SERIAL2: USB XR\_USB\_SERIAL device              |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.6: This device cannot do calls on its own. It is not a modem.   |
|                                                                                              |
| cdc\_xr\_usb\_serial 1-1.1:1.6: ttyXR\_USB\_SERIAL3: USB XR\_USB\_SERIAL device              |
+----------------------------------------------------------------------------------------------+

2、通过lsusb查看usb设备状态：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# lsusb                                      |
|                                                               |
| Bus 001 Device 003: ID 0bda:b720                              |
|                                                               |
| **Bus 001 Device 004: ID 04e2:1414 //该转换芯片的vid和pid**   |
|                                                               |
| Bus 001 Device 002: ID 0424:2514                              |
|                                                               |
| Bus 001 Device 001: ID 1d6b:0002                              |
+---------------------------------------------------------------+

查看dev下是否生产节点：

+-----------------------------------------+
| root@fl-imx6ull:~# ls /dev/ttyXRUSB\*   |
+-----------------------------------------+

打印信息如下：

+---------------------------------------------------------------+
| /dev/ttyXRUSB0 /dev/ttyXRUSB1 /dev/ttyXRUSB2 /dev/ttyXRUSB3   |
+---------------------------------------------------------------+

3、扩展的四个串口与设备节点的对应关系如下图：

|image21|

4、测试方法参考“\ `*UART串口测试* <#uart串口测试>`__\ ”。

3.1.12 FlexCAN设备
------------------

将板子的CAN1与CAN2 H与H相连，L与L相连。

|image22|

1. 设置CAN1服务如下：

+--------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig can0 down //关闭can1                                                 |
|                                                                                                  |
| root@fl-imx6ull:~# ip link set can0 up type can bitrate 125000 triple-sampling on //设置波特率   |
|                                                                                                  |
| flexcan 2090000.can can0: writing ctrl=0x0e312085                                                |
|                                                                                                  |
| IPv6: ADDRCONF(NETDEV\_CHANGE): can0: link becomes ready                                         |
|                                                                                                  |
| root@fl-imx6ull:~# ifconfig can0 up //开启can1                                                   |
+--------------------------------------------------------------------------------------------------+

设置can0的can设备波特率为125000

2. 设置CAN2服务如下：

+-------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# ifconfig can1 down //关闭can2                                    |
|                                                                                     |
| root@fl-imx6ull:~# ip link set can1 up type can bitrate 125000 triple-sampling on   |
|                                                                                     |
| flexcan 2094000.can can1: writing ctrl=0x0e312085                                   |
|                                                                                     |
| IPv6: ADDRCONF(NETDEV\_CHANGE): can1: link becomes ready                            |
|                                                                                     |
| root@fl-imx6ull:~# ifconfig can1 up //开启can2                                      |
+-------------------------------------------------------------------------------------+

设置can1的can设备波特率为125000

3. 设置CAN2 接收数据：

+-------------------------------------+
| root@fl-imx6ull:~# candump can1 &   |
|                                     |
| [1] 755                             |
+-------------------------------------+

CAN1 发送数据：

+--------------------------------------------------------+
| root@fl-imx6ull:~# cansend can0 123#1234567891234567   |
+--------------------------------------------------------+

CAN2接收数据：

+-----------------------------------------------------------+
| root@fl-imx6ull:~# can1 123 [8] 12 34 56 78 91 23 45 67   |
+-----------------------------------------------------------+

3.1.13 LCD 测试
---------------

**3.1.13.1 显示**

根据uboot菜单选择不同尺寸及不同分辨率的LCD显示。上下左右无偏差，颜色显示正常，无花屏等异常现象，即显示正常。

**3.1.13.2 背光测试**

1. 查看当前屏幕背光最大值(7)

+-------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/backlight/backlight/max\_brightness   |
|                                                                         |
| 7                                                                       |
+-------------------------------------------------------------------------+

2. 查看当前屏幕背光值(6)

+--------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/backlight/backlight/brightness   |
|                                                                    |
| 6                                                                  |
+--------------------------------------------------------------------+

3. 设置当前屏幕背光值(3)

+-------------------------------------------------------------------------+
| root@fl-imx6ull:~# echo 3 > /sys/class/backlight/backlight/brightness   |
+-------------------------------------------------------------------------+

查看是否设置成功：

+--------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/backlight/backlight/brightness   |
|                                                                    |
| 3                                                                  |
+--------------------------------------------------------------------+

**3.1.13.3 触摸**

查看输入设备列表：

**其中goodix-ts为电容触摸设备，iMX6UL TouchScreen
Controller为电阻触摸设备**

+------------------------------------------------------------------+
| root@fl-imx6ull:~# export DISPLAY=:0.0                           |
|                                                                  |
| root@fl-imx6ull:~# DISPLAY=:0 xinput list                        |
|                                                                  |
| ⎡ Virtual core pointer id=2 [master pointer (3)]                 |
|                                                                  |
| ⎜ ↳ Virtual core XTEST pointer id=4 [slave pointer (2)]          |
|                                                                  |
| ⎜ ↳ **iMX6UL TouchScreen Controller** id=6 [slave pointer (2)]   |
|                                                                  |
| ⎜ ↳ **goodix-ts** id=7 [slave pointer (2)]                       |
|                                                                  |
| ⎣ Virtual core keyboard id=3 [master keyboard (2)]               |
|                                                                  |
| ↳ Virtual core XTEST keyboard id=5 [slave keyboard (3)]          |
+------------------------------------------------------------------+

查看id=6（电阻触摸）的设备信息：

+---------------------------------------------------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# DISPLAY=:0 xinput list-props 6                                                                                                 |
|                                                                                                                                                   |
| Device 'iMX6UL TouchScreen Controller':                                                                                                           |
|                                                                                                                                                   |
| Device Enabled (113): 1                                                                                                                           |
|                                                                                                                                                   |
| Coordinate Transformation Matrix (114): 1.000000, 0.000000, 0.000000, 0.000000, 1.000000, 0.000000, 0.000000, 0.000000, 1.000000                  |
|                                                                                                                                                   |
| Device Accel Profile (234): 0                                                                                                                     |
|                                                                                                                                                   |
| Device Accel Constant Deceleration (235): 1.000000                                                                                                |
|                                                                                                                                                   |
| Device Accel Adaptive Deceleration (236): 1.000000                                                                                                |
|                                                                                                                                                   |
| Device Accel Velocity Scaling (237): 10.000000                                                                                                    |
|                                                                                                                                                   |
| Device Product ID (238): 0, 0                                                                                                                     |
|                                                                                                                                                   |
| Device Node (239): "/dev/input/event1"                                                                                                            |
|                                                                                                                                                   |
| Evdev Axis Inversion (240): 0, 0                                                                                                                  |
|                                                                                                                                                   |
| Evdev Axis Calibration (241): <no items>                                                                                                          |
|                                                                                                                                                   |
| Evdev Axes Swap (242): 1                                                                                                                          |
|                                                                                                                                                   |
| Axis Labels (243): "Abs X" (232), "Abs Y" (233)                                                                                                   |
|                                                                                                                                                   |
| Button Labels (244): "Button Unknown" (231), "Button Unknown" (231), "Button Unknown" (231), "Button Wheel Up" (119), "Button Wheel Down" (120)   |
|                                                                                                                                                   |
| Evdev Scrolling Distance (245): 0, 0, 0                                                                                                           |
|                                                                                                                                                   |
| Evdev Middle Button Emulation (246): 0                                                                                                            |
|                                                                                                                                                   |
| Evdev Middle Button Timeout (247): 50                                                                                                             |
|                                                                                                                                                   |
| Evdev Third Button Emulation (248): 0                                                                                                             |
|                                                                                                                                                   |
| Evdev Third Button Emulation Timeout (249): 1000                                                                                                  |
|                                                                                                                                                   |
| Evdev Third Button Emulation Button (250): 3                                                                                                      |
|                                                                                                                                                   |
| Evdev Third Button Emulation Threshold (251): 20                                                                                                  |
|                                                                                                                                                   |
| Evdev Wheel Emulation (252): 0                                                                                                                    |
|                                                                                                                                                   |
| Evdev Wheel Emulation Axes (253): 0, 0, 4, 5                                                                                                      |
|                                                                                                                                                   |
| Evdev Wheel Emulation Inertia (254): 10                                                                                                           |
|                                                                                                                                                   |
| Evdev Wheel Emulation Timeout (255): 200                                                                                                          |
|                                                                                                                                                   |
| Evdev Wheel Emulation Button (256): 4                                                                                                             |
|                                                                                                                                                   |
| Evdev Drag Lock Buttons (257): 0                                                                                                                  |
+---------------------------------------------------------------------------------------------------------------------------------------------------+

-  **注意：因8吋电阻屏（800x600分辨率）与10.4吋电阻屏（800x600分辨率）在使用xinput\_calibrate进行校准时，触摸方向不同。默认是支持8吋屏的触摸方向，如果使用10.吋屏，需要按下述方法交换触摸的xy轴。**

可在命令行输入：

+-------------------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# DISPLAY=:0 xinput --set-prop 'iMX6UL TouchScreen Controller' 'Evdev Axes Swap' 1   |
+-------------------------------------------------------------------------------------------------------+

或者：

+-------------------------------------------------------------------------+
| root@fl-imx6ull:~# DISPLAY=:0 xinput set-prop '6' 'Evdev Axes Swap' 1   |
+-------------------------------------------------------------------------+

如果需要默认支持识别10.4吋屏，可将上述命令添加到/etc/rc.local中。如下：

+------------------------------------------------------------------------------------------+
| lcd\_screen\_arg() {                                                                     |
|                                                                                          |
| geom=\`fbset \| grep geometry\`                                                          |
|                                                                                          |
| w=\`echo $geom \| awk '{ print $2 }'\`                                                   |
|                                                                                          |
| h=\`echo $geom \| awk '{ print $3 }'\`                                                   |
|                                                                                          |
| echo -n "${w}x${h}"                                                                      |
|                                                                                          |
| }                                                                                        |
|                                                                                          |
| LCD\_SIZE=\`lcd\_screen\_arg\`                                                           |
|                                                                                          |
| if [ "$LCD\_SIZE" == "800x480" ] ; then                                                  |
|                                                                                          |
| DISPLAY=:0 xinput --set-prop 'iMX6UL TouchScreen Controller ' 'Evdev Axes Swap' 1        |
|                                                                                          |
| elif [ "$LCD\_SIZE" == "640x480" ] ; then                                                |
|                                                                                          |
| DISPLAY=:0 xinput --set-prop ' iMX6UL TouchScreen Controller ' 'Evdev Axes Swap' 1       |
|                                                                                          |
| **elif [ "$LCD\_SIZE" == "800x600" ] ; then**                                            |
|                                                                                          |
| **DISPLAY=:0 xinput --set-prop ' iMX6UL TouchScreen Controller ' 'Evdev Axes Swap' 1**   |
|                                                                                          |
| fi                                                                                       |
+------------------------------------------------------------------------------------------+

**3.1.13.4 进入/退出待机**

进入待机模式：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo "4" > /sys/class/graphics/fb0/blank   |
+---------------------------------------------------------------+

退出待机模式：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo "0" > /sys/class/graphics/fb0/blank   |
+---------------------------------------------------------------+

3.1.14 LED测试
--------------

OKMX6ULL-S底板上的LED1、LED2为用户LED 灯，分别对应/sys/class/leds
目录下的led1、led2。

查看触发条件：

+----------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/leds/led1/trigger                        |
|                                                                            |
| [none] rc-feedback nand-disk mmc0 timer oneshot heartbeat backlight gpio   |
+----------------------------------------------------------------------------+

其中[none]表示当前led1的触发条件为无。往trigger中写上述字符串，可以修改触发条件。

用户控制

当led触发条件设置为none时，用户可通过命令来控制led灯的亮灭。

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo none > /sys/class/leds/led1/trigger   |
+---------------------------------------------------------------+

控制 LED1 亮：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo 1 > /sys/class/leds/led1/brightness   |
+---------------------------------------------------------------+

控制 LED1 灭：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo 0 > /sys/class/leds/led1/brightness   |
+---------------------------------------------------------------+

控制 LED2 亮：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo 1 > /sys/class/leds/led2/brightness   |
+---------------------------------------------------------------+

控制 LED2 灭：

+---------------------------------------------------------------+
| root@fl-imx6ull:~# echo 0 > /sys/class/leds/led2/brightness   |
+---------------------------------------------------------------+

设置为心跳灯：

设置触发条件为系统心跳，如下：

+--------------------------------------------------------------------+
| root@fl-imx6ull:~# echo heartbeat > /sys/class/leds/led1/trigger   |
+--------------------------------------------------------------------+

此时LED1有系统时钟控制，按一定节奏闪烁。

3.1.15 数据库测试
-----------------

SQLite3是一个轻型的嵌入式数据库，占用资源非常低，处理速度快，不需要安装数据库服务器进程。OKMX6ULL-S开发板移植的是3.11.0版本的sqlit3。

+------------------------------------------------------------+
| root@fl-imx6ull:~# sqlite3                                 |
|                                                            |
| SQLite version 3.11.0 2016-02-15 17:29:24                  |
|                                                            |
| Enter ".help" for usage hints.                             |
|                                                            |
| Connected to a transient in-memory database.               |
|                                                            |
| Use ".open FILENAME" to reopen on a persistent database.   |
|                                                            |
| sqlite>                                                    |
+------------------------------------------------------------+

测试SQLite软件：

+---------------------------------------------------------------------------------+
| SQLite version 3.11.0 2016-02-15 17:29:24                                       |
|                                                                                 |
| Enter ".help" for usage hints.                                                  |
|                                                                                 |
| Connected to a transient in-memory database.                                    |
|                                                                                 |
| Use ".open FILENAME" to reopen on a persistent database.                        |
|                                                                                 |
| sqlite> create table tbl1 (one varchar(10), two smallint); //创建表tbl1         |
|                                                                                 |
| sqlite> insert into tbl1 values('hello!',10); //tbl1表内插入数据hello!\|10      |
|                                                                                 |
| sqlite> insert into tbl1 values('goodbye', 20); //tbl1表内插入数据goodbye\|20   |
|                                                                                 |
| sqlite> select \* from tbl1; //查询表tbl1中内容                                 |
|                                                                                 |
| hello!\|10                                                                      |
|                                                                                 |
| goodbye\|20                                                                     |
|                                                                                 |
| sqlite>                                                                         |
+---------------------------------------------------------------------------------+

退出数据库：

+-------------------------------------------------+
| sqlite> .exit //退出数据库（或使用.quit命令）   |
|                                                 |
| root@fl-imx6ull:~#                              |
+-------------------------------------------------+

3.1.16 调频测试
---------------

当用户有修改CPU频率需求时，OKMX6ULL-S开发板支持通过指令方式调节CPU频率。

1. 当前内核中支持的所有cpufreq governor类型：

+---------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/devices/system/cpu/cpu0/cpufreq/scaling\_available\_governors   |
|                                                                                             |
| interactive conservative userspace powersave ondemand performance                           |
+---------------------------------------------------------------------------------------------+

其中userspace表示用户模式，在此模式下允许其他用户程序调节CPU频率。

2. 查看当前CPU支持的频率档位：

+-----------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/devices/system/cpu/cpu0/cpufreq/scaling\_available\_frequencies   |
|                                                                                               |
| 198000 396000 528000 792000                                                                   |
+-----------------------------------------------------------------------------------------------+

2. 修改为用户模式，修改频率为792000：

+----------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# echo userspace > /sys/devices/system/cpu/cpu0/cpufreq/scaling\_governor   |
|                                                                                              |
| root@fl-imx6ull:~# echo 792000 > /sys/devices/system/cpu/cpu0/cpufreq/scaling\_setspeed      |
+----------------------------------------------------------------------------------------------+

2. 查看当前频率：

+----------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo\_cur\_freq   |
|                                                                                  |
| 792000                                                                           |
+----------------------------------------------------------------------------------+

3.1.17 温度
-----------

-  **说明：**

-  **uboot中cpu结温设置105度；**

-  **内核默认设置中cpu结温，超过85度cpu会降频；超过105度，cpu会重启；**

1、查看CPU当前温度值：

+-----------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/class/thermal/thermal\_zone0/temp   |
|                                                                 |
| 51890 //温度值即为51.890℃（51890/1000）。                       |
+-----------------------------------------------------------------+

2、查看内核中cpu降频温度值

+-------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/devices/virtual/thermal/thermal\_zone0/trip\_point\_0\_temp   |
|                                                                                           |
| 85000 //温度值85℃                                                                         |
+-------------------------------------------------------------------------------------------+

    3、查看内核中cpu重启温度值

+-------------------------------------------------------------------------------------------+
| root@fl-imx6ull:~# cat /sys/devices/virtual/thermal/thermal\_zone0/trip\_point\_1\_temp   |
|                                                                                           |
| 105000 //温度值105℃                                                                       |
+-------------------------------------------------------------------------------------------+

3.1.18 蓝牙测试
---------------

开发板集成了wifi&bt模块RTL8723du模块(或者RTL8723bu模块)，现在测试该模块的bluetooth蓝牙功能。

目前只有QT版本文件系统支持蓝牙测试。

我们使用bluez5.37的工具进行测试。

1. 开启bluez守护进程

+-------------------------------------------+
| root@imx6ulevk:~# source /usr/bin/bt.sh   |
|                                           |
| Starting bluetooth                        |
|                                           |
| bluetoothd                                |
+-------------------------------------------+

1. 配置蓝牙

+--------------------------------------------------------+
| root@imx6ulevk:~# bluetoothctl //打开bluez蓝牙工具     |
|                                                        |
| Agent registered                                       |
|                                                        |
| [CHG] Controller 0C:CF:89:7C:79:E3 Pairable: yes       |
|                                                        |
| [bluetooth]# power on //启动蓝牙设备                   |
|                                                        |
| Changing power on succeeded                            |
|                                                        |
| [CHG] Controller 0C:CF:89:7C:79:E3 Powered: yes        |
|                                                        |
| [bluetooth]# pairable on //设置为配对模式              |
|                                                        |
| Changing pairable on succeeded                         |
|                                                        |
| [bluetooth]# discoverable on //设置为可发现模式        |
|                                                        |
| Changing discoverable on succeeded                     |
|                                                        |
| [CHG] Controller 0C:CF:89:7C:79:E3 Discoverable: yes   |
|                                                        |
| [bluetooth]# agent on //启动代理                       |
|                                                        |
| Agent is already registered                            |
|                                                        |
| [bluetooth]# default-agent //设置当前代理为默认        |
|                                                        |
| Default agent request successful                       |
+--------------------------------------------------------+

**3.1.18.1 被动配对**

经过以上配置之后，在手机上可扫描到该蓝牙设备，并点击此蓝牙尝试配对。

|image23|

同时板端打印如下，输入yes。

+----------------------------------------------------+
| [bluetooth]# rtk\_btcoex: hci accept conn req      |
|                                                    |
| rtk\_btcoex: connected, handle 0005, status 0x00   |
|                                                    |
| rtk\_btcoex: Page success                          |
|                                                    |
| rtk\_btcoex: io capability request                 |
|                                                    |
| [NEW] Device BC:2E:F6:57:30:68 honor               |
|                                                    |
| Request confirmation                               |
|                                                    |
| [agent] Confirm passkey 500686 (yes/no):yes        |
+----------------------------------------------------+

然后在手机点击蓝牙进行配对。

|image24|

板端再次出现打印信息：

+-------------------------------------------------------------------------------+
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000111f-0000-1000-8000-00805f9b34fb    |
|                                                                               |
| [CHG] Device BC:2E:F6:57:30:68 Modalias: bluetooth:v010Fp107Ed1436            |
|                                                                               |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000046a-0000-1000-8000-00805f9b34fb    |
|                                                                               |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001105-0000-1000-8000-00805f9b34fb    |
|                                                                               |
| ……此处省略不关键信息                                                          |
|                                                                               |
| [agent] Authorize service 0000111e-0000-1000-8000-00805f9b34fb (yes/no):yes   |
+-------------------------------------------------------------------------------+

输入yes即配对成功。

移除配对设备。

+---------------------------------------------------------------------------------+
| [honor]# devices //查看已连接设备                                               |
|                                                                                 |
| Device BC:2E:F6:57:30:68 honor                                                  |
|                                                                                 |
| [bluetooth]# remove BC:2E:F6:57:30:68 //移除设备                                |
|                                                                                 |
| Device has been removed                                                         |
|                                                                                 |
| [honor]# rtk\_btcoex: disconn cmpl evt: status 0x00, handle 0006, reason 0x16   |
|                                                                                 |
| rtk\_btcoex: process disconn complete event.                                    |
|                                                                                 |
| Agent unregistered                                                              |
|                                                                                 |
| [DEL] Controller 0C:CF:89:7C:79:E3 BlueZ 5.54 [default]                         |
+---------------------------------------------------------------------------------+

出现如上打印移除成功。

**3.1.18.2 主动配对**

除了被动配对，也可以在开发板终端发送主动配对的请求。

+-------------------------------------------------------------------------------------+
| [bluetooth]# scan on //搜索可被发现蓝牙                                             |
|                                                                                     |
| rtk\_btcoex: hci (periodic)inq start                                                |
|                                                                                     |
| Discovery started                                                                   |
|                                                                                     |
| [CHG] Controller 0C:CF:89:7C:79:E3 Discovering: yes                                 |
|                                                                                     |
| [NEW] Device BC:2E:F6:57:30:68 honor                                                |
|                                                                                     |
| [CHG] Device BC:2E:F6:57:30:68 RSSI: -50                                            |
|                                                                                     |
| [NEW] Device 58:85:A2:D0:1A:6C wjy                                                  |
|                                                                                     |
| [bluetooth]# scan off //停止搜索                                                    |
|                                                                                     |
| rtk\_btcoex: hci (periodic)inq cancel/exit                                          |
|                                                                                     |
| Discovery stopped                                                                   |
|                                                                                     |
| [CHG] Controller 0C:CF:89:7C:79:E3 Discovering: no                                  |
|                                                                                     |
| [CHG] Device 4A:EF:9B:E7:AB:CB TxPower is nil                                       |
|                                                                                     |
| [CHG] Device 4A:EF:9B:E7:AB:CB RSSI is nil                                          |
|                                                                                     |
| [CHG] Device 58:85:A2:D0:1A:6C RSSI is nil                                          |
|                                                                                     |
| [CHG] Device BC:2E:F6:57:30:68 RSSI is nil                                          |
|                                                                                     |
| [bluetooth]# pair BC:2E:F6:57:30:68 //配对蓝牙                                      |
|                                                                                     |
| Attempting to pair with BC:2E:F6:rtk\_btcoex: hci create connection, start paging   |
|                                                                                     |
| 57:30:68                                                                            |
|                                                                                     |
| rtk\_btcoex: connected, handle 0001, status 0x00                                    |
|                                                                                     |
| rtk\_btcoex: Page success                                                           |
|                                                                                     |
| rtk\_btcoex: io capability request                                                  |
|                                                                                     |
| [CHG] Device BC:2E:F6:57:30:68 Connected: yes                                       |
|                                                                                     |
| Request confirmation                                                                |
|                                                                                     |
| [agent] Confirm passkey 772652 (yes/no):yes                                         |
+-------------------------------------------------------------------------------------+

同时手机界面出现配对请求点击配对，板端打印输入yes，手机端接受配对，出现如下打印配对成功。

+------------------------------------------------------------------------------+
| [CHG] Device BC:2E:F6:57:30:68 Modalias: bluetooth:v010Fp107Ed1436           |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000046a-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001105-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000110a-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000110c-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001112-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001115-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001116-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000111f-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000112f-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001132-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001200-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001800-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 00001801-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 UUIDs: 0000fe35-0000-1000-8000-00805f9b34fb   |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 ServicesResolved: yes                         |
|                                                                              |
| [CHG] Device BC:2E:F6:57:30:68 Paired: yes                                   |
|                                                                              |
| Pairing successful                                                           |
|                                                                              |
| [honor]#                                                                     |
+------------------------------------------------------------------------------+

输入quit退出命令行。

+-----------------+
| [honor]# quit   |
+-----------------+

**3.1.18.3 文件接收**

手机选择好文件选择通过蓝牙发送到开发板（前提已经做好蓝牙连接）。

|image25|

发送完成可以在/home/root/目录下看到发过来的图片文件。

+-----------------------------+
| root@imx6ulevk:~# ls        |
|                             |
| IMG\_20210710\_103755.jpg   |
+-----------------------------+

**3.1.18.4 文件发送**

+-----------------------------------------------------------------------------+
| root@imx6ulevk:~# obexctl //开启obexd守护进程                               |
|                                                                             |
| [NEW] Client /org/bluez/obex                                                |
|                                                                             |
| [obex]# connect BC:2E:F6:57:30:68 //连接需要通讯的蓝牙                      |
|                                                                             |
| ……                                                                          |
|                                                                             |
| rtk\_btcoex: rtk\_vendor\_cmd\_to\_fw: opcode 0xfc19                        |
|                                                                             |
| [NEW] Session /org/bluez/obex/client/session0 [default]                     |
|                                                                             |
| [NEW] ObjectPush /org/bluez/obex/client/session0                            |
|                                                                             |
| Connection successful                                                       |
|                                                                             |
| [BC:2E:F6:57:30:68]# send /home/root/IMG\_20210710\_103755.jpg //发送文件   |
+-----------------------------------------------------------------------------+

此时手机收到接受文件的询问。

|image26|

点击接受可以看到文件已经在传输了。

|image27|

传输完毕即可在手机看到发过来的图片。

3.1.19 开机自启设置
-------------------

在文件系统的/etc/rc.local为设置应用开机自启的脚本，用户可以在该脚本中加入自己需要开机自启动的程序，具体设置方法可以参考应用笔记中相关文件。

3.2 界面功能测试
----------------

-  **说明：**

-  **用户使用屏幕和Qt文件系统时进行该章节操作，若不用Qt操作，可跳过该章节。**

**本章节着重描述Qt中的功能，测试时默认设备连接正常，驱动加载正常，建议完成命令行功能测试后在测试界面功能。**

-  命令行测试程序源码路径：用户资料/Linux/测试程序/qt5.6

本节测试所用到的测试程序在飞凌提供的Demo中已有集成，故不做文件来源说明，直接进行操作。

在Qt文件系统里，移植的是qt版本为5.6.2，是带X协议的Qt显示方式，桌面管理程序为matchbox。启动开发板，屏幕会先显示飞凌LOGO，之后是一个开机动画，然后进入到下图的Qt桌面：

|image28|

3.2.1 网卡配置
--------------

点击桌面图标右上角的小鼠标按钮，选择“Preferences”，再点击左侧的 Ethernet
按钮对网络进行配置。默认为 DHCP 方式，也可以选择为静态 IP
的方式，点击右上角的键盘进行设置即可。

|image29|

IP设置界面如下：

|image30|

Configuration:设置分配IP方式，DHCP--设置动态分配IP，MANUAL--设置静态IP。

IP address：静态IP有效，设置IP地址。

Netmask：静态IP有效，设置子网掩码。

Gateway：静态IP有效，设置网关。

3.2.2 PING测试
--------------

运行 PING 测试程序 Ping ，进入如下界面（确保已进行了网络配置，在
hostname 一栏输入要测试的 ip 地址或域名，然后单击 ping
按钮，若出现如下界面说明网络测试成功：

|image31|

3.2.3 4G模块测试
----------------

目前支持的 4G 模块是华为 ME909和移远的EC20。运从桌面上点击Qt5
4G图标，进入到4G设置界面，用于测试4G拨号上网功能。

|image32|

连接好4G模块，模块上电，开发板上电。根据实际情况选择4G模块，以下以EC20为例，点击ec20，进行拨号，拨号过程没有打印信息，拨号完成后会有如下图提示：

|image33|

拨号成功后，可在IP栏输入同网段IP进行ping测试：

|image34|

3.2.4 GPRS模块测试
------------------

-  **说明：GPRS 模块与开发板之间采用串口连接，用户可以使用飞凌公司自产的
   GPRS 模块，也可以使用自己购买的串口 GPRS 模块。**

在确保模块和开发板串口 UART3 连接、上电 ok 情况下， 启动开发板子，运行
GPRS 测试程序 Gprs。

选择 GPRS 模块连接的串口
ttymxc2、设置串口波特率、数据位、奇偶较验、停止位、硬件流控，点击 set
按钮进行设置。

在 Phone 栏添加对方的电话号码，分别点击 call 和 msg-s
按钮，进行拨打电话、发送短信息测试；

GPRS 上网功能测试：单击界面上的 gprs 按钮，即可拨号上网；

用户可通过点击 Ping 按钮测试 GPRS 是否拨号成功：

|image35|

3.2.6 看门狗测试
----------------

运行看门狗测试程序 Watchdog ，进入如下界面：

|image36|

左上角的 feed dog 复选框可以选择是否喂狗，勾选了 feed dog 时，点击下部的
open watchdog 即可开启看门狗，应用程序 10s
会喂狗一次，因此系统不会重启。

当未勾选 feed dog 时， open watchdog 后，当 time files 到达 60s
时，系统将会复位。

3.2.5 WIFI模块测试
------------------

本节介绍的板载WIFI模块rtl8723bu的测试方法，测试前请先确保已焊接WIFI模块，并已连接天线。

运行 Qt5.6 Wifi 测试程序，点击右上角键盘，弹出键盘，输出用户名，密码等。

|image37|

SSID 为 wifi 热点的名称， PAWD 为 WIFI 密码， IP 为你要 ping 测试的 IP
地址，填写好之后点击 connect
按钮即可。连接完成后左侧对话框可以看到自动获取到的 IP，此时可以点击 Ping
按钮进行Ping 测试。

|image38|

3.2.7 放/录音测试
-----------------

|image39|

运行音频测试程序Audio ，进入音频测试界面。

1、录音测试

开发板上提供了两个麦克风接口，一个是板载的麦克风U16，另一个是3.5mm标准立体声音频接口MIC。系统默认使用板载麦克，当MIC有外部麦克风设备插入时，板载麦克U16自动断开，使用外部麦克风设备录音。

录音测试时，单击 record 按钮，然后对着麦克说话，持续数秒后单击 stop
按钮，之后会看到歌曲列表里会多出一个临时文件，单击 play 按钮可以回放录音

2、放音测试

音频芯片WM8960内部自带的D类功放输出端由两个XH2.54-2P白色插座P5、P6引出，可驱动两只8Ω喇叭，最高输出功率为1W。如果需要外接更大的功放，只能从耳机插座获取信号，不能从喇叭接口获取信号。

放音测试时，可以选择耳机或喇叭进行测试，连接好后，点击add选择要播放的音频文件，点击
play 按钮，若能听到音乐声音，则放音正常。

|image40|

3.2.8 RTC时钟驱动测试
---------------------

-  **注意：确保板子上已经安装了纽扣电池，并且电池电压正常**

RTC
测试是通过测试软件设置时间，设置完后可以断电再重新上电，再次运行测试软件，查看RTC
时钟是否同步。

运行RTC测试软件 RTC，出现以下界面：

|image41|

点击 set 设置系统时间，点击 save
保存时间设置，然后可以断电，过段时间再上电，再次运行
RTC测试软件读取时间，可以看到 RTC 时间已同步。

3.2.9 串口测试
--------------

本测试以测试UART2( ttymxc1)
为例，通过开发板的UART和电脑串口工具软件之间的数据收发，来进行串口测试。

1、开发板和电脑通过TTL转USB模块连接好后，给开发板上电，在电脑设备管理器查看识别为COM9（用户以自己实际识别的COM口设置参数）

|image42|

2、打开电脑串口工具，设置相关串口参数，波特率9600、8位数据位、1位停止位、无校验、无流控制，定时发送字符串abcdefg，设置完成后打开串口：

|image43|

1. 点击QT桌面的Qt5
   SerialPort串口测试程序，选择端口号，设置相关串口参数和电脑串口工具参数一致，如下图：

|image44|

4、打开串口，在接收串口能收到电脑串口工具发送的数据。发送窗口写入要发送的数据，点击发送数据

|image45|

5、电脑串口工具接收到qt程序发送的数据，开发板的UART2串口收发正常。

|image46|

3.2.10 FlexCAN测试
------------------

开发板有两路CAN，本次测试采用开发板上的CAN1与另一个开发板的CAN1连接，互相发送数据。将开发板CAN1的H与另一个开发板的CAN1的H相连，L与L相连。用户可根据实际情况使用CAN工具进行测试。

打开桌面程序Qt5
Can，如下图，CAN1与CAN2在界面里分别对应can0和can1，选择can0 会开启CAN1：

|image47|

开发板发送数据123456，另一端数据发送654321，开发板接收到数据654321如下图。

|image48|

另一个开发板的CAN1接收到数据123456，如下图

|image49|

3.2.11 RGB屏幕背光调节测试
--------------------------

左右调节进度条，可以调节背光亮度，如下：

|image50|

3.2.12 LED测试
--------------

点击LED test，进入如下界面，此时可看到用户LED 灯是灭的：

|image51|

勾选LED1 复选框选择是否点亮LED1 灯，勾选了LED1，LED1亮。

勾选LED2 复选框选择是否点亮LED2 灯，勾选了LED2，LED2亮。

.. |image0| image:: .//media/image3.png
   :width: 5.12500in
   :height: 2.12500in
.. |image1| image:: .//media/image4.jpeg
   :width: 3.93750in
   :height: 2.94792in
.. |image2| image:: .//media/image5.jpeg
   :width: 3.93750in
   :height: 2.94792in
.. |image3| image:: .//media/image6.png
   :width: 2.60417in
   :height: 0.59375in
.. |image4| image:: .//media/image7.png
   :width: 5.76042in
   :height: 0.86458in
.. |image5| image:: .//media/image8.png
   :width: 3.13542in
   :height: 3.72917in
.. |image6| image:: .//media/image9.png
   :width: 3.13542in
   :height: 2.89583in
.. |image7| image:: .//media/image10.emf
   :width: 5.55208in
   :height: 1.48958in
.. |image8| image:: .//media/image11.png
   :width: 5.84375in
   :height: 4.47917in
.. |image9| image:: .//media/image12.png
   :width: 5.84375in
   :height: 4.27083in
.. |image10| image:: .//media/image13.png
   :width: 3.93750in
   :height: 3.81250in
.. |Snipaste\_2020-06-04\_13-42-23| image:: .//media/image14.png
   :width: 3.60417in
   :height: 2.71875in
.. |image12| image:: .//media/image15.png
   :width: 4.71875in
   :height: 3.77083in
.. |image13| image:: .//media/image16.png
   :width: 3.70833in
   :height: 4.55208in
.. |image14| image:: .//media/image17.png
   :width: 3.87500in
   :height: 2.87500in
.. |image15| image:: .//media/image18.jpeg
   :width: 4.15625in
   :height: 3.08333in
.. |IMG\_20200602\_150817| image:: .//media/image19.jpeg
   :width: 2.77083in
   :height: 2.08333in
.. |image17| image:: .//media/image20.jpeg
   :width: 2.93750in
   :height: 2.28125in
.. |image18| image:: .//media/image21.png
   :width: 5.51042in
   :height: 3.96875in
.. |image19| image:: .//media/image22.png
   :width: 5.18750in
   :height: 4.15625in
.. |image20| image:: .//media/image23.png
   :width: 5.51042in
   :height: 4.36458in
.. |image21| image:: .//media/image24.png
   :width: 5.51042in
   :height: 2.62500in
.. |image22| image:: .//media/image25.png
   :width: 3.57292in
   :height: 3.18750in
.. |image23| image:: .//media/image26.png
   :width: 3.16667in
   :height: 6.69792in
.. |image24| image:: .//media/image27.png
   :width: 3.07292in
   :height: 2.81250in
.. |image25| image:: .//media/image28.png
   :width: 4.72917in
   :height: 1.37500in
.. |image26| image:: .//media/image29.png
   :width: 4.72917in
   :height: 1.25000in
.. |image27| image:: .//media/image30.png
   :width: 4.72917in
   :height: 1.16667in
.. |image28| image:: .//media/image31.png
   :width: 5.90625in
   :height: 3.54167in
.. |image29| image:: .//media/image32.png
   :width: 5.90625in
   :height: 3.54167in
.. |image30| image:: .//media/image33.png
   :width: 5.90625in
   :height: 3.54167in
.. |image31| image:: .//media/image34.png
   :width: 5.90625in
   :height: 3.54167in
.. |image32| image:: .//media/image35.png
   :width: 5.90625in
   :height: 3.45833in
.. |image33| image:: .//media/image35.png
   :width: 5.90625in
   :height: 3.45833in
.. |image34| image:: .//media/image36.png
   :width: 5.90625in
   :height: 3.45833in
.. |image35| image:: .//media/image37.png
   :width: 5.90625in
   :height: 3.54167in
.. |image36| image:: .//media/image38.png
   :width: 5.90625in
   :height: 3.54167in
.. |image37| image:: .//media/image39.png
   :width: 5.95833in
   :height: 3.56250in
.. |image38| image:: .//media/image40.png
   :width: 5.90625in
   :height: 3.54167in
.. |image39| image:: .//media/image17.png
   :width: 3.87500in
   :height: 2.87500in
.. |image40| image:: .//media/image41.png
   :width: 5.90625in
   :height: 3.54167in
.. |image41| image:: .//media/image42.png
   :width: 5.90625in
   :height: 3.39583in
.. |image42| image:: .//media/image43.png
   :width: 5.90625in
   :height: 4.08333in
.. |image43| image:: .//media/image44.png
   :width: 5.90625in
   :height: 4.51042in
.. |image44| image:: .//media/image45.png
   :width: 5.90625in
   :height: 3.54167in
.. |image45| image:: .//media/image46.png
   :width: 5.90625in
   :height: 3.54167in
.. |image46| image:: .//media/image47.png
   :width: 5.90625in
   :height: 4.36458in
.. |image47| image:: .//media/image48.png
   :width: 5.90625in
   :height: 3.54167in
.. |image48| image:: .//media/image49.png
   :width: 5.89583in
   :height: 3.54167in
.. |image49| image:: .//media/image50.png
   :width: 5.89583in
   :height: 3.54167in
.. |image50| image:: .//media/image51.png
   :width: 5.90625in
   :height: 3.54167in
.. |image51| image:: .//media/image52.png
   :width: 5.90625in
   :height: 3.54167in
