# Electrical Control Workshop

- [Electrical Control Workshop](#electrical-control-workshop)
  - [电路基本知识](#电路基本知识)
    - [1. 欧姆定律](#1-欧姆定律)
    - [2. 电阻电容电感](#2-电阻电容电感)
    - [3. 电源](#3-电源)
    - [4. 半导体](#4-半导体)
    - [5. 信号](#5-信号)
  - [嵌入式基本知识](#嵌入式基本知识)
    - [1. 什么是单片机，什么是SBC，与电脑区别](#1-什么是单片机什么是sbc与电脑区别)
    - [2. 嵌入式开发特点](#2-嵌入式开发特点)
    - [3. 外设](#3-外设)
  - [代码基本知识](#代码基本知识)
    - [1. c/cpp](#1-ccpp)
    - [2. micropython](#2-micropython)
  - [开发工具和环境](#开发工具和环境)
    - [1. Arduino IDE](#1-arduino-ide)
    - [2. VSCode + Platform IO](#2-vscode--platform-io)
    - [3. Clion + Platform IO](#3-clion--platform-io)
  - [传感器](#传感器)
    - [1. 传感器 与 传感器模块](#1-传感器-与-传感器模块)
    - [2. 常见传感器种类及使用](#2-常见传感器种类及使用)
  - [电机](#电机)
  - [机器人框架](#机器人框架)
  - [控制算法](#控制算法)
  - [demo](#demo)
  - [扩展](#扩展)

----------

## 电路基本知识

### 1. 欧姆定律

`notice` *欧姆定律只适用于纯电阻电路，但是仍然可以帮助分析很多电路*

欧姆定律的标准式是 `I=U/R` ，通过这个式子我们能够获得更多更有用的式子，比如 `P=U^2/R = I^2*R`。以及更加重要的经验:"串联分压并联分流"
在分析电阻的作用时，欧姆定律非常有用。

### 2. 电阻电容电感

notice *各种元件不可避免都具有一定的电阻电容电感，此处所指为电阻器，电容器，电感器*

1. 电阻
不同于初高中物理中的电阻，实际开发中我们不太会单独需要电阻来作为用电器消耗电能。
<details>
<summary>那么电阻可以用来干什么?</summary>
<code>
  上拉下拉
</code>
</details>

1. 电容
简单而言，电容是尽量保持电容两端的电压不变的元件
比如当电容与一个灯泡并联时，我们断开电源供电，如果没有电容电灯泡很快就会灭，但是有电容的情况下，电容会放电提供电能给电灯泡，也就是尽量保持电灯泡两端电压不下降。
`主要作用:滤波,耦合,降压,隔直,储能,旁路,谐波`

1. 电感
简单而言，电感是尽量保持通过电感的电流不变的元件。
比如当电感串联在电路中时，如果电流突然下降，则电感会提供额外的电流。
`主要作用:隔交,滤波,变压,储能`

### 3. 电源

1. 电池分类

2. 一些有用的知识(此处就以锂电池为例)
   1. 锂电池容量
      mAh  Wh
   2. 锂电池常见型号
      18650   21600 --> 代表的是电池外观大小
   3. 动力电池性能
      几S电池:表示几节电池串联
      几C电池:表示放电倍率(不是电容量)   公式:  `最大电流(A) = C * Ah`  *比如20c 1000mAh电池的最大电流为 20A.*
   4. 电池组与平衡充电
      锂电池组通常是单串联，但是有时为了增加容量也可以同时并联和串联。
      串联并联锂电池之前一定要保证电池之间性能相近，电压相近，建议使用全新满电同型号电池串联并联，且串联并联后不要改变电池组排列。
      串联锂电池之后，充电需要考虑平衡问题。
      一些动力电池不包含充电保护板，需要使用平衡充电器。其他电池组可能自带平衡充电板，不需要平衡充电器。

### 4. 半导体

1. 二极管
2. 三极管
3. mos管

### 5. 信号

信号主要分为两大类，数字信号与模拟信号。

1. 数字信号
是指自变量是离散的、因变量也是离散的信号，这种信号的自变量用整数表示，因变量用有限数字中的一个数字来表示。在计算机中，数字信号的大小常用有限位的二进制数表示。
绝大多数信号都是数字信号，比如开关的状态是开或关，对应1和0。计算机中数据也都是数字化的。

    > 优点：便于计算机处理，抗干扰性强。
    >
    > 缺点：丢失精度

2. 模拟信号
是指在时域上数学形式为连续函数的讯号。例如声波，是典型的模拟信号。
现实中很多信号的形式是模拟的而非数字的，但是数字计算机不具有直接存储模拟信号的能力，因为数字是有限的而模拟信号理论上来说分辨率是无限的。
比如一段音频文件，我们可以看到它具有一些属性：采样率、位深度等等。这些是模拟信号数字化的体现。
本质上，存储在电脑里的音乐文件是数字化的，尽管声音通常是模拟信号。

    > 优点：精度高，在特定情况下能解决数字系统无法解决的问题。
    >
    > 缺点：不论是存储，计算，转换都存在问题，容易混入噪声。

> 为什么要介绍这个?
>
> 不同于电脑上软件开发,基本只会接触数字信号. 两种类型的数据在嵌入式开发中都很常见. 比如一些传感器的返回值是模拟量(以电压高低),另一些传感器只有开关两种状态,或者采用数据总线

----------

## 嵌入式基本知识

### 1. 什么是单片机，什么是SBC，与电脑区别

1. 单片机

    > e.g. stm32,arduino,esp32等等

单片机也被称为单片微控器，属于一种集成式电路芯片。在单片机中主要包含CPU、只读存储器ROM和随机存储器RAM等，多样化数据采集与控制系统能够让单片机完成各项复杂的运算，无论是对运算符号进行控制，还是对系统下达运算指令都能通过单片机完成。 由此可见，单片机凭借着强大的数据处理技术和计算功能可以在智能电子设备中充分应用。简单地说，单片机就是一块芯片，这块芯片组成了一个系统，通过集成电路技术的应用，将数据运算与处理能力集成到芯片中，实现对数据的高速化处理。

2. 单板机（SBC）

    > e.g. 树莓派,香橙派,Jetson nano等等

单板机是把微型计算机的整个功能体系电路(CPU、ROM、RAM、输入/输出接口电路以及其他辅助电路）全部组装在一块印制电板上，再用印制电路将各个功能芯片连接起来。单板机一般而言性能要比单片机强很多,可以运行操作系统(比如 linux  andriod windows)
不严格的说,笔记本电脑的电路板拿出来也算单板机,但是一般而言SBC指代的是比较小的单板电脑.

![rpi](img/rpi.png)

3. 比较

单片机又称单片微控制器，它不是完成某一个逻辑功能的芯片，而是把一个计算机系统集成到一个芯片上。相当于一个微型的计算机，和计算机相比，单片机只缺少了I/O设备。概括的讲：一块芯片就成了一台计算机。它的体积小、质量轻、价格便宜、为学习、应用和开发提供了便利条件。

### 2. 嵌入式开发特点

### 3. 外设

![mcu](./img/mcu.jpeg)

1. GPIO引脚

2. AD/DA转换
   
3. RTC
   
4. 定时器
   
5. 各种数据接口

----------

## 代码基本知识

### 1. c/cpp

不同于几十年前在51单片机上使用汇编开发,和寄存器打交道,现代单片机开发大多数采用c/cpp语言.
arduino不但有各种开发板,arduino本身也代表了一种标准,在创建项目的时候可以看到选用的是arduino框架.
arduino虽然看起来使用 `.ino` 后缀,但本质是c/cpp
c/cpp语言需要有 `main()` 函数入点,arduino似乎没有,但是实际上翻看源代码是找得到`main()`函数,而arduino框架封装好了,用户只需要定义 `setup()` 和 `loop()` 函数.

### 2. micropython

很可惜,Atmel系列的芯片很少有支持micropython,确认此次材料包中的mega2560是不支持micropython

> *如果使用相对高级一些的mcu,例如stm32系列,esp32系列,rp2040,以及其他基于arm架构的mcu大多数都支持micropython.*

micropython是运行在嵌入式环境下的python解释器,能解释运行python代码,并且拥有嵌入式开发和硬件相关的库.

> 优点: "人生苦短我用python",python开发门槛相对于c/cpp低不少,而且因为其解释运行的特点,可以摆脱编译和烧写的过程
>
> 缺点: 性能开销较大,可能存在一些不稳定不兼容问题

----------

## 开发工具和环境

### 1. Arduino IDE

1. 安装[Arduino IDE](https://www.arduino.cc/en/software)（建议安装 ide 2）
2. 开箱即用，左上角可以指定开发板类型和串口端口号

> 优点：烧写代码相当方便，自带串口调试，开发板选型，代码示例，以及arduino第三方库
>
> 缺点：Arduino IDE代码功能相对简陋，启动慢，开发体验不是很好，默认.ino文件不方便项目管理

> tips:
> 可以在 `File - Preferences` 里面开启自动补全（比较下面位置的 Editor Quick Suggestions）

### 2. VSCode + Platform IO

1. 安装[vscode](https://code.visualstudio.com/)，[python](https://www.python.org/)
2. 安装c/c++，pio插件
3. 打开pio插件主页，新建项目（建立项目的时候需要科学上网）指定开发板类型，项目位置和名称等
4. 打开刚创建的项目，可以在底部看到验证（编译）、烧写、调试等功能

> 优点：可以使用大家熟悉的工具开发，vscode拥有的各种插件都可以使用，完整的c/cpp开发逻辑
>
> 缺点：需要配置环境、插件，platform IO相对臃肿，而且需要科学上网，第一次使用可能很难找到各种功能

<img decoding="async" src="./img/code_bottom.png" height = "80">

> tips:
>  
> 1. 建议对pio插件采用只在某些工作区启用，不然你的vscode会很卡。
> 2. pio插件与clangd插件冲突，喜欢用clangd代码提示的建议在工作区内禁用clangd且启用c/c++插件（虽然intelli sense代码提示很蠢）
> 3. 然后执行项目任务“Miscelleneous -> Rebuild IntelliSense Index”。（可以直接`ctrl+shift+p`搜索）

### 3. Clion + Platform IO

1. 安装[clion](https://www.jetbrains.com/clion/)，pio插件（因为本人已经先用vscode装好pio了所以不确定clion是否需要另外安装pio）
2. 新建项目时选择嵌入式--PlatformIO，然后指定platformio.exe的路径。（建议添加到环境变量，否则后续步骤可能出问题）
3. 选择配置文件（此处我们使用mega2560，选择对应的配置就行）
4. 右上角可以指定配置，下载就选择下载，调试就选择调试（虽然两者分开很奇怪但是pio就是这样没办法）

> 优点：Clion代码开发体验极好
>
> 缺点：需要配置环境、插件，不习惯ide项目管理的同学可能不适应，库安装不太方便

----------

## 传感器

传感器（英文名称：transducer/sensor）是能感受到被测量的信息，并能将感受到的信息，按一定规律变换成为电信号或其他所需形式的信息输出，以满足信息的传输、处理、存储、显示、记录和控制等要求的检测装置。

### 1. 传感器 与 传感器模块

传感器可以由一个单独的元件组成,比如一个光敏电阻,一个压敏电阻,这些传感器往往需要外围电路
而传感器模块是封装好的,外围电路甚至芯片已经集成在一个模块中,用户使用只需要负责供电和数据交互

### 2. 常见传感器种类及使用

----------

## 电机

----------

## 机器人框架

----------

## 控制算法

----------

## demo

----------

## 扩展
