## 概述
分子力学（molecular mechanics） 建立在*经典力学*理论基础上，借助经验和半经验参数计算分子结构和能量的方法，又称力场方法（force field method）。  
### 原理
基于玻恩–奥本海默近似(Born Oppenheimeri approximation)，系统能量表示为原子核位置的函数，忽略电子运动。分子被描述为一系列带电的点（原子），通过化学键相互连接。为了描述键长、键角、扭转角的时间演化，以及原子之间的非键范德华力和静电相互作用，人们使用力场来描述这些参数。力场通过参数化这些能量项的公式和常数，将原子之间的相互作用转化为力和能量的数学表示。这些参数可以通过实验数据、量子化学计算或经验拟合等方法获得。  
通过力场，可以计算分子系统中原子、分子之间的相互作用力，从而模拟分子的结构、动力学和热力学行为。力场在分子模拟和计算化学研究中起着重要的作用，能够预测分子的构象、热力学性质、反应机理等重要信息。

- 分子总能量=动能+势能
- 当温度为0 K，势能=总能量
- 势能=成键势能+非键势能，下面又分出若干小项，各个势能项都有函数表达式描述



## 力场分类
不同的力场可能适用于不同类型的分子和研究问题，选择合适的力场对于获得准确和可靠的模拟结果至关重要。
1. 全原子力场（all-atomforcefields）：每个原子都作为一个粒子明确考虑。体系的力点与分
子中的全部原子一一对应。
2. 联合原子力场（united-atomforcefields）：将与碳原子直接键连的氢原子的相对原子质量
叠加到碳原子上，形成联合原子整体。力点的数量少于分子中原子的数量，是分子的不完全描述。
3. 粗粒度力场（coarse-grained force fields）：粗粒度力场中可以把苯环及其键连的氢原子作
为一个整体力点，甚至把若干个水分子作为一个整体力点。
4. 反应性分子力场（reactiveforcefields）：为了在经典MD模拟中研究化学反应，引入反应性
分子力场以允许化学键的断裂和生成
>注意：蛋白质力场，小分子力场，和水分子模型的搭配

## 常见的分子动力学力场：
1. **AMBER力场**：AMBER（Assisted Model Building with Energy Refinement）力场是广泛应用的力场之一。它提供了多个版本，如AMBER94、AMBER99和AMBER ff03等。AMBER力场主要用于蛋白质和核酸的模拟研究。
2. **CHARMM力场**：CHARMM（Chemistry at Harvard Molecular Mechanics）力场是另一个常用的力场。它包括多个版本，如CHARMM22和CHARMM36等。CHARMM力场广泛应用于蛋白质、核酸、脂质和碳水化合物等生物分子的模拟。
3. **GROMOS力场**：GROMOS（Groningen Molecular Simulation）力场是由Groningen大学开发的力场。它适用于溶液中的生物分子模拟，并提供了多个版本，如GROMOS53A6和GROMOS96等。
4. **OPLS力场**：OPLS（Optimized Potentials for Liquid Simulations）力场是用于液体模拟的力场。它提供了多个版本，包括OPLS-AA（全原子力场）和OPLS-UA（United Atom力场）。OPLS力场广泛应用于脂质、多肽和蛋白质等系统的模拟。
5. **AMBER-ff系列力场**：AMBER-ff（AMBER Force Field）系列力场是AMBER力场的衍生版本，针对特定类别的分子进行了参数化。例如，AMBER-ff99SB力场专门用于蛋白质模拟，而AMBER-ff99SB-ILDN力场用于蛋白质折叠和去折叠的模拟。
6. **CVFF 力场**(Consistent Valence Force Field)，属传统力场。适应于有机小分子和蛋白质体系。扩展后可用于某些无机体系的模拟，如硅酸盐、铝硅酸盐、磷硅化合物等，主要用于预测分子的结构和结合自 由能。
7. CFF：consistent force field，一致性力场。PCFF，衍生，适用于聚合物和有机物
8. **MMX 力场**：包括 MM2 和 MM3，是目前应用最为广泛的力场，主要针对有机小分子。也可用于生物大分子体系，计算速度较慢。第二代力场比传统力场要更加复杂，涉及的力场参数更多，计算量也更大，当然也相应地更加准确。
9. 此外，还有其他一些力场，如MMFF（Merck Molecular Force Field）、UFF（Universal Force Field）等，它们适用于小分子和药物化学等领域的模拟研究。
*这些力场文件通常以特定的格式（如AMBER格式、CHARMM格式或GROMOS格式）存储，并包含描述分子的拓扑信息和相互作用参数。*
通用力场：以原子为基础的力场，ESFF、UFF、DREDING
- 常用的蛋白质力场
	- ==Amber系列==
	- Charmm系列
	- OPLS系列
	- Gromos系列
- 常用的脂质力场
	- Amber系列
	- Charmm系列
	- Slipid
	- Gromos系列
- 常用的小分子力场
	- GAFF
	- CGenFF
	- OPLS
- 常用的核酸力场
	- Amber系列
	- Charmm系列
- 常用的糖类力场
	- ==GLYCAM==
	- Charmm系列
	- OPLS系列
- 其它体系的力场
	- UFF, Dreiding
	- ReaxFF
	- MARTINI


## 参考
[力场选择-分子动力学 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/458825807)
[科学网—力场与拓扑之二：如何选择力场 - 李继存的博文 (sciencenet.cn)](https://blog.sciencenet.cn/blog-548663-942121.html)