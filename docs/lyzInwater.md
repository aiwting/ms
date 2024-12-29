@autoHeader:0
>目标：使用GROMACS执行溶菌酶在水中的MD模拟
# 引言
- 首先安装了GROMACS，这里默认你使用的是Windows版或者是WSL中的GROMACS
- 该教程来自弗吉尼亚理工大学生物化学系的分子动力学教程：www.mdtutorials.com
- 该模拟流程完整清晰，符合一个GROMACS模拟的标准流程，足够简单不需要复杂的准备工作，是了解GROMACS基本操作的**入门案例**
- 此案例将模拟一个包含一个溶菌酶分子的简单的水系统，通过此案例，将实际了解GROMACS执行MD的具体流程，基本命令操作
- GROMACS的所有工具都是以命令形式调用，所以需要在命令行界面操作。命令格式形如：`gmx <module> <parameters>`
  - module 是各种GROMACS工具，以完成生成拓扑参数、构建盒子、添加溶剂、添加离子、执行模拟计算等功能
  - parameters 规定了一条GROMACS命令具体的输入与输出文件，一级一些可选项
  - 例如，想要查询一个GROMACS工具可以接受的参数，使用`gmx <module> -h`，输出依次是：程序在电脑中的路径、GROMACS的安装路径、当前的工作目录、输入的命令、该工具的语法、描述、可选项、其它选项、最后是GROMACS程序的提示语（提示语没用，但看到它表示此次命令执行成功）。
```term
// GROMACS命令示例
$ gmx pdb2gmx -h
               :-) GROMACS - gmx pdb2gmx, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/ms
Command line:
  gmx pdb2gmx -h

SYNOPSIS

gmx pdb2gmx [-f [<.gro/.g96/...>]] [-o [<.gro/.g96/...>]] [-p [<.top>]]
            [-i [<.itp>]] [-n [<.ndx>]] [-q [<.gro/.g96/...>]]
            [-chainsep <enum>] [-merge <enum>] [-ff <string>] [-water <enum>]
            [-[no]inter] [-[no]ss] [-[no]ter] [-[no]lys] [-[no]arg]
            [-[no]asp] [-[no]glu] [-[no]gln] [-[no]his] [-angle <real>]
            [-dist <real>] [-[no]una] [-[no]ignh] [-[no]missing] [-[no]v]
            [-posrefc <real>] [-vsite <enum>] [-[no]heavyh] [-[no]deuterate]
            [-[no]chargegrp] [-[no]cmap] [-[no]renum] [-[no]rtpres]

DESCRIPTION
...
OPTIONS
...
Other options:
...
GROMACS reminds you: "If we knew what it was we were doing, it would not be called research, would it?" (Albert Einstein)
```

