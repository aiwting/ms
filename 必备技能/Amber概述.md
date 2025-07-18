# Amber概述

什么是amber?

**Amber**，是一组用于模拟生物分子的分子力学力场；也是一套生物分子模拟程序。

## Amber programs

- Amber全称Assisted Model Building with Energy Refinemegnt， 是由University of California, San Francisco的Kollman教授课题组开发的一套分子力学和分子动力学模拟软件，包括大约60个程序，如 sander. xleap, antechamber、ptraj、mm_pbsa和nmode等。
- Amber尤擅长于生物体系的分子动力学模拟,功能全面。对于材料体系,则可以考虑使用Lammps。
- Amber的优点是计算精度高，自定义新分子和新模型比较方便。缺点是计算效率低，速度慢(相比于GROMACS)
- 与CHARMM不同，Amber不是一个all-in-one的分子动力学软件，也不是某一个程序的名字，而是由多个各司其职的程序所构成的工具集，除了高性能动力学引擎外的其他组分统称AmberTools
- Amber软件套件分为两个部分:
  - AmberTools, 一组主要根据GPL许可证免费提供的程序
  - Amber，在AmberTools的基础上构建，增加了pmemd模拟程序，并按照更严格的许可证获得许可, **现在Amber24可以免费用于非商业用途**
- Amber 是一个完整的分子动力学模拟软件套件，提供了模拟引擎、力场参数化工具和分析工具等功能。
- AmberTools 提供了一些额外的工具和实用程序，用于辅助分子模拟的前后处理。如构象生成、参数化、模型构建、能量计算、轨迹分析等
- Amber可以用于生物大分子体系的分子力学计算和分子动力学模拟，也可以采用MM_PBSA和MM_GBSA方法研究受体和配体之间的结合自由能，还可进行量子力学和分子力学结合(QM/MM)计算。

## Amber Force Fields

