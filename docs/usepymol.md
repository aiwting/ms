# PyMol

- PyMol是一款功能强大且广泛使用的分子可视化软件，主要用于创建高质量的生物大分子（如蛋白质）的三维结构图像。

## 版本

- Pymol现在由Schrödinger开发、支持和管理
- 分为企业版、政府和学术版、教育版，还有开源版。除了开源版和教育版外，其他都是需要付费购买的。
- #开源 #跨平台
- 官网：[PyMOL | pymol.org](https://www.pymol.org/)

## 使用技巧

- 显示氢键：
`Action -> preset ->technical`
- 显示静电势：
`Action -> generate -> vaccum electrostatics -> Protein contact protential (local)`
- 测量两个原子之间的距离，一般来讲氢键要求的距离要短于3.5埃
`wizard->measurement` 
- 显示序列窗口，可以快速的选择氨基酸残基、水分子、配体分子、辅因子等`Display -> Sequence`
- 结构叠合：同时导入多个结构，点击叠合目标对象后的“A -> align -> all to this（*/CA）”即可基于序列进行结构叠合
- 对象: 右窗口的一行结构表示一个对象(object).
- 插件: 菜单"Plugin"可以安装丰富的插件, 以绘制复杂的图形或额外的功能
- 移动标签: `edit`进入编辑模式然后选择 `Mouse >"3 Button Editing"` (或点击右下角mode进入) >按住ctrl再用左键拖动标签.
- 导出图片: 右上角 >Draw/Ray

PyMol中一些操作的命令
```pymol
//设置C原子颜色
color gray, objectname & name *C
//设置α螺旋颜色
color magenta, ss h
//将多个对象合成新的组
group groupname, object1 object2 object3 ...
//设置某个对象stick模型, 卡通模型的透明度
set stick_transparency, 0.9, objectname
set cartoon_transparency, 0.9, protein,
//设置球模型大小, 透明度
set sphere_scale, 0.3, obj01
set sphere_transparency, 0.4, obj01
//设置线条
set dash_gap, 0.2
set dash_width, 5
set line_width, 5
//计算质心位置
centerofmass sele
//center_of_mass 插件, 计算选择原子的质心位置
com sele,object=objectname

rotate axis,angle,object-name,state     //旋转
translate vector,object-name,state      //移动
// 设置显示小数后2位
set label_distance_digits, 2

creat complex, Aname Bname   //合并两个对象A B，比如合并配体与蛋白为一个对象，方便保存为复合物结构。
align Aname,Bname    //可以计算两个对象间的RMSD
```

## 参考阅读

- [使用PyMOL绘制蛋白与配体分子结合模式图_pymol如何显示结合位点-CSDN博客](https://blog.csdn.net/qq_37126941/article/details/115793062)
- [pymol&vmd相关_vmd和pymol对比-CSDN博客](https://blog.csdn.net/yin1331102028yin/article/details/128827601)