# 准备文件
- 获得蛋白质结构文件，去[RCSB](https://www.rcsb.org/)网站下载溶菌酶的pdb文件，搜索PDB ID 1AKI >点击右侧“Download Files” >PDB Format，将得到"1aki.pdb"
- 现在进入VScode，复制一份"1aki.pdb"文件并打开，可以看到包含了结晶水"HOH"，删除所有"HOH"所在行，命名为"1aki-h2o.pdb"
- 注意检查蛋白质pdb文件结构，一个标准的蛋白质pdb文件包含了：
```

```

# 生成拓扑结构
>GROMACS工具：pdb2gmx

- pdb2gmx命令会生成三个文件：
  1. 分子的拓扑结构.top
  2. 位置限制文件.itp
  3. GROMACS格式的结构文件.gro
- 需要输入数字来选择力场和水模型，相关信息将写入top文件中。力场选择"AMBER99SB"，水模型无特殊需求选择简单点电荷水即可"TIP3P"
```term
$ gmx pdb2gmx -f 1aki-h2o.pdb -p 1aki.top -i 1aki.itp -o 1aki.gro
               :-) GROMACS - gmx pdb2gmx, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/teach
Command line:
  gmx pdb2gmx -f 1aki-h2o.pdb -p 1aki.top -i 1aki.itp -o 1aki.gro

Select the Force Field:
From '/home/zz/miniconda3/envs/md/share/gromacs/top':
 1: AMBER03 protein, nucleic AMBER94 (Duan et al., J. Comp. Chem. 24, 1999-2012, 2003)
 2: AMBER94 force field (Cornell et al., JACS 117, 5179-5197, 1995)
 3: AMBER96 protein, nucleic AMBER94 (Kollman et al., Acc. Chem. Res. 29, 461-469, 1996)
 4: AMBER99 protein, nucleic AMBER94 (Wang et al., J. Comp. Chem. 21, 1049-1074, 2000)
 5: AMBER99SB protein, nucleic AMBER94 (Hornak et al., Proteins 65, 712-725, 2006)
 6: AMBER99SB-ILDN protein, nucleic AMBER94 (Lindorff-Larsen et al., Proteins 78, 1950-58, 2010)
 7: AMBERGS force field (Garcia & Sanbonmatsu, PNAS 99, 2782-2787, 2002)
 8: CHARMM27 all-atom force field (CHARM22 plus CMAP for proteins)
 9: GROMOS96 43a1 force field
10: GROMOS96 43a2 force field (improved alkane dihedrals)
11: GROMOS96 45a3 force field (Schuler JCC 2001 22 1205)
12: GROMOS96 53a5 force field (JCC 2004 vol 25 pag 1656)
13: GROMOS96 53a6 force field (JCC 2004 vol 25 pag 1656)
14: GROMOS96 54a7 force field (Eur. Biophys. J. (2011), 40,, 843-856, DOI: 10.1007/s00249-011-0700-9)
15: OPLS-AA/L all-atom force field (2001 aminoacid dihedrals)

$ 5
Using the Amber99sb force field in directory amber99sb.ff
Opening force field file /home/zz/miniconda3/envs/md/share/gromacs/top/amber99sb.ff/watermodels.dat
Select the Water Model:
 1: TIP3P     TIP 3-point, recommended
 2: TIP4P     TIP 4-point
 3: TIP4P-Ew  TIP 4-point optimized with Ewald, recommended
 4: TIP5P     TIP 5-point (see https://gitlab.com/gromacs/gromacs/-/issues/1348 for issues)
 5: SPC       simple point charge
 6: SPC/E     extended simple point charge
 7: None

$ 1
going to rename amber99sb.ff/aminoacids.r2b
Opening force field file /home/zz/miniconda3/envs/md/share/gromacs/top/amber99sb.ff/aminoacids.r2b
......

Analyzing pdb file
......

Total mass 14313.332 a.m.u.
Total charge 8.000 e
Writing topology
Writing coordinate file...

                --------- PLEASE NOTE ------------
You have successfully generated a topology from: 1aki-h2o.pdb.
The Amber99sb force field and the tip3p water model are used.

                --------- ETON ESAELP ------------
GROMACS reminds you: "The Carpenter Goes Bang Bang" (The Breeders)
```
- 现在使用VMD观察.top文件，在命令窗口输入`pbc box`回车，会发现蛋白质没有完全在盒子内，接下来进行调整

# 调整模拟盒子
>GROMACS工具：editconf
- 使用简单的立方体作为盒子，将蛋白质剧中，距离边界至少0.2nm
```term
$ gmx editconf -f 1aki.gro -o 1aki_newbox.gro -bt cubic -c -d 0.2
               :-) GROMACS - gmx editconf, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/teach
Command line:
  gmx editconf -f 1aki.gro -o 1aki_newbox.gro -bt cubic -c -d 0.2

Note that major changes are planned in future for editconf, to improve usability and utility.
Read 1960 atoms
Volume: 123.376 nm^3, corresponds to roughly 55500 electrons
No velocities found
    system size :  3.817  4.234  3.454 (nm)
    diameter    :  5.010               (nm)
    center      :  2.781  2.488  0.017 (nm)
    box vectors :  5.906  6.845  3.052 (nm)
    box angles  :  90.00  90.00  90.00 (degrees)
    box volume  : 123.38               (nm^3)
    shift       : -0.076  0.217  2.688 (nm)
new center      :  2.705  2.705  2.705 (nm)
new box vectors :  5.410  5.410  5.410 (nm)
new box angles  :  90.00  90.00  90.00 (degrees)
new box volume  : 158.35               (nm^3)

Back Off! I just backed up 1aki_newbox.gro to ./#1aki_newbox.gro.1#

GROMACS reminds you: "Predictions can be very difficult - especially about the future." (Niels Bohr)
```
- 再次使用VMD观察"1aki_newbox.gro"，可以看到盒子蛋白质位于盒子中央

# 添加溶剂（溶剂化蛋白质）
>GROMACS工具：solvate

- 使用水模型 spc216.gro（这是一个通用的三点水模型）填充盒子，并指定更新top文件（因为引入了新的分子，每次向盒子中添加新分子都需要更新top文件）
- 注意这里重复使用了"1aki.top"文件名，因此原来的文件将被备份为"#1aki.top.1#"
```term
$ gmx solvate -cp 1aki_newbox.gro -cs spc216.gro -o 1aki_solvate.gro -p 1aki.top
               :-) GROMACS - gmx solvate, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/teach
Command line:
  gmx solvate -cp 1aki_newbox.gro -cs spc216.gro -o 1aki_solvate.gro -p 1aki.top

Reading solute configuration
......
Generating solvent configuration
......

Output configuration contains 15373 atoms in 4600 residues
Volume                 :     158.347 (nm^3)
Density                :     1004.79 (g/l)
Number of solvent molecules:   4471

Processing topology
Adding line for 4471 solvent molecules with resname (SOL) to topology file (1aki.top)

Back Off! I just backed up 1aki.top to ./#1aki.top.1#

GROMACS reminds you: "I wanted to have a real language where programmers could write real programs, and see whether they would find the idea of data abstraction at 
all useful." (Barbara Liskov)
```
- 重新观察一下新的"1aki.top"文件，最下面将多一行"SOL 4471"表示4471个溶剂分子
```
[ molecules ]
; Compound        #mols
Protein_chain_A     1
SOL              4471
```
- 还可以用VMD查看一下新的"1aki_solvate.gro"，可以看到水分子充满了盒子，溶菌酶被包裹在中间。

# 添加离子
>GROMACS工具：grompp和genion

- 如果你注意pdb2gmx命令的输出，可以看到溶菌酶的净电荷为+8e ："Total charge 8.000 e"，因此需要添加离子中和。
- genion工具需要读取扩展名为.tpr的文件，因此首先用grompp工具先生成tpr文件
- 要使用grompp生成一个.tpr文件，还需要一个扩展名为.mdp的文件，可以在此处下载示例[ions.mdp](https://aiwting.github.io/ms/ions.mdp)文件
- grompp将动力学参数、结构信息、拓扑参数三部分信息整理成二进制的tpr文件。
```term
$ gmx grompp -f ions.mdp -c 1aki_solvate.gro -p 1aki.top -o ions.tpr
                :-) GROMACS - gmx grompp, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/teach
Command line:
  gmx grompp -f ions.mdp -c 1aki_solvate.gro -p 1aki.top -o ions.tpr

Setting the LD random seed to 1608515534

Generated 2145 of the 2145 non-bonded parameter combinations
Generating 1-4 interactions: fudge = 0.5

Generated 2145 of the 2145 1-4 parameter combinations

Excluding 3 bonded neighbours molecule type 'Protein_chain_A'

Excluding 2 bonded neighbours molecule type 'SOL'

NOTE 1 [file 1aki.top, line 18409]:
  System has non-zero total charge: 8.000000
  Total charge should normally be an integer. See
  https://manual.gromacs.org/current/user-guide/floating-point.html
  for discussion on how close it should be to an integer.



Analysing residue names:
There are:   129    Protein residues
There are:  4471      Water residues
Analysing Protein...
Number of degrees of freedom in T-Coupling group rest is 32703.00
The integrator does not provide a ensemble temperature, there is no system ensemble temperature

NOTE 2 [file ions.mdp]:
  You are using a plain Coulomb cut-off, which might produce artifacts.
  You might want to consider using PME electrostatics.



This run will generate roughly 1 Mb of data

There were 2 NOTEs

GROMACS reminds you: "The future always gets twisted and turned" (Lisa o Piu)


```
- 接下来正式添加离子，指定阳离子为钠离子，阴离子为氯离子，使体系保持电中性"-neutral"，并更新top文件。
- 过程中需要输入数字选择溶剂组"SOL"
```term
$ gmx genion -s ions.tpr -o 1aki_solvate_ions.gro -pname NA -nname CL -neutral -p 1aki.top
                :-) GROMACS - gmx genion, 2024.3-conda_forge (-:

Executable:   /home/zz/miniconda3/envs/md/bin.AVX2_256/gmx
Data prefix:  /home/zz/miniconda3/envs/md
Working dir:  /mnt/e/share/teach
Command line:
  gmx genion -s ions.tpr -o 1aki_solvate_ions.gro -pname NA -nname CL -neutral -p 1aki.top

Reading file ions.tpr, VERSION 2024.3-conda_forge (single precision)
Reading file ions.tpr, VERSION 2024.3-conda_forge (single precision)
Will try to add 0 NA ions and 8 CL ions.
Select a continuous group of solvent molecules
Group     0 (         System) has 15373 elements
Group     1 (        Protein) has  1960 elements
Group     2 (      Protein-H) has  1001 elements
Group     3 (        C-alpha) has   129 elements
Group     4 (       Backbone) has   387 elements
Group     5 (      MainChain) has   517 elements
Group     6 (   MainChain+Cb) has   634 elements
Group     7 (    MainChain+H) has   646 elements
Group     8 (      SideChain) has  1314 elements
Group     9 (    SideChain-H) has   484 elements
Group    10 (    Prot-Masses) has  1960 elements
Group    11 (    non-Protein) has 13413 elements
Group    12 (          Water) has 13413 elements
Group    13 (            SOL) has 13413 elements
Group    14 (      non-Water) has  1960 elements
Select a group: 
$ 13
Selected 13: 'SOL'
Number of (3-atomic) solvent molecules: 4471

Processing topology
Replacing 8 solute molecules in topology file (1aki.top)  by 0 NA and 8 CL ions.

Back Off! I just backed up 1aki.top to ./#1aki.top.2#
Using random seed -54592018.
Replacing solvent molecule 855 (atom 4525) with CL
Replacing solvent molecule 232 (atom 2656) with CL
Replacing solvent molecule 1855 (atom 7525) with CL
Replacing solvent molecule 4162 (atom 14446) with CL
Replacing solvent molecule 72 (atom 2176) with CL
Replacing solvent molecule 3463 (atom 12349) with CL
Replacing solvent molecule 4122 (atom 14326) with CL
Replacing solvent molecule 1838 (atom 7474) with CL

GROMACS reminds you: "Torture numbers, and they'll confess to anything." (Greg Easterbrook)
```
- 现在检查一下新的top文件，最下面应该多了"CL 8"
```
[ molecules ]
; Compound        #mols
Protein_chain_A     1
SOL         4463
CL               8
```

# 能量最小化
>GROMACS工具：grompp和mdrun

- 在开始动力学之前，必须确保系统没有空间冲突或不适当的几何形状（尤其是对于自行绘制的分子结构，你没有办法精确的控制没个键长、键角的数值），通过能量最小化（EM）的过程来使得分子结构合理化。
- 但凡设计到使用grompp生成tpr文件，都需要mdp动力学参数文件，可以使用这里的示例[em.mdp]()文件。
```term
$ gmx grompp -f em.mdp -c 1aki_solvate_ions.gro -p 1aki.top -o em.tpr

```
- 然后执行能量最小化计算
  - `-deffnm em`是简写方法，意思是让mdrun工具以em为名的tpr文件为输入，并使得使出的各种格式的文件以em名字命名。
  - `-v`表示在命令行中显示计算过程
```term
$ gmx mdrun -deffnm em -v


```
- 计算完成后将得到四个文件
  1. em.log：日志文件
  2. em.edr：二进制能量文件
  3. em.trr：二进制轨迹文件
  4. em.gro：能量最小化结构
- 如何确定EM是否成功？

# NVT平衡


# NPT平衡


# MD模拟


# 结果分析