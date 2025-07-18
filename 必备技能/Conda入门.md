# Conda入门

Conda是软件包、依赖项和环境管理工具，可在 Windows、macOS 和 Linux 上运行, 用于快速安装、运行和更新开源软件包及其依赖项。这里我们使用conda来建立虚拟环境, 安装和管理GROMACS等分子模拟软件.

<center style="color:#C0C0C0; text-decoration:underline"><img src="http://101.201.29.162:8899/i/2025/07/17/xil24m-0.png" width=99%><br/>图1. Conda包管理器</center>

Anaconda是Conda的开发者, 也是一个预先建立和配置好的软件包发行版, Anaconda Distribution 中包含了Conda 包管理器、Python 解释器，以及 1500+ 预安装科学计算包.
- Anaconda官方文档: [Anaconda Documentation - Anaconda](https://www.anaconda.com/docs/main)
- Conda中文文档: [Conda 文档 — conda 25.1.1 文档 - Conda 包管理器](https://docs.conda.org.cn/projects/conda/en/stable/index.html)

Miniconda 是一个 Anaconda 公司提供的轻量级发行版，包含 **Conda 包管理器**、**Python** 和一些必需的基础包, 安装教程参见[1.2安装GROMACS](../快速入门/安装GROMACS.md); 或者根据官方文档安装：
- 区别：Anaconda/Miniconda 都是打包好的Conda安装程序, 可以帮你一键安装 Python + Conda + 一些软件包, 不同之处在于Anaconda同时打包了1500+个常用的软件包, 可以一次性安装到你到python环境中; 而Miniconda时一个最小的python+conda安装程序, 只包含了最必要的包, 适合用户按需安装软件.
- [如何选择 Anaconda Distribution or Miniconda?](https://www.anaconda.com/docs/getting-started/getting-started)
- [安装 Miniconda - Anaconda](https://www.anaconda.com/docs/getting-started/miniconda/install)

注意事项：

- 安装后将conda环境启动时终端命令提示符中会出现“(base)”字样[Linux终端出现“(base)“linux (base)-CSDN博客](https://blog.csdn.net/wxuzero/article/details/127958224)。
- 如果希望在启动时不激活conda的base环境，通过在conda的配置文件`.condarc`中中将 `auto_activate_base` 参数设置为 false
    - 执行命令：`conda config --set auto_activate_base false`
- 想在启动Linux时默认进入某个环境，在 ~/.bashrc文件末尾加一行`conda activate your_envs`

## conda命令解释

1. `conda init --all`: 配置 Conda 环境, 自动修改系统的 shell 配置文件（如 `.bashrc`、`.zshrc`、PowerShell 配置等）, 使得每次打开终端时，Conda 会自动加载并准备就绪。
2. 环境配置
    - `conda config --add channels <channel_name>`: 配置软件源（Channel）, 指定 Conda 从哪些渠道搜索和下载包
    - `conda config --remove channels <channel_name>`: 移除软件源
    - `conda create -n <env_name>`：创建新环境
        - env_name是你自定义的环境名
    - `conda activate <env_name>`：激活环境
    - `conda deactivate <env_name>`：退出当前环境
    - `conda env list` 或者 `conda info --envs`：列出所有环境
    - `conda remove -n <env_name>`：删除环境
3. 包管理
    - `conda install <package_name>`：安装包
    - `conda update <package_name>`：更新包
    - `conda remove <package_name>`：卸载包
    - `conda list`：查看已安装的包
    - `conda clean`：清理缓存
4. 环境导出与克隆
    - `conda env export`：导出环境配置
    - `conda env create --file env.yml`：通过环境配置文件创建环境
    - `conda list --explicit`：导出所有包的精确安装列表
    - `conda clone`：克隆现有环境
5. 搜索和查看
    - `conda search`：搜索包
    - `conda info`：显示环境和配置的详细信息
