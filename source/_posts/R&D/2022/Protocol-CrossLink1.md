---
title: Protocol:化学交联质谱实验方案
date: 2022-11-28
tags:
- rd
- tutorial
categories:
- rd
---

## 0 前言  

化学交联质谱是一种获取蛋白质结构，分析蛋白质相互作用的有力的工具。**化学交联质谱分析流程如下**：在蛋白单体或复合物溶液中加入适量交联剂，交联剂为线性结构，两端的反应基团由间隔臂所连接起来，反应基团可与氨基酸残基侧链的官能团进行反应，若两个可反应的氨基酸侧链其空间距离小于或等于交联剂间隔臂臂长，则可以两个位点可被交联剂共价连接起来；使用蛋白酶将样品蛋白酶切为肽段，进入液质联用系统获得数据和数据处理，得到大量成对的交联位点。
**该技术目前已经成功应用于蛋白质结构信息获取，蛋白质相互作用鉴定。具有样品需求量少、对蛋白质所处环境要求低、不受分子量限制、可进行原位交联、操作简便、获取信息量大等优点**。本文将对化学交联质谱的实验操作流程及后续数据分析进行简介

<!--more-->

![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/1.png?raw=true)
 

## 1 选择合适的交联剂与缓冲溶液 

### 交联剂 

化学交联质谱技术的核心是交联剂，交联剂可分为以下几个模块，其中红色的是必要的结构，灰色是可选结构。**交联剂主体** 是由间隔臂连接两个**反应基团**，**反应基团**有多种类型，可以和蛋白质不同氨基酸侧链进行反应。 交联剂**间隔臂**由碳链或聚乙二醇链构成，**间隔臂碳上的H可进行氘代**，从而在质谱中根据同位素进行定量或增强识别。有些交联剂间隔臂上会被改造成在质谱中**可断裂**的C-N或C-S键，这样搜索交联肽段的问题就简化成搜索修饰肽段，降低搜库难度。交联剂的**富集基团**和**释放基团**可纯化交联肽段，提高交联肽丰度。 

![交联剂结构](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/2.png?raw=true)

**我们需要根据蛋白质的序列和氨基酸分布去选择合适的交联剂，使空间距离较近或存在相互作用的两个区域有较多可以参与反应的氨基酸。交联剂间隔臂臂长也是要考虑的因素之一，若想要捕获精度更高的结构信息，可选用臂长较短的交联剂；想要捕获更多的相互作用信息，可采用臂长较长的交联剂**。
以下为笔者使用过的交联剂，其中效果较好的有BS<sup>3</sup> DSS DOPA2 Sulfo-SDA DMTMM，PDH交联效果较差，搜索到的交联对数目较少；EDC在酸性条件下交联效果好，而中性条件交联效果较差。

![实验室现有交联剂](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/3.png?raw=true)

### 缓冲条件

**缓冲液需要有适当的pH缓冲能力，且不能影响交联剂与蛋白质发生反应**。以BS<sup>3</sup>交联剂缓冲条件为例：溶液中不能含氨基（如Tris）或铵根离子。交联反应体系中不能加DTT，尽可能少的去垢剂（0.5% Triton X-100或更少），10 mM MgCl<sub>2</sub>和1 M NaCl（不能更高）以及20%甘油是可以的，但是不能百分百保证对交联反应没影响。笔者推荐使用20mM HEPES， 150mM NaCl（如果需要加盐的话），pH 7.4  该条件可与大多数交联剂反应条件兼容。 