- 最初AMBER力场是专门为了计算蛋白质和核酸体系而开发的，计算其力场参数的数据均来自实验值。後来随著AMBER力场的广泛应用，包括Kollman在内的很多课题组对AMBER力场的内容不断进行丰富，逐渐开发出了一个可以用於生物大分子、有机小分子和高分子模拟计算的力场体系。
- AMBER力场的优势在于对生物大分子的计算，其对小分子体系的计算结果常常不能令人满意。AMBER力场的势能函数形势较为简单，所需参数不多，计算量也比较小。主要适用于蛋白质和核酸体系、多糖。
- 对于不同的物质，amber有不同的力场，[The Amber Force Fields (ambermd.org)](https://ambermd.org/AmberModels.php)
  - a protein force field (recommended choice is ff14SB)  
  - a DNA force field (recommended choice is OL15)  
  - an RNA force field (recommended choice is OL3)  
  - a carbohydrate force field (recommended choice is GLYCAM_06j)  
  - a lipid force field (recommended choice is lipid17)  
  - a water model with associated atomic ions (more variable); popular choices are spc/e, tip4pew, and OPC. Not needed if you are using an implicit solvent model.  
  - a general force field, for organic molecules like ligands (recommended choice is gaff2)  
  - other components (such as modified amino acids or nucleotides, other ions), as needed

## amber的文件

amber 的文件格式规范，在amber官网的 File Formats 页面可以找到各种文件的描述：[Amber File Formats (ambermd.org)](https://ambermd.org/FileFormats.php)

### 力场参数文件

- 在 ambertools 安装目录的/dat/leap目录下存放着力场相关文件，包括：
  - 拓扑参数文件格式(.prmtop,/.top/.parm7)
  - 结构坐标（和速度）文件格式(.inpcrd/.restrt/.rst7)
  - 轨迹文件格式(.mdcrd/.crd/.nc)
  - 力场参数加载命令文件(leaprc.xxx.xx)
  - 力场参数文件（.dat）
  - 力场参数文件(.lib/.off)
  - 力场参数文件(.prepi/.in/.prepin)
  - 力场参数修改文件(frcmod)
- 为了在 Leap 中添加参数集，需要一个 lib 文件和一个 frcmod 文件，leaprc 文件是调用推荐组合的预制脚本。
- $AMBERHOME/dat/leap/ 目录下有四个文件夹，主要几类文件分别如下：
  - list of leaprc files available - $AMBERHOME/dat/leap/cmd
  - list of lib/off files available - $AMBERHOME/dat/leap/lib
  - list of frcmod/dat files available - $AMBERHOME/dat/leap/parm
  - list of prepi/in/prepin files available - $AMBERHOME/dat/leap/prep

#### leaprc.*

/cmd 文件夹下的力场参数加载命令文件 leaprc.GLYCAM_06j-1 从上到下内容依次是：  

- 元素及其对应的原子类型
- 加载原子类型及其杂化信息      .dat 文件
- 加载主力场参数及残基参数      .prep  .lib
- 定义匹配PDB文件中的氨基酸名
- HIS残基的质子化处理（此力场没有）
- 加载溶剂及离子

```terminal
addAtomTypes {
        { "C"   "C" "sp2" }
        { "Cg"  "C" "sp3" }
        { "H"   "H" "sp3" }
        { "H1"  "H" "sp3" }
     …………
        { "NT"  "N" "sp3" }
        { "N3"  "N" "sp3" } 
        { "Oh"  "O" "sp3" }
        { "Os"  "O" "sp3" }
        { "S"   "S" "sp3" }
        { "P"   "P" "sp3" } 
}

glycam_06 = loadamberparams GLYCAM_06j.dat

loadamberprep GLYCAM_06j-1.prep

loadOff GLYCAM_amino_06j_12SB.lib
loadOff GLYCAM_aminoct_06j_12SB.lib
loadOff GLYCAM_aminont_06j_12SB.lib

addPdbResMap { 
        { 0 "OLS" "NOLS" } { 1 "OLS" "COLS" }
        { 0 "OLT" "NOLT" } { 1 "OLT" "COLT" }
        { 0 "OLP" "NOLP" } { 1 "OLP" "COLP" }
        { 0 "HYP" "NHYP" } { 1 "HYP" "CHYP" }
        { 0 "NLN" "NNLN" } { 1 "NLN" "CNLN" }
} 

loadOff solvents.lib  # solvents 
HOH = TP3  # set default water model
WAT = TP3 
loadOff atomic_ions.lib  # load ions library
ionsff = loadamberparams frcmod.ionsjc_tip3p  # set ion params for TIP3P
```

#### \*.dat

/pram 文件夹下力场参数文件 GLYCAM_06j.dat 从上到下内容依次是：

- 描述信息（第一行）
- 原子的类型(符号 atom symbol)，以及质量、化学环境；对应gromacs的.atp文件
- 亲水原子符号
- 键长参数
- 键角参数
- 二面角参数
- 不适当的二面角参数
- 氢键 10-12势参数
- 非键合 6-12势参数的等价原子符号
- 非键合 6-12势参数

```terminal
GLYCAM_06_H PARAMETERS (FOR AMBER 11.0, RESP 0.010), COPYRIGHT CCRC 2011
CT 12.01                 sp3 C aliphatic
2C 12.01                 sp3 aliphatic C with two (duo) heavy atoms copied from ff12SB
H  1.008                 H Bonded to nitrogen atoms
H1 1.008                 H aliph. bond. to C with 1 electrwd. groups
…………

C   H   Ho  Ng  NT  N3  O   Oh  Os  P   O2 

Cg-N   337.0    1.450       Copy of Cg-Ng from GLYCAM06
CT-Os  320.0    1.410       Copy of CT-OS from parm10.dat
2C-Os  320.0    1.410       Copy of CT-OS from parm10.dat
…………
H -N -Cg     30.00    118.00   Copy of H -Ng-Cg from GLYCAM06
Os-Cg-N     106.90    107.90   Copy of Os-Cg-Ng from GLYCAM06
Cg-Cg-N      80.00    109.70   Copy of CT-CT-N  from parm99.dat
…………
Os-Cj-Cj-Cg   1  -13.00        0.0      2    SCEE=1.0 SCNB=1.0 copy of Os-Ck-Ck-Cg
Os-Ck-Ck-Cg   1  -13.00        0.0      2    SCEE=1.0 SCNB=1.0 Lipids, from 1-propenylmethylether
Ck-Cj-Cj-Ha   1  -15.00        0.0      2    SCEE=1.0 SCNB=1.0 copy of Cj-Ck-Ck-Ha
…………
```

#### frcmod.* 或 \*frcmod

也在 /pram 文件夹下，frcmod.*文件与  \*.dat文件的格式基本相同。可以像标准的 \*.dat文件一样读入tleap，将 \*.dat文件中的默认参数与 frcmod.* 中的新参数合并。
frcmod.ff14SB 从上到下内容依次是：

```terminal
ff14SB protein backbone and sidechain parameters
MASS
CO 12.01         0.616  !            sp2 C carboxylate group 
2C 12.01         0.878               sp3 aliphatic C with two (duo) heavy atoms
3C 12.01         0.878               sp3 aliphatic C with three (tres) heavy atoms
C8 12.01         0.878               sp3 aliphatic C basic AA side chain

BOND
C -2C  317.0    1.5220
C*-2C  317.0    1.4950
…………

ANGL
N -C -2C    70.0      116.60
O -C -2C    80.0      120.40
…………

DIHE
C8-CX-N -C    1    0.000         0.0            -4.    phi'
C8-CX-N -C    1    0.800         0.0            -3.
…………

IMPR
X -O2-CO-O2        10.5          180.          2.
2C-O -C -OH        10.5          180.          2.

NONB
  2C          1.9080  0.1094             Spellmeyer
  3C          1.9080  0.1094             Spellmeyer

```

#### \*.prep 或 \*prepin 或 \*in

/prep 文件夹下拓扑数据库文件 GLYCAM_06J-1.prep 记录了各种残基信息：

```terminal
4ZB  INT 0     ;残基名 内坐标 二进制输出
CORRECT OMIT DU BEG      ;内坐标有着正确的顺序 删除DU原子 从头开始
-1.0000        ;截断值用于环闭合处理
 1 DUMM DU  M  0 -1 -2  0.000   0.000    0.00     0.0000    ;虚拟原子，用来定义残基的空间轴 
 2 DUMM DU  M  1  0 -1  0.500   0.000    0.00     0.0000    ;虚拟原子
 3 DUMM DU  M  2  1  0  1.296  74.264    0.00     0.0000    ;虚拟原子
 ;编号 原子名-原子符号 拓扑连接类型 键-角-二面角 键平衡值-角值-二面角值 电荷
 4 C1   Cg  M  3  2  1 23.413  98.374 -136.24     0.2510
 5 H1   H2  E  4  3  2  1.091  70.111  -94.11     0.0000
 6 O5   Os  M  4  3  2  1.460 133.490  168.28    -0.4130
 …………
21 H2O  Ho  E 20 18 14  0.962 106.072  172.93     0.4220
22 O4   Os  M 12  7  6  1.449 108.907 -172.84    -0.4980

LOOP      ;环闭合处
C2 C1

DONE
```

注意：prepi和prepc分别是内坐标和卡迪尔坐标系的prep文件, 是以前程序prep使用的 (现已由leap整合),可由antechamber或prepgen产生, 现常用于非标准残基读入和处理

#### \*.lib

- /lib 文件夹下的 .lib 文件记录了氨基酸残基的符号及每个残基的描述
- 例如 GLYCAM_animo_06j_12SB.lib 文件，为了将聚糖与蛋白质连接起来，必须加载含有修饰氨基酸残基（Ser、Thr、Hyp 和 Asn）的lib。要使用 ff14SB 构建糖蛋白，必须加载 `GLYCAM_amino_06j_12SB.lib` `GLYCAM_aminont_06j_12SB.lib` 和 `GLYCAM_aminoct_06j_12SB.lib`，并且还必须加载所需的蛋白质力场。

### 模拟相关文件

- prmtop文件，又名parm7，top，包含分子，原子名称、种类，电荷，键关系，键参数和非键参数信息
- .prmtop 和 .prmcrd：topology and coordinate files，可以由 LEaP 程序生成。
- .top 和 .crd
- .inpcrd：input initial coordinate file，包含了原子的所有坐标，以及模拟盒子的形状。
- .mdcrd：轨迹文件

## amber的力场

对于不同的物质，amber有不同的力场：  
• a protein force field (recommended choice is ff14SB)  
• a DNA force field (recommended choice is OL15)  
• an RNA force field (recommended choice is OL3)  
• a carbohydrate force field (recommended choice is GLYCAM_06j)碳水化合物力场，推荐选择GLYCAM_06j  
• a lipid force field (recommended choice is lipid17)  
• a water model with associated atomic ions (more variable); popular choices are spc/e, tip4pew, and OPC.  
  Not needed if you are using an implicit solvent model.  
• a general force field, for organic molecules like ligands (recommended choice is gaff2)  
• other components (such as modified amino acids or nucleotides, other ions), as needed

### 蛋白质的力场 ff14SB

对于蛋白质，amber推荐使用ff14SB力场。SB蛋白质力场家族还有(ff19SB, ff14SB, and ff99SB)。

ff14SB是早期ff99SB力场的继续演变，FF14SB基于FF12SB, FF12SB是FF99SB的更新版本, 而FF99SB力场又是基于原始的Amber的Cornell等人(1995)的ff94力场. FF14SB力场最显著的变化包括更新了蛋白质Phi-Psi的扭转项, 并重新拟合了侧链的扭转项。这些变化一起改进了对这些分子中α螺旋的估计。

ff19SB是SB蛋白力场的最新模型，与更准确的水模型OPC最匹配，建议将ff19SB与OPC水一起使用，而不建议与TIP3P水一起使用。

在 gromacs 官网的[User contributions](https://www.gromacs.org/user_contributions.html#)可以找到三个版本的 ff14SB 力场：包括(amber14sb, amber14sb_OL15, amber14sb_parmbsc1)
- amber14sb.ff：只有蛋白质力场
- amber14sb_parmbsc1.ff：[Parmbsc1](https://www.nature.com/articles/nmeth.3658) 代表 DNA
- amber14sb_OL15.ff_corrected-Na-cation-params.tar：包含了用于 DNA 的amber OL15 力场和用于 RNA 的 OL3 （chiOL3） 力场，并更正了Na+参数

**parmbsc1和OL15是适用于核酸模拟的力场，添加了核酸方面参数的补丁**。
- ParmBSC1 forcefield：[Molecular Modeling and Bioinformatics Group (irbbarcelona.org)](https://mmb.irbbarcelona.org/ParmBSC1/index.php)
- OL Force Field：[OL Force Field Refinement for RNA and DNA Simulations (upol.cz)](https://fch.upol.cz/ff_ol/gromacs.php)

amber14SB是用于蛋白模拟的Amber力场；而Parmbsc1和OL15是适用于核酸模拟的力场，尤其是DNA，目前Amber官方是建议模拟DNA用OL15，模拟RNA用OL3。

### 糖类的力场 GLYCAM

支持多糖的力场有：CHARMM、OPLSAA、GROMOS、GLYCAM
目前，使用比较广泛的多糖力场是GLYCAM
[GROMACS多糖模拟简单示例|Jerkwin](https://jerkwin.github.io/2017/04/10/GROMACS%E5%A4%9A%E7%B3%96%E6%A8%A1%E6%8B%9F%E7%AE%80%E5%8D%95%E7%A4%BA%E4%BE%8B/)

GLYCAM力场家族，特别是GLYCAM06，已被广泛应用。GLYCAM06是一个一致且可转移的参数集，用于模拟碳水化合物和糖缀合物。
([GLYCAM官网主站点](http://www.glycam.org/))
GLYCAM在线文档主页：http://legacy.glycam.org/docs/
GLAYCAM Force Field: http://legacy.glycam.org/docs/forcefield/
最新版：2014-03-28，the current GLYCAM parameters are version 06j-1
其他版本：http://legacy.glycam.org/docs/forcefield/all-parameters/

与GLYCAM兼容的蛋白质力场：
ff12, ff12SB and later, Compatible GLYCAM leaprc files: leaprc.GLYCAM-06j-1 , leaprc-1.GLYCAM_06i-12SB , leaprc.GLYCAM_06h-12SB

以GLYCAM 06力场为例，在GLYCAM官网在线文档主页的 Force Field子页面的PARAMETERS & ENERGY FUNCTIONS项：[Parameters & Energy Functions | Force Field (glycam.org)](https://glycam.org/docs/forcefield/parameters-energy-functions/)可以下载到最新的GLYCAM力场参数。
目前最新版本为2014-03-28发布的 GLYCAM_06j-1 和 GLYCAM_06EPb ，这些参数也可以在ambertools安装安装目录的leap目录下找到。

GLYCAM力场文件包括 GLYCAM06 和 GLYCAM06EP 两部分：
1. GLYCAM06力场
    - leaprc.GLYCAM_06j-1: 使用GLYCAM06的LEaP配置文件, 可单独用于碳水化合物或与ff14SB力场联合使用
    - GLYCAM_06j.dat: 寡糖参数
    - GLYCAM_06j-1.prep: 糖基残基的结构和电荷
    - GLYCAM_lipids_06h.prep: 一些脂类残基的结构和电荷
    - GLYCAM_amino_06j_12SB.lib: 与ff14SB力场兼容的糖蛋白库文件
    - GLYCAM_aminoct_06j_12SB.lib
    - GLYCAM_aminont_06j_12SB.lib
2. 使用孤对电子(额外点)的GLYCAM06EP力场
    - GLYCAM_06EPb.dat: 寡糖参数
    - GLYCAM_06EPb.prep: 糖残基结构和电荷
    - leaprc.GLYCAM_06EPb: 用于GLYCAM-06EP的LEaP配置文件

注意: **glycam力场中的羧基是去质子化的**

### 小分子力场 GAFF

通用AMBER力场(GAFF, general AMBER force field)，
GAFF完全兼容AMBER大分子力场，在模拟中可以与其它amber力场混合使用，共同用于处理蛋白和药物分子形成的复合物. 就像传统的AMBER力场一样, GAFF使用简单的简谐势函数来描述键和键角. 但与针对蛋白质和DNA的传统AMBER力场不同的是, GAFF使用的原子类型通用得多, 因此可以涵盖大部分有机分子.
GAFF 是一个完整的力场（因此很少出现缺失的参数），它几乎涵盖了由 C、N、O、S、P、H、F、Cl、Br 和 I 组成的所有有机化学空间。此外，由于GAFF与AMBER大分子力场完全兼容，它应该被证明是合理药物设计的有用分子力学工具。特别是在结合自由能计算和分子对接研究中。

## 拓展阅读

- [Amber官网(ambermd.org)](https://ambermd.org/)
- [Amber Tutorials (ambermd.org)](https://ambermd.org/tutorials/)
- [轻松上手Amber&AmberTools|Tanglab@PKU](http://tanglab.pku.edu.cn/2022/12/27/R&D/2022/20221227-AmberTutorial0/)
- [AMBER基础教程|Jerkwin](https://jerkwin.github.io/2018/01/17/AMBER%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8BB0-AMBER%E5%88%86%E5%AD%90%E5%8A%A8%E5%8A%9B%E5%AD%A6%E6%A8%A1%E6%8B%9F%E5%85%A5%E9%97%A8/#amber%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8Bb0)
- [AMBER入门进阶|Hexo](https://fy-han.github.io/tags/AMBER/)
- [WSL2 上的 Amber24 与 Ambertools 24 编译](https://zhuanlan.zhihu.com/p/666826404?utm_id=0)
- [多糖中羧基质子化后的MD拓扑文件如何快速获取 - 分子模拟 (Molecular Modeling) - 计算化学公社 (keinsci.com)](http://bbs.keinsci.com/thread-42684-1-1.html)
- [向GLYCAM添加新残基](https://glycam.org/docs/help/2014/04/04/adding-chemical-derivatives-to-glycam-residues/)
- [纤维素（多糖）及其衍生物GLYCAM力场拓扑文件的构建](https://zhuanlan.zhihu.com/p/684696716)
