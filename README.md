<p align = "cenetr>
# 基于云平台的智能语音交互式灌溉系统
            </p>

[Demo视频演示](https://www.bilibili.com/video/BV1aV411W7Gf?share_source=copy_web)
   
> 项目已于2021年5月停止维护，信息仅供个人实现参考，相关问题不提供解答服务！

**摘要：** 为实现小型农业灌溉系统的信息化和自动化，设计了一款基于云平台的智能语音交互式灌溉系统。通过空气温、湿度以及土壤湿度、环境光敏传感器获取作物相关信息，并通过Arduino和Esp8266配合，进行数据收发、处理，相关指令操作。最后，接入机智云云服务平台，实现作物数据可视化，并通过客户端实现数据实时监测，可通过客户端或语音模式实现操控，实现自动化或远程手动作物补光和灌溉两大功能。

**关键词：Arduino；Esp8266；物联网；传感器；机智云平台；语音交互；**

 

**0** **引言** 

在学校农机特色的感召、物联网产业的高速发展、语音助手的普及下，特设计出基于云平台的智能语音交互式灌溉系统。本系统以ESP8266模块取代传统蓝牙模块，整体设计功耗低、较稳定，通过多种必需传感器和云平台在一定程度上实现了小型灌溉系统的信息化、自动化、数据可视化、语音交互。该灌溉系统可应用在浇花、育苗等小型灌溉场景，实现远程监控以及补水、补光主要功能，亦可通过语音交互实现相关操作，提升系统智能化、交互性。

**1** **灌溉系统总体设计**

**1.1 软、硬件配置**

硬件：Arduino开发版、RGB LED模块、ESP8266模块、水泵、软管、4*4按键模块、光敏电阻传感器、语音传感器、继电器、温湿度模块、土壤湿度传感器、电机驱动模块、杜邦线、面包板、硬板、螺丝

软件：Arduino IDE、机智云客户端、机智云云平台、MCU代码、Arduino控制程序

**1.2** **设计原理**

基于云平台的智能语音交互式灌溉系统以Arduino开发版为主控板，集成空气温、湿度以及土壤湿度、光敏电阻传感器获取作物相关数据项，并通过ESP8266将设备注册到机智云平台，在机智云WEB端实现作物数据统计、可视化，便于观测、分析作物环境变化，最后，将传感器获取数据与作物生长所需的光照、湿度阈值比较，实现自动化灌溉、线性化补光，也可通过机智云客户端实现远程数据实时显示和灌溉、补光两大操作，亦可通过语音传感器发出指令。                 

**2** **灌溉系统硬件设计**

**2.1 Arduino 开发版**

Arduino是一个基于开房原始码的软硬件平台，可以通过Arduino IDE调用原码和编程控制，编程语言为C语言。本系统采取Arduino UNO R3开发版在于Arduino能通过各种各样的传感器来感知环境，通过控制灯光、声音、马达等来反馈、影响环境，是非常容易上手的编程学习和应用平台。采取该开发版带来的好处是具有价格低廉、多种供电方式、丰富的I/O接口、模拟口等特点，从而使得灌溉系统整体功耗更低、稳定性更佳、扩展性更强。

**2.2 传感器模组**

环境温湿度传感器采用DHT11温湿度模块，配合土壤湿度传感器，负责实时获取作物环境的温湿度以及土壤的湿度数据最终上传显示到机智云客户端，便于作物管理员了解作物环境状态，采取相应措施。

**2.3** **通信模组**

灌溉系统的通信模组采取ESP8266 CH340G物联网测试板带，该模块基于从expressif esp8266系芯片探索而来。该模块与Arduino开发版配合，支持MQTT、AIRLINK、SOFTAP三种通信模式，上传传感器数据至云端并显示到手机端，同时实现手机端的远程操控。

**2.4** **语音模组**

为实现灌溉系统的智能化、交互性，特集成语音传感器LD3320开发版。通过IIC通讯设置语音识别语句和获取识别结果，方便快捷，支持三种识别模式（循环检测、口令触发、按键触发，本系统采用循环检测模式）和掉电脑保存功能。模块可扩展性强，可添加50条识别语句，便于提升识别准确度和灵敏性。语音模块可实现补光、灌溉两大功能。

**2.5** **灌溉模组**

灌溉模组采用5V自吸水泵，体积小、移动性良好，吸程和扬程满足灌溉需求。Arduino烧录程序将土壤湿度传感器获取的土壤湿度与阈值对比，实现水泵的自动开合，便于管理，亦可手机远程控制、中断。

**2.6** **光照模组**

光照在作物的生长中具有重要地位，因此在设计之初便考虑添加三色LED单片机开发小板，实现光弱环境下的补光需求。Arduino烧录程序将光敏电阻获取的实时环境光强与作物最佳光强阈值对比，通过线性函数实现自动线性补光，亦可通过手机远程控制，同时支持R、G、B值提供多种色源，使得不同作物获得最佳稳定光强和光色。

**3 灌溉系统软件设计**

**3.1 Arduino 烧录程序设计**

首先，通过机智云开发者中心创建灌溉系统产品并生成唯一Product Key（设备配网注册到机智云云平台的唯一身份识别码），添加所需数据点，通过MCU生成通讯协议与硬件程序代码（如图5），最后在该代码中设计灌溉系统的Arduino烧录程序。

**3.2** **机智云客户端**

利用机智云客户端选择对应配网模式将灌溉系统注册到机智云云平台.

**3.3** **机智云云平台网页端**

机智云云平台网页端可实现作物相关数据可视化。

同时，支持灌溉系统活跃状态。

**5** **灌溉系统提升空间**

该灌溉系统的语音交互功能保留了一定的提升空间，因为本系统采取语音传感器这一措施在当下手机人工助手（以小米语音助手小爱同学为例）大量普及的背景下缺乏技术性。可在Arduino烧录程序中调用相关库文件以及小爱开发平台API接口，将设备接入小爱语音助手，利用小爱同学的自然语言处理大大提升设备语音交互的准确度、灵敏度、稳定性以及操控性。

同时，本灌溉系统的Arduino开发版的I/O口丰富，可根据作物实际需求增减传感器类型及其数目，以达到不同场景下的理想效果。

**6** **灌溉系统小结**

本文设计了一款基于云平台的智能语音交互式灌溉系统，可应用在养花、育苗等小型农作物栽培场景，具有一定的信息化、自动化、交互性。该系统以Arduino开发板为主控区，通过温湿度传感器、光敏电阻传感器与对应阈值进行对比，控制光照模组和灌溉模组的自动化开关，同时支持手机端（即机智云客户端）模式或语音模式实现对作物数据实时监测、光照、灌溉等功能，机智云网页端实现作物数据的统计、可视化，方便作物管理员对作物生长环境状态整体把控，做出对作物更针对性的管理。
