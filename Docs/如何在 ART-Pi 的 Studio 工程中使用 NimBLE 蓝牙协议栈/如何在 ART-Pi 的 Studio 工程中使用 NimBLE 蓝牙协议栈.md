# 如何在 ART-Pi 的 Studio 工程中使用 NimBLE 蓝牙协议栈

本文主要介绍 ART-Pi Studio 工程下 NimBLE 软件包的使用

-   RT-Thread Studio 工程中配置使用 NimBLE
-   目前 NimBLE 支持 BLE Host 层，还需要搭配外接蓝牙 Control 芯片使用（可使用片上 ap6212）

## 新建 ART-Pi 示例工程

按照下图新建一个 art_pi_blink_led 示例工程，等待创建完成。

![image-20220819003537014](figures/new-project.png)

## 配置使用 NimBLE

进入工程 RT-Thread Settings 界面， 点击添加软件包

![](./figures/setting.png)

在软件包中心找到 NimBLE ，并点击添加

![](./figures/pkg center.png)

添加完成后关闭界面，这时可以在 RT-Thread Setting 中看到 NimBLE 软件包：

![](./figures/nimble.png)

软件包添加完成。

添加完成后还需要进行一些配置，点击软件包的**配置项**，进入详细配置界面

![](./figures/config.png)



按照以下步骤进行配置：

1、关闭 Controller 支持： 将 **Controller Configuration - Bluetoorh Controller support** 关闭；

2、打开 HCI Transport 支持，并配置相关使用的串口： 将 **HCI Transport support - HCI Transport using rt-thread uart** 打开， 并且 修改 **The uart for HCI Transport** 为实际与蓝牙Control卡片连接的串口，如 uart3。

3、选择使用相应的蓝牙例程：在 **Bluetooth Samples** 中选择相应的例程。目前支持以下几个例程：

-   BLE peripheral heartrate sensor
-   BLE peripheral cycling speed and cadence sensor
-   BLE central role sample
-   BLE peripheral role sample
-   BLE beacon sample
-   BLE advertiser sample

4、选择最新版本代码： 在 **Version** 中选择 “latest”。

最终配置结果如下图：

![](./figures/config-done.png)

配置完成后保存，studio 将自动更新下载软件包。

## 配置相关串口

1、在 RT-Thread Settings 下硬件选项页中使能对应串口，如下图，按照实际需求开启。

![image-20220819115330415](figures/config-uart.png)

保存退出。

2、在 borad.h 头文件中添加对应串口的引脚定义。

![image-20220819115637321](figures/config-uart-pin.png)

## 编译运行

1、这里使用 RT-Thread Studio 下 ART-Pi 开发板的示例工程 ` art_pi_blink_led ` 进行演示，添加和配置完成NimBLE软件包后，编译完成烧写到板子上运行。

注意：如果遇到无法下载的情况，可以对照下图看一下**构建配置**中**外部下载算法**是否有问题；

一般是显示： `${workspace_loc:/${ProjName}/board/stldr/ART-Pi_W25Q64.stldr}` , 有问题的话点击 Workspace 按钮重新添加一下。

![image-20220819003952936](figures/download_error.png)

2、串口连接蓝牙 Control 芯片（这里直接使用 ART-Pi 板载的 AP6216 芯片）。关于其他蓝牙控制器选择可以参考 [蓝牙控制器固件](https://github.com/RT-Thread-packages/nimble/tree/master/docs/firmwares) （或 NimBLE 软件包目录下 /docs/firmwares/README.md），注意替换 uart 设备。

3、连接串口终端，可以使用 `hlep` 看到 BLE 相关例程命令，运行即可，可以看到相关日志输出

![](./figures/sample-run.png)

使用 **nRF Connect** 手机 APP 即可成功观察到蓝牙设备，名称为 **blehr_sensor** ：

![](./figures/app.jpg)



 点击连接后，在 CLIENT 下即可看到 Heart Rate 相关数据。

![](./figures/app-connect.jpg)

