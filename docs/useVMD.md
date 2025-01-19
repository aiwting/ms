# VMD

- VMD（Visual Molecular Dynamics）是一款由美国伊利诺伊大学厄巴纳-香槟分校的理论和计算生物物理学小组开发的开源分子可视化软件。
- 可用于蛋白、核酸、脂质双层组装体等生物系统的建模、可视化和分析
- #免费 #开源  #windows #linux
- 下载：[VMD - Visual Molecular Dynamics](https://www.ks.uiuc.edu/Research/vmd/), 注册登录即可下载
- 教程：[VMD User's Guide](https://www.ks.uiuc.edu/Research/vmd/current/ug/)
- VMD支持使用CUDA的GPU加速，静电(即“volmap coulomb”和“volmap coulombmsm”)，隐式配体采样(即“volmap ils”)，径向分布函数的计算，混合拟合方法的拟合质量相互关联计算，轨迹聚类分析，以及分子轨道的计算和渲染
- 最新版本的VMD还采用了基于NVIDIA OptiX和CUDA的gpu加速批处理和交互式版本的Tachyon光线追踪引擎
- VMD有两种主要的操作模式，一种是典型的全功能图形启用模式，另一种是纯基于文本的操作模式。全功能图形启用模式需要一个支持OpenGL的图形加速器和最新的驱动程序。

## 基本显示

- 启动VMD会打开三个窗口
  - 显示窗口
  - 主窗口: 用于点击调整图像的显示
  - 命令行窗口: 执行一些指令操作(如果是Linux命令行窗口启动, 那么原来的Linux命令行窗口即为VMD的命令行窗口)
- 显示窗口有三种基本的鼠标模式:旋转、平移、缩放(rotation, translation, and scaling)，对应键盘上的r、t或s键
- 更改显示模式：`Graphic > Representation`
- 在Selected Atoms文本输入区, 可以通过原子名称、残留物名称或其他标识符来选择，且可以处理正则表达式，还可以根据坐标、原子质量等来选择.
  - `resname xxx` 表示选择xxx残基，名字对应pdb文件中的ATOM字段第四例
  - `atom C` 表示选择原子
  - `name C CA N O` 表示选择元素
  - `Creat Rep` 表示新建一个显示图层
  - 可以选择对不同的结构、不同的分子展示不同的模型

## 输出

- 输出图像/渲染：
`File > Render > Render the current scene using:`
  - Tachyon             选择渲染器
  - Start Rendering     按开始渲染按钮。经过一些处理后，应该在命令行中看到：Info) Rendering complete.
- 如果一切正常，您将在当前工作目录中得到一个名为plot.dat.tga(在MacOS X或Unix上)或plot.dat.bmp(在Windows上)的图像文件。
- 使用VMD的tachyon_WIN32程序将渲染得到的.dat文件转换为高清bmp图片：
`tachyon_WIN32.exe xxx.dat -aasamples 24 -mediumshade -trans_vmd -res 1920 1080 -format BMP -o xxx.bmp`

## 查看动画

- 轨迹动画：需要轨迹文件及其对应的结构文件
- 在主VMD窗口底部有动画控件

## 状态保存

- 可以保存当前工作为.vmd文件, 存当前调整调整的所有设置
`File > save visualisation state`
- 或者控制台中，键入命令`save state filename`，其中filename是要保存工作的文件的名称。后缀.vmd
- 打开工作：
`File > load visualisation state`
- 或者在命令行中，使用命令 `VMD -e filename` 启动VMD，其中filename是之前保存的文件的名称。
- 或者启动VMD后，从文本控制台中输入 `play filename`

## 拓展阅读

- [2023 10月Vmd 下载安装教程，Windows&Linux_vmd-CSDN博客](https://blog.csdn.net/weixin_46483785/article/details/134017419)
- [VMD(Visual Molecular Dynamics)教程（1） - 知乎](https://zhuanlan.zhihu.com/p/101565900)
- [入门教程 — VMD中文教程 2024.09 文档](http://vmd.chenzhaoqiang.com/intro/startManual.html)
- [pymol&vmd相关_vmd和pymol对比-CSDN博客](https://blog.csdn.net/yin1331102028yin/article/details/128827601)