## 2 选择合适的蛋白质交联剂比例
该步骤是为了防止交联剂过多，导致蛋白质结构发生变化；或者蛋白质发生交联反应后，可以参与后续酶切的位点数目过少而导致质谱上无信号。
蛋白质单体交联浓度建议为**0.5-1.0mg/mL**，当然也不是绝对的，其他浓度也可以进行交联反应。蛋白质复合物交联时浓度建议为**10倍KD，若KD未知或浓度较难达到，则用1.0mg/mL**，建议测试如下蛋白质：交联剂的比率，8:1，4:1 ，2:1，1:1 到 1:2（w / w）。 交联后，单个亚基的强度在SDS-PAGE上降低50-80％（为避免过度交联，不超过90％），如果是复合物，需要有较亮的高分子量的条带。**若复合物交联未产生较量的条带，也可能检测到复合物的交联对，但最好有明显的复合物条带**。 
**选择交联剂浓度需要预先进行交联实验，也就是3中的①-③步，完成后可将反应后的样品一部分进行SDS-PAGE，另一部分可将比例适宜的样品进行3中后续的处理步骤，制备到可以送样的程度**。

## 3 交联实验步骤
1-6为制备蛋白固样的操作流程，制备至此即可送样。而1-3完成后，可以选择进行SDS-PAGE来看蛋白质交联剂比例是否合适，或直接给平台送PAGE胶样品进行鉴定。**但PAGE胶样品的效果不如蛋白固样**

以BS<sup>3</sup>和DSS为例
1. 使用DMSO溶解适量交联剂至适当浓度使用适宜的buffer将蛋白稀释或浓缩至适当浓度 
2. 加入适量交联剂，室温反应1h
3. 加入2μL 1M pH 7.5 的Tris溶液，使溶液中Tris浓度为20-50mM，终止反应 ，室温静置15min
4. 加入6倍体积的-20度下预冷的丙酮来沉淀蛋白。混匀，于-20℃下放置3h以上。
5. 在离心机里以14,000rpm离心20 min。吸出大部分上清液，留下最后一滴，以免吸到沉淀。如果样品中有去垢剂则再用500μL -20℃预冷的丙酮清洗两次。
6. 打开管盖，风干样品 

## <font color=Blue>  4 制备质谱样品 </font>
若平台不支持蛋白固样，则可按该步骤处理为无盐肽段样品再进行送样，通常用不到
1. 溶解蛋白于20μL 8M urea，100mM Tris pH8.5。超声15min-20min辅助蛋白溶解。（先配制0.5M Tris pH8.5，然后称取240mg urea、100μL 0.5M Tris pH8.5和220μL ddH2O配制成500μL 的8M urea）
2. 还原蛋白二硫键：加入1μL的100mM TCEP，终浓度为5mM,于室温反应20min。
3. 封闭二硫键：加入0.4μL的500mM IAA，终浓度为10mM,于室温反应15min。
4.  加入3倍体积的100mM Tris pH8.5，将urea稀释到2M。
5.  抑制糜蛋白酶活性：加入0.8μL 100mM CaCl2，终浓度为1mM.
6.  减少urea对肽段N端的氨甲酰化修饰：加入0.8μL 的2M 甲胺，终浓度为20mM。
7.  加入胰蛋白酶：酶和蛋白的质量比为1：20-1：100，37度避光酶切12-16小时。（胰蛋白酶购自promega, Catalog number selected: V5280）
8.  终止酶切反应：加入4μL 99%的甲酸终止酶切反应，终浓度为5%。
9.  使用脱盐Tip对样品进行除盐，最后抽干溶剂。（该步骤需要结合Tip的使用说明书进行）

## 5 pLink2的使用
### pLink2的下载与安装
#### pLink2是一款由中科院计算所贺思敏课题组开发的交联肽搜库软件，安装pLink2需要如下依赖软件
- Windows 7/8/10，64位
- .NET Framework 4.5 <https://www.microsoft.com/en-US/download/details.aspx?id=30653>
- Java 8，64位  <https://www.java.com/zh_CN/>
- MSFileReader ,≤3.1 SP2，64位  <https://github.com/pFindStudio/pLink2/wiki/FAQ>

#### pLink2下载地址与安装要求

- 下载地址 
  - Site1: <http://pfind.ict.ac.cn/software/pLink/index.html> 
  - Site2: <https:/lgithub.com/pFindStudio/pLink2> 

