---
layout: post
title: 实习总结
tagline: 车站信号控制系统
category: School Study
tags: [实习, 思考]

---

## 实习内容
这次实习其实不算是真正意义上的“企业实习”，因为我们实际上是在学院朱老师的带领下，在学校完成了一个类似“课程设计”的小项目。项目的题目是“车站信号控制系统”，功能与6502或者微机联锁类似，用于控制铁路站场三大件——信号机、道岔、轨道电路实现联锁。这个项目说小挺小，只有我们5个人，做了大约两个月的时间，但是说大的话，要想真正实现，可能需要一个公司花上一年的时间。

## 由来
之前上过朱老师的电路课和电子技术课。朱老师之前做过很多电子设计方面的工作，也做过铁路方面的项目，所以早就有做这个项目的想法。在上课时提出了这个想法，我们几个人觉得跟朱老师挺好，所以在上个学期开始就跟朱老师在这个项目了，上个学期主要是在学习6502。

## 过程

### 6502
上个学期主要在学习6502，参考了几本书：

- [车站信号自动控制][1]
- [车站信号自动控制][2]
- [6502电气集中电路图册][3]

首先应该学习继电器的特性和画法，这个是必须的。

### FPGA与Verilog HDL
学习FPGA，使用的是周立功公司的开发套件EasyFPGA060，使用Libro8.5作为开发平台，参考书目是[可编程逻辑电路设计基础教程][4]。主要学习的是比较简单的组合逻辑的实现，因为6502主要功能是靠继电器实现，与组合逻辑比较相似。

### 设计
考虑使用组合电路实现继电器的功能：

<img src="/assets/images/Summary-of-internship/relay.jpg" align="center" />

公式大约是这样的：
	
	j = trigger || ( j && en)

利用上述的带环的组合电路，考虑其他继电器电路的实现，如方向继电器等。

然后是考虑实现选择组功能，我们只是实现了选择组，而没有考虑其他后续操作。选择组的思路是为每个道岔定反位确定在哪些进路排列时触发。

>这个从单个功能入手的方法是很有必要的，因为如果把所有逻辑全部学习，一次完成，可能需要的时间更长，甚至最后一下子无从下手。

接着是检查电路的实现，思路是为每个进路设置专门的检查逻辑。

接着是锁闭，思路与选择组类似。

最后是解锁，现在还没有完成，但是思路大约是通过形成与轨道形状相似的电路，以此作为三点检查的条件。

## 学习方法总结
可能6502现在已经有些过时了，但是学习的方法可能对今后的学习会有帮助。

6502是一个成熟的系统，因为6502好像是65年左右开始研发的，到现在已经接近60年的历史，一直在使用，所以非常成熟。但是成熟的同时，也意味着复杂。

我想学习的过程其实应该和设计、实现一个系统的过程是一致的。

1. 首先应该**明确需求**，即用户希望有什么样的操作完成什么样的功能。这就要学习站场值班员的操作，如何排列进路，各种按钮的作用。
2. 其次应该**了解架构**，包括结构和流程。

	- 6502的结构比较混乱，但是实际上还是有结构的，比如将1-6线作为选择组，将其他线作为执行组等。
	- 流程则是一个功能分为多个模块，依次执行各个模块来实现一个功能。应该首先有一个大体流程的概念，然后一个模块一个模块进行学习。
	6502的电路中有很多小细节，是为了实现特殊功能而增加的。这些细节在开始学习时应该忽略，比如重复开放信号的FKJ等，可以再后面学习到相应的阶段再重新学习。

3. 最后应该注意总结，总结系统的思路的亮点和重点内容。

Verilog HDL，学习期间参考了网上的其他一些教程，最需要注意的是，这是一种硬件描述语言，也就是说，你用这个语言，描述的是硬件结构。而我们常用的语言C、Python描述的大多是解决问题的流程，这是有本质不同的，我认为这两者甚至不能做类比。

