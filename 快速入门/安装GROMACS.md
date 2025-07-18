# 安装GROMACS
## GROMACS的版本和平台：

- GROMACS更新很快，有多个版本：
    1. 支持CUDA加速的版本：需要 nVidia 独立显卡.
    2. 支持并行计算的版：一般用于含多个处理器(CPU)的计算服务器.
    3. 单/双精度版本：一般计算, 单精度版本就够了, 单精度版速度更快生成文件更小, 双精度版不支持GPU加速.
- GROMACS是开源的，理论上可以编译到任何你想要的平台使用：Windows、Linux、Mac, 但最常用的是Linux平台.

注意事项:

1. 只用来学习和熟悉GROMACS基本操作，或者没有显卡，同时是Windows系统，推荐完成 *在Windows系统安装*  即可。
2. 有显卡的情况下，或者想要执行一个模拟全流程，推荐 *通过conda安装*。

## 在Windows系统安装

- GROMACS一般都是在Linux下运行，效率也更高，**优先选择Linux系统**
- 如果实在不想安装Linux系统，有以下解决方案：
    - 从源码编译原生的Windows版gromacs：*此方法过于复杂，建议别试！*
    - 这里提供两个由[sobereva](http://sobereva.com/458)编译的版本：可直接在Windows命令行使用，选择对应的AVX指令集下载解压, **然后添加PATH环境变量即可**。缺点是版本较老，只有2020年版。
        1. [2020.3 CUDA GPU加速版，AVX指令集（93 MB）](http://sobereva.com/soft/gmx/gmx2020.3_AVX_CUDA_win64.rar)
        2. [2020.6 CUDA GPU加速版，AVX2指令集（88 MB）](http://sobereva.com/soft/gmx/gmx2020.6_AVX2_CUDA_win64.rar)

> CUDA GPU加速版并不需要在机子里安装CUDA toolkit，只要确保nVidia显卡驱动足够新就可以直接使用。同时核显也能用。

## 在Linux系统安装

**首先需要安装一个Linux环境**, 参见 *安装Linux*，再进行以下的步骤。大抵来说有两种解决方案：

1. 某些发行商预编译的版本
    - 一些Linux发行版的包管理工具就能安装GROMACS, 但一般版本较老.
    - 经过我的摸索发现 Conda-forge 社区编译的GROMACS版本最全最新。因此通过 [Conda](https://docs.anaconda.com/miniconda/) 包管理工具，直接搭建一个用于分子模拟虚拟环境是个不错的选择。
    - 此外，这个环境可以不仅仅包含GROMACS，还能安装其它模拟过程中需要用到的工具，因此非常推荐使用该方式。省去了编译中易报错的缺点，同时可以一步到位安装其它必要的工具。**强烈推荐新手选择此安装方式**
2. 从源码编译安装（理论上效率最高, 也最复杂, 但比在Windows上编译容易些），*有能力的可以尝试*

### 通过conda安装

#### 安装conda环境

> 以下演示命令均在 Linux shell 中执行

打开Linux命令行，输入以下命令：安装并配置仓库镜像源.

```sh
cd ~
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
conda init --all
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda activate
```

注意:  
理论上conda是不挑操作系统的, 官网安装教程: [Installing Miniconda - Anaconda](https://www.anaconda.com/docs/getting-started/miniconda/install)

#### 只安装GROMACS

运行命令 `conda search gromacs`可以产看有哪些 GROMACS 版本，部分输出如下：

|         | gromacs版本                                          |
| ------- | -------------------------------------------------- |
| gromacs | 2024.3 mpi_openmpi_cuda_he6b8466_1  conda-forge    |
| gromacs | 2024.3 mpi_openmpi_dblprec_h8652d4b_1  conda-forge |
| gromacs | mpi_openmpi_h9e48474_1  conda-forge                |
| gromacs | 2024.3 nompi_h5f56185_101  conda-forge             |
| gromacs | 2024.3 nompi_cuda_h5cb645a_0  conda-forge          |
| gromacs | 2024.3 nompi_cuda_h5cb645a_1  conda-forge          |
| gromacs | 2024.3 nompi_dblprec_h76c4001_0  conda-forge       |
| gromacs | 2024.3 nompi_dblprec_h76c4001_1  conda-forge       |
| gromacs | 2024.3 nompi_h5f56185_100  conda-forge             |
| gromacs | 2024.3 nompi_h5f56185_101  conda-forge             |

这里我们创建一个名为 gmx 的环境，并安装对应的GROMACS版本：
- 个人电脑(单个CPU)安装带nompi标识版
- 有nvidia显卡安装带cuda标识版
- 通常情况选择单精度版即不带dblprec标识版(计算速度更快体积更小)

```bash
conda create -n gmx
conda activate gmx
# 下面两个版本选择一个, 区别在于有无独立显卡
conda install gromacs=2024.3=nompi_cuda_h5cb645a_1
conda install gromacs=2024.3=nompi_h5f56185_101
# 这个是并行版本(个人计算机不用安装)
conda install gromacs=2024.3=mpi_openmpi_cuda_he6b8466_1
```

#### 安装GROMACS及其它软件

- **推荐使用此方式安装**, 一步到位, 几乎包含了所有你会用到的模拟相关程序. 这些软件在今后的案例中可能会用到, 在后面的章节中也会单独介绍.
- 安装GROMACS及一些其它的分子模拟相关软件（可选）：注意按照顺序执行命令, 否则可能出现软件依赖关系错误!

```bash
# 创建并激活环境
conda create -n ms
conda activate ms
# 安装gmx_mmpbsa及ambertools
conda install gmx_mmpbsa=1.6.3 ambertools=23.6
# 安装GROMACS及LAMMPS(根据有无显卡选择一个适合的版本安装)
conda install gromacs=2024.3=nompi_cuda_h5cb645a_1
conda install gromacs=2024.3=nompi_h5f56185_101
conda install lammps=2024.08.29=cuda118_py39_h23eba0e_nompi_0
conda install lammps=2024.08.29=cpu_py39_h12dfb8d_nompi_0
# 安装结构文件可视化软件pymol及VMD
conda install pymol-open-source vmd
# 安装静电势计算工具
conda install apbs=1.5 pdb2pqr=3.6.1
# 安装二级结构计算工具
conda install dssp
# 安装拓扑结构处理工具acpype, openbabel, vermouth
conda install vermouth acpype openbabel
```

注意事项:

- 如果你觉得上述过程还是麻烦，可以使用下面配置好的conda环境文件, 将他们复制保存到工作目录, 然后运行下面的命令安装, 可以代替上述命令步骤. 
    - 有显卡选择: [ms_cuda_env.yml](https://aiwting.github.io/ms/files/ms_cuda_env.yml)
    - 没有显卡选择: [ms_env.yml](https://aiwting.github.io/ms/files/ms_env.yml)
    - 备用: [百度网盘下载](https://pan.baidu.com/s/1J1wxRVTDOeZzyYDF9gFr2g?pwd=bd2e)
    - 然后在执行命令 `conda env create --file ms_cuda_env.yml`
- 由于gmx_mmpbsa的依赖限制原因, 模拟软件都选择安装nompi版本, 如果需要mpi版本, 则不要将gmx_mmpbsa与模拟软件安装在一个环境中, 重新创建一个环境安装gmx_mmpbsa.
    - 示例: 

```
conda install lammps=2024.08.29=cuda118_py312_hfe4aceb_mpi_openmpi_0 ambertools=23.6=cuda_11.8_openmpi_py312h755114c_5 gromacs=2024.3=mpi_openmpi_cuda_he6b8466_1 nccl
```

- **安装过程需要联网下载, 请耐心等待.**
- 如果不幸出现报错, 多半是一些依赖库缺失(lib文件缺失, lib链接错误, lib找不到打不开等等), 将报错信息复制给ai工具, 描述你遇到的问题, 根据ai给出的建议, 安装上即可.
- GROMACS版本更新很快, 不一定非要用我示例的版本, 根据上面的原则选择合适的即可.

#### 检查安装

设置默认激活ms环境: `conda config --set default_env ms`, 至此你可以在终端运行 `gmx`命令查看GROMACS是否成功安装，输出如下：

```bash
$ gmx
                   :-) GROMACS - gmx, 2024.3-conda_forge (-:

Executable:   /home/username/miniconda3/envs/ms/bin.AVX2_256/gmx
Data prefix:  /home/username/miniconda3/envs/ms
Working dir:  /home/username
Command line:
  gmx

SYNOPSIS

gmx [-[no]h] [-[no]quiet] [-[no]version] [-[no]copyright] [-nice <int>]
    [-[no]backup]

OPTIONS

Other options:

 -[no]h                     (no)
           Print help and quit
 -[no]quiet                 (no)
           Do not print common startup info or quotes
 -[no]version               (no)
           Print extended version information and quit
 -[no]copyright             (no)
           Print copyright information on startup
 -nice   <int>              (19)
           Set the nicelevel (default depends on command)
 -[no]backup                (yes)
           Write backups if output files exist

Additional help is available on the following topics:
    commands    List of available commands
    selections  Selection syntax and usage
To access the help, use 'gmx help <topic>'.
For help on a command, use 'gmx help <command>'.

GROMACS reminds you: "...."
```

**恭喜你！成功安装了开源、高性能的分子模拟软件GROMACS !**

### 从源码编译

待更新……