- 默认路径安装 
  - C:pFindStudiolpLinkl2.3.9
  - 文件夹需要有写入权限
  - 勿安装到C:\Program Files、C:\Program Files (x86)
  - 安装路径不能有中文、空格等特殊字符 

- 打开pLink2填写申请表单后激活使用即可

### 使用pLink2进行搜库分析数据
#### 主要使用默认参数即可
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/4.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/5.png?raw=true)

**<font color=Red>由于除raw之外的其他格式存在转换问题，可能导致pLink2无法识别，建议均采用.raw后缀文件导入**  </font>

![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/6.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/7.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/8.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/9.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/10.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/11.png?raw=true)
**<font color=Red>一般无需定量方法，选择Labeling-None即可**  </font>
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/12.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/13.png?raw=true)
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/14.png?raw=true)

**pLink2会以如下网页报表形式呈现，分为三个层次和四种肽段，四种肽段如图所示，其中Inter-link也被称为Cross-link是交联获取的最有价值的信息。**
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/15.png?raw=true)

![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/16.png?raw=true)


**网页报表中的各项内容均以CSV格式存储在任务文件夹下的reports文件夹下，但内容和格式较为复杂**
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/17.png?raw=true)

**使用脚本plink2_report_v5.py可将数据处理为图中简洁的格式，将脚本文件复制到reports文件夹下，再将脚本中的work_path修改为reports文件夹的绝对路径，保存后运行即可得到以v5.csv为结尾的文件**

## 6 交联数据可视化
### 使用xinet可视化

