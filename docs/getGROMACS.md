<!-- autoHeader:0 -->

> 目的：安装开源、高速、灵活的MD模拟软件GROMACS

# 引言

GROMACS的版本和平台：

- GROMACS更新很快，有多个版本：
  - 支持CUDA加速的版本：需要nVidia独立显卡
  - 支持并行计算的版：一般用于含多个处理器的计算服务器
  - 单/双精度版本：一般计算单精度版本就够了
- GROMACS是开源的，理论上可以编译到任何你想要的平台使用：Windows、Linux、Mac, 但最常用的是Linux平台。

推荐：

- 只用来学习和熟悉GROMACS基本操作，或者没有显卡，同时是Windows系统，推荐完成[&#34;在Windows系统安装&#34;](#在windows系统安装)即可。
- 有显卡的情况下，或者想要执行一个模拟全流程，推荐完成[“安装conda预编译版本”](#通过conda安装)。

# 在Windows系统安装

- GROMACS一般都是在Linux下运行，效率也更高，优先选择Linux系统
- 如果实在不想安装Linux系统，有以下解决方案：
- 从源码编译原生的Windows版gromacs：此方法过于复杂，别试！如果想找罪受就试逝。
- 这里提供两个由[sobereva](http://sobereva.com/458)编译的版本：可直接在Windows命令行使用，选择对应的[AVX指令集]，[添加PATH环境变量]即可。

> [2020.3 CUDA GPU加速版，AVX指令集（93 MB）](http://sobereva.com/soft/gmx/gmx2020.3_AVX_CUDA_win64.rar)
> [2020.6 CUDA GPU加速版，AVX2指令集（88 MB）](http://sobereva.com/soft/gmx/gmx2020.6_AVX2_CUDA_win64.rar)
> CUDA GPU加速版并不需要在机子里安装CUDA toolkit，只要确保nVidia显卡驱动足够新就可以直接使用。核显也能用。

# 在Linux系统安装

首先需要[安装一个Linux环境](getLinux.md)，再进行以下的步骤。大抵来说有两种解决方案：

- 某些发行商预编译的版本
  - 一些Linux发行版的包管理工具就能安装GROMACS, 但一般版本较老.
  - 经过我的摸索发现 Conda-forge 社区编译的GROMACS版本最全最新。因此通过 [Conda](https://docs.anaconda.com/miniconda/) 包管理工具，直接搭建一个用于分子模拟虚拟环境是个不错的选择。
  - 此外，这个环境可以不仅仅包含GROMACS，还能安装其它模拟过程中需要用到的工具，因此非常推荐使用该方式。省去了编译中易报错的缺点，同时可以一步到位安装其它必要的工具。**强烈推荐新手选择此安装方式**
- 从源码编译安装（理论上效率最高, 也最复杂, 但比在Windows上编译容易些），有能力的可以尝试。

## 通过conda安装

### 安装conda环境

**以下命令均在 Linux shell 中执行**

打开Linux命令行，输入以下命令：

```bash
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

### 安装分子模拟程序包

1. 运行命令 `conda search gromacs`可以产看有哪些 GROMACS 版本，输出如下：

|         | gromacs版本                                  |
| ------- | -------------------------------------------- |
| gromacs | 2024.3 nompi_cuda_h5cb645a_0  conda-forge    |
| gromacs | 2024.3 nompi_cuda_h5cb645a_1  conda-forge    |
| gromacs | 2024.3 nompi_dblprec_h76c4001_0  conda-forge |
| gromacs | 2024.3 nompi_dblprec_h76c4001_1  conda-forge |
| gromacs | 2024.3 nompi_h5f56185_100  conda-forge       |
| gromacs | 2024.3 nompi_h5f56185_101  conda-forge       |

- 我们需要创建一个名为 ms 的环境，并安装对应的GROMACS版本：个人电脑安装nompi版，有nvidia显卡安装cuda版，没有dblprec表示单精度版（足够）。

```bash
conda create -n ms python=3.9 -y
conda activate ms
conda install gromacs=2024.3=nompi_cuda_h5cb645a_1
```

2. 继续安装一些其它的分子模拟相关软件（可选）：

```bash
conda install gmx_mmpbsa=1.6.3 -y
conda install apbs=1.5
conda install pdb2pqr=3.6.1
conda install pymol-open-source
conda install vmd
conda install vermouth
conda install dssp
conda install acpype
```

- 如果你觉得上述过程还是麻烦，可以使用这里写好的文件：[ms_env.yml](https://aiwting.github.io/ms/files/ms_env.yml)，下载到工作目录, 然后运行下面的命令安装，可以代替上面 1、2 两个步骤: `conda env create --file env.yml`

3. 设置默认激活ms环境: `conda config --set default_env ms`, 至此你可以在终端运行 `gmx`命令查看GROMACS是否成功安装，输出如下：

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

**恭喜你！成功安装了开源、高性能的GROMACS !**

## 从源码编译

待更新……
