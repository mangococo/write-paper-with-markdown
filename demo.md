---
pandoc_args: ['--filter=pandoc-fignos', '--filter=pandoc-eqnos', '--filter=pandoc-tablenos', '--filter=pandoc-citeproc', '--bibliography=myref.bib', '--csl=chinese-gb7714-2005-numeric.csl']
title: "基于LoRa的饮水机水量无线监测送水系统设计"
output:
  word_document
---

---
fignos-cleveref: TRUE
fignos-plus-name: 图
fignos-caption-name: 图

tablenos-cleveref: TRUE
tablenos-caption-name: 表格
tablenos-plus-name: 表
...


# 摘 要
随着物联网应用的普及，人们越来越多地去考虑如何利用物联网使得生活的各个方面更加便携。而，饮水作为人类赖以生存的重要保证，也越来越被人们重视。如何使得饮水更加健康，更加便携智能，成为当下的迫切需要。近几年已经开始有智能饮水机方面研究的趋势。但大多将目光局限于“智能化”一词，研究如何使得饮水机具有更多的功能，但或多或少忽略了饮水机的本质——使得用户饮水更便捷。本论文提出了基于 LoRa 通信技术的饮水机水量无线监测送水系统。系统包括传感终端、手机应用、应用服务器端。

在低功耗广域网（Low Power Wide Area Network，LPWAN）产生之前，似乎远距离和低功耗只能二选其一，无线连接技术也不再满足于近距离通信，正在向着距离更远、覆盖更广的方向发展。当 LPWAN 技术陆续推广采用之后，通信网络规划设计人员可将远距离与低功耗两者兼顾，最大程度的实现高效通信传输，同时，大大降低中继的成本。LoRa 作为物联网通信技术的一种可以很大程度的提高系统的传输距离，降低系统功耗。该系统集成了单片机，重量传感器、LoRa 模块，用户终端。LoRa 作为通信模块将数据发送到应用服务器，然后数据被保存，分析。LoRa 通信距离能够达到约 3km 范围，TX 功率仅为 120mA，与其他无线技术的相比较功耗较低。此外，在系统中还设计了一个易于使用的图形用户界面和 App 应用。 首先，本论文完成了集成了传感终端的监测系统的硬件实现。采用重量传感器作为终端的微控制器控制传感器模块进行有规律的数据采集，将数据存储并通过 LoRa 传输给数据中心。

LORA 技术的核心在于数据的传输，首先对 LORA 数据包进行全面的定义及解析，然后由通信规约实现的业务逻辑引申出唤醒机制在整个系统中数据传递的作用，其次对智能送水饮水机送水系统中各模块的设计思路进行详细描述与验证，且通过对 SX1278 无线芯片 LORA 技术的研究，极大的扩充和填补了目前嵌入式无线通信领域的通信方式，为了提高饮水机送水效率。经过参考大量相关的国内外文献，并结合市场实际研发与应用的情况,在综合分析物联网关键技术的基础上，延伸出 SX1278 无线射频芯片的 LORA 技术。也为无线水量检测送水系统的发展带来了跨越式的机会。

关键词：LoRa; SX1278; 智能饮水机;

# Abstract

With the popularity of IoT applications, people are increasingly considering how to use the Internet of Things to make all aspects of life more portable. However, drinking water, as an important guarantee for human survival, is getting more and more attention. How to make drinking water healthier, more portable and intelligent, has become an urgent need. In recent years, there has been a trend in research on smart water dispensers. But most of them focus on the term "intelligence" to study how to make the water dispenser more functional, but more or less ignore the essence of the water dispenser - making it easier for users to drink water. This paper proposes a wireless monitoring water delivery system for water dispensers based on LoRa communication technology. The system includes a sensing terminal, a mobile application, and an application server.

Before the generation of Low Power Wide Area Network (LPWAN), it seems that long distance and low power consumption can only be chosen one by one. The wireless connection technology is no longer satisfied with short-range communication, and is moving farther and covering. A broader direction of development. After the adoption of LPWAN technology, communication network planners can combine both long-distance and low-power consumption to maximize efficient communication and reduce the cost of relays. As a kind of IoT communication technology, LoRa can greatly improve the transmission distance of the system and reduce the system power consumption. The system integrates a single chip microcomputer, a weight sensor, a LoRa module, and a user terminal. LoRa sends the data to the application server as a communication module, and the data is saved and analyzed. The LoRa communication distance can reach about 3km, and the TX power is only 120mA, which is lower than other wireless technologies. In addition, an easy-to-use graphical user interface and App application have been designed in the system. First of all, this paper completed the hardware implementation of the monitoring system integrated with the sensing terminal. The weight sensor is used as the terminal's microcontroller control sensor module for regular data acquisition, and the data is stored and transmitted to the data center through LoRa.

