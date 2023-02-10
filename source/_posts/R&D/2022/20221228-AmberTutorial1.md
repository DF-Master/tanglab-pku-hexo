---
title: Tutorial:轻松上手Amber&AmberTools【1】
date: 2022-12-28
categories:
- rd
tags:
- rd
- tutorial
- amber
---



---

> 本文面向Tanglab及类似的希望针对生物体系做分子动力学模拟的科研人士
> 

> 本文在一定程度上参考了[**Amber Tutorials**](https://ambermd.org/tutorials/) ，本系列教程旨在提供一些作者的使用经验。若想更为全面地掌握Amber分子动力学模拟软件，应当阅读官方教程。
> 

> 本系列教程所需的预备知识（prerequisite）包括掌握Linux的基本命令，以及对分子动力学模拟理论的大致了解。
> 

---
<!--more-->
# 实验目的

起这个标题名字的时候，感觉就像是在写实验报告捏

本系列教程从第一篇起，开始真正地上手计算。在本节中，我们使用Amber自带的建模程序tleap构建一条短肽并溶剂化。通过一段时间的分子动力学模拟观察模拟过程中的轨迹，并对模拟结果做简要的分析。通过本节的模拟，笔者带大家Amber进行Amber的初体验，力图以实操的形式阐明分子动力学模拟的基本流程。

# 搭建模型

搭建模拟体系永远是分子动力学模拟中最为重要的步骤。

正如本系列教程第0章所说的，在模拟之肇始，Amber依靠两个文件，prmtop与inpcrd[1]，来识别一个分子。prmtop文件包含了原子名称，原子类型，残基名称，力场参数等信息，inpcrd包含了原子的所有坐标，以及模拟盒子的形状。

## 打开tleap

倘若已在系统中成功安装了AmberTools，那么直接在命令行窗口输入tleap[2]

```bash
$ tleap
```

那么你应当看到如下字样：

```
-I: Adding /smb/open-apps/amber22/amber22/dat/leap/prep to search path.
-I: Adding /smb/open-apps/amber22/amber22/dat/leap/lib to search path.
-I: Adding /smb/open-apps/amber22/amber22/dat/leap/parm to search path.
-I: Adding /smb/open-apps/amber22/amber22/dat/leap/cmd to search path.

Welcome to LEaP!
(no leaprc in search path)
```

且命令提示符由 **$** 变为 **>** ，这意味着tleap已被成功打开。path因Amber的安装地址而异。

## 加载力场

分子动力学模拟当然少不了力场。本节中我们要模拟一条肽链，因此选择使用专用于蛋白质的力场，ff系列。具体为ff19SB。关于力场本身，包括其加载方式，还有许多有趣的subtle。此处暂且按下不表。

```
> source leaprc.protein.ff19SB
```

你应当见到如下所示的输出：

```
----- Source: /smb/open-apps/amber22/amber22/dat/leap/cmd/leaprc.protein.ff19SB
----- Source of /smb/open-apps/amber22/amber22/dat/leap/cmd/leaprc.protein.ff19SB done
Log file: ./leap.log
Loading parameters: /smb/open-apps/amber22/amber22/dat/leap/parm/parm19.dat
Reading title:
PARM99 + frcmod.ff99SB + frcmod.parmbsc0 + OL3 for RNA + ff19SB
Loading parameters: /smb/open-apps/amber22/amber22/dat/leap/parm/frcmod.ff19SB
Reading force field modification type file (frcmod)
Reading title:
ff19SB AA-specific backbone CMAPs for protein 07/25/2019
Loading library: /smb/open-apps/amber22/amber22/dat/leap/lib/amino19.lib
Loading library: /smb/open-apps/amber22/amber22/dat/leap/lib/aminoct12.lib
Loading library: /smb/open-apps/amber22/amber22/dat/leap/lib/aminont12.lib
```

source即加载力场命令，leaprc.protein.ff19SB为力场名称。如果想查看source的详细用法，可以参考amber reference manual，也可以在tleap中执行。适用于其他命令。

```
> help source
```

---
>tips: tleap是大小写不敏感的（case-insensitive）。故“help source” 也可写成 “HELP source”，“help SOURCE”，“HelP SoURce”等形式。
---

## 创建多肽链

接下来我们使用tleap创建一条肽链。命名该段肽链为polyppd，并将其保存为pdb文件。

```
> polyppd = sequence { ACE ALA MET LYS LEU GLU NME }
> savePdb polyppd polyppd_prm.pdb
```

---
>tips： Amber中分子模型分为三级，自下而上分别是原子（atom）、残基（res/residue）、单元（unit）。在本例中polyppd即一个unit，其包含了七个res，每个res又含有若干个原子。
---

打开pdb文件，看一下我们的多肽链长啥样

![创建的多肽链](https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/polyppd_prm.png 'polypeptide')

**相关的文件会在每节的最后给出：**

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/polyppd_prm.pdb" target="_blank">**polyppd_prm.pdb**</a>

## 溶剂化

接下来我们给多肽添加一个厚度为10 $\AA$ 的立方体水溶剂盒子，使用TIP3P水模型。

```
> source leaprc.water.tip3p 
> solvatebox polyppd TIP3PBOX 10
  Solute vdw bounding box:              17.889 23.061 10.214
  Total bounding box for atom centers:  37.889 43.061 30.214
  Solvent unit box:                     18.774 18.774 18.774
  Total vdw box size:                   41.123 45.969 33.270 angstroms.
  Volume: 62893.049 A^3 
  Total mass 25940.282 amu,  Density 0.685 g/cc
  Added 1404 residues.
```

最后一行说明该溶剂化命令添加了1404个水分子。

## 保存文件

我们先检查一下搭建的模型有无参数缺失

```
> check polyppd
Checking 'polyppd'....

Warning: Close contact of 1.247 angstroms between nonbonded atoms HG2 and H
-------  .R<MET 3>.A<HG2 9> and .R<LYS 4>.A<H 2>

Warning: Close contact of 1.247 angstroms between nonbonded atoms HG2 and H
-------  .R<LYS 4>.A<HG2 9> and .R<LEU 5>.A<H 2>

Warning: Close contact of 1.247 angstroms between nonbonded atoms HG and H
-------  .R<LEU 5>.A<HG 9> and .R<GLU 6>.A<H 2>

Warning: Close contact of 1.248 angstroms between nonbonded atoms HG2 and H
-------  .R<GLU 6>.A<HG2 9> and .R<NME 7>.A<H 2>
Checking parameters for unit 'polyppd'.
Checking for bond parameters.
Checking for angle parameters.
check:  Warnings: 4
Unit is OK.
```
~~众所周知，warning是向来不用管的~~ $~$ Warning提示我们有两对原子距离太近了。这是很trivial的。tleap只是帮我们把残基连起来，它并不会考虑结构的合理性。后面做分子动力学模拟时我们会优化结构。

保存文件

```
> saveAmberParm polyppd prmtop inpcrd
Checking Unit.
Building topology.
Building atom parameters.
Building bond parameters.
Building angle parameters.
Building proper torsion parameters.
Building improper torsion parameters.
 total 13 improper torsions applied
Building H-Bond parameters.
Incorporating Non-Bonded adjustments.
Not Marking per-residue atom chain types.
Marking per-residue atom chain types.
  (Residues lacking connect0/connect1 - 
   these don't have chain types marked:

	res	total affected

	WAT	1404
  )
 (no restraints)

> quit
```

命令提示符变为熟悉的$符号，这代表我们回到了Linux界面。看一下我们现在得到了什么。

```bash
$ ls
inpcrd  leap.log  polyppd_prm.pdb  prmtop
```
leap.log记录了你在tleap中执行的操作。其余三个文件为我们在tleap中生成的。

**以下是得到的溶剂化后的Amber输入文件：**

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prmtop" target="_blank"> **prmtop** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/inpcrd" target="_blank">**inpcrd**</a> 

# 准备输入文件

建模部分顺利结束。接下来是正式的模拟。一般说来，一个完整的模拟流程包括最小化minimization、升温heating、成品production。根据模拟目的不同，单个流程中可以包含多段模拟。

## 极小化

我们需要准备参数文件，来告诉Amber我们希望做怎样的模拟。这里使用vim文本编辑器。

```bash
$ vim min.in
```
---
> tips: 强烈建议使用Linux端的文本编辑器。windows端的文本编辑软件（尤其是word）默认使用的换行符，及其他相关的格式有可能导致程序报错。如使用windows端文本编辑软件，推荐使用notepad++或其他文本编辑器。

>但是如果没有vim使用经验的话，请先查阅相关教程，掌握基本的vim命令。
---

写入以下内容

```
Minimize        ! title line. type anything you want
 &cntrl         ! this line's called namelist
  imin=1,       
  ntx=1,        
  irest=0,      
  maxcyc=2000,  
  ntmin=1,      
  ncyc=1000,    
  ntpr=100,     
  ntwx=0,       
  cut=8.0,      
 /              ! signal for end of namelist
 ```

---
> tips: Amebr是基于Fortran语言编写的，在输入文件中，半角叹号 **!** 引导注释内容。其不会被软件读取。
---

在参数输入文件中，第一行是标题，尽情发挥。模拟参数由namelist引导。在学习初期，我们只涉及 &cntrl 这一个namelist。每个namelist由 / 作为结束。行首空格只是排版好看。可顶格写，也可一行写入多个参数。

+ imin=1 $~~~~$ 单点能计算
+ ntx=1 $~~~~$ 从输入文件中读取坐标信息。不读取速度信息。
+ irest=0 $~~~~$ 不要重启模拟 (重启模拟不适用极小化)
+ maxcyc=2000 $~~~~$ 做2000步的极小化模拟
+ ntmin=1 $~~~~$ 在一开始的ncyc步使用最速下降法，然后转换到共轭梯度法
+ ncyc=1000 $~~~~$ 使用最速下降法做1000步的极小化
+ ntpr=100 $~~~~$ 每100步输出一次能量信息到输出文件
+ ntwx=0 $~~~~$ 不输出坐标信息
+ cut=8.0 $~~~~$ 非键相互作用的截断距离

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/min.in" target="_blank"> **min.in** </a>

## 升温

极小化结束后体系还是0K的。贸然做成品MD，有可能导致体系的崩溃。故我们还需要升温。

将以下内容写入heat.in

```
Heat
 &cntrl
  imin=0,
  ntx=1,
  irest=0,
  nstlim=10000,
  dt=0.002,
  ntf=2,
  ntc=2,
  tempi=0.0,
  temp0=300.0,
  ntpr=100,
  ntwx=100,
  cut=8.0,
  ntb=1,
  ntp=0,
  ntt=3,
  gamma_ln=2.0,
  nmropt=1,
  ig=-1,
 /
&wt type='TEMP0', istep1=0, istep2=9000, value1=0.0, value2=300.0 /
&wt type='TEMP0', istep1=9001, istep2=10000, value1=300.0, value2=300.0 /
&wt type='END' /
```

+ imin=0 $~~~~$ 运行分子动力学模拟，而不是最小化
+ nstlim=10000 $~~~~$ MD运行步数 (nstlim * dt = 模拟时长（ps）)
+ dt=0.002 $~~~~$ 每步的演化时长，亦即相邻两步的时间间隔。单位为皮秒，ps
+ ntf=2 $~~~~$ 不计算SHAKE算法所约束的化学键的受力
+ ntc=2 $~~~~$ 使用SHAKE约束所有含氢原子的化学键
+ tempi=0.0 $~~~~$ 初始的恒温器温度。单位：K
+ temp0=300.0 $~~~~$ 最终的恒温器温度。单位：K
+ ntwx=1000 $~~~~$ 每1000步将体系的坐标信息输出到Amber轨迹文件mdcrd
+ ntb=1 $~~~~$ 固定模拟盒子体积的周期性边界条件
+ ntp=0 $~~~~$ 不做压强约束
+ ntt=3 $~~~~$ 使用郎之万（Langevin）恒温器做温度控制
+ gamma_ln=2.0 $~~~~$ 郎之万恒温器碰撞频率
+ nmropt=1 $~~~~$ 使用核磁限制信息与读取重量变化
+ ig=-1 $~~~~$ 郎之万恒温器中产生随机数的随机种子。除非调试程序，强烈建议设置ig=-1

heat.in最后三行代表体系将用9000步从0K升温至300K，并在从9001步到10000步这一段维持300K的温度。

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/heat.in" target="_blank"> **heat.in** </a>

## 成品

做最终的成品MD，我们选用NPT系综来模拟真实体系。对于不同的模拟目的，应谨慎选取所使用的系综。
将以下内容写入prod.in

```
Production
 &cntrl
  imin=0,
  ntx=5,
  irest=1,
  nstlim=100000,
  dt=0.002,
  ntf=2,
  ntc=2,
  temp0=300.0,
  ntpr=500,
  ntwx=500,
  ntwv=500,
  cut=8.0,
  ntb=2,
  ntp=1,
  ntt=3,
  gamma_ln=2.0,
  ig=-1,
 /
 ```

+ ntx=5 $~~~~$ 从rst7坐标输入文件中读取坐标与速度信息。
+ irest=1 $~~~~$ 重启先前的MD模拟。这意味着rst7坐标输入文件中应当有速度信息，并被程序读取作为原子的初始速度 
+ temp0=300.0 $~~~~$ 恒温器初始温度。
+ ntwv=500 $~~~~$ 每500步将速度信息输出到Amber速度输出文件mdvel
+ ntb=2 $~~~~$ 使用恒压周期性边界条件
+ ntp=1 $~~~~$ 使用贝伦德森（Berendsen）恒压器调节体系压强

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.in" target="_blank"> **prod.in** </a>

# 模拟

贵组服务器资源充足，可爽用GPU/CPU。

## sander

以CPU单核运算为例。通过以下命令调用sander进行模拟计算

```bash
$ sander -O -i min.in -o min.out -p prmtop -c inpcrd -r min.rst
$ sander -O -i heat.in -o heat.out -p prmtop -c min.rst -r heat.rst
$ sander -O -i prod.in -o prod.out -p prmtop -c heat.rst -r prod.rst -x prod.crd -v prod.vel -inf prod.info
```

对上命令的各参数做出说明

+ -O $~~~~$ Overwrite the output files if they already exist
+ -i min.in $~~~~$ Choose input file min.in (default mdin)
+ -o min.out $~~~~$ Write output file min.out (default mdout)
+ -p prmtop $~~~~$ Choose input parameter and topology file prmtop
+ -c inpcrd $~~~~$ Choose input coordinate file inpcrd
+ -r min.rst $~~~~$ Write output restart file with coordinates and velocities (default restrt)
+ -inf 0min.info $~~~~$ Write MD info file with simulation status (default mdinfo)
+ -x prod.crd $~~~~$ Write output trajectory file prod.crd (default mdcrd)
+ -v prod.vel $~~~~$ Write output velocity file prod.vel (default mdvel)

简而言之，一个模拟（无论是最小化，升温还是成品）需要三个输入文件，分别是MD参数文件mdin、分子参数与拓扑文件prmtop、分子坐标文件inpcrd/restrt。而其输出包括输出文件mdout、重启文件restrt[3]、轨迹坐标文件mdcrd、轨迹速度文件mdvel。

在运行sander（pmemd同理）的命令中，若不指定输入文件名，则程序会根据默认名称搜索当前目录下有无对应文件。若不指定输出文件名，则程序以默认文件名进行输出。

restrt、mdcrd、mdvel文件默认是二进制的。

# sander.MPI

做多核并行运算则使用sander.MPI程序。例如使用96个CPU做并行运算：

```bash
$ mpirun -np 96 sander.MPI -O -i min.in -o min.out -p prmtop -c inpcrd -r min.rst
$ mpirun -np 96 sander.MPI -O -i heat.in -o heat.out -p prmtop -c min.rst -r heat.rst
$ mpirun -np 96 sander.MPI -O -i prod.in -o prod.out -p prmtop -c heat.rst -r prod.rst -x prod.crd -v prod.vel -inf prod.info
```
---
> tips: 若做多核运算，使得模拟计算并行效率最大的做法是为模拟任务分配的CPU数目为2的n次幂个，如2，4，8，16，32，等。当然在计算资源充足的情况下总是核数越多，计算越快。例如总的而言96核总是快于64核。此外有**极个别**体系[4]，倘若不分配 $2^n$ 个CPU，将导致程序报错。此规则在模拟时不需要刻意遵守，遇到所谓的“极个别”体系时另行修正即可。
---

# pmemd.CUDA

使用pmemd.cuda程序做GPU加速运算。

```bash
$ nvidia-smi  # find out if any GPU available
$ export CUDA_VISIBLE_DEVICES=0 # declare which GPU to use
$ pmemd.cuda -O -i min.in -o min.out -p prmtop -c inpcrd -ref inpcrd -r min.rst
$ pmemd.cuda -O -i heat.in -o heat.out -p prmtop -c min.rst -ref min.rst -r heat.rst
$ pmemd.cuda -O -i prod.in -o prod.out -p prmtop -c heat.rst -ref heat.rst -r prod.rst -x prod.crd -v prod.vel -inf prod.info
```

不愧是GPU，一颗4090足以薄纱96个CPU。

然而遗憾的是有很多体系不支持GPU加速。在这类体系的mdout报错信息中会有相应提示。

让我们看看得到了哪些输出文件（此处没有列出全部输出文件）吧！

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/min.out" target="_blank"> **min.out** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/min.rst" target="_blank"> **min.rst** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/heat.out" target="_blank"> **heat.out** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/heat.rst" target="_blank"> **heat.rst** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.out" target="_blank"> **prod.out** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.rst" target="_blank"> **prod.rst** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.crd" target="_blank"> **prod.crd** </a> 



# 数据处理

## 关于mdout文件

以prod.out文件的片段为例，说明输出文件中含有哪些信息。

```
NSTEP =    16500   TIME(PS) =      53.000  TEMP(K) =   299.55  PRESS =    -1.6
 Etot   =    -11003.3768  EKtot   =      2576.9051  EPtot      =    -13580.2819
 BOND   =        14.9609  ANGLE   =        44.6457  DIHED      =        24.1955
 1-4 NB =        18.5009  1-4 EEL =       343.7709  VDWAALS    =      1978.2249
 EELEC  =    -16005.1318  EHBOND  =         0.0000  RESTRAINT  =         0.0000
 EKCMT  =      1269.5247  VIRIAL  =      1270.9801  VOLUME     =     43385.0145
 CMAP   =         0.5512
                                                    Density    =         0.9929
 Ewald error estimate:   0.2975E-03
```

目前先介绍几个基本信息
+ NSTEP $~~~~$ 步数，即体系在当前过程中运算到了哪一步。
+ TIME $~~~~$ 模拟所经历的时间。注意倘若输入文件中irest=1，则该时间将从上一段模拟中开始累加。
+ TEMP $~~~~$  温度。
+ PRESS $~~~~$  压强。注意其输出的是相对波动值，单位0.000001 bar[5]。
+ Etot、EKtot、EPtot $~~~~$ 体系的总能量、动能、势能。单位kcal/mol。
+ Density $~~~~$ 密度。单位$g/cm^3$。

此外在mdout文件（prod.out）末尾也会包含体系性质的平均值与涨落。

```
      A V E R A G E S   O V E R  100000 S T E P S


 NSTEP =   100000   TIME(PS) =     220.000  TEMP(K) =   299.57  PRESS =   -40.9
 Etot   =    -10942.2359  EKtot   =      2577.0728  EPtot      =    -13519.3087
 BOND   =        15.6765  ANGLE   =        46.1061  DIHED      =        24.0492
 1-4 NB =        16.5357  1-4 EEL =       337.6756  VDWAALS    =      1955.1475
 EELEC  =    -15918.2853  EHBOND  =         0.0000  RESTRAINT  =         0.0000
 EKCMT  =      1258.5262  VIRIAL  =      1304.9923  VOLUME     =     44752.7114
 CMAP   =         3.7860
                                                    Density    =         0.9681
 Ewald error estimate:   0.1451E-03
 ------------------------------------------------------------------------------


      R M S  F L U C T U A T I O N S


 NSTEP =   100000   TIME(PS) =     220.000  TEMP(K) =     4.38  PRESS =   275.0
 Etot   =       136.4000  EKtot   =        37.6385  EPtot      =       131.5170
 BOND   =         3.3531  ANGLE   =         5.5782  DIHED      =         2.4814
 1-4 NB =         1.7208  1-4 EEL =         5.2796  VDWAALS    =        51.5011
 EELEC  =       150.5856  EHBOND  =         0.0000  RESTRAINT  =         0.0000
 EKCMT  =        26.4471  VIRIAL  =       274.1954  VOLUME     =      3800.4649
 CMAP   =         1.8078
                                                    Density    =         0.0653
 Ewald error estimate:   0.1102E-03
 ------------------------------------------------------------------------------
```

## 数据评估

使用amber自带的分析脚本简单看一下成品MD跑得怎么样。

```bash
$ mkdir analysis
$ cd analysis
$ process_mdout.perl ../prod.out
```

每个性质都输出了summary_avg.xxx, summary.xxx, summary_rms.xxx，代表了对应性质的平均值、列表汇总、标准差。既然成品MD是在NPT系综下进行的，我们关注体系的体积、能量变化曲线，以判断体系是否达到平衡。

![模拟盒子体积曲线](https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/VOLUME.png 'VOLUME')

对体积而言，其在21ps[6]至约50ps不断下降，此后保持稳定。这是由于系统自动的溶剂化时添加的水分子往往排列太过“稀疏”，使得模拟盒子体积过大。那么在MD模拟时就会有一个收缩的过程。倘若我们需要获知体系的平衡性质，那么显然应当只取50ps之后的轨迹。

![体系动能曲线](https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/EKTOT.png 'EKTOT') | ![体系势能曲线](https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/EPTOT.png 'EPTOT') | ![体系总能量曲线](https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/ETOT.png 'ETOT') 
---|---|---

对于动能曲线，可见其变化大致在200kcal/mol的范围内波动，即恒温器较好地控制了体系的温度。而势能曲线与总能量曲线在约30ps处出现了一个明显的能量下降过程，对应了模拟盒子的收缩。而在50ps后，势能曲线与总能量曲线没有再发生大的变化，也是在大致在200kcal/mol的范围内波动，由此可以认为体系最终确实到了比较平衡、稳定的状态。当然可能有人认为这很难讲，尤其是看总能量变化的时候，波动似乎还是挺大的。固然200ps的模拟时长确实略显不足。不过这只是一个演示实验，大体已经可以说明原理了。说白了，只是从性质变化曲线上判断体系平衡与否，并没有什么很统一的标准。你能说服自己即可。

## 结果的可视化

VMD软件是一款非常强大的可视化软件，支持多种文件格式，可用于分子动力学模拟轨迹的可视化及其他一些后处理工作。遗憾的是，尽管VMD支持Windows与Linux两个系统，但Windows版本暂时无法识别amber输出文件。一个较为曲折的解决方案是安装WSL（windows subsystem for Linux），然后安装子系统的虚拟桌面，远程连接虚拟桌面后使用VMD。好在贵组的新服务器是有图形化界面的，并且也已部署好VMD，也就省去了诸多麻烦。

关于如何使用VMD，请参考 [**Using VMD with AMBER**](http://ambermd.org/tutorials/basic/tutorial2/index.php) 。这篇教程写得非常详尽，笔者不再班门弄斧。这里直接列出模拟结果，不再说明VMD如何操作。

值得一提的是在VMD中，prmtop格式选择Amber 7 Parm，rst文件（包括mdrst、mdcrd）格式为NetCDF，rst7文件对应格式Amber Restart。

![成品MD轨迹](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202212/AT1/traj0.gif?raw=true 'traj')

使用VMD制作动图的步骤在上面列出的参考教程中亦有提及，此处不再赘述。这里直观展示一下得到的模拟轨迹。可以看到模拟初期确实有盒子（蓝色边框）缩小的过程。

![多肽链最终构象](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202212/AT1/polyppd_final.png?raw=trueg 'polyppd_final')

成品MD轨迹最后一帧（Frame）对应的分子构象。与刚建模时的构象相比，变化还挺大的。我们这里只是目测一下，不做进一步的定量讨论。顺带一提将rst文件转换为pdb文件的方法：

```
$ cpptraj -p prmtop -y prod.rst -x prod.rst7
$ ambpdb -p prmtop -c prod.rst7 > prod.pdb
```
这样的话得到的最终构象也可以通过GaussianView等软件打开了。

<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.rst7" target="_blank"> **prod.rst7** </a> $~~~~$
<a href="https://raw.githubusercontent.com/DF-Master/tanglabpicbed/main/2022/202212/AT1/prod.pdb" target="_blank"> **prod.pdb** </a> 

# 总结

简而言之本教程就图一乐，展示了MD模拟的基本流程。最终我们得到了一段轨迹，以及多肽在溶液中的稳定构象。此外还通过分析体系的体积、能量变化曲线，判断其是否达到平衡状态。由于没有明确的探究问题，因此最终也没有什么robust的结论。希望读者能够掌握如何使用tleap建模，通过调节参数控制模拟条件，并最终跑一段MD轨迹。

# 注

[1] 这两个默认文件名的含义：prmtop 即 parameter & topology， inpcrd 即 input coordinate。也可自定义文件名。

[2] 本系列教程约定：除非额外说明，命令提示符为 **$** 时，代表命令直接在Linux终端中输入。命令提示符为 **>** 时，代表其为在打开的软件中执行的命令。没有命令提示符则代表为命令的输出内容或文件内容（总之不是命令就对啦）。

[3] 重启文件restrt实时记录、更新模拟过程中轨迹的最后一帧(frame)的空间坐标。其有两个作用。第一是模拟因为意外中断时，可以以restrt文件作为坐标输入文件恢复模拟，从而节省时间；第二是上一步模拟的重启文件是下一步模拟的坐标输入文件。

[4] 在笔者近一年的使用经历中，只遇到过一个这种体系，即对非键相互作用做出修正的12-6-4模型。

[5] 存疑。待考据。

[6] 回顾成品MD的模拟中，我们设置了参数irest=1，即重启MD，因此计时也是承接升温过程的。升温模拟时长20ps，因此成品MD轨迹对应的时间段为21ps-220ps。