**xinet网站：<https://crosslinkviewer.org/index.php> 点击左侧的upload，将后缀为v5_xinet.csv的文件和fasta文件上传即可。v5_xinet.csv文件可由XINET.py生成，将XINET.py复制到reports文件夹下，首先按前述步骤使用plink2_report_v5.py生成v5.csv文件，再将XINET.py脚本中的work_path修改为reports文件夹的绝对路径，保存后运行即可得到以v5_xinet.csv为结尾的文件**
![](https://github.com/DF-Master/tanglabpicbed/blob/main/2022/202211/18.png?raw=true)
### 使用pymol可视化

**使用v5_xinet.csv文件，可利用excel或python生成pymol中测量距离的命令，复制到命令行内即可,例如**
`dist /ranked_0//A/297/CA, /ranked_0//A/310/CA`


## 7 附录
### `plink2_report_v5.py`
```python
# coding: utf-8
"""
This script can help you to summary the plink2 report file
"""

import os
import re

work_path = r"C:\Users\86181\Documents\pLink\HOIPSDApLink_task_2022.11.19.16.30.07\reports" 


spec_cutoff = 1  # spectra number cut-off
Best_evalue_cutoff = 2 # 交联位点对层次最好的e-value cutoff
E_value_cutoff_SpecLvl = 2 # 谱图层次的e-value cutoff


################Don't change the following lines###############

if work_path:
    reports_path = work_path
else:
    reports_path = os.getcwd()



def count_keyIndic(ele, ele_dic):
    if ele not in ele_dic:
        ele_dic[ele] = 1
    else:
        ele_dic[ele] += 1


#根据交联位点信息和交联肽段信息判断inter还是intra
def judgeHomoHetro(linked_site, pepXL):
    pos_list = re.findall("\((\d*)\)", linked_site)
    position1 = pos_list[0]
    position2 = pos_list[1]
    p = linked_site.find(")-")
    m = linked_site.find("(" + position1 + ")-")
    n = linked_site.find("(" + position2 + ")", p)
    protein1 = linked_site[:m]
    protein2 = linked_site[p + 2:n]

    if protein1 != protein2:
        return "Inter"
    else:
        linkPosPep1, linkPosPep2 = re.findall("\((\d*)\)", pepXL)
        p1 = pepXL.find(")-")
        m1 = pepXL.find("(" + linkPosPep1 + ")")
        n1 = pepXL.find("(" + linkPosPep2 + ")", p1)
        pep1 = pepXL[:m1]
        pep2 = pepXL[p1 + 2:n1]
        deltaList = [
            int(position1) - int(linkPosPep1),
            int(position2) - int(linkPosPep2)
        ]
        pep1IDX = [1 + deltaList[0], len(pep1) + deltaList[0]]
        pep2IDX = [1 + deltaList[1], len(pep2) + deltaList[1]]
        if pep1IDX[1] < pep2IDX[0] or pep1IDX[0] > pep2IDX[1]:
            return "Intra"
        else:
            return "Inter"


# 位点处理，将位点对里面的反库蛋白和污染蛋白的交联对剔除
def site_list_process(site_list, pepXLlist=["WFC(2)-XSV(2)"]):
    i = 0
    while i < len(site_list):
        if "REVERSE" in site_list[i] or "gi|CON" in site_list[i]:
            site_list.remove(site_list[i])
        else:
            i += 1

    if site_list == "":
        return "", None
    else:
        link_type_list = []
        for i in range(len(site_list)):
            boolInter = False
            for j in range(len(pepXLlist)):
                if judgeHomoHetro(site_list[i], pepXLlist[j]) == "Inter":
                    boolInter = True
                    break
                else:
                    continue
            if boolInter:
                link_type_list.append("Inter")
            else:
                link_type_list.append("Intra")
        linkType = "/".join(link_type_list)
        site = "/".join(site_list)
        return site, linkType


# 获取报告文件的名称，读取上级文件夹下的.plink文件查找交联剂和酶的名称,若找不到则返回“pLink_summary.csv”
def get_report_file_name(): 
    path_d = os.path.dirname(os.getcwd())
    try:
        file_list = os.listdir(path_d)
        for fl in file_list:
            if fl.endswith("plink"):
                para = open(os.path.join(path_d, fl)).readlines()
                for line in para:
                    if line.startswith("spec_title"):
                        spec_title = line.split("=")[1].strip()
                    if line.startswith("enzyme_name"):
                        enzyme = line.split("=")[1].strip()
                    if line.startswith("linker1"):
                        linker = line.split("=")[1].strip()
                report_file_name = "_".join([spec_title, linker, enzyme, "v5.csv"])
                return report_file_name
        return "pLink_summary.csv"
    except:
        return "pLink_summary.csv"


# 根据报告文件获取所有raw文件的名称
def get_crosslink_site_info(site_table):
    raw_name_list = []
    for line in site_table[2:]:
        line_list = line.rstrip("\n").split(",")
        if line_list[0] == "":
            raw_name = line_list[2].split(".")[0]
            if raw_name not in raw_name_list:
                raw_name_list.append(raw_name)
    raw_name_list.sort()
    return raw_name_list


#计算openedfl的某一列k的和
def cal_sumOfOneColumn(openedfl, k_column):
    f = openedfl
    k = k_column
    sumNum = 0
    for line in f[1:]:
        lineList = line.rstrip("\n").split(",")
        val = lineList[k]
        if val:
            sumNum += int(lineList[k])
    return sumNum


#计算openedfl某一列k的取值范围
def cal_numRange(openedfl, k_column):
    f = openedfl
    k = k_column
    valList = []
    for line in f[1:]:
        lineList = line.rstrip("\n").split(",")
        if lineList[k]:
            valList.append(float(lineList[k]))
    valList.sort()
    return "{0:.1e}~{1:.1e}".format(valList[0], valList[-1])


def count_peptides(f):
    num_pep = 0
    for line in f[1:]:
        peps = line.split(",")[4]
        num_pep += peps.count(";") + 1
    return num_pep



def statistic_report_file():
    report_file_name = get_report_file_name()
    c = open(report_file_name, 'a')
    f = open(report_file_name, 'r').readlines()
    col_num = len(f[0].split(","))
    stat_list = [""]*col_num
    total_colom = len(f[0].split(","))
    ttl_sites_num = len(f) - 1
    intra_num = 0
    for i in range(1, len(f)):
        if f[i].strip("\n").split(",")[5] == "Intra":
            intra_num += 1
    stat_list[5] = round(intra_num / ttl_sites_num, 2)
    stat_list[0] = ttl_sites_num
    stat_list[1] = cal_sumOfOneColumn(f, 1)
    stat_list[4] = count_peptides(f)
    column_sub_dic = {}
    for k in [6, 8]:
        for j in range(k, total_colom, 3):
            stat_list[j] = cal_sumOfOneColumn(f, j)

    for j in range(7, total_colom, 3):
        stat_list[j] = cal_numRange(f, j)

    c.write(",".join([str(ele) if type(ele) != str else ele \
                        for ele in stat_list]) + "\n\n")
    
    c.write("Raw_Name,# of Pep,# of Spec,e-value range\n")
    for i in range(6, col_num, 3):
        raw_name_list = f[0].split(",")[i].split("_")[:-1]
        raw_name = "_".join(raw_name_list)
        wlist = [stat_list[i+2], stat_list[i], stat_list[i+1]]
        wlist.insert(0, raw_name)
        c.write(",".join([str(ele) for ele in wlist])+"\n")
    c.close()


def splitResult(openedfl, raw_name_list, spec_cutoff, Best_evalue_cutoff, E_value_cutoff_SpecLvl=2):
    finalList = []
    f = openedfl
    n = 2
    while n < len(f):
        spec_dic = {}
        pep_dic = {}
        evalue_dic = {}

        line_list = f[n].rstrip("\n").split(",")
        if line_list[0].isdigit():
            site_list = [line_list[1]]
            p = n + 1
        else:
            print("文件格式错误，line%d"%n)

        while p < len(f):
            line_list = f[p].rstrip("\n").split(",")
            if line_list[0] == "" and line_list[1].isdigit():
                break
            else:
                if line_list[0] == "SameSet":
                    site_list.append(line_list[1])
                p += 1

        site = site_list_process(site_list)[0]
        if site == "":
            while p < len(f):
                if f[p].rstrip("\n").split(",")[0].isdigit():
                    break
                else:
                    p += 1
        else:
            bestSVMscore = f[p].rstrip("\n").split(",")[9]
            pep_std_list = []  # f[p].rstrip("\n").split(",")[5]
            while p < len(f):  # and f[p].rstrip("\n").split(",")[0] == "":
                line_list = f[p].rstrip("\n").split(",")
                if line_list[0].isdigit():
                    break
                else:
                    pep_std = line_list[5]
                    if pep_std not in pep_std_list:
                        pep_std_list.append(pep_std)

                    evalue = float(line_list[8])
                    if evalue < E_value_cutoff_SpecLvl:
                        pep = line_list[5]
                        raw_name = line_list[2][:line_list[2].find(".")]
                        if raw_name not in spec_dic:
                            spec_dic[raw_name] = 1
                        else:
                            spec_dic[raw_name] += 1

                        if raw_name not in pep_dic:
                            pep_dic[raw_name] = [pep]
                        else:
                            if pep not in pep_dic[raw_name]:
                                pep_dic[raw_name].append(pep)

                        if raw_name not in evalue_dic:
                            evalue_dic[raw_name] = evalue
                        else:
                            if evalue < evalue_dic[raw_name]:
                                evalue_dic[raw_name] = evalue

                    p += 1

            totalPep = ";".join(pep_std_list)
            link_type = site_list_process(site_list, pep_std_list)[1]
            total_spec = 0
            min_evalue = 1
            for key in evalue_dic:
                total_spec += spec_dic[key]
                if evalue_dic[key] < min_evalue:
                    min_evalue = evalue_dic[key]
                else:
                    continue
            if total_spec >= spec_cutoff and min_evalue < Best_evalue_cutoff:
                rep_list = [site, total_spec, min_evalue,\
                    bestSVMscore, totalPep, link_type]

                for raw in raw_name_list:
                    if raw not in spec_dic:
                        SEP = ["", "", ""]
                    else:
                        SEP = [
                            spec_dic[raw], evalue_dic[raw],
                            len(pep_dic[raw])
                        ]
                    rep_list.extend(SEP)

                finalList.append(",".join([str(ele) for ele in rep_list]))

        n = p

    finalList = sorted(finalList,
                       key=lambda x: int(x.split(",")[1]),
                       reverse=True)
    return finalList


def find_xlPeptides_File(reports_path):
    for fl in os.listdir(reports_path):
        if fl.endswith("cross-linked_sites.csv"):
            return fl
    return ""


def write2report(raw_name_list, final_list):
    report_file_name = get_report_file_name()
    b = open(report_file_name, 'w')
    
    col = ["XL-sites",  "Total Spec", \
        "Best E-value", "Best Svm Score", "XL-peptide", "Inter or Intra Molecular"
    ]
    for name in raw_name_list:
        col.append(name + "_SpecNum")
        col.append(name + "_E-value")
        col.append(name + "_PepNum")

    b.write(','.join(col)+"\n")
    for line in final_list:
        b.write(line + "\n")
    b.close()


def main_flow(reports_path, spec_cutoff, Best_evalue_cutoff):
    os.chdir(reports_path)
    xlsitesfl = find_xlPeptides_File(reports_path)
    if xlsitesfl == "":
        print("Please check your file")
    else:
        f = open(xlsitesfl).readlines()
        raw_name_list = get_crosslink_site_info(f)
        finalList = splitResult(f, raw_name_list, spec_cutoff, Best_evalue_cutoff)
        write2report(raw_name_list, finalList)
        print("The task is finished")
        statistic_report_file()


if __name__ == "__main__":
    main_flow(reports_path, spec_cutoff, Best_evalue_cutoff)
```

### `XINET.py`

```python
import os
import re

wk_dir = r"C:\Users\86181\Documents\pLink\HOIPSDApLink_task_2022.11.19.16.30.07\reports"
os.chdir(wk_dir)

def get_input_file():
    for fl in os.listdir(wk_dir):
        if fl.endswith("v5.csv"):
            return fl
    print("Wrong")

input_file = get_input_file()

def get_linked_site_inform(linked_site):
    pos_list = re.findall("\((\d*)\)", linked_site)
    position1 = pos_list[0]
    position2 = pos_list[1]
    inf1, inf2 = linked_site.split(")-")
    protein1 = inf1[:inf1.find("(" + position1)]
    protein2 = inf2[:inf2.find("("+position2)]
    # p = linked_site.find(")-")
    # m = linked_site.find("(" + position1 + ")-")
    # n = linked_site.find("(" + position2 + ")", p)
    # protein1 = linked_site[:m].strip()
    # protein2 = linked_site[p + 2:n].strip()
    if protein1 != protein2:
        link_type = "Inter"
    else:
        if abs(int(position1)-int(position2)) < 5:
            link_type = "Inter"
        else:
            link_type = "Intra" 
    return protein1, protein2, position1, position2, link_type


def main():
    file_list = os.listdir(os.getcwd())
    for file in file_list:
        if input_file == file:
            f = open(file).readlines()
            rep_name = file[:-4]+"_xinet.csv"
            b = open(rep_name, "w")
            b.write(",".join(["Score", "Protein1", "Protein2", "LinkPos1", "LinkPos2"]))
            b.write("\n")

            for line in f[1:]:
                line_list = line.rstrip("\n").split(",")
                site = line_list[0]
                spectra = line_list[1]
                write_list = [spectra]
                if site.isdigit():
                    break
                else:                
                    if "/" in site:
                        continue
                    else:
                        if "Molecular" in site:
                            continue
                        else:
                            site_extrac = get_linked_site_inform(site)
                            if site_extrac[-1] == "Inter" or site_extrac[-1] == "Intra" :
                                write_list.extend(site_extrac[:-1])
                                b.write(",".join(write_list))
                                print(write_list)
                                b.write("\n")
            b.close()
            print("Done")
        else:
            continue


if __name__ == "__main__":
    main()
```