The core of LORA technology lies in the transmission of data. Firstly, the LORA data package is fully defined and parsed. Then the business logic realized by the communication protocol extends the role of the wake-up mechanism in the whole system, and then the intelligent water dispenser water delivery system. The design ideas of each module are described and verified in detail, and through the research on the SLO1278 wireless chip LORA technology, the communication method in the field of embedded wireless communication is greatly expanded and filled in order to improve the water delivery efficiency of the water dispenser. After referring to a large number of relevant domestic and foreign literatures, combined with the actual development and application of the market, based on the comprehensive analysis of the key technologies of the Internet of Things, the LORA technology of the SX1278 wireless RF chip is extended. It also brings leaps and bounds to the development of wireless water detection and water supply systems.

Key words: LoRa; SX1278; intelligent water dispenser;

# 0 引言

20世纪90年代，麻省理工学院的 Kevin Ashton提出了物联网的概念，并作如下定义：所有的事物都是通过传感器，比如无线射频识别(radio frequency identification，RFID)连接到互联网，实现智能化管理。[@xiaojiang2010services]物联网可以实现物理世界与网络空间的互联与整合，它代表了未来网络化的趋势，引领信息产业革命的第３次浪潮。信息网络已广泛地应用于日常生活的各个方面，提供了很多设备之间的互联，比如娱乐设备、家庭应用、飞机、个人电脑和各种传感器设备等[@ma2011internet]。饮水机在家庭、办公场所市场潜力巨大，然而传统的上电后独立工作的饮水机已经不能满足信息时代的需求，智能化和网络化是其发展的必然趋势。对于只能饮水机的研究，其他研究者已经针对“如何让饮水机功能更多元化”做出了足够多的付出与贡献。[@夏鲲2018基于物联网的智能饮水机系统设计与实现]

但是这些研究无不忽略了饮水机带给用户的困扰不仅仅是功能的单一，更是每次练习饮用水公司送水的烦恼。而本系统，针对传统饮水机智能化和网络化应用方面的不足，本系统基于物联网的基本架构，基于LoRa无线通信协议，辅以互联网交互，通过手机app与微信扫码技术与后台数据中心服务器进行数据交互，实现了对饮水机、用户、饮用水配送公司、服务器站点管理人员等数据的统一管理和维护。从而一定程度上地免去用户在购买，配送引用绶时的繁琐。

# 1 绪论
## 1.1  课题研究的背景及来源
生活用水一直是人们所关注的重要课题，而饮用水更是重中之重。目前，下置式饮水机由于其美观及卫生较上置式饮水机好，因此下置式饮水机越来越受人们欢迎。然而，下置式饮水机有一个体验不好的问题是无法直观地看到桶装水中的水量，当桶装水中缺水时用户也无法第一时间感知。如此时没有多余储备的桶装水可以更换，用户将面临没有水喝的问题，体验十分不好。而对于大多数居民而言，家庭饮用水主要还是以饮水机加送水公司的形式，而送水对于大多数居民而言仍是一大烦恼。在这样的大背景下，如何提高饮水机送水效率的重要性显得愈发重要。近些年来，随着城市的快速发展及人口大规模迁移，城市高层住宅规模也越来越大，对送水公司的需求也急剧增加，同时也造成送水工作的难度不断加大，同时增加了不必要的人力资源浪费。如何应对激增的用户数量，对庞大且分散的用户群提供进行及时、准确、有效的送水工作成为饮用水企业迫切需要解决的问题。传统的“用户呼叫要求送水--饮用水公司送水”的商业模式需要用户在需要时呼叫客服，又增加了用户的等待时间，效率非常低下，劳动强度大，也易因交通问题引起相当多不必要的纠纷。在这种情况下，传统的送水方式已经不能适应社会需求的发展，自动检测水量无线送水系统成为饮用水企业未来发展的必然趋势，然而除了检测水量功能，如何将用户需要饮用水的需求数据快速准确地传送给饮用水管理者，同样也变得十分的重要，从而顺应物联网发展的无线检测饮水机水量送水技术的发展应运而生[@赵静2016lora]。

饮水机水量无线监测送水系统是一种不需要用户主动请求送水服务就可以完成自动检测用户饮水机剩余水量，提供及时送水服务的智能化送水服务系统。该系统主要运用传感器技术、信号处理技术、通信技术、计算机技术等现代化技术，实现了数据采集、数据处理、数据传输、数据管理等内容。而水量无线监测系统根据通信媒介的不同可分为有线监测送水和无线监测送水两种方式，有线监测采用M-BUS、485 等总线方式，抄读快速，技术成熟，但有线通信需敷设大量长距离线路，数据传输稳定性会下降，且线路容易遭受到人为的破坏，造成故障点筛查难度增加[@赵静2016lora]。无线通信方式目前主要采用 GSM、GPRS、WIFI、ZigBee、LORA 等通信技术，自适应性极强，采用无线自动组网或通过无线通信公网的方式，安装、调试便捷，无需大量线路施工，大大降低线路成本和浪费，且易于维护，性能稳定，由于没有长距离敷设线路，相比于有线方式，其故障筛查更为精准与简便，更符合城镇化建设的发展需要[@戴国华2016nb]。

