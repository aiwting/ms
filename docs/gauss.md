@autoHeader:0
>目标：安装并使用最流行的精确量子化学计算软件。
# 概述
- Gaussian是一款功能非常强大的量子化学综合软件包，基于半经验计算和从头计算。主要对象：分子、团簇等孤立体系。体系大小：几个至几十个、几百个原子。
- 权威性：在计算化学领域具有重要地位，被广泛用于学术研究和工业应用
- 精确性：Gaussian采用完整的数学和化学方法，能够提供精确、可靠的结果，是其他电子结构程序的参考标准。
- GaussView：GaussView是Gaussian的图形建模和结果显示界面，是Gaussian的前端和后台使用的辅助工具，是独立模块。Gaussian是运算程序，GaussView是视图界面程序，后者可以图形化前者的输入/输出运算文件。GaussView内设Gaussian的链接，只有安装了两者，且在同一个目录中，可直接从GaussView中调用Gaussian进行计算。
- GaussView可以用来创建高斯输入文件 .gif
- 能量计算与结构优化原理：通过各种近似去找到Schrödinger方程的解，而不同的近似方法就对应了不同的精度，这些理论方法也称为 ~~姿势~~ 理论水平（Levels of Theory）。
- 理论水平（Levels of Theory）：Gaussian针对于不同大小的体系，可以选用不同的方法，如使用牛顿力学的分子力学方法（**MM2、UFF**）、半经验方法（**PM6、AM1**）、Hartree-Fock理论（**HF**）、Møller-Plesset微扰理论（**MP2、MP4**）、耦合簇理论（**CCSD(T)**）、密度泛函理论DFT（**B3LYP、APFD、M06、CAM-B3LYP**）等等。
- 基组（Basis Set）：基组是量子力学用来描述分子波函数的一系列数学函数。基组将电子限制在特定的空间区域之中，是由原子轨道的概念发展而来。常见的基组包括：最小基组，劈裂价键基组（极化基组，弥散基组），以及涉及到电子相关作用的高角动量基组。  
![alt text](<_media/Pasted image 20240824105926.png>)
# 安装
gaussian既可以在Linux也可以在Windows安装，Windows安装更加简单，且win版有图形化界面，更适合新手。  
因为是商业软件，提供网络资源>>[百度网盘]()<<
## Windows上安装
- 先安装 Gaussian 09W 再安装 GaussView 5.0
- Gaussian 09W 和 GaussView 5.0 要安装在同一文件夹，不要写中文路径
- 安装步骤根据随附的pdf操作即可

# 使用
## 认识 .gif文件
- 高斯输入文件在关键词部分，不区分大小写，并且可以适当缩减，只要不产生歧义
- 高斯输入文件内容：
```.gif
%mem=3GB
%nproc=6
%chk=C:\Users\Administrator\Desktop\Methoxyacetic_acid.chk
# hf/6-31g* opt freq pop=mk iop(6/33=2,6/42=6,6/50=1)

0 1
 O    -1.33600000   -0.38800000   -0.00900000
 O     2.01000000    0.89200000   -0.00800000
 O     1.24500000   -1.25300000    0.00300000
 C    -0.32000000    0.60600000    0.01200000
 C     1.03300000   -0.05000000    0.00300000
 C    -2.63300000    0.19200000   -0.00100000
 H    -0.40800000    1.23800000   -0.87700000
 H    -0.41100000    1.20400000    0.92500000
 H    -2.78300000    0.78400000    0.90800000
 H    -3.37100000   -0.61400000   -0.01800000
 H    -2.77900000    0.81800000   -0.88600000
 H     2.90500000    0.49100000   -0.01500000

C:\Users\Administrator\Desktop\CMC_ini.gesp  

C:\Users\Administrator\Desktop\CMC.gesp


```
从上到下依次是：
1. 基本设置：%标识
   - 内存、核数、检查点文件输出位置
2. 计算关键词：#标识
   - 基组、计算任务（opt优化、freq振动分析、）……
   - `Pop=MK`是生成Antechamber可用的gesp文件的关键词
   - `iop(6/33=2)`是进行RESP Fitting并输出到Gaussian的.log文件。
   - `iop(6/42=6)`是指定精度的关键字之一。
   - `iop(6/50=1)`是Gaussian 09 C.01之后推荐的独立于高斯输出文件的resp文件格式。在antechamber中称为"gesp"，使用时需要在高斯输入文件末尾指定单独gesp的文件名称。
   - 注意：Gaussian 09B.01版本（可能还有G09A，没有该版本不知道）“误删”了RESP Fitting的代码，所以以上关键字没一个管用。  Gaussian 09C.01及后续版本复了误删的代码并且加上了gesp的代码，可以使用。
   - 详细参考：[Overlay 6 | Gaussian.com](https://gaussian.com/overlay6/#iop_%286/42%29)
3. 结构信息：
   - 分子名
   - 电荷 自旋多重度
   - 原子坐标
   - 键连关系
4. 输出控制
5. 文件末尾至少有两个空行，防止报错

注意：
1. 在win版下，用gview直接提交的任务，输出默认是.log。用Gaussian打开的输入文件计算，默认输出.out。
2. 输出文件记得指定输出路径，若不指定路径会默认输出到安装目录下的 Scratch文件夹。
3. Scratch文件夹：‌高斯Scratch文件夹‌是一个专门用于Gaussian计算的临时文件夹，用于存放计算过程中产生的临时文件。

# 拓展阅读
- [Gaussian入门 - 清化科协 (thuchemst.github.io)](https://thuchemst.github.io/2016/09/22/gaussian-introduction/)
- [使用gaussian和antechamber拟合RESP电荷过程 - 计算之道 - 博客园 (cnblogs.com)](https://www.cnblogs.com/jszd/p/14163254.html)
- [使用gaussian和antechamber拟合RESP电荷过程_antechamber使用高斯方法-CSDN博客](https://blog.csdn.net/weixin_42486623/article/details/129055384)
- [利用高斯和Amber生成乙酰辅酶A配体的拓扑文件_em=gd3bj-CSDN博客](https://blog.csdn.net/yu_bio/article/details/109645826?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7Ebaidujs_baidulandingword%7ECtr-1-109645826-blog-129055384.235%5Ev43%5Epc_blog_bottom_relevance_base7&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7Ebaidujs_baidulandingword%7ECtr-1-109645826-blog-129055384.235%5Ev43%5Epc_blog_bottom_relevance_base7&utm_relevant_index=2)
- [科学网—Gaussian高斯里计算resp电荷方法 - 徐扬的博文 (sciencenet.cn)](https://blog.sciencenet.cn/home.php?mod=space&uid=3366368&do=blog&id=1079996)
- [用gaussian和AmberTool拟合resp电荷出现问题 - 量子化学 (Quantum Chemistry) - 计算化学公社 (keinsci.com)](http://bbs.keinsci.com/thread-2019-1-1.html)
- [科学网—使用AmberTools+ACPYPE+Gaussian创建小分子GAFF力场的拓扑文件 - 李继存的博文 (sciencenet.cn)](https://blog.sciencenet.cn/blog-548663-942117.html)
- [Gaussian的安装方法及运行时的相关问题 - 思想家公社的门口：量子化学·分子模拟·二次元 (sobereva.com)](http://sobereva.com/439)
- 官网：[Gaussian.com | Expanding the limits of computational chemistry](https://gaussian.com/)