---
title: Tuturial for running Alphafold on Tanglab.pku.edu.cn
date: 2021-12-07
categories: 
- news
tags:
- news
---


# Tuturial for running Alphafold on Tanglab.pku.edu.cn

---

# Query

![unrelax, relax and final state 平均85.6分](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071511460.png)

![alphafold-multitimer输出的rank0-rank5结构](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071511739.png)

![alphafold vs standard structure](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510780.png)

alphafold vs standard structure

<!--more-->
---

# Example

## 绿色tanglab.pku.edu.cn，蓝色北极星

![GST_UBE2S_UBD](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510045.png)

![diUb_L67S](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510073.png)

![His_E2_25K](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510063.png)



![tau](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510149.png)

![pten](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510584.png)

![1tce](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071510400.png)



## Dimer

蓝色部分是晶体结构

![4DFG](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071509880.png)



![3NDX](https://raw.githubusercontent.com/DF-Master/yidapicbed/main/202112071509008.png)



---

---

# Start Alphafold-Multitimer

<aside>
💡 ！！！注意！文件名应使用英文且不含空格！！

</aside>

## Prepare .fasta

### **Folding a homomer**

```bash
>sequence_1
<SEQUENCE>
>sequence_2
<SEQUENCE>
>sequence_3
<SEQUENCE>
```

### **Folding a heteromer**

```
>sequence_1
<SEQUENCE A>
>sequence_2
<SEQUENCE A>
>sequence_3
<SEQUENCE B>
>sequence_4
<SEQUENCE B>
>sequence_5
<SEQUENCE B>
```

## Start Fold!

our alphafold2 is installed in /smb/jiangyd/alphafold 

权限: 777,请勿删除文件！

```bash
# log in tanglab.pku.edu.cn 
# bash shell

# activate alphafold python environment
module load conda/anaconda3-2021-05
source activate
conda activate alphafold

# cd root dir
## !! run_alphafold.sh must be run in the root directory!！
cd /smb/jiangyd/alphafold-multitimer/alphafold/

# code to start alphafold! 
## for monomer(Single Protein)
bash /smb/jiangyd/alphafold-multitimer/alphafold/run_alphafold.sh \
-d /smb/jiangyd/alphafold/alphafold_data/ \
-o /your_output_dir \
-m monomer \
-f /smb/jiangyd/alphafold/your_fasta_file \
-t 2021-11-20 \


## for multimer
bash /smb/jiangyd/alphafold-multitimer/alphafold/run_alphafold.sh \
-d /smb/jiangyd/alphafold/alphafold_data/ \
-o /your_output_dir \
-m multimer \
-f /smb/jiangyd/alphafold/your_fasta_file \
-t 2021-11-20 \


# Test code
bash /smb/jiangyd/alphafold-multitimer/alphafold/run_alphafold.sh -d /smb/jiangyd/alphafold/alphafold_data/ -o /home/jiangyd/test/ -m monomer -f /smb/jiangyd/alphafold/example/query.fasta -t 2021-09-23 -g False
```

or you can make a script(such as run_alphafold_yida.sh) in your love directory

```bash
# Auto run alphafold bash script
module load conda/anaconda3-2021-05
source activate
conda activate alphafold
cd /smb/jiangyd/alphafold-multitimer/alphafold/

# the code below can be edited
nohup bash /smb/jiangyd/alphafold-multitimer/alphafold/run_alphafold.sh \
-d /smb/jiangyd/alphafold/alphafold_data/ \
-o /home/jiangyd/test/ \
-m monomer \
-f /smb/jiangyd/alphafold/example/query.fasta \
-t 2021-12-07 &> nohup_20211207.out&  
echo$(echo -e "\n")
## the output will be saved in nohup file in /smb/jiangyd/alphafold-multitimer/alphafold/
## the code will be run in background

cd -
## back to the directory before
```

## Model

对于上面的 -m 选项，有以下的备选方法：

1. You can control which AlphaFold model to run by adding the `-model_preset=` flag. We provide the following models:
    - **monomer**: This is the original model used at CASP14 with no ensembling.
    - **monomer_casp14**: This is the original model used at CASP14 with `num_ensemble=8`, matching our CASP14 configuration. This is largely provided for reproducibility as it is 8x more computationally expensive for limited accuracy gain (+0.1 average GDT gain on CASP14 domains).
    - **monomer_ptm**: This is the original CASP14 model fine tuned with the pTM head, providing a pairwise confidence measure. It is slightly less accurate than the normal monomer model.
    - **multimer**: This is the [AlphaFold-Multimer](https://github.com/deepmind/alphafold#citing-this-work) model. To use this model, provide a multi-sequence FASTA file. In addition, the UniProt database should have been downloaded.

## run_alphafold.sh的具体参数

```bash
Usage: ./run_alphafold_v21.sh <OPTIONS>
Required Parameters:
-d <data_dir>         Path to directory of supporting data
-o <output_dir>       Path to a directory that will store the results.
-f <fasta_path>       Path to a FASTA file containing sequence. If a FASTA file contains multiple sequences, then it will be folded as a multimer
-t <max_template_date> Maximum template release date to consider (ISO-8601 format - i.e. YYYY-MM-DD). Important if folding historical test sets
Optional Parameters:
-g <use_gpu>          Enable NVIDIA runtime to run with GPUs (default: true)
-n <openmm_threads>   OpenMM threads (default: all available cores)
-a <gpu_devices>      Comma separated list of devices to pass to 'CUDA_VISIBLE_DEVICES' (default: 0)
-m <model_preset>     Choose preset model configuration - the monomer model, the monomer model with extra ensembling, monomer model with pTM head, or multimer model (default: 'monomer')
-c <db_preset>        Choose preset MSA database configuration - smaller genetic database config (reduced_dbs) or full genetic database config (full_dbs) (default: 'full_dbs')
-p <use_precomputed_msas> Whether to read MSAs that have been written to disk. WARNING: This will not check if the sequence, database or configuration have changed (default: 'false')
-l <is_prokaryote>    Optional for multimer system, not used by the single chain system. A boolean specifying true where the target complex is from a prokaryote, and false where it is not, or where the origin is unknown. This value determine the pairing method for the MSA (default: 'None')
-b <benchmark>        Run multiple JAX model evaluations to obtain a timing that excludes the compilation time, which should be more indicative of the time required for inferencing many proteins (default: 'false')
```

- optionally set the `-is_prokaryote_list` flag with booleans that determine whether all input sequences in the given fasta file are prokaryotic. If that is not the case or the origin is unknown, set to `false` for that fasta.

## Scripts

How to run a number of fasta files at once?

```bash
# make sure you are in right dir
cd /smb/jiangyd/alphafold

# make a folder to put your fasta files
mkdir example
scp ……（略）

# write a script to start alphafold!
# eg. run_ubtest.sh(in /smb/jiangyd/alphafold !)
vim run_ubtest.sh
## 以下部分需要你在.sh脚本文件里手动编辑

for fastafile in ubtest/*
do

    echo "##############################"
    echo "##############################"
    echo "##############################"
    date +"%Y-%m-%d-%H-%M-%S"
    echo $fastafile
    echo "##############################"
    echo "##############################"
    echo "##############################"
		bash /smb/jiangyd/alphafold-multitimer/alphafold/run_alphafold.sh -d /smb/jiangyd/alphafold/alphafold_data/ -o /your_output_dir -m monomer -f /smb/jiangyd/alphafold/your_fasta_file -t 2021-09-23 -g False
done

#save scripts
```

后台运行alphafold并记录输出文档到nohup_20210924ubtest.out

```bash
nohup bash run_ubtest.sh &> nohup_20210924ubtest.out&  
```

## Monitor

```bash
##(可选)
nohup vmstat 10 &> nohup_20210924ubtest_vmstat.out&

#如果你想看程序有没有在跑，输入top查看有无高占用进程即可
```

# Reference

[GitHub - deepmind/alphafold: Open source code for AlphaFold.](https://github.com/deepmind/alphafold)

[https://github.com/kalininalab/alphafold_non_docker](https://github.com/kalininalab/alphafold_non_docker)

[CASP14：谷歌DeepMind的AlphaFold 2到底取得了什么成就? 它对蛋白质折叠, 生物学和生物信息学意味着什么?](http://t066v5.coding-pages.com/2020/12/15/CASP14-%E8%B0%B7%E6%AD%8CDeepMind%E7%9A%84AlphaFold_2%E5%88%B0%E5%BA%95%E5%8F%96%E5%BE%97%E4%BA%86%E4%BB%80%E4%B9%88%E6%88%90%E5%B0%B1-_%E5%AE%83%E5%AF%B9%E8%9B%8B%E7%99%BD%E8%B4%A8%E6%8A%98%E5%8F%A0,_%E7%94%9F%E7%89%A9%E5%AD%A6%E5%92%8C%E7%94%9F%E7%89%A9%E4%BF%A1%E6%81%AF%E5%AD%A6%E6%84%8F%E5%91%B3%E7%9D%80%E4%BB%80%E4%B9%88/)

---