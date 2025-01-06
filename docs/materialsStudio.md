@autoHeader:0

# 引言

> 目的：Materials Studio的简单介绍和安装

Materials Studio是化学和**材料科学**建模和模拟科学工具，提供了一个集成的建模和分析环境，用于研究分子或晶体结构与性质之间的关系。高度集成、具有图形化界面、上手容易。

## 特点

**一、多尺度**

1. 涵盖理论领域广。在理论方法上，MS涵盖了量子力学（QM）、分子力学（MM）、蒙特卡洛（MC）、粗粒化（Coarse-grained，如耗散粒子/介观动力学）理论方法，此外还提供晶体形貌计算（Morphology模块）晶体衍射计算与分析（Reflex模块）、混合能计算（Blends）、气体吸附（Sorption）等模块；
2. 涵盖模拟尺度广。在时间尺度上，涵盖飞秒（fs）、皮秒（ps）、纳秒（ns）乃至微秒（μs）的范围；在空间尺度上，涵盖埃（Å）、纳米（nm）乃至微米（μm）的范围。
   ![alt text](_media/Pasted image 20240405112719.png)

**二、高集成**![alt text](_media/Pasted image 20240405112937.png)**三、界面友好、可操作性强****四、多种模块**

- MS模块的类型，可大致分为5类：量子力学类模块、分子力学类模块、蒙特卡洛类模块、粗粒化或介观类模块、晶体学类模块。每个模块都有其他模块没有的、自己特有的计
  算功能。举几个例子：

> 能带、态密度 →量子力学类的模块：CASTEP／or DMoI3
> 静电势图 →量子力学类的模块：DMo13／
> 晶体形貌 →晶体学类模块：Morphology
> 结合能、吸附能 → DMol3  or CASTEP  or Forcite
> 力学性质、弹性常数 → DMo3  or CASTEP  or Forcite

- 最常用模块：forcite、CASTEP、DMol3、amorphous cell

## 力场

- COMPASSCOMPASS（Condensed-phase Optimized Molecular Potentials for Atomistic Simulation Studies）代表了力场方法的技术突破。它是第一个从头开始的力场，能够准确、同时地预测各种分子和聚合物的气相性质（结构、构象、振动等）和凝聚相性质（状态方程、内聚能等）。它也是第一个将有机和无机材料参数集成在一起的高质量力场。
  COMPASS力场广泛覆盖共价分子，包括最常见的有机物、小无机分子和聚合物。COMPASS开发已将覆盖范围扩大到包括无机材料：金属，金属氧化物和使用各种非共价模型的金属卤化物
  COMPASS II（Sun et al.， 2016）是COMPASS力场的重要扩展。COMPASS II扩展了COMPASS的现有覆盖范围，包括研究离子液体的研究人员感兴趣的大量药物和化合物。COMPASS II是与上海交通大学的孙怀教授合作开发的。
  COMPASS III 涵盖与 COMPASS II 相同的扩展，但仅使用 DMol3 进行参数化。
- PCFFThe polymer consistent forcefield (pcff)，聚合物一致性力场 （pcff） 是在早期力场 CFF91 的基础上开发的，旨在应用于聚合物和有机材料。可用于聚碳酸酯、三聚氰胺树脂、多糖、其他聚合物、有机和无机材料、约20种无机金属，以及碳水化合物、脂质和核酸。
- cvffThe consistent-valence forcefield (cvff)，一致价力场 （cvff） 是一种广义价力场 。提供了氨基酸、水和各种其他官能团的参数。适配了小的有机（酰胺、羧酸等）晶体和气相结构
- Universal
- Dreiding

## 其它事项

- MS软件中自带help手册，遇到问题可以随时查询
- .xtd 是轨迹文件，不能直接操作
- [MS入门手册（4）：Materials Studio文件和目录的建立、打开、关闭、命名/重命名、拷贝、移动和删除 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/575492259?utm_id=0)
- [Materials Studio功能模块简介 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/643891639?utm_id=0)

# 安装

因为是商业软件，这里提供网络资源：>>[百度网盘](https://pan.baidu.com/s/1aZJzqw0lvSc3wCDkOBBEVg?pwd=b8yp)<<
