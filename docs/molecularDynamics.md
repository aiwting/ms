# 概述
为了生成代表性平衡系综，有两种方法可用：1. 蒙特卡罗模拟 2. 分子动力学模拟。

- **分子动力学模拟（Molecular Dynamics Similation，MD模拟）**：在分子动力学模拟中，物质被看作是由原子构成的基本单元，通过模拟这些基本单元在时间上的演化，就可以探究物质的动力学性质，从而帮助人们更好地理解和预测物质的行为和性质。
- **分子动力学模拟是一种基于牛顿运动定律的分子模拟方法**，用于计算分子体系与时间相关的性质。可以依据当前分子体系的位置、速度和动能等信息，推测该体系未来的位置、速度和动能，从而揭示分子运动的客观规律。

# 原理
1. MD 模拟求解 N 个相互作用原子系统的牛顿运动方程：  
   - $m_i\frac{\partial^2r_i}{\partial t^2}=F_i$, i=1,2...N.
2. 根据经典力学，原子 i 所受的力 F 是势函数 V （r1， r2， . . . ， rN） 的负导数（梯度）：  
   - $F_i=-\frac{\partial V}{\partial r_i}$, i=1,2...N.  
   - 或分量形式：$\overrightarrow{F_i}=-\nabla _iV=-(\overrightarrow{i}\frac{\partial}{\partial x_i}+\overrightarrow{j}\frac{\partial}{\partial y_i}+\overrightarrow{k}\frac{\partial}{\partial z_i})V$

3. 利用牛顿运动定律。先由系统中各分子位置计算系统的势能，再计算系统中各原子所受的力及加速度，然后再设置一非常短的时间间隔 δt，则可计算经过 δt 后各分子的位置及速度。重复以上的步骤，由新的位置计算系统的势能，计算各原子所受的力及加速度，预测再经过 δt 后各分子的位置及速度。如此反复循环，可得到各时间下系统中分子运动的位置、速度及加速度等数据。一般将各时间下的分子位置称为运动轨迹​，模拟软件中得到的文件称为轨迹文件。
   - 根据牛顿运动定律，原子 i 经时间 t 后的速度 $\overrightarrow{v}$  和位置 $\overrightarrow{r}$
   - $\overrightarrow{a_i}=\frac{\overrightarrow{F_i}}{m_i}$
   - $\frac{d^2}{dt^2}=\frac{d}{dt}\overrightarrow{v_i}=\overrightarrow{a_i}$
   - $\overrightarrow{v_i}=\overrightarrow{v}_i^0+\overrightarrow{a_i}t$
   - $\overrightarrow{r_i}=\overrightarrow{r}_i^0+\overrightarrow{v}_i^0t+\frac{1}{2}\overrightarrow{a_i}t^2$

# 分子动力学模拟一般流程：
分子动力学是从经典物理的统计力学出发的计算方法，它通过对分子间相互作用势函数及运动方程的求解，分析其分子运动的行为规律，模拟体系的动力学演化过程。模拟计算的一般步骤包含以下几点：  
A. 分子体系模型建立和优化  
B. 给定条件参数（温度、粒子数、时间等）  
C. 计算作用于所有粒子上的力  
D. 求解牛顿方程，计算极短时间内粒子的新位置  
E. 计算粒子新的速度和加速度  
F. 重复C-E直至体系达到平衡，然后记录原子的坐标位置。  
G. 继续计算直到取得足够的信息，分析体系各粒子运动轨迹，得到体系的统计性质。  
# 分子间相互作用
interactions：**计算原子间相互作用是分子动力学模拟的核心问题**
>成键相互作用
- bond 键
- angle 角
- dihedral 二面体

>非键相互作用
- 短程相互作用
  - 主要指Van der Waals相互作用（包括色散和排斥）
  - 表现形式如短程作用势 Lennard-Jones势
  - 周期性边界条件、截断半径和近邻搜索都是针对短程相互作用的
- 长程相互作用
  - 库仑、偶极、静电
  - 处理长程相互作用的方法：Ewald求和、反应场方法、PPPM方法

# MD模拟软件工具
- **MS**：Materials Studio
- **DS**：Discovery Studio
- **GROMACS**：（GROningen MAchine for Chemistry Simulation）是一款集成了高性能分子动力学模拟和结果分析功能的**免费开源软件**,高度优化的代码使GROMACS成为迄今为止分子模拟速度最快的程序。可以模拟具有数百至数百万个粒子的系统的牛顿运动方程，模拟具有许多复杂键合相互作用的生化分子，例如s蛋白质，脂质和核酸。另外，GROMACS能够⾮常快速地计算⾮键作⽤，因此也可⽤于⾮⽣物体系，如聚合物、⼀些有机物、⽆机物等。
- **LAMMPS**：材料
- **CHARMM**：磷脂
- **Amber**：AMBER是一个广泛使用的生物分子模拟软件，主要用于蛋白质和核酸的分子动力学模拟和分析。它具有多种分子力场和高度优化的性能，可以进行高精度的模拟和分析。缺点是相对较复杂，需要一定的物理化学和计算机知识来使用。DNA/RNA
- **NAMD**：是一个专门用于生物大分子模拟的软件，具有高效的并行计算和可视化工具。它可以模拟大规模的蛋白质、核酸和膜系统，具有高精度和高效率。缺点是需要高性能计算环境和专业知识来使用。
- **OpenMM**：大多用于生物体系的模拟当中，同时也支持CHARMM， Amber， Drude 等大多数常用力场。 
- **HyperChem**
- **tinker**