Module是Verilog HDL非常重要的概念，书上说Module里的语句是并行执行的，我认为这是有误导性的。Module描述的是一个硬件模块，硬件模块哪有什么语句，更没有语句执行顺序的问题。硬件结构决定了这个模块内信息的流动方式，而不能说这个Module里的语句是怎么执行的，Module里的语句会被翻译成硬件结构，然后由硬件处理输入生成输出。

所以我认为要想学好FPGA、Verilog HDL，是一定要学好电子电路，了解硬件才可能真正正确、高效得利用好这些工具实现想法。


## 其他总结

在答辩时，又遇到了“意义”问题，就是老师问我们有没有想过做这个东西意义何在，其实说实话我没有做过非常详细的思考，所以有些感触。

在一开始做的时候，我也很好奇，做这件事与现在的微机联锁的优势在哪些方面，也向老师咨询过这个问题，但是老师只是说了个大体，大概是微机联锁的成本高，专业的安全计算机很昂贵，而FPGA则便宜得多。微机联锁的执行部分还是使用继电器实现，成本高，维护困难。但是FPGA实现逻辑的话与微机联锁有没有优势没有论证。

另一个问题是：为什么要学习6502？为什么不用微机联锁的逻辑？

我们其实学过，也用过微机联锁的逻辑，但是为什么还要学习6502，真的没有仔细思考过这件事。朱老师让我们学6502，我们就一路学了下来，到底为什么学，跟学习微机联锁相比哪个更好，没有自己考虑过，这种状态肯定是不对的，今后做事要想清楚做事到底合适不合适。

另一个最致命的问题是：已经有在使用的电子联锁了，你们做的跟这种东西一样。

我从来没有仔细调查过目前这一领域的情况，而是埋头做事，这可能是今后做科研最最要不得的事了，**花几天的时间了解当前发展状况无论是做工程还是做研究都是非常重要的事**，而我做了将近半年，也从来没做过，现在花些时间补一补：

>[2007年7月6日，中国铁路第一个实行全电子计算机联锁在乌鲁木齐西客技站顺利开通 (全站28组道岔)。][2007乌鲁木齐]乌鲁木齐乌西客技站，联锁规模28组联锁道岔，室内外设备总造价200万人民币。由新疆铁道勘察设计院设计，兰州交通大学自动控制研究所提供计算机联锁全电子执行单元设备。  由兰州交通大学自动控制研究所、兰州大成自动化工程有限公司 历时 9 年 联合开发研制的“计算机联锁全电子执行单元”于2005年5月20 日 通过了铁道部科学技术司组织的鉴定，产品的研制成功为我国铁路信号全电子智能化控制、确保行车安全提供了一种新型技术装备。

>[一个PPT，介绍兰州交大的全电子系统][介绍]

>[官方介绍][]

到底现状如何，还要咨询一下朱老师。


[1]: 王永信，喻喜平.车站信号自动控制[M].北京：中国铁道出版社，2007.8
[2]: 徐洪泽.车站信号自动控制[M].北京：中国铁道出版社，2012.3
[3]: 林瑜筠.6502电气集中电路图册[M].北京：中国铁道出版社，2012.5
[4]: 周立功. 可编程逻辑电路设计基础教程[M].北京：北京航空航天大学出版社，2012.8 
[2007乌鲁木齐]: http://wenku.baidu.com/link?url=yiq3p9EQdkYvYWehmTeCHU_Dp0LF0QFYFg8Wn3dQ3e_YWJSqluRbMC9hU0Ci1JTmky0T21ymIBr8T7WVJSfaexzG0-Zr3V5QVHCrVPclzLK
[介绍]: http://wenku.baidu.com/link?url=woRW6WMS0eGL8Vx3TR20eZ62idljmkL6Zi71uHeQZekxTs3heUtwOH51H5T9lmU0H8egWh-ZZ9k1ZuK0riCW2-nntiB9NMB-ED34gSK9NgS
[官方介绍]: http://www.lzdcrs.com/chanpinjieshao/tieluxinhaoxiliezhuangbeijieshao/2012-06-08/91.html