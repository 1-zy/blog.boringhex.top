---
title: 详解LDO
tags:
  - 硬件
  - LDO
mathjax: true
date: 2023-02-13 17:33:01
categories:
---


LDO（低压差稳压器）是一种电子器件，其作用是将高电压输入转换为稳定的低电压输出。LDO是一种线性稳压器，因其具有低噪声和低纹波的优点，而且具有较低的热损失，因此在各种应用中广泛使用。

LDO的主要结构包括输入电容、滤波电路、放大器、稳压元件和输出电容。在工作过程中，LDO首先对输入电压进行滤波，以减少电压波动，随后进行放大，最后由稳压元件进行稳压。

LDO的应用非常广泛，主要用于各种数字电子设备，例如手机、笔记本电脑、摄像机等。此外，LDO还用于各种工业和医疗设备，如PLC、仪器仪表、医疗设备等。

总的来说，LDO是一种高效、稳定和可靠的稳压器，具有广泛的应用前景。如果您正在寻找一种用于您的应用的稳压器，不妨考虑使用LDO。

<!-- more -->

LDO的主要参数有：

1. 输入电压范围：指LDO所能接受的电压范围。
2. 输出电压：指LDO的稳定输出电压。
3. 输出电流：指LDO的最大输出电流。
4. 压差：指输入电压与输出电压的差值。
5. 噪声：指LDO的电压噪声，以mV RMS为单位表示。
6. 纹波：指LDO的输出电压波动，以mVpp为单位表示。
7. 动态特性：指LDO的输出电压随输入电压变化的速率。
8. 热损失：指LDO在工作过程中产生的热量。
9. 工作温度范围：指LDO的工作温度范围。
10. 供电电流：指LDO的供电电流。
11. 封装形式：指LDO的封装形式，例如SOT-23、TO-220等。

这些参数是评估LDO性能的重要因素，在选择LDO时需要考虑。不同的应用需要不同的LDO参数，因此在选择LDO时需要根据具体应用需求进行选择。

以TI [TPS7A80](https://www.ti.com.cn/product/cn/TPS7A80)系列LDO为例，看看这些参数在数据表中的表示。

打开[TPS7A80数据表](https://www.ti.com.cn/cn/lit/ds/symlink/tps7a80.pdf)，可以看到数据表中的特性说明也是主要围绕上面几点给出的：

![](https://imgs.boringhex.top/blog/20230213145221.png)

典型应用：

![](https://imgs.boringhex.top/blog/20230213161514.png)

## 额定值

![](https://imgs.boringhex.top/blog/20230213150050.png)

在这个表中，输入电压、输出电流和工作温度一起给出，通常情况下这三个参数再加上输出电压就构成了LDO的工作点，因为这个系列有固定输出和可调输出的多个版本，所以输出电压没有列在这个表里。

### 输入电压

通常说的输入电压是LDO正常工作的输入电压范围，而**不是**绝对最大电压，可以看到这个LDO的输入电压范围是2.2V - 6.5V。

### 输出电压

固定输出的版本通常把反馈环路内置，或者有一个引脚直连输出，输出固定的电压，常见的电压值有1.2V、1.5V、1.8V、2.5V、3.3V、5.0V等。

可调输出的版本通过外部反馈电阻，可以将输出电压调整到期望的值。TPS7A8001是可调输出电压。

### 输出电流

LDO正常工作的最大输出电流，可以看到是1A。

### 温度

这个表里给出两个温度，$T_J$是结温，可以简单理解为芯片内部温度；$T_A$是环境温度，可以简单理解为芯片外表面的环境温度。

## 绝对最大值

![](https://imgs.boringhex.top/blog/20230213160225.png)

这个表也很重要，超过这个表里的限值，意味着芯片要承受过应力，这个应力可能是电应力，也可能是热应力。

注意：几乎任何电气元件都是热敏感元件，所以在应用中一定要时刻关注**温度**。

## 热相关

![](https://imgs.boringhex.top/blog/20230213161020.png)

热设计与应用环境息息相关，良好的热设计是长期稳定工作的基础。热设计是一个很大的话题，作为基本结论，热阻越小，导热越好，温差越小。

国产厂商很少在数据表里给出热阻这么细节的参数，而是以最大允许热耗散功率来表征，这个热耗散功率也是在一定条件下测得的，如果应用中散热条件更好，则能允许更大的热功率；反之，则达不到数据表中的热功率芯片就已经废了。

这两种表征方式其实本质没有差别，直接给出最大允许热耗散功率更有利于快速估计，而给出热阻等更多细节更有利于设计优化。

想要对芯片的热性能参数有更多了解，可以参考TI[这篇文档](http://www.ti.com/cn/lit/pdf/spra953)。

## 电气参数

![](https://imgs.boringhex.top/blog/20230213173615.png)

### 线调整率

表征输出电压随输入电压的变化。

### 负载调整率

表征输出电压随输出电流的变化。

### 压差

表征在一定输出电流下输入电压与输出电压的最小差值。在LDO应用中，最大压差受输出电流和允许最大热耗散功率限制，最小压差则受这个参数限制，其背后是由芯片晶体管工艺和设计决定的。

举个例子，如果输出电压固定为3.3V，输出电流为1A，那么如果输入电压为6V，则压差为2.7V，热耗散功率为2.7V，

$$
2.7 \times 47.8 \approx 129
$$

那除非环境温度很低，否则芯片肯定会挂掉，这是由热耗散功率限制的。

输出电压和输出电流要求固定后，输入电压太大不行，那小能小到啥程度呢？厂商承诺的是$3.3V + 500mV = 3.8V$，每颗芯片会有一定的差异，但肯定不会超过500mV。也就是说，输出固定3.3V 1A的情况下，输入电压要大于3.8V才行。如果小于3.8V会怎样呢？比如输入是3.5V，输出1A，那输出电压就保不住3.3V了，应该在3.0V左右。

### PSRR

电源抑制比，这个参数容易被忽略，在一些对电源纹波要求比较高的应用中，PSRR和输出噪声这两个参数就很重要，而输出噪声主要由芯片工艺决定，PSRR则跟芯片设计有很大关系。这个参数表征了输出纹波对输入纹波的抑制，值越大，抑制越好。

### 静态电流

这个没有对应到数据表中，在低功耗应用中，这个参数非常重要，不同的厂商可能会用不同的参数来表征。TI这个芯片有使能端，所以可以用表中的$I_{SHDN}$即关断电流这个参数来评估。这个表中还给出了$I_{GND}$，线性稳压器以前常被叫作三端稳压器，GND引脚跟负载是分流关系，GND引脚电流表征了芯片本身的功耗(不含热功耗)，所以这个参数可以用来评估芯片在低功耗场景下的表现，从表中可以看到，1mA输出时，$I_{GND}$最大为100uA或120uA，这个值在很多低功耗应用中是有点偏高的。有很多数据表将输出电流为0时的输入电流定义为静态电流。

其实定义本身并不重要，重要的是我们一定要找到参数定义所表征出来的电气特性，以及这些特性是如何影响系统表现的。大厂的芯片数据表本身就是非常好的学习资料，里边一般都有比较详尽的测试和说明。