## 1.2 无线发射式水量无线监测送水方式
现代无线通信技术的迅猛发展为饮水机水量无线监测送水方式提供了更为先进的手段，即在饮水机内部集成无线发射模块，根据其应用的特殊性，此类无线发射模块
具备稳定的低功耗和长距离等优点。水量无线监测智能送水方式的应用，大大降低了施工和维护成本，但就表体而言实际成本略有增加。由于无线方式涉运营商通信频点和带宽的使用，所以需缴纳一定的费用，且民用通信产品对无线发射信号也会存在一定的同频干扰等问题。即便无线方式存在一些弊端，但就其优势而言则更为明显。随着 LORA、NB-IOT 等技术的突破，世界各国正大力建设低功耗广域网的基础骨干网，使得基于物联网的饮水机水量无线监测送水越来越受到青睐[@戴国华2016nb]。

# 2 LORA 技术基础
## 2.1 LORA 概述
LORA 技术是基于 SEMTECH 公司所研发的无线射频芯片，其采用线性调频扩频 LORATM 调制技术，用于将传感器采集的各类信号（例如声音、压力、磁场等）通过无线远距离发送，也可用于接收控制中心或手持自检预置器的命令。在低功耗方面，LORA 和FSK 调制都属于超低功耗的无线传输技术，但LORA 技术在通信距离和抗干扰能力方面，优势更为明显。其特殊的扩频技术和容错能力能在同一的频率下完美的接收接收来自不同频率终端的信号，避免了目前无线通信领域同频干扰的瓶颈，也使得嵌入式无线通信领域的产业局面也发生了彻底的改变。

## 2.2 LORAWAN 及其网络结构
LORAWAN 是基于LORA技术而制定的网络协议，它可以为使用电池供电的无线设备提供区域、国家或全球的网络连接，对嵌入式应用的研发具有强大的兼容性和可扩展性，能实现无缝连接。由于 LORAWAN 搭建了一整套的基础协议框架，使得全球从事物联网领域的厂家可以在此基础上进行产品的研发、生产、制造，且性价比相对其他网络更有优势。

LORAWAN 采用星型拓扑结构，支持区域级、城市级、省级、国家级等不同大小的网络规模，在一定规模的网络架构中，网关的数量往往会在关键节点大量部署，实现应用端和服务端中继路由、透明转发的功能，以达到全网覆盖的目的。

应用端设备调制解调出适用于LORAWAN的LORATM或 FSK 信号，与网关进行通信与数据传输，网关则以GPRS、3G、4G 等网络与后台服务端进行通信与数据传输。LORAWAN 中设备间的通信支持不同的信道频率，LORAWAN 网络传输速率范围在 0.3kbps~50kbps 之间，即便设备在同一频率，如果使用不同速率来进行通信，也不存在同频干扰的问题。自适应数据速率（ADR，Adaptive Data Rate）策略，可以改变实际的数据速率以确保可靠的数据包传送，优化网络性能和终端节点容量规模。ADR 策略科适应网络基础设施的变化，支持变化的路劲损耗。为使应用端设备的电池寿命和总体网络容量达到最大化，LORAWAN 通过 ADR 实现对每个应用端节点的数据速率和 RF 输出功率进行管理[@裴凤芹2013无线远传抄表系统关键技术;@原羿2004基于]。

