# GLYCAM力场介绍

## GLYCAM 的糖类命名规则

- GLYCAM_06j-1.prep 是GLYCAM力场中的拓扑数据库文件，文件中记录了糖类残基的分子结构和原子电荷
- 各个残基名称采用一套三字符的编码命名方法: 基于 RCSB PDB Advisory Committee 对蛋白质pdb文件施加的标准[wwPDB: File Format](https://www.wwpdb.org/documentation/file-format)，蛋白和核酸残基都采用三字符编码表示，且所有的建模和模拟软件都可以读入三字符编码

<center style="color:#C0C0C0; text-decoration:underline"><br/>表1. 糖的三字符命名规则<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/cellulose_naming.gif" width=90%></center>

- 对以下内容进行编码：碳水化合物残基名称（Glc、Gal等）、环形（吡喃糖或呋喃糖）、异构构象（α或β）、对映异构体形式（D或L）和占据的键合位置（2-、2,3-、2,4,6- 等）。

GLYCAM_06j-1.prep 文件中的第一个残基示例：

```.prep
0AA  INT 0

CORRECT OMIT DU BEG
 0.1940
 1 DUMM DU  M  0 -1 -2  0.000   0.000    0.00     0.0000
 2 DUMM DU  M  1  0 -1  0.500   0.000    0.00     0.0000
 3 DUMM DU  M  2  1  0  1.296  74.264    0.00     0.0000
 4 C1   Cg  M  3  2  1 25.676  79.166 -133.49     0.4480
 5 H1   H2  E  4  3  2  1.091  24.223   90.04     0.0000
 6 C2   Cg  3  4  3  2  1.547  97.401  -30.99     0.2680
 7 H2   H1  E  6  4  3  1.092 109.940  -51.47     0.0000
 8 C3   Cg  3  6  4  3  1.543 115.075 -176.20     0.3350
 9 H3   H1  E  8  6  4  1.091 110.217 -167.01     0.0000
10 C4   Cg  3  8  6  4  1.543 111.532  -44.66     0.2450
11 H4   H1  E 10  8  6  1.091 108.482  -68.58     0.0000
12 C5   Cg  3 10  8  6  1.532 112.944   51.47     0.3790
13 H5E  H1  E 12 10  8  1.090 110.447 -175.26     0.0000
14 H5A  H1  E 12 10  8  1.091 110.923   64.60     0.0000
15 O5   Os  E 12 10  8  1.479 108.551  -55.64    -0.5640
16 O4   Oh  S 10  8  6  1.437 110.197  173.67    -0.7480
17 H4O  Ho  E 16 10  8  0.960 109.412  -61.02     0.4340
18 O3   Oh  S  8  6  4  1.441 106.949   75.66    -0.7720
19 H3O  Ho  E 18  8  6  0.960 109.473   59.41     0.4610
20 O2   Oh  S  6  4  3  1.437 106.880   64.80    -0.7350
21 H2O  Ho  E 20  6  4  0.960 109.480 -177.98     0.4430

LOOP
O5 C1

DONE
```

以文件中第一个残基 0AA 为例：

残基0AA的含义: 
- 0 = 没有氧原子成键
- A = 阿拉伯糖
- A = α configuration

(1). 第一个字符表示共价键合的位置，0表示该残基为末端（非还原端）残基没有氧原子能够成键，4、6分别表示在4号、6号位氧原子上连接有其它残基，U表示在4号、6号位氧原子上均连接有其它残基

<center style="color:#C0C0C0; text-decoration:underline"><br/>表2. D-吡喃型戊糖和己糖残基的共价键和位置与端基异构构象表<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/table2.jpg" width=90%></center>

<center style="color:#C0C0C0; text-decoration:underline"><br/>表3. L-吡喃型戊糖和己糖残基的共价键和位置与端基异构构象表<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/table4-5.jpg" width=90%></center>

<center style="color:#C0C0C0; text-decoration:underline"><br/>表4. D-呋喃型戊糖和己糖残基的共价键和位置与端基异构构象表<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/table3.jpg" width=90%></center>

<center style="color:#C0C0C0; text-decoration:underline"><br/>表5. 单糖衍生物的键合位置和异构构象表<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/table4-5.jpg" width=90%></center>

(2). 第二个字符表示单糖类型，一般取自单糖名称的第一个字母，是残基名称的核心，**大写表示D型、小写表示L型**

<center style="color:#C0C0C0; text-decoration:underline"><br/>表6. 单糖残基的单字母编码表<img src="https://glycam.org/docs/forcefield/wp-content/uploads/sites/6/2013/09/table1.jpg" width=90%></center>

(3). 第三个字符表示构象，**在吡喃糖中B表示β构象、A表示α构象，在呋喃糖中U表示β构象、D表示α构象**

<center style="color:#C0C0C0; text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/yr0qr7-0.png" width=30%><br/>图1. 糖的不同构象</center>

(4). 其它特殊，ROH表示还原端羟基，ACX表示乙酰基，MEX表示甲基，OME表示甲氧基，SO3表示磺酸基，TBT表示叔丁基，Ca2+，NLN天冬酰胺，OLS丝氨酸，ZOLS丝氨酸的两性离子，OLT苏氨酸，ZOLT苏氨酸的两性离子

详细的命名规则查看Glycam官方文档-力场-命名：

- [Glycam Naming | Force Field](https://glycam.org/docs/forcefield/index.html)
- [糖及其GLYCAM力场中的命名约定|Jerkwin](https://jerkwin.github.io/2017/03/31/%E7%B3%96%E5%8F%8A%E5%85%B6GLYCAM%E5%8A%9B%E5%9C%BA%E4%B8%AD%E7%9A%84%E5%91%BD%E5%90%8D%E7%BA%A6%E5%AE%9A/)