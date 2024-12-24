# 认识Amber
**Amber**，是一组用于模拟生物分子的分子力学力场；也是一套生物分子模拟程序。
# Amber programs
Amber程序分为两部分：AmberTools 和 Amber，AmberTools是免费的，Amber是收费的、提供学术版。
- Amber是一个完整的分子动力学模拟软件套件，提供了模拟引擎、力场参数化工具和分析工具等功能。
- AmberTools是Amber软件套件的一部分，提供了一些额外的工具和实用程序，用于辅助分子模拟的前后处理。如构象生成、参数化、模型构建、能量计算、轨迹分析等
- Amber全称Assisted Model Building with Energy Refinemegnt， 是由University of California, San Francisco的Kollman教授课题组开发的一套分子力学和分子动力学模拟软件，包括大约60个程序，如 sander. xleap, antechamber、ptraj、mm_pbsa和nmode等。
- Amber可以用于生物大分子体系的分子力学计算和分子动力学模拟，也可以采用MM_PBSA和MM_GBSA方法研究受体和配体之间的结合自由能，还可进行量子力学和分子力学结合(QM/MM)计算。
- Amber的优点是计算精度高，自定义新分子和新模型比较方便。缺点是计算效率低，速度慢
# Amber Force Fields
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

# amber的文件
amber 的文件格式规范，在amber官网的 File Formats 页面可以找到各种文件的描述：[Amber File Formats (ambermd.org)](https://ambermd.org/FileFormats.php)
## 力场参数文件
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
### leaprc.*
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
### \*.dat
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
### frcmod.* 或 \*frcmod
也在 /pram 文件夹下，frcmod.* 文件与  \*.dat文件的格式基本相同。可以像标准的 \*.dat文件一样读入tleap，将 \*.dat文件中的默认参数与 frcmod.* 中的新参数合并。
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
### \*.prep 或 \*prepin 或 \*in
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
### \*.lib
- /lib 文件夹下的 .lib 文件记录了氨基酸残基的符号及每个残基的描述
- 例如 GLYCAM_animo_06j_12SB.lib 文件，为了将聚糖与蛋白质连接起来，必须加载含有修饰氨基酸残基（Ser、Thr、Hyp 和 Asn）的lib。要使用 ff14SB 构建糖蛋白，必须加载 `GLYCAM_amino_06j_12SB.lib` `GLYCAM_aminont_06j_12SB.lib` 和 `GLYCAM_aminoct_06j_12SB.lib`，并且还必须加载所需的蛋白质力场。
## 模拟相关文件
- prmtop文件，又名parm7，top，包含分子，原子名称、种类，电荷，键关系，键参数和非键参数信息
- .prmtop 和 .prmcrd：topology and coordinate files，可以由 LEaP 程序生成。
- .top 和 .crd
- .inpcrd：input initial coordinate file，包含了原子的所有坐标，以及模拟盒子的形状。
- .mdcrd：轨迹文件

# 拓展阅读
- [Amber官网(ambermd.org)](https://ambermd.org/)
- [Amber Tutorials (ambermd.org)](https://ambermd.org/tutorials/)
- [轻松上手Amber&AmberTools|Tanglab@PKU](http://tanglab.pku.edu.cn/2022/12/27/R&D/2022/20221227-AmberTutorial0/)
- [AMBER基础教程|Jerkwin](https://jerkwin.github.io/2018/01/17/AMBER%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8BB0-AMBER%E5%88%86%E5%AD%90%E5%8A%A8%E5%8A%9B%E5%AD%A6%E6%A8%A1%E6%8B%9F%E5%85%A5%E9%97%A8/#amber%E5%9F%BA%E7%A1%80%E6%95%99%E7%A8%8Bb0)
- [AMBER入门进阶|Hexo](https://fy-han.github.io/tags/AMBER/)