![LORAWAN 网络架构图](https://i.loli.net/2018/09/28/5badca7c264f7.jpg){#fig:demo_lora_net_build}

- 终端节点（含传感器）
LORAWAN 网络中的终端无线模块采用 LORATM（线性扩频调制技术）或FSK（频移键控调制技术）来实现通信与数据传输，且适用于通信网络中的七层协议，LORA 技术的稳定性同时也为低功耗和远距离提供了重要保证。
- 网关/集中器
网关分布在 LORAWAN 的特定位置，其信号范围覆盖所有终端节点，一个终端节点发射的信号和数据可能被多个网关接收并处理，网关再将数据通过 TCP/IP协议发送至上行汇聚节点，由汇聚节点统一判断数据和信息的有效性[@裴凤芹2013无线远传抄表系统关键技术]。网关的路由路径可以根据信号的变化产生变化，实现路由和中继转发的功能。
- 网络服务器
网络服务器作为网关和应用服务器之间的桥梁，实现上行链路之间的底层数据包和控制逻辑的处理，以保障上行通信的安全性与可靠性。
- 应用服务器
应用服务器对数据进行最终的整合、分析与控制，以直观界面的形式将终端节点的状态、运行情况、控制情况等重要参数予以呈现，并根据管理者需要对终端节点进行参数调整等。许多现存的网络都是网状结构，在网状网络中，各个终端节点可以转发其他节点的信息，充当路由的功能，这样提高了网络通信距离和通信范围。但缺点是增加了系统的复杂性，降低了网络容量，并且为了接收和处理这些信息，电池的消耗量非常大，而这些信息中只有很少的部分和这个节点有关，节点电池消耗在处理与自己无关的信息上。LORAWAN 采用远距离星型结构，优势在于不仅能实现远距离连接，而且能够很好的保护电池寿命。

终端节点并不与特定的网关连接，相反，由终端发出的数据通常可以被多个网关接收，每个网关再通过 TCP/IP 网络（可以是 GPRS、WIFI、卫星或以太网）将接收到的数据包发往云网络服务器，服务器将通过时间表来提出多个网关发来的重复数据，如果一个终端是移动的，那么此功能可以保证数据的正常收发，并且可以侦测到静止终端的非正常移动，保证了资产安全，除此之外，服务器还会执行安全检查和自适应数据速率等，将智能性和复杂性转移到服务器，使设备得到简化[@裴凤芹2013无线远传抄表系统关键技术]。

## 2.3 LORA 数据包结构
LORA 调制解调器具有两种数据包模式：显示模式和隐式模式。区别在于，显示数据包模式有一个包含字节数、编码率以及数据包是否启用 CRC 校验等信息的报头。LORA 数据包(如{@fig:loradatagram})主要包含前导码、可选报头、数据有效负载和负载的 CRC校验。

![LORA 数据包结构](https://i.loli.net/2018/09/25/5ba9d0045b28e.jpg){#fig:loradatagram}

1．前导码（Preamble）
前导码是位于数据包起始处的一组 bit 组，前导码用于保持接收机与接收数据流之间的同步。前导码有长前导码和短前导码两种模式，其长度是一个可通过程序进行设置的变量，默认长度为 12 个符号，设置范围在 6 到 65536 之间。在接收数据量较大或者网络同步性要求较高的应用中，可通过缩短前导码的长度，缩短终端接收的占空比。前导码最小允许长度就可以满足通信要求，其可变长度主要应用在唤醒设备及兼容其他网络设备的过程中。终端会定期检测发射机信号的前导码，接收机只有检测到前导码长度等于自身设定的长度时才会开始接收数据，并向CPU发出中断或者将中断寄存器置1，以供CPU定时查询该寄存器，否则保持休眠状态。

2．报头（Header）
通过对寄存器的设定，可以选择两种不同的报头。显式报头是默认的操作模式，报头主要包含有效负载的相关信息，包括有效负载字节数、前向纠错码率和
是否打开 16  位负载  CRC  校验。如果显式报头的相关信息在开发时已确定，则可以选择隐式报头来缩短发送时间，同时需要为接收机和发射机软件写入已知的有效负载长度、前向错码率和  CRC。

3．有效负载（payload）
有效负载实际上就是数据段，即你要发或者要收的数据。在实际传输中，指令不同，返回数据不同，所以数据包有效负载长度并不是固定的。显式模式下长度在报头中指定，隐式模式下长度可通过寄存器来决定。LORA 数据包中采用了自动纠正传输误码的前向纠错编码技术，数据包中增加了冗余信息，允许接收方检测可能出现在信息任何地方的有限个差错，并且通常可以纠正这些差错而不用重传，从而实现数据包在传输过程中的自我修复，但前向纠错编码技术需要消耗固定的带宽。该项技术多用于大型组网中信号远距离传输或复杂路径衰减时体现出优异的纠错功能。注入前向纠错编码的 LORA 数据包将每一比特时间划分为众多码片，划分的范围为 64-4096 码片/比特（ZigBee 仅能划分的范围为 10-12 码片/比特）[@裴凤芹2013无线远传抄表系统关键技术;@原羿2004基于]，所以即便调制噪声较大，LORA 也能完美地进行应对。

## 2.4 LORA 唤醒方式
在星型的网络结构中，为了延长电池寿命，终端节点通常需要唤醒来进行数据传输。唤醒方式不同，数据的传输方式也不同，可分为以下两种：

- 主动唤醒
终端利用  MCU  内部定时器或者  RTC  时钟定时将设备唤醒，唤醒后终端主动将收集到的数据上传到服务器，随后再次进入休眠态。此种方式的唤醒适用于数据更新周期较长的设备，且不需要服务器突发访问该设备。

- 空中唤醒
此种方式适用于需要突发试访问的设备。终端设备和服务器并没有约定某个时刻彼此通信，服务器随时都可能读取终端中的数据。在  LORA  通信中，空中唤醒过程如下：若终端休眠时间为  T  秒，那就意味着终端每  T  秒主动唤醒一次，但是并不是为了主动上传数据，而是主动检测是否有设备发送前导码。当服务器需要和某个终端链接，会通过网关发送持续  T  秒的前导码，以覆盖终端的休眠周期，确保终端主动唤醒后可以检测到前导码。若终端唤醒后检测到前导码，则进入正常状态，立即接收处理数据，若主动唤醒后没有检测到前导码，则立即进入休眠态，以节省电量。

本设计是基于 LORA 技术在智能检测水量送水服务上的应用，而数据交互往往是只有当水量低于阈值时才会触发，且往往对于不同的用户而言其触发时机也不同，所以应采用主动唤醒的方式（依靠传感器触发脉冲唤醒射频模块）。

## 2.5 LORA 空中传输时间
在空中唤醒时，需要连续发送覆盖接收设备休眠周期的前导码，前导码传输时间的计算对准确唤醒设备至关重要，所以前导码的空中传输时间将在计算空中传输时间时单独介绍。在设备初始化时，用户可设置的关键参数包括信号带宽($BW$)、扩频因子($SF$)、编码率($CR$)，利用公式 ({@eq:L_s}) 计算 LORA 符号速率：

$$
L_s=\frac {2^{sf}} {BW}
$$ {#eq:L_s tag="2.1"}

前导码的传输时间通过公式 ({@eq:T_p}) 来计算:
$$
T_p = ({N_p}+4.25)L_s
$$ {#eq:T_p tag="2.2"}

$N_p$表示已设定的前导码长度，其值可通过编程来确定。当已知休眠时间$T_p$，就可通过公式({@eq:T_p})来确定需要设定前导码的个数。除了前导码，空中传输时间还包括报头和有效负载，其符号个数 $N_d$可通过公式 ({@eq:N_d}) 来计算：

$$
N_d=8+max(ceil[ \frac {(8PL-4SF+28=16CRC-201H)}{4(SF-2DE)}](CE+4), 0)
$$ {#eq:N_d tag="2.3"}

公式中符号含义如下：
- $PL$  表示有效负载的字节数(范围在1到 255)
- $SF$  表示扩频因子（范围在 6 到 12）
- $IH=0$  表示使能报头；$IH=1$  表示禁用报头
- 开启低数据速率优化时,$DE=1$；否则 $DE=0$
- $CR$表示编码率(范围在 1 到 4)报头和有效负载的空中传输时间 $T_d$为：

$$
T_d=N_d \times L_s
$$ {#eq:T_d tag="2.4"}

数据包空中传输时间即为公式 ({@eq:N_d}) 和公式 ({@eq:L_s}) 的和：

$$
T_{packet} = T_p + T_d
$$ {#eq:T_packet  tag="2.5"}

## 2.6 LORA 跳频
跳频扩频技术  FHSS(Frequency-Hopping  Spread  Spectrum)，是指收发双方在同时且同步的情况下，按照事先约好的跳频图案跳转通信频率。无线通信的健壮性主要来自外部干扰和多经衰退这两方面的挑战：外部干扰主要来自生活中经常使用无线通信如手机、无线路由、电台、遥控玩具等；多经衰退比较复杂，分析其全部影响因素几乎不可能，在实际环境中墙壁、门、树木、建筑物以及走动的人都可能造成信号的反射，所以收发双方除了无线信号直线传播路径外，还存在着多重反射路径，这些信号混合后可能造成很大的干扰[@米沃奇2016n]。解决外部干扰和多径反射的措施就是跳频技术，通过跳转通信频率用以规避某频段的干扰和信号反射。

LORA  跳频原理为：跳频发射和接收都始于信道 0。发射机将前导码和报头首先在信道 0 发送，前导码和报头发送完成后产生第一次跳频中断信号，MCU 响应中断，按照事先约定的频率跳转到信道 1，完成第一次跳变，跳转的同时信道计数器开始计数，计满一个跳变周期时，发出跳频中断，MCU  响应中断跳转到信道 2，再次重复上述过程。接收机接收从信道 0 开始，有效前导码和报头接收完成后，同样执行发射机的跳频流程。

信道驻留时间为单个符号发送时间的整数倍，可通过寄存器设定，且必须大于完成跳频所需的时间，这样数据才能在每个信道剩余时间发送。{@fig:lora_jump}为 LORA 跳频流程，当前导码和报头首先在信道 0  发送和接收完成后才进行跳频，有效负载则在多个信道内被分段发送接收。

![LORA 跳频流程](https://i.loli.net/2018/09/28/5badcb3169fed.jpg){#fig:lora_jump}

# 3 水量无线监测送水系统架构
## 3.1 系统总体架构
物联网可分为感知层、网络层和应用层3个层次。底层是感知层，主要用于物理世界中物体信息的感知、采集、汇聚和识别，包括RFID标签和读写器、全球定位系统（global positioning system，GPS）、传感器等，大量的底层结构按照系统需要合理的分布在物理空间中，大部分底层结构都必须同时具有路由和信息采集的双重功能；第2层是网络层，用于传输和处理感知层获得的信息和数据，并为应用层提供可靠的通信和处理支持；顶层是程序应用层，用于智能化的处理数据，各种类型的数据聚合，交互显示等, 如{@fig:lora_connect}所示网络连接结构。[@zhang2011security;@胡向东2010一种改进的物联网感知层簇维护优化算法]。

![LoRaWAN网络连接框架](https://i.loli.net/2018/09/28/5badcb6220297.jpg){#fig:lora_connect}

依照上述思想,基于LoRa的饮水机水量无线监测送水系统的设计如{@fig:sys_design}所示，分为3个层次。
1. 第一层，采集层。采集模块是整个系统的最基础模块，也是最重要的数据来源。采集模块即包含用于监测采集水量信息的重量传感器，用于LoRa通信的LoRa射频模块和LoRa网关；也包含了用户终端app，用户所选择的预约时间也是极为重要的数据源。采集过程，由重量传感器监测、收集饮水机水量信息，交给微处理模块进行简单判定（仅为简单的比较运算）后经由LoRa射频模块，LoRa网关，广域网发送至应用服务器。
2. 第二层，服务器层。即应用服务器，其主要作用是云存储、计算、处理由采集层所提交的数据。应用服务器会对采集层说提交的监测水量进行判别，并根据用户以往所选择的预约送水时段进行计算，由此为用户推荐更符合用户需求，更加人性化的服务。
3. 第三层，终端层。主要为用户群与用户移动端app。移动app是用户与服务进行交互的有限的途径之一，当用户的饮水机水量告竭时，服务器会收到由LoRa发送的剩余水量信息，服务器对这些信息进行处理后，向用户终端发送“邀请预约送水服务”的推送。而服务器会收集用户的反馈（用户对于推送的响应时间，用户选择的预约时间，哪怕是用户未作相应……），进而通过这些数据为用户提供更加符合用户习惯的服务。
![系统的设计总图](https://i.loli.net/2018/09/28/5badca7c264f7.jpg){#fig:sys_design}

本系统所设计的基于物联网的智能送水饮水机系统的感知层主要为重量传感器，用于实现检测饮水机剩余水量的功能；网络层主要为LoRa、TCP/IP、HTTP等，，LoRa模块作为数据采集点向LoRa网管上行由重量传感器采集的信息，而LoRa网管通过接入广域网与数据服器进行交互；完成底层和程序应用层之间的远距离数据和信息传输；程序应用层主要为MySQL数据库和服务器站点等，将从底层和网络层传输的大数据进行整合、处理，具有强大的通信和存储功能。最后，用户终端app以主观行为影响和控制整个智能送水饮水机系统，从而实现物与物、人与物、物与人之间信息的互联互通。工作流程如下：

<!-- ![](https://i.loli.net/2018/10/05/5bb645954cff4.jpg) -->

```flow
st=>start:  开 始
connect=>operation: 饮水机检测系统通过LoRa模组
发送水量过低数据
first=>operation: LoRa网关将数据通过广域网
发送至应用服务器
second=>operation: 应用服务器处理数据并告知用户
饮水机水量过低，询问是否预约送水（用户响应时长为 t ）
judge_warn=>condition: 检测剩余水量是否到达警戒值？
cond=>condition: 用户在 t 时长内作出相应?
user_agree=>condition: 用户同一预约送水？
inspect_balance=>condition: 检查用户账户余额是否充足？
later_send=>operation: 延时 T
third=>operation: 安排送水
recharge=>operation: 引导用户充值
recharge_agree=>condition: 用户同意充值？
end=>end: 结束

st->connect->first->second->cond
cond(yes)->user_agree
cond(no)->judge_warn

user_agree(yes)->inspect_balance
user_agree(no)->end

judge_warn(yes)->later_send->second
judge_warn(no)->end

inspect_balance(yes)->third->end
inspect_balance(no)->recharge->recharge_agree

recharge_agree(yes)->inspect_balance
recharge_agree(no)->end

```

## 3.2 LoRa监测送水系统部署

如{@fig:lora_gateway_communication}所示，将一定范围（以LoRa所支持的城市无线传输距离为依据）内的社区划分为一个区域，进而为该区域设置一个LoRa网关。

![区域LoRa模块与LoRa网关进行数据传输](https://i.loli.net/2018/09/28/5badcbe2b006b.png){#fig:lora_gateway_communication}

为了实现万物互联，需要一个服务器处理经由loRa的上行数据，而由于LoRa网关可以接入广域网，所以我们只需要为某一范围（以城市或地区为宜）的区域设定一个网络服务器，设计如{@fig:demo02}所示。

![区域LoRa模块与LoRa网关进行数据传输](https://i.loli.net/2018/09/28/5badcc04b9421.png){#fig:demo02}

## 3.3 智能送水饮水机硬件设计方案

感知层及数据收集层工作主要由称重传感器完成，称重传感器是一种将质量信号转变为可测量的电信号输出的装置。本系统的传感器模块采用数字式称重传感器。数字式传感器系统是在传统电阻应变式传感器基础上，结合现代微电子技术、微型计算机技术集成而发展起来的一种新型电子称重传感器。数字式称重传感器是由模拟传感器（电阻应变式）和数字化转换模块两部分组成的。数字模块由高度集成化的电子电路，采用SMT表面贴装技术制成，主要包括放大器、A/D转换器、微处理器（CPU）、存储器、接口电路（RS485）和数字化温度传感器等。

数字式称重传感器具有以下特点：
- 数字式传感器采用集成化的A/D转换电路、数字化信号传输和数字滤波技术，传感器的信号传输距离较远，可达1200m，抗干扰能力强，数字传感器内模拟信号的传输距离极短，同时传感器外壳（弹性体）本身又是一个良好的屏蔽罩，仅这两个特点就决定了其抗干扰能力的优势，在很大程度上提高了传感器的稳定性。

- 保密性好，具有防作弊功能，能有效防止遥控器作弊，一旦发现就会自动采取出错报警，有力保障了数据的安全性与准确性。使用模拟式传感器的汽车衡被安装作弊器的情况比较普遍，首秦公司周围的几家有模拟式汽车衡的公司几乎全被安装过作弊器，造成了很大的经济损失。由于作弊器本身体积小，加之安装极其简便，因此不容易被发现，给计量数据的安全性造成了极大的隐患。首秦公司的大型贸易衡器（除动态轨道衡外）已经全部进行了数字化改造，成为数字式汽车衡，从而使计量数据的安全性得到了良好的保障，也更好地维护了首秦公司的经济利益。

- 由于数字式传感器具有自动采集预处理、存贮和记忆功能，并具有唯一标记，多只传感器并联组秤后可分别检查每个传感器的状态，便于故障诊断。首秦公司现在使用的数字式传感器为大和公司生产的YCCA-Ⅱ-D型，单只传感器承载量为50000kg。最初，首秦公司的汽车衡及轨道衡所使用的均为模拟式传感器，设备工作状态不稳定，每次出现问题很难判断是哪一只传感器有问题，只能通过打开接线盒检查每一根传感器的信号情况，用排除法逐个进行检查，花费很长时间才能找到有问题的传感器。问题解决后还要联系质监局的检定人员进行检定，检定费用从正常的一年两次也变为多次，人力、物力、财力都造成了很大的浪费，最重要的是给公司的正常生产带来了负面影响。自从进行了数字化的改造以后，以前出现过的诸多问题都已经解决，设备状态良好，运行比较稳定，即使在二期工程投产后，公司每日的进料量比以前增加了1倍的情况下， 数字式传感器的状态仍非常稳定，没有出现过任何问题，充分证明了数字式传感器的优越性能及其在实际生产中起到的重要作用。
四、.数字传感器在出厂时均已进行量化处理,一致性极佳,替换后无须重新标定. 2.每只数字式传感器均带高精度A/D块及CPU,所以上位机能读取每只数字式传感器的实时状态,并以此作为监控传感器是否处于正常工作状态. 3.采用标准异步串行接口RS-485通讯,所以传输远(可达几公里)且抗干扰能力强.

重量传感器通过测量由重量产生的静压力来确定重量高度，通过LoRaWAN™无线模块将数据上报到网关，用户通过后台系统看到相关数据。

![数字式称重传感器](https://i.loli.net/2018/10/04/5bb5e337a8854.jpg){#fig:weight_sensor}

该智能饮水机的数据采集层，主要实现对饮水机中饮用水水桶水量的检测，通过重量传感器进行检测，达到特定重量时响应，并将采集到的信息数据反馈给单片机，通过单片机中数字运算与逻辑比较，判断饮水机水桶剩余水量，以及是否达到临界值，从而决定是否通过 LoRa 模块向服务器发送数据。即，根据所述桶装水的重量计算水桶中的储水剩余量。涉及的硬件有单片机开发板、重量传感器等，可利用重量传感器饮水机中饮用水水桶水量的实时检测。智能饮水机工作流程如{@fig:flowmcu}所示。
1. 数字式传感器实时检测智能检测水量饮水机中桶装水的重量
2. 单片机根据所述桶装水的重量计算水桶中的储水剩余量
3. 判断所述储水剩余量是否小于预设水量
4. 若所述储水剩余量小于所述预设水量则判定智能检测水量饮水机缺水，则将饮水机的剩余水量通过LoRa模块上行发送；否则结束。

<!-- ![饮水机内部设计流程](https://i.loli.net/2018/10/05/5bb646efdd2ae.jpg){#fig:flowmcu} -->

```flow
st=>start: 开始
get_data=>operation: 称重传感器实时监测重量
wakeup=>operation: 饮水机水桶水量过低，称重传感器唤醒电路
first=>operation: 称重传感器将数据反馈
给单片机
cond=>condition: 水量是否小于临界值?
lora=>operation: 通过LoRa模块上行水量数据
end=>end: 结束

st->get_data->wakeup->first->cond
cond(yes)->lora->end
cond(no)->end
```
{#fig:flowmcu}

上述检测过程依赖于以下条件：，包括:
1. 获取所述水桶的空桶重量；
   - 首次使用时，接通饮水机电源，并放置空水桶通过按键进行初始化；
   - 通过对未装水的所述水桶单独称重以获取所述水桶的空桶重量；并通过用户终端 app 进行初始化

2. 根据所述桶装水的当前重量和所述空桶重量计算所述水桶中的储水重量，并进一步根据所述储水重量计算所述储水剩余量。

综上，饮水机内部数据采集主要模块如{@fig:collect_data_model}：
- 检测模块，所述检测模块用于实时检测智能检测水量饮水机中桶装水的重量；
- 计算模块，所述计算模块用于根据所述桶装水的重量计算水桶中的储水剩余量；
- 判断模块，所述判断模块用于判断所述储水剩余量是否小于预设水量；
- 判定模块，所述判定模块用于饮水机储水剩余量小于所述预设水量时，判定饮水机缺水，并提交数据给 LoRa 模块。

![饮水机内部数据采集模块工作划分](https://i.loli.net/2018/10/05/5bb641db59a8e.jpg){#fig:collect_data_model}


提示模块，所述提示模块用于在判定智能检测水量饮水机缺水时，发出缺水提示信息。8.根据权利要求6或7所述的下置式饮水机的缺水检测装置，其特征在于，所述计算模块包括:
获取单元，用于计算所述水桶的空桶重量；
计算单元，用于根据所述桶装水的当前重量和所述空桶重量计算所述水桶中的储水重量，并进一步根据所述储水重量计算所述储水剩余量。
9.根据权利要求8所述的下置式饮水机的缺水检测装置，其特征在于，所述获取单元通过对未装水的所述水桶单独称重以获取所述水桶的空桶重量。
10.根据权利要求8所述的下置式饮水机的缺水检测方法，其特征在于，所述获取单元通过按键输入的方式获取所述水桶的空桶重量。




### 无线协议栈发送流程
无线模块软件设计是基于 SX1278 芯片的，其关键核心流程在于无线协议栈发送和接收逻辑。如图{@fig:wireless_protocol_process1}所示，无线数据在发送时，会启动发送流程事件程序，进入流程后首先检测 SX1278 芯片是否空闲，不空闲则在超时后进入复位事件程序停止数据发送，结果发送给网络层，如为空闲则调用 SX1278 芯片冲突回避机制接口进行空中信号检测，若有干扰则再次进入复位事件程序停止数据发送，无干扰则调用 SX1278 芯片的发送接口将数据发送出去[@李善荣2011Si1000;@RonPrice2008无线网络原理与应用]。 无线数据在接收时，模块软件设计流程相对简单，在 SX1278 底层接受到数据时，会触发一个检测帧格式的事件，只对接收到的数据进行数据包格式上的判断，格式正确则回复 ACK 应答帧给网络层进行拆解应用，不正确则丢弃数据[@王瑞2015基于]


```flow
start=>start: 开始
oper1=>operation: 触发MAC_EVT_RADIO_TX_START事件
（此事件启动发送流程）
cond1=>condition: 检查SX1278是否空闲?
cond2=>condition: 判断发送是否会超时？
oper2=>operation: 1秒之后触发
MAC_EVT_RADIO_RETRANSMIT事件
（此事件将重新尝试发送）

end=>end: 结束

start->oper1->cond1
cond1(yes)->end
cond1(no)->cond2

cond2(yes)->end
cond2(no)->oper2->cond1

```
{#fig:wireless_protocol_process1}


## 3.4 服务器软件设计


其中，应用服务器，即服务器通过智能网关和HTTP协议等已编写的PC端应用程序，其工作可以概括为两部分：判断由LoRa发送的数据并通知用户预约送水时间；根据用户的反馈，预约时间，响应时间进行数据分析进而为用户提供更贴切的服务。同时，现场控制系统将检测到的异常信息（溢流、非法入侵、紫外线灯管损坏等）或者出水状态信息上传到远程监控系统，智能送水饮水机的出水状态信息主要包括刷卡、扫码等。

数据库中包含饮水机设备和用户的实时数据信息，PC端应用程序可以对数据库信息进行查、改、增、删等操 作。现场控制系统主要包含中控屏显示模块、实时温度采集模块、水量监测模块、4G或WIFI数据传输模块、流量计计数模块、电磁阀驱动模块、加热器模块以及消费模块。消费模块主要包括微信扫码充值、刷卡消费、扫码消费、M1卡防克隆安全体系、中控屏数据通信与显示等，根据功能需求，控制相应的执行机构实现对应功能，并实时传输饮水机当前状态数据。各子系统设计相对独立，依赖聚合性较低，具有很好的 可操作性、可扩展性和应用性。[@xia2016design]

# 参考文献



A B
- -
0 1

Table: Caption. {#tbl:id}

|Name|B|C|
|:---|---|----|
|hw|123|1212|
|tw|123|1212|

Table: 测试. {#tbl:test tag="2.1"}

例如{@tbl:test}
