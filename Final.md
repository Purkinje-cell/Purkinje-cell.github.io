

# LEB G03组期末总结报告

LEB2023 G03组成员：易鼎程、王沛宇、朱亦文、孙伟棠

这份期末总结报告是从我们小组的各次讨论报告，以及我们小组在课上和大家分享的内容(Conda, RNA-seq, GitHub使用)中选取了主要部分，经过再次排版和修改总结得到的。其中部分内容与我们之前的讨论报告中有一部分重叠，但也添加了许多新内容（包括Linux Shell命令总结、TBTools、生物信息数据库、GitHub Pages等等）。我们希望这份总结报告能够对我们本学期在Linux生物技术基础这门课上学到的知识和掌握的技能做一个良好的总结。

感谢本学期在课程的学习和课堂报告的准备中，罗静初老师和饶希晨助教学长对我们的指导和帮助!

[toc]

# EMBOSS

## EMBOSS 简单介绍

﻿EMBOSS（European Molecular Biology Open Software Suite）是一个开源的生物信息学软件包，旨在为生物学家和生物信息学家提供一套强大的工具，用于分析和处理生物序列数据。

EMBOSS提供了超过100个命令行工具，涵盖了从序列比对、序列搜索、序列编辑、序列分析到序列可视化等多个生物信息学领域。这些工具包括了常用的算法和方法，如Smith-Waterman比对、BLAST搜索、序列转换、序列特征预测等。

EMBOSS的优势在于其灵活性和可扩展性。用户可以根据自己的需求自由组合和定制工具链，以适应不同的研究问题。此外，EMBOSS还支持多种常见的序列文件格式，如FASTA、GenBank、EMBL等，方便用户导入和导出数据。

EMBOSS的使用也非常简单，只需在命令行中输入相应的命令和参数即可运行。同时，EMBOSS还提供了详细的文档和示例，帮助用户快速上手和理解各个工具的功能和用法。

总之，EMBOSS是一个功能强大、灵活易用的生物信息学软件包，为生物学家和生物信息学家提供了丰富的工具和方法，帮助他们更好地分析和解释生物序列数据。无论是基础研究还是应用研究，EMBOSS都是一个不可或缺的工具。

## EMBOSS `dot `功能介绍

1. `EMBOSS`中的不同`dot`功能的区别：

   * `dottup`：一个word要在两个序列里面完全一模一样才有`dot`，即基于相同位点，可以做蛋白核核酸

   * `dotmatcher`：不完全匹配，但是只要在`threshold`以上，就有`dot`，即基于罚分矩阵，只能做蛋白

   * `dotpath`：只显示主对角线相对应的区域

   * `polydot`：生成一个对于所有序列的相似度文件，两两产生

2. 讨论`EMBOSS`软件包`dottup`和`dotmatcher`产生绘图错误的原因：

我们观察了`EMBOSS`软件包，位于集群`/rd1/home/public/EMBOSS-6.6.0.tar.gz`的安装包，其中`emboss/dottup.c`文件包含代码行：

```c
# dottup.c
# line 99
	seq1    = ajAcdGetSeq("bsequence");
    seq2    = ajAcdGetSeq("asequence");

# line 352
    ajGraphSetYlabelC(graph,ajSeqGetNameC(seq1));
    ajGraphSetXlabelC(graph,ajSeqGetNameC(seq2));
```

出现了`ajAcdGetSeq`函数的参数填写`bsequence`和`asequence`顺序错误，推测是造成图片`x`轴和`y`轴颠倒的原因；只需要调换即可获得正确结果；测试数据`HBA_HUMAN`和`HBB_HUMAN`

修改之前：

![](D:/2023Spring/LEB23_G03/Purkinje-cell.github.io/img/scr_LinuxDiscussion1_20230624161950.png)
   修改之后：

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion1_20230624162006.png)
   我们的组长同学尝试了`conda`上的`emboss-6.5.7`，相应的输出结果与错误图一致；

# BLAST

## 基本介绍

BLAST（Basic Local Alignment Search Tool）是一种常用的生物信息学工具，用于在生物序列数据库中进行快速的序列比对和相似性搜索。BLAST通过比较待查询序列与数据库中已知序列的相似性，帮助研究人员识别和注释未知序列的功能和结构。

BLAST具有许多强大的功能和特点。首先，它支持多种类型的序列比对，包括蛋白质序列比对（BLASTP）、核酸序列比对（BLASTN）以及蛋白质-核酸序列比对（BLASTX）。这使得BLAST适用于各种生物学研究领域，如基因组学、蛋白质结构预测和进化分析等。

其次，BLAST具有高度的灵活性和可定制性。用户可以根据自己的需求选择合适的数据库进行比对，如NCBI的非冗余蛋白质数据库（nr）或特定物种的数据库。此外，BLAST还提供了多种参数设置，如比对阈值、比对长度等，以满足不同研究问题的需求。

BLAST的使用也非常简单。用户只需提供待查询的序列和选择合适的比对程序，然后运行相应的命令即可。BLAST还提供了直观的结果输出，包括比对得分、E值、相似性百分比等，帮助用户分析和解释比对结果。

## PAM 和 BLOSUM 打分矩阵的构建

- PAM：PAM（Point accepted mutation）矩阵是利用氨基酸点突变概率构建的蛋白质替换打分矩阵，它假设氨基酸的取代速率与分歧时间无关，而只与取代前后的两个氨基酸有关。PAM 矩阵描述了在给定时间内两个氨基酸之间发生替换的概率，这个时间被称为 PAM 距离。例如，PAM250 矩阵描述了在 250 个 PAM 距离内两个氨基酸之间发生替换的概率。
- BLOSUM：BLOSUM (BLOcks SUbstitution Matrix) 矩阵是基于氨基酸 blocks 的替换比例来计算单个氨基酸的替换分值。例如，BLOSUM62 矩阵是使用序列一致性高于 62%的序列来进行构建的。
- PAM 和 BLOSUM 打分矩阵的区别
  - PAM 后缀数字越大，用于表征两个序列的进化距离越远。而 BLOSUM 正好相反
  - PAM 矩阵的构建是对全长序列进行全局比对，而 BLOSUM 则是进行局部比对

## `BLAST` 本地化安装和使用测试

按照 PPT 上的使用方法在服务器上安装 `BLAST` 软件，测试时发现了一些问题

1. 某些箭头的标注有误，如下图中过滤期望值高于 1 的阳性结果应该指的是 evalue 那里

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion2_20230624162117.png)

2. PSI-BLAST 的命令为 `psiblast`，后面没有 p。而且 uniport 拼错了

![wq](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion2_20230624162127.png)

- 按 PPT 上的方式对 ZMTF_PEP. FASTA 文件建立索引之后可以看到新生成了三个分别以 `.phr`, `.pin`, `.psq` 结尾的文件，我们希望研究它们各自的功能是什么，经过查询我们知道了：
  - `.phr` 文件：Protein Hash Recovery 文件，用于快速查询蛋白质序列的索引文件。 
  - `.pin` 文件：Protein Index 文件，也是用于加速蛋白质序列查询的索引文件，但它使用了不同的索引算法，适用于更大的序列数据集。 
  - `.psq`文件：Protein Sequence文件，是保存蛋白质序列本身的文件。

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion2_20230624162142.png)

## 国家生物中心新版 BLAST 网站的使用测试

发现存在 BUG：使用 PAM250 打分矩阵会导致报错 No result 

如图所示使用 PAM250 矩阵，会导致结果中没有找到的匹配序列
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion2_20230624162156.png)

使用 BLOSUM62/PAM70 矩阵可以找到相似序列，但其他的计分矩阵都会报错 No result

# HMMER

## 基本介绍

调用服务器上的默认环境下`HMMER`软件：

根据`HMMER`官网的结果，目前最新的版本为`v4.3.2`，而服务器上根据`hmmbuild -h`得到的默认版本为`v3.1b`，故使用`conda`安装最新版的`HMMER`，具体代码如下：

```bash
conda create -n hmmer hmmer=3.3.2 # create hmmer environment
conda activate hmmer
hmmbuild -h # version output: v3.3.2
```

安装后，查看`HMMER`官网的官方document：[点击进入](http://eddylab.org/software/hmmer/Userguide.pdf)

根据`HMMER`描述，它是一款用概率统计模型构建概蛋白、DNA序列结构域族的软件，可以完成的工作有：

* 查找精准的同源序列
* 查找单一序列的同源序列
* 蛋白质结构域的自动注释
* 深度比对结果的聚簇

用于实现以上功能的软件包括如下18个：

其中，`hmmbuild`，`hmmsearch`，`hmmscan`和`hmmalign`包含蛋白质结构域分析与注释的主要流程，比如用数据库如`Pfam`

> `Pfam`数据库系统由剑桥大学开发，收集蛋白质家族和蛋白质结构域的信息，可以对蛋白质进行结构和功能、进化分析等，应用于蛋白质结构功能预测、基因组注释和系统生物学研究。

而`phmmer`和`jackhmmer`搜索单个蛋白质序列，类似`BLASTP`和`PSI-BLAST`，`nhmmer`则是能够搜索长、染色体级别的DNA序列，`nhmmscan`能够根据DNA序列搜索蛋白质数据库。

**这里有一个问题，根据官网文献，在HMMER安装的目录下应存在一个`tutorial`目录，包括其测试样例数据，但是我们从`conda`的安装目录中没有找到，从服务器的默认目录也没有找到**

## 使用技巧

`HMMER`支持UNIX系统的管道数据传输，需要用`‘-’`来标识传入的文件名称，例如：

```bash
hmmsearch globins64.hmm uniprot_sprot.fasta
cat globins4.hmm | hmmsearch - uniprot_sprot.fast
cat uniprot_sprot.fasta | hmmsearch globins4.hmm
```

用on-the-fly的方式创建子序列请求：

```bash
cat mytargs.list | hmmfetch -f Pfam.hmm - | hmmsearch - uniprot_sprot.fasta
```

对于`.gz`结尾的压缩格式，`HMMER`程序能够直接阅读并及时地解压缩他们再进行处理，需要`gunzip`（默认包含）作为依赖

## 使用案例

#样例数据的试运行：对globin4数据的分析
从 https://github.com/EddyRivasLab/hmmer/tree/master/tutorial 获取测试数据globin4.sto

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

打印观察数据，发现globin4.sto由HBB_HUMAN、HBA_HUMAN、MYG_PHYCA、GLB5_PETMA四种球蛋白的比对后的部分序列组成

```{r see globin4.sto ,eval=FALSE}
(base) leb3b@bbt:~/0330$ cat globin4.sto
# STOCKHOLM 1.0

HBB_HUMAN   ........VHLTPEEKSAVTALWGKV....NVDEVGGEALGRLLVVYPWTQRFFESFGDLSTPDAVMGNPKVKAHGKKVL
HBA_HUMAN   .........VLSPADKTNVKAAWGKVGA..HAGEYGAEALERMFLSFPTTKTYFPHF.DLS.....HGSAQVKGHGKKVA
MYG_PHYCA   .........VLSEGEWQLVLHVWAKVEA..DVAGHGQDILIRLFKSHPETLEKFDRFKHLKTEAEMKASEDLKKHGVTVL
GLB5_PETMA  PIVDTGSVAPLSAAEKTKIRSAWAPVYS..TYETSGVDILVKFFTSTPAAQEFFPKFKGLTTADQLKKSADVRWHAERII

HBB_HUMAN   GAFSDGLAHL...D..NLKGTFATLSELHCDKL..HVDPENFRLLGNVLVCVLAHHFGKEFTPPVQAAYQKVVAGVANAL
HBA_HUMAN   DALTNAVAHV...D..DMPNALSALSDLHAHKL..RVDPVNFKLLSHCLLVTLAAHLPAEFTPAVHASLDKFLASVSTVL
MYG_PHYCA   TALGAILKK....K.GHHEAELKPLAQSHATKH..KIPIKYLEFISEAIIHVLHSRHPGDFGADAQGAMNKALELFRKDI
GLB5_PETMA  NAVNDAVASM..DDTEKMSMKLRDLSGKHAKSF..QVDPQYFKVLAAVIADTVAAG.........DAGFEKLMSMICILL

HBB_HUMAN   AHKYH......
HBA_HUMAN   TSKYR......
MYG_PHYCA   AAKYKELGYQG
GLB5_PETMA  RSAY.......
//
```

先建立profile 文件名：globin4.hmm

```r
(base) leb3b@bbt:~/0330$ hmmbuild globin4.hmm globin4.sto
# hmmbuild :: profile HMM construction from multiple sequence alignments
# HMMER 3.3.2 (Nov 2020); http://hmmer.org/
# Copyright (C) 2020 Howard Hughes Medical Institute.
# Freely distributed under the BSD open source license.
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# input alignment file:             globin4.sto
# output HMM file:                  globin4.hmm
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

# idx name                  nseq  alen  mlen eff_nseq re/pos description
#---- -------------------- ----- ----- ----- -------- ------ -----------
1     globin4                  4   171   149     0.96  0.589

#
```

运行结果显示共有4条序列
alen表示比对长度，mlen表示共有序列的长度
eff_nseq表示比对的效率，re/pos 表示两个函数或概率的相对熵值，差异越大则熵越大，完全相同则熵为0

less查看hmm文件

```r
HMMER3/f [3.3.2 | Nov 2020]
NAME  globin4
LENG  149
ALPH  amino
RF    no
MM    no
CONS  yes
CS    no
MAP   yes
DATE  Thu Mar 30 16:16:10 2023
NSEQ  4
EFFN  0.964844
CKSUM 2027839109
STATS LOCAL MSV       -9.8664  0.70955
STATS LOCAL VITERBI  -10.7223  0.70955
STATS LOCAL FORWARD   -4.1641  0.70955
HMM          A        C        D        E        F        G        H        I        K        L        M        N        P        Q        R        S        T        V        W        Y
            m->m     m->i     m->d     i->m     i->i     d->m     d->d
  COMPO   2.36800  4.52198  2.96929  2.70577  3.20715  3.01836  3.40293  2.90711  2.55482  2.34699  3.67768  3.19591  3.45289  3.16361  3.08198  2.67062  2.85417  2.56574  4.55599  3.63079
          2.68638  4.42245  2.77499  2.73143  3.46374  2.40505  3.72514  3.29307  2.67761  2.69375  4.24709  2.90366  2.73721  3.18166  2.89820  2.37880  2.77499  2.98440  4.58497  3.61523
          0.55970  1.87270  1.29132  1.73023  0.19509  0.00000        *
      1   1.75819  4.17850  3.77264  3.37715  3.71018  3.31297  4.28273  2.37054  3.30099  2.52201  3.62331  3.56321  3.94404  3.62536  3.57311  2.73559  2.84831  1.62714  5.32308  4.09587      9 v - - -
          2.68618  4.42225  2.77519  2.73123  3.46354  2.40513  3.72494  3.29354  2.67741  2.69355  4.24690  2.90347  2.73739  3.18146  2.89801  2.37887  2.77519  2.98518  4.58477  3.61503
          0.03182  3.85936  4.58171  0.61958  0.77255  0.34183  1.23951
      2   2.62833  4.47006  3.32205  2.82847  3.63376  3.49826  2.70724  3.02941  2.75408  2.74418  3.64789  3.24903  2.69825  3.12221  3.11180  2.79427  2.89380  1.86602  5.05970  3.76787     10 v - - -
          2.68618  4.42225  2.77519  2.73123  3.46354  2.40513  3.72494  3.29354  2.67741  2.69355  4.24690  2.90347  2.73739  3.18146  2.89801  2.37887  2.77519  2.98518  4.58477  3.61503
          0.02321  4.17053  4.89288  0.61958  0.77255  0.48576  0.95510
      3   3.50771  4.88753  4.66754  4.31907  3.27776  4.35743  4.88268  2.50779  4.08449  0.57907  3.22569  4.56607  4.74802  4.37991  4.20749  3.97946  3.79191  2.62059  5.25407  4.04279     11 L - - -
          2.68618  4.42225  2.77519  2.73123  3.46354  2.40513  3.72494  3.29354  2.67741  2.69355  4.24690  2.90347  2.73739  3.18146  2.89801  2.37887  2.77519  2.98518  4.58477  3.61503
          0.02321  4.17053  4.89288  0.61958  0.77255  0.48576  0.95510
      4   2.34111  4.28748  3.51587  3.21760  4.36970  3.06332  4.28987  3.74396  3.23937  3.46805  4.31385  3.39181  3.80269  3.55639  3.55063  1.10193  1.96408  3.23023  5.71943  4.49082     12 s - - -
          2.68618  4.42225  2.77519  2.73123  3.46354  2.40513  3.72494  3.29354  2.67741  2.69355  4.24690  2.90347  2.73739  3.18146  2.89801  2.37887  2.77519  2.98518  4.58477  3.61503
          0.02321  4.17053  4.89288  0.61958  0.77255  0.48576  0.95510
      5   2.08645  4.88226  2.76555  1.92911  4.33063  3.33581  3.78790  3.73824  2.62367  3.34530  4.17189  2.96745  2.11202  2.95012  3.08047  2.70587  2.97273  3.36490  5.57314  4.22372     13 e - - -
          2.68618  4.42225  2.77519  2.73123  3.46354  2.40513  3.72494  3.29354  2.67741  2.69355  4.24690  2.90347  2.73739  3.18146  2.89801  2.37887  2.77519  2.98518  4.58477  3.61503
          0.02321  4.17053  4.89288  0.61958  0.77255  0.48576  0.95510
      6   1.69820  4.80882  2.77457  1.97347  4.35553  2.28499  3.84394  3.77021  2.71313  3.38434  4.21011  2.99279  3.85574  3.01647  3.17214  2.68980  2.96833  3.37473  5.61184  4.27002 
```

hmm文件中，
LENG表示模型中匹配状态的数量
DATE  创建的日期和时间
NSEQ表示序列号
EFFN  有效序列号，确定的有效序列总数
CKSUM是非负无符号32位整数，根据训练数据计算所得
STATS 是E值计算的统计参数 第二位为对齐模式配置
MSV、VITERBI和FORWARD是分数分布的名称
之后的两个数是分别控制每个分布的位置和斜率的两个参数

下方矩阵的值为：每三行为一个有数字标号的大行，每个大行表示序列的一位。每一大行包括三小行，第一行为该位m时各个氨基酸出现的概率，第二行表示各个位上发生i时每种氨基酸出现的概率，观察发现每一大行的第二小行值是相同的。第三行为状态转移矩阵中i/m/d之间变化的值，注意到i和d之间没有转化，因为序列中不允许出现缺失和插入相连的情况。

建立到本地uniprot的软连接，以方便进行数据库中的比对

```{r make softlink0 ,eval=FALSE}
ln -s /rd1/home/public/HMMER/uniprot_sprot.fasta
```


发现软链接不可用，服务器中HMMER文件夹下uniprot\_sprot.fasta为指向原位置（../blast\_data/swiss-prot/uniprot\_sprot.fasta)的软链接，而原文件位置已经移动到了新位置/rd1/home/public/BLAST/BLAST\_DB/uniprot\_sprot.fasta。
    
重新创建软连接至uniprot_sprot.fasta

```{r make softlink1 ,eval=FALSE}
ln -s /rd1/home/public/BLAST/BLAST_DB/uniprot_sprot.fasta

```

使用命令hmmsearch 在uniport 数据库中根据已经生成的隐马尔科夫模型寻找同源序列

```{r hmmsearch ,eval=FALSE}
hmmsearch globins4.hmm uniprot_sprot.fasta > globins4.search.out
less globins4.search.out
```

运行的结果如下

```{r show query ,eval=FALSE}
# hmmsearch :: search profile(s) against a sequence database
# HMMER 3.3.2 (Nov 2020); http://hmmer.org/
# Copyright (C) 2020 Howard Hughes Medical Institute.
# Freely distributed under the BSD open source license.
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# query HMM file:                  globin4.hmm
# target sequence database:        uniprot_sprot.fasta
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Query:       globin4  [M=149]
Scores for complete sequences (score includes all domains):
   --- full sequence ---   --- best 1 domain ---    -#dom-
    E-value  score  bias    E-value  score  bias    exp  N  Sequence              Description
    ------- ------ -----    ------- ------ -----   ---- --  --------              -----------
    4.9e-65  223.2   0.1    5.5e-65  223.0   0.1    1.0  1  sp|P02024|HBB_GORGO    Hemoglobin subunit beta OS=Gorilla gor
    6.9e-65  222.7   0.1    7.7e-65  222.6   0.1    1.0  1  sp|P68871|HBB_HUMAN    Hemoglobin subunit beta OS=Homo sapien
    6.9e-65  222.7   0.1    7.7e-65  222.6   0.1    1.0  1  sp|P68872|HBB_PANPA    Hemoglobin subunit beta OS=Pan paniscu
    6.9e-65  222.7   0.1    7.7e-65  222.6   0.1    1.0  1  sp|P68873|HBB_PANTR    Hemoglobin subunit beta OS=Pan troglod
    1.2e-64  222.0   0.1    1.3e-64  221.8   0.1    1.0  1  sp|P02025|HBB_HYLLA    Hemoglobin subunit beta OS=Hylobates l
    2.1e-64  221.2   0.2    2.3e-64  221.0   0.2    1.0  1  sp|P02033|HBB_PILBA    Hemoglobin subunit beta OS=Piliocolobu
    2.2e-64  221.1   3.3    2.5e-64  220.9   3.3    1.0  1  sp|P02185|MYG_PHYMC    Myoglobin OS=Physeter macrocephalus OX
    9.2e-64  219.1   0.1      1e-63  218.9   0.1    1.0  1  sp|P02028|HBB_CHLAE    Hemoglobin subunit beta OS=Chlorocebus
    1.4e-63  218.5   0.2    1.6e-63  218.3   0.2    1.0  1  sp|P02040|HBB_CEBAL    Hemoglobin subunit beta OS=Cebus albif
    1.8e-63  218.1   0.1      2e-63  218.0   0.1    1.0  1  sp|P02032|HBB_SEMEN    Hemoglobin subunit beta OS=Semnopithec
      2e-63  218.0   0.2    2.3e-63  217.8   0.2    1.0  1  sp|Q6WN25|HBB_LAGLA    Hemoglobin subunit beta OS=Lagothrix l
    2.4e-63  217.7   0.2    2.7e-63  217.6   0.2    1.0  1  sp|P19885|HBB_COLPO    Hemoglobin subunit beta OS=Colobus pol
    2.5e-63  217.7   0.1    2.7e-63  217.5   0.1    1.0  1  sp|P08259|HBB_MANSP    Hemoglobin subunit beta OS=Mandrillus
    2.6e-63  217.6   0.2    2.9e-63  217.5   0.2    1.0  1  sp|P67821|HBB_SAPAP    Hemoglobin subunit beta OS=Sapajus ape
    3.1e-63  217.4   0.2    3.5e-63  217.2   0.2    1.0  1  sp|P67822|HBB_CEBCA    Hemoglobin subunit beta OS=Cebus capuc
    5.9e-63  216.5   1.0    6.5e-63  216.3   1.0    1.0  1  sp|P13557|HBB_CARSF    Hemoglobin subunit beta OS=Carlito syr
      7e-63  216.2   0.1    7.8e-63  216.1   0.1    1.0  1  sp|P02030|HBB_PAPCY    Hemoglobin subunit beta OS=Papio cynoc
    7.2e-63  216.2   0.1      8e-63  216.0   0.1    1.0  1  sp|Q9TSP1|HBB_PAPAN    Hemoglobin subunit beta OS=Papio anubi
    7.9e-63  216.0   0.1    8.8e-63  215.9   0.1    1.0  1  sp|P68222|HBB_MACFU    Hemoglobin subunit beta OS=Macaca fusc
    7.9e-63  216.0   0.1    8.8e-63  215.9   0.1    1.0  1  sp|P68223|HBB_MACFA    Hemoglobin subunit beta OS=Macaca fasc
    7.9e-63  216.0   0.1    8.8e-63  215.9   0.1    1.0  1  sp|P68224|HBB_MACSP    Hemoglobin subunit beta OS=Macaca spec

```

找到的数据根据E-value升序，发现结果排序靠前的均为血红蛋白，与训练数据中含两个人血红蛋白相关

## 本地数据的扩展探究

在使用`HMMER`前，先观察服务器上`/rd1/home/public/HMMER/`目录和`/rd1/home/public/hmmer-data/`目录下的文件，他们的格式以及内容：

* `15ATSBPD.aln`，比对结果文件，第一行有表示多序列比对结果的标识，后续是多个序列ID，序列以及用符号表示的结果：

  ```
  CLUSTAL 2.1 multiple sequence alignment
  
  
  SPL04A          RLCQVDRCTADMKEAKLYHRRHKVCEVHAKASSVFLSGLNQRFCQQCSRFHDLQEFDEAK
  SPL05A          RLCQVDRCTVNLTEAKQYYRRHRVCEVHAKASAATVAGVRQRFCQQCSRFHELPEFDEAK
  SPL03A          GVCQVESCTADMSKAKQYHKRHKVCQFHAKAPHVRISGLHQRFCQQCSRFHALSEFDEAK
  SPL01A          AVCQVENCEADLSKVKDYHRRHKVCEMHSKATSATVGGILQRFCQQCSRFHLLQEFDEGK
  SPL12A          ICCQVDNCGADLSKVKDYHRRHKVCEIHSKATTALVGGIMQRFCQQCSRFHVLEEFDEGK
  SPL14A          PMCQVDNCTEDLSHAKDYHRRHKVCEVHSKATKALVGKQMQRFCQQCSRFHLLSEFDEGK
  SPL08A          PRCQAEGCNADLSHAKHYHRRHKVCEFHSKASTVVAAGLSQRFCQQCSRFHLLSEFDNGK
  SPL06A          PLCQVYGCSKDLSSSKDYHKRHRVCEAHSKTSVVIVNGLEQRFCQQCSRFHFLSEFDDGK
  SPL13A          PICLVDGCDSDFSNCREYHKRHKVCDVHSKTPVVTINGHKQRFCQQCSRFHALEEFDEGK
  SPL09A          PRCQVEGCGMDLTNAKGYYSRHRVCGVHSKTPKVTVAGIEQRFCQQCSRFHQLPEFDLEK
  SPL15A          ARCQVEGCRMDLSNVKAYYSRHKVCCIHSKSSKVIVSGLHQRFCQQCSRFHQLSEFDLEK
  SPL10A          PRCQIDGCELDLSSSKDYHRKHRVCETHSKCPKVVVSGLERRFCQQCSRFHAVSEFDEKK
  SPL11A          PRCQIDGCELDLSSAKGYHRKHKVCEKHSKCPKVSVSGLERRFCQQCSRFHAVSEFDEKK
  SPL02A          PHCQVEGCNLDLSSAKDYHRKHRICENHSKFPKVVVSGVERRFCQQCSRFHCLSEFDEKK
  SPL07A          ARCQVPDCEADISELKGYHKRHRVCLRCATASFVVLDGENKRYCQQCGKFHLLPDFDEGK
                    *    *  ::.  : *: :*::*   :. . .      :*:****.:** : :**  *
  ```

```
* `15ATSBPD.FASTA`，序列文件，标准`fasta`格式，表示需要多序列比对的序列

* `15ATSBPD.hmm`，`HMMER`运行结果文件，含有`HMMER`版本，运行名称，长度，标识符，运行时间，序列数目

了解了这些文件的含义和内容，我们参照李帅同学的指导和官方的tutorial进行实验，首先构建索引：

```bash
hmmbuild ./15ATSBPD.hmm ./HMMER/15ATSBPD.aln
diff ./15ATSBPD.hmm ./HMMER/15ATSBPD.hmm
```

得到输出：

```bash
# hmmbuild :: profile HMM construction from multiple sequence alignments
# HMMER 3.1b2 (February 2015); http://hmmer.org/
# Copyright (C) 2015 Howard Hughes Medical Institute.
# Freely distributed under the GNU General Public License (GPLv3).
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# input alignment file:             ./HMMER/15ATSBPD.aln
# output HMM file:                  ./15ATSBPD.hmm
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

# idx name                  nseq  alen  mlen eff_nseq re/pos description
#---- -------------------- ----- ----- ----- -------- ------ -----------
1     15ATSBPD                15    79    79     0.71  0.714

# CPU time: 0.09u 0.00s 00:00:00.09 Elapsed: 00:00:00.11
```

以及差异：

```
1c1
< HMMER3/f [3.1b2 | February 2015]
---
> HMMER3/f [3.3.2 | Nov 2020]
10c10
< DATE  Thu Mar 30 16:45:53 2023
---
> DATE  Sat Apr  9 22:04:10 2022
```

可见两者结果没有差异。

> 根据输出信息，我们可以得到如下的内容：
>
> * `nseq`，15条序列的比对结果
> * `alen`，长度均为79，且`mlen`，共有序列长度79
> * `eff_nseq`，有熊序列0.71，意味着有约11条序列相似，`re/pos`代表每个位置的相对熵值

根据`hmmbuild`的参数，我们可以知道其可以用于蛋白质、DNA、RNA同源序列的索引构建，可以通过更改策略、调整序列权重，先验频率参数等调整结果，**这一部分有待继续深入探索**

接下来我们用`hmmsearch`搜索序列数据库：

```bash
hmmsearch ./15ATSBPD.hmm uniprot_sprot.fasta > ./15ATSBPD.search.out
cat ./15ATSBPD.search.out | head
```

选取其中一栏作为参考分析：

```bash
# hmmsearch :: search profile(s) against a sequence database
# HMMER 3.1b2 (February 2015); http://hmmer.org/
# Copyright (C) 2015 Howard Hughes Medical Institute.
# Freely distributed under the GNU General Public License (GPLv3).
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# query HMM file:                  ./15ATSBPD.hmm
# target sequence database:        uniprot_sprot.fasta
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Query:       15ATSBPD  [M=79]
Scores for complete sequences (score includes all domains):
   --- full sequence ---   --- best 1 domain ---    -#dom-
    E-value  score  bias    E-value  score  bias    exp  N  Sequence              Description
    ------- ------ -----    ------- ------ -----   ---- --  --------              -----------
    1.1e-40  142.6  10.0    2.7e-40  141.4  10.0    1.7  1  sp|Q94JW8|SPL6_ARATH   Squamosa promoter-binding-like protein

Domain annotation for each sequence (and alignments):
>> sp|Q94JW8|SPL6_ARATH  Squamosa promoter-binding-like protein 6 OS=Arabidopsis thaliana OX=3702 GN=SPL6 PE=1 SV=2
   #    score  bias  c-Evalue  i-Evalue hmmfrom  hmm to    alifrom  ali to    envfrom  env to     acc
 ---   ------ ----- --------- --------- ------- -------    ------- -------    ------- -------    ----
   1 !  141.4  10.0   2.7e-44   2.7e-40       2      77 ..     123     198 ..     122     200 .. 0.98

  Alignments for each domain:
  == domain 1  score: 141.4 bits;  conditional E-value: 2.7e-44
              15ATSBPD   2 lCqvegCeadlseakdyhrrhkvCevhskaskvvvsgleqrfCqqCsrfhllsefdegkrsCrrrlaghnerrrkk 77
                           lCqv gC++dls++kdyh+rh+vCe+hsk+s+v+v+gleqrfCqqCsrfh lsefd+gkrsCrrrlaghnerrrk+
  sp|Q94JW8|SPL6_ARATH 123 LCQVYGCSKDLSSSKDYHKRHRVCEAHSKTSVVIVNGLEQRFCQQCSRFHFLSEFDDGKRSCRRRLAGHNERRRKP 198
                           8**************************************************************************8 PP
```

输出中单行的结果表示序列查询结果：

* 1-3列：按`E-value`从小到大排序，表示显著性更高，*E-value的大小依赖数据库的大小，大的数据库意味着更大的假阳性率*：`score`，完整序列的似然分数，`bias`，偏差，只有与`score`数量级接近时需要注意

* 4-6列描述得分最高的结构域，其值与前文的描述类似
* 7-8列：`exp`描述目标序列中结构域的期望数，结果中为1.7，而`N`则描述经过处理后最终决定鉴定到的结构域的数目为1

输出中多行的是每个序列的结构域注释，其中选择的为`SPL6_ARATH`序列，其中有1个结构域，从左至右依次表示：序号，是否满足每个序列和每个结构域的阈值（**什么意思**），`score`和`bias`，条件`E-value`，独立`E-value`，`hmm`在索引中的起始点，比对目标序列中的起始点，目标序列上定位的范围，平均后验概率

输出中后续的代码：表示比对共有序列的比对情况，字母代表匹配，`+`代表残基有正的`log-odds`发射分数，目标序列以及每个残基的后验概率

# Vim 编辑器的使用

Vim 编辑器（Vi IMproved）是一种高效的模式型编辑器，在不同模式下键盘上的按键具有不同的含义。大部分 UNIX 系统都内置了 vi 文书编辑器，vim (vi improved)由 vi 发展而来，功能更全面。

Vim 主要具有4种模式，它们分别是普通模式 (Normal mode, 又称命令模式)，插入模式 (Insert mode)、可视模式 (Visual mode)和底线命令模式（Last line mode）。还有一些相对少用的模式，如替换模式 (Replace mode)和终端模式 (Terminal Mode, 在 neovim 的内置终端中使用)

使用 vim+文件名打开文件时，默认进入命令模式，此时输入 i (或 a/o)可以进入插入模式，输入: 可以进入底线命令模式。在插入模式下，使用 esc 可以回到命令模式；底线命令模式下，使用 enter 键执行命令可以回到命令模式。输入: wq，文件被保存并退出。以上模式间关系如图所示:
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion4_20230624161332.png)

## 普通模式

Vim 的强大之处主要体现在它的普通模式中。在这个模式下，键盘上的许多字母键被赋予了特有的含义，可以使用它们来组成许多方便的编辑命令，快速地编辑文本。使用 Vim 的程序员们曾有一句口号叫做“Edit the text at the speed of thought”，生动形象的表现了 Vim 的强大之处。

普通模式下最主要的命令可以分成3大类，编辑类命令 (operator)，查找类命令 (find)和移动类命令 (motion)。列表总结如下：

| 命令                  | 描述                                       |
| --------------------- | ------------------------------------------ |
| 编辑类命令 (operator) |                                            |
| d                     | 删除 (Delete) 命令                         |
| c                     | 更改 (Change) 命令，即删除之后进入插入模式 |
| y                     | 复制 (Yank)命令                            |
| p                     | 粘贴 (Paste)命令                           |
| =                     | 自动格式化 (Format)文本                    |
| 查找类命令 (find)     |                                            |
| f                     | 在当前行向后查找字符 (Find)                |
| F                     | 向前查找字符 (Find backwards)              |
| /                     | 全局查找 (Find globally)                   |
| ?                     | 向前全局查找 (Find backwards globally)     |
| 移动类命令 (motion)   |                                            |
| $                     | 移动到行尾                                 |
| ^                     | 移动到行首第一个非空字符                   |
| 0                     | 移动到行首                                 |
| h                     | 向左移动一个字符                           |
| l                     | 向右移动一个字符                           |
| j                     | 向下移动一个字符                           |
| k                     | 向上移动一个字符                           |
| w                     | 向后移动一个单词                           |
| b                     | 向前移动一个单词                           |
| e                     | 向后移动到单词的末尾                       |
| ge                    | 向前移动到单词的末尾                       |
| gg                    | 移动到文件的第一行                         |
| G                     | 移动到文件的最后一行                       |

但是，只有这些命令还不能构成 Vim 的独特之处。Vim 的独特之处在于，它实现了一种特殊的语法，使得编辑命令能够组合使用。这个语法的基本模式是：

**{count}{operator}{motion}** 或 **{operator}{count}{motion}**

它的意思是，对 **{motion}** 指定的范围使用 **{operator}** ，总共使用 **{count}** 次。

下面举几个具体的例子来展示这一语法的作用。

1. d2w (delete two words) :
   命令使用前：one **t**wo three four
   命令使用后：one **f**our
2. d2j or 2dd (delete two lines)：删除两行
3. ggdG (move to first line, then delete to the end)：删除文件中所有内容

普通模式下，假如重复一个 **{operator}** 两次，或者按 shift + **{operator}**，那么默认这个操作是对目前光标所在行执行，如 dd 将会删除（剪切）这一行，而 yy 将会拷贝这一行。

普通模式下的其他命令，如 o, O, A 等，本质上都可以看做是这些基本命令的组合。

## 插入模式

命令模式下按键 i (insert) /a (append) /o (open a new line) 将进入插入模式，插入模式下将同一般文本编辑器插入或回车删除修改文本。命令模式下按键 r 进入替换模式，输入字符替换已有字符而不是插入。插入模式下按键 esc 回到命令模式。

插入模式中，有一些命令让我们能够即时地修改输入的内容，这样我们无需退出到普通模式便可以进行常用的编辑操作。以下的命令中，\<C-a\>这样的记号表示 Ctrl+a 同时按下。这些命令包括：

| 命令              | 描述                           |
| ----------------- | ------------------------------ |
| \<C-h\>           | 删除前一个字符                 |
| \<C-w\>           | 删除前一个单词                 |
| \<C-u\>           | 删至行首                       |
| \<C-r\>{register} | 指定寄存器内的内容粘贴至光标处 |
| \<C-\[\>          | 退至正常模式                   |
| \<esc\>           | 退至正常模式                   |


## 可视模式

在普通模式下键入 v，就进入了可视模式。可视模式下我们可以使用与普通模式中相同的移动命令进行光标的移动。与普通模式不同的是，可视模式下我们可以看到当前光标移动所建立的选区，并且可以对这个选区使用编辑命令进行编辑。可视模式对于初学使用 Vim 的新手来说很有帮助，因为在可视模式下命令的操作范围是可见而直观的。

## 底线命令模式

在普通模式下输入 ': '，就进入了底线命令模式，在底线命令模式中我们可以运行一些对于文件操作的命令。

例如，输入: w，再按下回车，就可以保存文件；: q 退出 vim ；: wq 保存文件并退出。

较为常用的命令是替换命令（substitute），它的基本模式是:
:[range]s[ubstitute]/{pattern}/{string}/[flags]

在命令中，range 代表要替换的范围，可以使用行号或使用%指定全局范围。{pattern}是一处正则表达式，指定需要替换的文本。{string}表示要替换成为的字符串，而[flags]表示替换模式。其中用{}包裹的参数必须指定，而[]包裹的参数可以缺省。常用替换模式有 g, c 等，分别表示在一行中进行全局替换，和在每一个替换处提醒是否进行替换。

另一个常用的命令是帮助（help）。输入: h 并按下回车，我们就打开了 Vim 自带的帮助文档。输入: h 再按下 Tab 键，我们看到 Vim 会给我们自动补全出各种 Vim 中配置的帮助。

## .vimrc 文件

Vim 的强大之处还在于它能够完全自主地定制。这种定制模式是通过. vimrc 文件配置实现的。. vimrc 文件是 vim 的配置文件，启动 vim 时会自动运行该配置，从而用户可以通过修改. vimrc 改变 vim 配置。. vimrc 的配置是使用一种特殊的脚本语言：vimscript 来表示的，具体的配置可以参考许多 github 上已有的配置。

在自己的 home 目录下，使用 vim .vimrc 创建配置文件，由于该文件为隐藏文件，需要用 ls -a 进行查看。

使用 set 关键字可以设定一些 vim 参数，如 set relativenumber 可以添加相对行号，便于定位文本位置进行跳行、翻页等。使用 map 可以进行快捷键设置，调整自己习惯的键位。set colorscheme=主题 (如 desert)可以改变颜色显示。

用户自定义的内容要想在 vim 中保持，就需要通过. vimrc 文件，因此安装 vim 插件等也都需要通过修改. vimrc 文件实现。

## Vim 插件

vim 本身是高效的文本处理工具，为了使这种能力在脚本书写和 debug 等实际应用中发挥作用，仅仅修改. vimrc 文件较为繁琐，需要安装一些插件。同其他工具一样，安装插件需要插件管理器，较为常用的插件管理器为 Vim-Plug。执行以下命令以在 vim 文件夹下创建插件路径，事实上这是从 Github 下载 Vim-Plug 插件的过程。

安装完成后，在.vimrc中添加加载插件的命令:

```vimscript
call plug#begin('~/.vim/plugged')
# 中间放要安装的插件及配置
call plug#end()
```

将加载要安装的插件的命令置于两行之间，便可以使vim获得其功能。以vim which key为例: 由于vim有众多的快捷键，且允许用户自行设置快捷键，用户不能记忆快捷键时情况将变得较为复杂。Vim which key插件想要实现快捷键提示，使用户能够快速完成文本编辑。

首先在加载插件的命令中间添加Plug: Plug 'liuchengxu/vim-which-key'，随后根据插件对应的提示修改.vimrc，使得快捷键可被查询。修改完成后，重启vim，直接输入vim，在vim界面输入:PlugInstall完成插件安装。

值得提出的是，插件安装的过程中，会发现许多插件因连接问题难以从国内直接下载，可以通过科学上网或从 [Vim Awesome](https://vimawesome.com/) 寻找其他来源进行下载。

## Neovim 以及其他软件中的 Vim 模式

Vim 这种模式化编辑的设计是编辑器历史上最成功的设计之一（另一个较成功的设计是 Emacs，事实上，它们分别被称为编辑器的神和神的编辑器）。这可以从几乎所有 Linux 发行版都内置 Vim，以及许多代码编辑器（例如 VSCode）和 IDE（例如 Idea, Pycharm 等）中都有 Vim 插件看出。下面简要介绍 Neovim，以及 VSCodeVim 插件。

Neovim 是一个新时代的 Vim，它最早由巴西程序员 Thiago de Arruda Padilha 开发。开发 Neovim 的原因很有意思：为了扩展和升级 Vim，使得它更加现代并支持许多新的语言特性。本来 Thiago 想在 Vim 的基础上开发，并在 Vim 项目的开源仓库上提交了他对 Vim 的重构。但是这一重构被 Vim 的作者 Bram Moolenaar 拒绝了。这是因为 Bram 认为对 Vim 这样一个成熟的大型项目进行这样规模的重构具有太大的风险。于是，Thiago 就发起了 Neovim 项目。Neovim 和 Vim 的主要区别在于它支持使用 Lua 语言对 Vim 进行配置，并且具有许多新特性，如异步 IO，浮动窗口，原生 LSP 支持，内置终端模拟器等。这些特性使得 Neovim 的开发大为火热，有许多开发者加入到 Neovim 的开发中，并产生了许多由开源社区维护的 Neovim 配置包，如 LunarVim, SpaceVim 和 LazyVim 等。

LazyVim 配置完成后效果图：
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion4_20230624161656.png)

VSCodeVim 插件是当前最流行的编辑器 VSCode 中一个模拟 Vim 编辑模式的插件，可以通过插件市场安装。该插件提供了大多数 Vim 中的操作模式和命令，对于初学者来说是一个非常好的开始使用 Vim 的起点。但是，它并没有实现 Vim 的全部功能，如在 VSCodeVim 中无法使用宏（Macro）和表达式寄存器 (Expression Register)。


## Vim 的学习

Vim 内置了一个简短的教程，只需要在 Bash shell 中输入 vimtutor，或在 Vim 中输入: tutor 命令，即可进入 Vim 教程。

另一个比较好的 Vim 教程是 VSCode 中的插件 LearnVim 所提供的，它讲述了大多数 Vim 的常用命令，以及一些由 Vim 插件实现的命令，如 Easymotion，Vim-surround 等。

另外也有许多有关 Vim 的书籍，例如《Vim 实用技巧》就是非常好的一本介绍如何使用 Vim 进行文本编辑的入门书。

Vim 的官方网站是 www.vim.org ，其中提供了许多有用的 Vim 插件资源和 Vim 的官方文档。

# Linux 常用shell命令总结

## 目录切换

cd + 目标文件夹，可以使用相对路径，也可以使用绝对路径
cd .. 返回上一级文件夹
cd ~ 返回当前用户的home目录，等效于cd $HOME 即登录时的初始位置
cd / 返回根目录（根目录对所有用户而言共享）

## 查看文件

cat filename  打印整个文件，注意当不清楚文件大小时应该慎用
cat -n 打印行号
less filename 可用于查看较大文件，可以支持滚轮↑ ↓翻页
head filename 打印文件头几行内容，默认10行
tail filename 打印文件后几行内容，默认后

*若不小心cat输出过大的文件导致卡死，或误运行其他耗时长的程序需要中断操作，可以*

1. ctrl+c中断
2. ctrl+z将文件转到后台 然后kill [pid] (pid通常为1,2等，取决于后台运行的任务数，通过jobs查看)


## 创建文件

1. touch file1.txt.

2. touch file1.txt file2.txt file3.txt.

3. > file1.txt.

4. cat > file1.txt.

5. echo "Some line" > file1.txt.

6. echo > file1.txt.

7. nano name 创建文件
   也可以使用文本编辑器如vim查看和修改文件，相关操作见本小组Vim相关操作实验报告

## 删除文件和文件夹

rm 文件名 用于删除单一文件
rm -r 递归式删除文件夹及其中内容
rm -i 删除文件时逐一询问
rm -f 直接删除文件，不询问
慎用 rm -rf

## 查看文件夹

tree / tree 文件夹 以树状结构打印出文件夹中的文件内容
ls 直接输出当前文件夹中的文件和子文件夹list ， 也可以 ls 目标文件夹 输出指定文件夹的内容 

- -a 显示所有文件及目录 (. 开头的隐藏文件也会列出)
- -d 只列出目录（不递归列出目录内的文件）。
- -l 以长格式显示文件和目录信息，包括权限、所有者、大小、创建时间等。
- -r 倒序显示文件和目录。
- -t 将按照修改时间排序，最新的文件在最前面。
- -A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)
- -F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"
- -R 递归显示目录中的所有文件和子目录

## 创建文件夹

mkdir 文件夹名称

## 移动文件、重命名

mv  源文件 目标位置文件夹（相对路径或绝对路径） 以移动文件
mv 原文件名 新文件名  重命名文件



## 其他常用操作

**Tab 自动补全【若相同文件夹下无前缀相同的文件，可以大大节约输入时间】**
**↑ 调用上一个命令**

w=who 
查看当前登录情况等
whoami  
查看用户名称
创建软链接
ln -s 文件路径
top q退出
chomd -w wenjianming 去掉可修改属性
ln -s wenjian lianjieming 创建链接
修改文件访问权限
chmod XXX
为数字 （0-7）= 1+2+4，分别代表读取，写入和运行权限
三个X依次代表文件所有者（Owner）、用户组（Group）、其它用户（Other Users）的权限



# Bash脚本

`bash`脚本包含很多比较复杂的语法和逻辑，在编写`bash`脚本的时候，不仅要考虑变量之间的转换关系与字符串操作，还需要注意各类结构符号的使用，本次主要针对不同括号种类进行了讨论

## 关于括号的作用：

以下内容基于Bash版本`4.4.20(1)-release`

### 小括号

`()`的作用，总结为**执行命令组**和**初始化数组**两种功能：

* 执行命令组：

  ```bash
  #!/bin/bash
  
  echo $( VAR="Hello!";echo $VAR )
  ```

  执行该命令完成了如下的任务：

  * 该bash由`/bin/bash`路径下的可执行文件`bash`解释，在执行`bash`时，先运行相应的`.bashrc`文件构建新的`bash`进程上下文

  * 相应在`echo ( ... )`中，小括号中的内容被作为命令组，传入该程序`fork`创建的子进程`bash`中，该子`bash`继承父进程的环境变量；创建子进程时，不会再调用`.bashrc`中的指令初始化，从而`.bashrc`中相关的`alias`设定没有效果，但会继承父进程的变量，而父进程不能访问子进程的变量，继续输入：

    ```bash
    echo $VAR
    ```

    会输出空

  * 使用`$`符号将执行子命令的`bash`标准输出的字符串序列拷贝到父`bash`中的变量

* 初始化数组

  不常用，但可以用于定义一些常量：

  ```bash
  #!/bin/bash
  
  samples=( SRR1059081 SRR1059082 SRR1059083 )
  
  for sample in samples; do
  	echo $sample
  done
  echo ${sample[2]}
  ```

  会一次输出三个样本号和第三个样本号，即支持通过数组下标访问

### 双小括号

`(( ... ))`用于算数表达式，可以进行整数运算，在`(( ... ))`之中的语句会被解释为算数语言：

```bash
> i=0
> echo $i
0
> ((i+=2))
> echo $i
2
```

### 中括号

方括号`[]`主要用于**条件测试**以及**数组取下标**

* 对于`[ ... ]`，等价于`test`命令的间歇，用于对条件表达式进行测试，可以跟在`if`，`while`和`for`后面，具体的格式此处省略，例如下：

  ```bash
  if [ -f myfile.txt ]; then
  	echo "File exists"
  fi
  ```

  是一个常见的检验文件是否存在的方法，其他的常见测试条件见下：

  | 符号 | 意义（ == True）                     |
  | ---- | ------------------------------------ |
  | -n   | 长度非0，或已定义                    |
  | -z   | 长度为0，或未定义                    |
  | =    | （对于字符串，全部）相等             |
  | !=   | （对于字符串，任一）不等             |
  | -eq  | （对于字符串，第一个字符）相等       |
  | -ef  | 设备号与节点号相同                   |
  | -f   | 存在且是正规文件（非符号连接、目录） |
  | -e   | 存在                                 |
  | -d   | 存在且是目录                         |
  | -h   | 存在且是符号链接                     |
  | -r   | 存在且可读                           |
  | -w   | 存在且可写                           |
  | -x   | 存在且可执行                         |
  | -a   | 表达式都为真                         |
  | -o   | 表达式有一个为真                     |

  *相关的整数比较大小，如`-gt`，`-le`等，由于较为简单不在列举，部分不常用不列出*

* 对于`[[ ... ]]`，其属于`bash`中对于`shell`本身`test`指令所不能满足的条件的扩展，提供了更多功能，比如模式匹配和正则表达式：

  ```bash
  if [[ $VAR == *"World"* ]]; then
  	echo "Contains World"
  fi
  ```

### 大括号

大括号`{}`主要用于**命令组**和**变量替换**

* 在作为命令组时，与`()`代表的命令组不同的是，大括号会在当前的`bash`进程中执行，而不会`fork`子`bash`，这对于变量赋值和修改当前环境变量有用：

  ```bash
  #!/bin/bash
  
  { VAR="Hello!";echo $VAR } > var.out
  ```

  同时，可以对命令组中的指令进行相同的操作，例如将他们的标准输出一同重定向到其他文件中

* 变量替换，即使用`${ ... }`的形式对变量字符串进行相关操作，或者用于划分变量名称的界限，一些相关的操作可以总结如下：

  定义变量`file=/dir1/dir2/dir3/my.file.txt`，我们可以用 `${ } `分别替换获得不同的值：

  * `${#file}`：表示变量长度

  使用取子串操作：

  * `${file#*/}`：拿掉第一条` / `及其左边的字符串：`dir1/dir2/dir3/my.file.txt`
  * `${file##*/}`：拿掉最后一条` / `及其左边的字符串：`my.file.txt`
  * `${file#*.}`：拿掉第一个 `. `及其左边的字符串：`file.txt`
  * `${file##*.}`：拿掉最后一个` . `及其左边的字符串：`txt`
  * `${file%/*}`：拿掉最后条 `/ `及其右边的字符串：`/dir1/dir2/dir3`
  * `${file%%/*}`：拿掉第一条` / `及其右边的字符串：(空值)
  * `${file%.*}`：拿掉最后一个` . `及其右边的字符串：`/dir1/dir2/dir3/my.file`
  * `${file%%.*}`：拿掉第一个` . `及其右边的字符串：`/dir1/dir2/dir3/my`

  总结为`%`为去掉右边，`#`为去掉右边，单一符号是最小匹配，两个符号是最大匹配

  同时也可以使用模式匹配：

  * `${file:0:5}`：最左边的五个字节：`/dir1`
  * `${file:5:5}`：第五个字节右边连续的5个字节：`/dir2`

  我们也可以对变量值里的字符串作替换：

  * `${file/dir/path}`：将第一个 `dir `提换为` path`：`/path1/dir2/dir3/my.file.txt`

  * `${file//dir/path}`：将全部 `dir` 提换为`path`：`/path1/path2/path3/my.file.txt`

  利用 `${}`还可针对不同的变量状态赋值(没设定、空值、非空值)： 

  * `${file-my.file.txt} ` ：假如 `$file`没有设定，则使用 `my.file.txt` 作传回值

  * `${file:-my.file.txt}` ：假如 `$file` 没有设定或为空值，则使用 `my.file.txt` 作传回值

  * `${file+my.file.txt}` ：假如 `$file` 设为空值或非空值，均使用 `my.file.txt` 作传回值

  * `${file:+my.file.txt}` ：若 `$file` 为非空值，则使用 `my.file.txt` 作传回值
  * `${file=my.file.txt}` ：若 `$file` 没设定，则使用 `my.file.txt` 作传回值，同时将`$file`赋值为 `my.file.txt`

  * `${file:=my.file.txt}` ：若 `$file` 没设定或为空值，则使用 `my.file.txt` 作传回值，同时将 `$file` 赋值为 `my.file.txt` 
  * `${file?my.file.txt}` ：若 `$file` 没设定，则将 `my.file.txt` 输出至标准错误输出

  * `${file:?my.file.txt}` ：若 `$file` 没设定或为空值，则将 my.file.txt 输出至标准错误输出

  除此之外，我们还发现这样的用法：

  * `${!file}`：会将`file`变量作为变量名，再返回该变量名的内容，即为间接引用

### 补充

* 除以上`()`，`[]`与`{}`的功能外，还有一些使用方法，可以与命令结合进行理解，常见于命令`specific`的语法中，例如`find`指令：

  ```bash
  find . -name "*.txt" -exec cp {} /tmp \;
  ```

  可以认为`{}`是`find`指令为标准输出结果定义的占位符

* `[]`与`(())`的有些用法相同，可以用于算数运算，当前方用`$`进行访问时

## 关于bash的环境变量

`bash`脚本本身包括一些预定义的环境变量，可以在运行中访问获得相关的`bash`信息，除`$BASH`打印`bash`可执行文件路径与`$BASH_VERSION`打印`bash`版本外，还有以下常用的环境变量

> 此处补充`export`的作用：
>
> `export`命令用于将变量输出为环境变量，或者将函数输出为环境变量，子进程可以通过`export`将变量“广播”给父进程，但同时其他程序也会知晓

### 运行时特殊变量，由`bash`进程控制，用户无法改变地变量

* `$i`，上一次执行命令的退出码

  ```bash
  > ls ./
  test.sh
  > echo $?
  0
  > ls noexists
  ls: cannot access noexists: No such file or directory
  > echo $?
  2
  ```

* `$$`，当前`shell`进程`ID`

* `$_`，上一个命令的最后一个参数

* `$!`，最后一个后台异步执行的命令的进程`PID`

* `$0`，当前`shell`名称

* `$@,$#,$N`，表示`bash`进程装载时参数的数量、值和第N个参数

> 在`bash`中可以使用`readonly`指令来声明只读变量，用`declare`指令来声明可读可写变量，将普通变量改为特殊变量

### 系统预设变量，由`bash`预设的环境变量

* `$BASH`，`$BASH_VERSION`

* `$EUID`，当前使用交互式`shell`的用户
* `$HISCMD`，当前指令的序号，在`history`中的
* `$PWD`，当前目录
* `$PATH`，`bash`命令的搜索路径
* `$BASHPID`，当前的`bash`进程`PID`
* `$BASH_SUBSHELL`，记录了一个`bash`进程实例中多个`subbash`嵌套深度的累加器
* `$SHLVL`，记录了多个`bash`进程实例嵌套深度的累加器

例如：

```bash
#!/bin/bash
 
task1() {
	# do something...
}
 
task2() {
	# do something...
}
 
task3() {
	# do something...
}
 
# define some other tasks...
 
# use & if we want to implement multiprocess
# (task1 &)
(task1)
(task2)
(task3)
# run some other tasks...
 
# run commands from command line arguments
for cmd in "$@"; do
  ($cmd)
done
```

# Conda环境管理

Miniconda3是轻量化的python环境版本管理器，可以非常方便的进行环境安装和软件包版本管理。同时，它还托管了一部分非Python的软件包，只要指定源便可安装，是生物信息学中非常常用的软件。

## 下载安装

* 通过官网下载
  [下载地址：https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

  指令`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

* 安装

  * 运行权限

    `chmod +x Miniconda3-latest-Linux-x86_64.sh`

  * 安装

    `bash Miniconda3-latest-Linux-x86_64.sh`或`./Miniconda3-latest-Linux-x86_64.sh`

  * 接受许可协议

  * 确认安装路径，比如`/home/leb03/miniconda`

  * 确认初始化程序

    > 作用：在你的`~/.bashrc`文件里添加了这样一段代码：
    >
    > ```bash
    > # >>> conda initialize >>>
    > # !! Contents within this block are managed by 'conda init' !!
    > __conda_setup="$('/rd1/home/wangpy/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
    > if [ $? -eq 0 ]; then
    >  eval "$__conda_setup"
    > else
    >  if [ -f "/rd1/home/wangpy/miniconda3/etc/profile.d/conda.sh" ]; then
    >      . "/rd1/home/wangpy/miniconda3/etc/profile.d/conda.sh"
    >  else
    >      export PATH="/rd1/home/wangpy/miniconda3/bin:$PATH"
    >  fi
    > fi
    > unset __conda_setup
    > # <<< conda initialize <<<
    > ```
    >
    > 其作用是将conda宏指令、conda/bin中的模块装入系统环境

* 确认安装

  运行一下重载bash：`source ~/.bashrc`

  确认conda安装：`conda --version`

## 源、包与环境管理

## 源设置

* 常用源

  `conda config –-add channels conda-forge`

  `conda config –-add channels r`

  `conda config --add channels bioconda`

* 清华软件源加速：

  `conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free`

  `conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge`

  `conda config --set show_channel_urls yes`

* 查看所有源：

  `conda config --show channels`

* 删除源：

  `conda config –remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free`

## 环境管理

* 系统启动时你已经处于`base`环境中，是conda启动时的默认环境

* 创建环境

  `conda create -n foo python=3.8`，以创建一个名为`foo`并安装了python3.8的环境

* 进入环境

  `conda activate foo`

* 退出环境

  `conda deactivate`

* 删除环境

  `conda remove -n foo –-all`

* 查看所有环境

  `conda env list`或`conda info -e`

  > 其他环境与base环境的关系，base是conda的默认环境，可以做修改；创建环境时，base环境中的包被默认认为是相应的备选版本，但并不会默认继承

* 环境添加变量

  `conda-envs config vars set my_var=blablabla`

## 包管理

* 搜索包

  `conda search pkg_name`

* 包安装

  `conda install pkg_name`或指定版本`conda install pkg_name=version`

  > 可以同时安装多个`conda install pkg_name1 pkg_name2 ...`
  >
  > 指定从某个源安装`conda install -c channel pkg_name`
  >
  > 指定某个网站`conda install -i src pkg_name`

* 包删除

  `conda remove pkg_name`

* 列出环境中所有包

  `conda list`或指定环境`conda list -n foo`

* 安装官网下载包

  首先将安装包文件复制到anaconda安装目录下的pkgs目录，然后运行`conda install --use-local pkg_name`

  同时conda对于已经下载的对应版本包，不会在配置环境时从新下载


# Git与GitHub

## Git

Git 是一个开源免费的分布式版本管理系统，由芬兰程序员 Linus Torvalds 开发。Git 的版本控制可以对代码、文档和日常工作所用到的文件进行方便的管理，而不用多次对源代码进行复制。并且，Git 的分布式版本管理使得它能够在本地文件损毁之后仍然保持备份，减少了因为操作不当丢失代码的可能性。

Git 的常用命令分为本地和远程两部分。本地命令主要是在本地进行对代码的版本控制和分支，远程命令则包含了与远程存储库通信的方法。

| Git 本地命令 | 用途                                                         |
| ------------ | ------------------------------------------------------------ |
| git init     | 在当前的工作目录下新建 git 存储库，内容会保存在.git 文件夹下 |
| git add      | 将当前的工作目录中更改的文件暂存到缓冲区                     |
| git commit   | 将缓冲区中的文件提交，创建一个新的版本。提交信息和版本内容与之前的差异会被存储 |
| git status   | 查看当前缓冲区状态                                           |
| git log      | 查看 git 提交日志                                            |
| git branch   | 创建一个新的工作分支，方便独立的修改文件中的某一部分         |
| git merge    | 将两个分支合并，例如在修改完 bug 之后将代码合并回主分支      |
| git checkout | 在不同的分支或提交之间切换                                   |
| git diff     | 查看当前工作区和最近一次提交之间的差异                       |

| git 远程命令 | 用途                                       |
| ------------ | ------------------------------------------ |
| git clone    | 将远程仓库中的内容克隆到本地               |
| git pull     | 从远程仓库获取更新并合并到本地             |
| git push     | 向远程仓库提交更新                         |
| git fetch    | 从远程仓库获取更新，但不与本地分支进行合并 |
| git remote   | 查看远程仓库                               |

## GitHub

GitHub 是一个基于 Git 的远程代码托管仓库，目前由微软公司所有。它提供了一个用户友好的 Web 界面，让开发者可以在浏览器中轻松地管理 Git 仓库。GitHub 不仅提供公共仓库，让开发者可以共享和协作开源项目，还提供私有仓库，供团队内部协作使用。GitHub 的功能包括：代码审查、问题跟踪、团队协作工具、代码托管等。

Git 和 GitHub 的关系在于，GitHub 是一个基于 Git 的服务。Git 本身是一个独立的分布式版本控制系统，可以在本地和任何远程服务器上使用，而 GitHub 则为 Git 提供了一个便捷的在线平台，使得开发者可以轻松地共享、协作和托管代码。

## Git 与 GitHub 实际使用练习

我们小组在讨论时尝试创建了一个 GitHub 仓库，用于协作本次讨论报告和之后的报告。
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162333.png)

在使用这个存储库时，首先使用 `git clone` 命令将 GitHub 上的远程存储库克隆至本地。并使用 `git log` 命令查看提交记录。
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162342.png)

由此可见这个存储库本地共有两个分支（main, swt），远程也有两个分支（origin/main, origin/swt）。最近一次提交是由 WeitangSun 在 4 月 23 日 16:57 进行的。

然后在本地对文件进行修改和编辑。例如，在本地向 LinuxDiscusstion6. md 文件添加一些讨论内容，然后使用 `git status` 查看本地仓库的状态。
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162353.png)

可以发现 git 发现了我们对该文件的修改。使用 `git diff` 命令具体查看修改内容。
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162403.png)
使用 `git add` 命令将修改暂存至缓冲区，然后使用 `git commit -m "xxx"` 命令提交更改。
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162412.png)
可以发现，git 为我们的提交自动生成了一个唯一的 SHA 值，用于唯一确定提交记录。并且它提示了我们这次提交的修改内容（改变一个文件，新增 17 行)。

最后，使用 `git push` 将我们的修改结果推送到远程仓库。发现此时出现了报错，通过阅读报错信息我们发现，这是由于没有同步远程仓库的更改造成的。因此我们首先使用 `git pull` 命令拉取远程仓库的修改并和本地仓库合并（`git pull` 命令是 `git fetch` 和 `git merge` 的合并）。

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162422.png)
使用 `git log` 命令查看提交，发现经过 pull 之后远程仓库和本地仓库的 main 分支已经合并。

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162432.png)
此时再进行 `git push`，发现已经能够向远程仓库提交。


![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162450.png)
在 GitHub 上查看仓库，发现已经更新至最新一次提交。

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_LinuxDiscussion6_20230624162441.png)
最后，我们将小组之前所有的报告上传至 GitHub，准备用于之后的报告写作。

## Git 冲突处理

在多人协作项目时，常常会碰到的一个场景是两个人同时对同一个文件进行了不同的修改。

### 使用 merge 

1. 两人同时修改一个文件，一个添加行，一个修改另一行 pull时merge auto merge use 'ort' strategy 不需要单独处理冲突
2. 两人修改了同一行显示冲突
3. 两个分支一个删除文件，另一个对其进行修改，merge时保留修改的文件，但需要进一步确认
4. 两个分支其中一个修改了文件的第一行和最后一行，另一个修改了第一行和倒数第二行，merge后git 两处修改的版本均被保留（倒数两行的修改被识别为一处）需要手动处理冲突后 git add、git commit 冲突的文件之后方可切换至其他分支进行操作。

### 在分支上创建/修改文件后未保存造成的影响

1. 在某一在分支中创建文件后未保存（未add+commit) 就checkout 到其他分支，可以在新的分支上正常add commit 未保存的文件。
2. 在某一分支中修改文件后未保存（未add+commit），再checkout 到其他分支，则所进行的修改丢失。* 修改文件后务必记得保存文件/*



# TBTools

## TBtools简介 

TBtools是一个着重界面化、图形化的软件，内置了许多的生物信息工具。TBtools将Linux命令转化为简单的界面交互，让没有写代码经验的同学也能轻松获得生物信息数据；此外它集成了众多功能，能让使用者连续地完成一套工作。

TBtools使用指南 https://www.yuque.com/cjchen/hirv8i

## TBtools功能

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image002.jpg)

官方使用指南的TBtools功能一览

分为五个板块：序列工具，Blast比对，GO和KEGG分析，作图，以及其他工具

## TBtools应用：建立系统发育树

以这篇文献的数据为例：文献研究的是PR-1基因族，是小麦基因组中病程相关的一系列基因，在NCBI上可以找到其对应的基因accession ID为HQ541961至HQ541981，相应的氨基酸序列ID为AEH25616至AEH25636，共42个序列，如果从NCBI手动下载较为麻烦，这里用TBtools的序列下载功能。

### 序列下载

TBtools-Sequence Toolkit菜单下找到NCBI Sequence Fetch，其中NCBI Sequence Downloads可以从NCBI上下载序列。

下载序列一共只需三步：在左侧输入accession ID list，右侧选择文件格式（默认fasta格式），最后选择输出文件地址并start即可，非常方便。TBtools会自动将序列合并到一个文件。

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image006.png)

得到文件如下所示

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image008.png)

### 建立ML树

使用TBtools的搜索功能，搜索tree，找到One Step Build a ML Tree

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image010.png)

选择下载的氨基酸序列，设置输出地址，点击start开始建树

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image012.png)

得到简单的系统发育树

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image014.jpg)

在输出目录下还能够查看树文件(.treefile)和多序列比对文件(.fa)

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image016.png)

# RNA-seq 数据分析

## RNA-seq基本概念

* 不同的建库方法可以通过“采样”的方式获得细胞中不同的“样本”数据

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230622183556.png)

### 数据获取

数据的获取有很多方式，GEO database是属于NCBI的转录组数据库（Gene Expression Omnibus），TCGA是属于NIH的人类肿瘤数据库，包括很多肿瘤（处理、对照）转录组数据，数据可以通过SRA-EBI-DDBJ数据库进行访问。


![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/TCGA.png)

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/GEO.png)


### RNA-seq的前期准备


参考文章： *<u>A survey of best practices for RNA-seq data analysis</u>*

## RNA-seq上游分析流程

该文档主要演示一种可以用来质控、比对、定量的过程，OS：`Ubuntu 18.04.6 LTS`，`Conda`环境

### 准备工作

* 数据的下载

  获取`RNA-seq`数据有很多方式，本试验采用NCBI的方式，通过搜索Bio-Project可以较为简便地获取感兴趣的测序数据，也可以通过查询相关`Pubmed`文献查看其raw data获取网址直接下载，TCGA等平台也有针对某些组织或实验方法的测序数据，但部分需要申请和额外权限。

  

我们使用以下这篇文献的原始实验数据：

![paper](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623220906.png)

  根据文章底部提供的原始数据网址搜索GEO（Gene Expression Omnibus）即可得到全部[原始数据](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE153637)：

![GEO_data](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623220733.png)

  由于NCBI下载数据需要`sratools`（安装麻烦，也不好使），我们选择使用直接在由EBI维护的ENA数据库上提供免费的数据维护源下载，具体方法是根据GEO页面上的SRA代号即可，我们通过勾选相应的field即可显示可以直接下载的连接（建议安装一些高级的下载软件，比如aria2、aspera等协议工具等），也可以直接：

  ```bash
wget -nc -c -b ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR585/003/SRR5859403/SRR5859403.fastq.gz
  ```

  这样就可以下载得到原始的`fastq.gz`文件了。

* 软件的安装

  处理RNA-seq数据需要一些基础的软件，本次主要用到了`cutadapt`（去接头和低质量序列），`fastqc`（可视化质控），`STAR`（比对）以及`stringtie`（转录本定量）

  我们通过`conda`安装这些软软件，注意你的源的排序，首先检查：

  ```bash
  conda config --show channels
  ```

  其中应包括`bioconda`源，没有就手动添加：

  ```bash
  conda config --add channels bioconda
  ```

  包含源后，创建新环境专门用于处理RNA-seq数据：

  ```bash
  conda create -n rnaseq
  conda activate rnaseq
  ```

  之后安装最新版的需要的软件即可（不考虑相应的依赖版本，让`conda`自行解决）：

  ```bash
  conda install cutadapt STAR fastqc stringtie
  ```

  分别运行这些软件的帮助指令，如果都未出现`command not found`，那么就是环境配置成功了。

### 质控

质控过程主要通过你的需求关注`fastqc`软件为你提供的质量报告，一般我们普通的RNA-seq需要关注的是测序质量随测序位置的分布、序列接头，以方便在后续进行序列的去低质量序列和接头序列。

* 我们使用`fastqc`进行质检：

  ```bash
  gunzip SRR12129121_1_fastq.gz
  fastqc -o . SRR12129121_1.fastq
  ```

  关于`fastqc`的报告的其他解读，可以参考该篇[CSDN博客](https://blog.csdn.net/win_win223/article/details/127923307)。

  产生两个文件，其中一个是超文本html文件，我们可以通过在windows/mac端的shell运行：

  ```bash
  scp <用户名>@<ip>:<路径>/SRR12129121_1_fastqc.html <本地路径>
  ```

  下载到本地，再用默认浏览器打开，这里也推荐使用`Filezilla`的协议传输。

  检查结果：

  * 序列的质量情况

![data_quality](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623220946.png)

  * 序列的接头情况

![adapter](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623221018.png)

  > 这里需要注意的是，不同的测序平台（platform）或建库方式可能使用不常用的接头，`fastqc`只能检测到Illumina平台常用的接头，需要注意对于公共数据我们需要针对GEO Data Processing栏关注文献的处理方法，也可以查看文献的Supplementary进行确定。
  >
  > 除接头外，有时还需要注意序列的长度分布信息，方便之后的比对工作。

* 对序列进行去低质量序列和接头序列

  对于低质量序列和接头序列，都可以使用`cutadapt`软件进行去除：

  `cutadapt`的功能很强大，具体的可以查看[官方说明文档](https://cutadapt.readthedocs.io/en/stable/)。

  此处由于该样本没有需要去除的末端低质量序列和接头序列，只使用命令进行一次演示，下面的指令同时进行了3’端接头序列（Trueseq 2 Universal Adapter）和末端低质量序列的去除：

  ```bash
  cutadpat -a AGATCGGAAGAGCACACGTCTGA -O 13 -e 0.1 -m 10 -q 10 -o SRR12129121_1.no_adapt.fastq -j 10 SRR12129121_1.fastq
  ```

  这是一种单端去除的方法，解读：

  * `-a`，制定adapter的序列
  * `-O`，制定最小的overlap的长度，一个adapter必须与序列中的末端有13个碱基的一致才会被剪切
  * `-e`，最大错误率
  * `-m`，剪切后最短的read长度
  * `-q`，去除末端质量<10（Phred Score）的序列
  * `-o`，输出fastq
  * `-j`，制定运行的线程，推荐8以下

  对于双端测序还可以使用pair-end同时去除的方法，具体的指令是加入一个`-o <output_1> -p <output_2> <input_1> <input_2>`的格式，所有的选项对应大写，具体见官方文档`Trimming  Paired-end Reads`

### 比对

基于参考基因组的比对的目的是对于得到的mRNA读段，需要知道这些细胞中抽样的结果来自怎么样的一个总体，这个总体的一个针对某些量的分布是什么样的，例如普通的RNA-seq表达分析实验关注转录本的Read Counts，或者关注TPM和FPKM，这样的指标反应的是转录本/基因的表达水平分布，这个过程就需要我们将Read与相应的已经注释的参考基因组片段对应起来。

* 参考基因组及注释

  比对之前需要先下载参考基因组：可以从`genecode`找到对应版本下载，注意UCSC版本：

  ```bash
  wget -bc https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_19/GRCh37.p13.genome.fa.gz
  wget -bc https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_19/gencode.v19.annotation.gtf.gz
  ```

  下载完成后`gunzip`解压。

* STAR Library Buildup

  在比对之前还有一个工序就是建索引，可以理解为将参考基因组提取信息并将每个contig进行拆分和建立标签，这样帮助一个Fragment快速选择对应拆分好的片段的种子再进行精细比对，加速了整个步骤，建库方法：

  ```bash
  nohup STAR \
  --runMode genomeGenerate \
  --runThreadN 18 \
  --genomeDir ./hg19 \
  --genomeFastaFiles ./GRCh37.p13.genome.fa
  --sjdbGTFfile ./gencode.v19.annotation.gtf
  --sjdbOverhang 99
  ```

  解释：

  * `--runMode`制定是建索引
  * `--sjdbOverhang`对应的是你的读段的长度-1最为合适，之前用`fastqc`检查的结果是100，这里用99最合适

  > 建索引过程比较漫长，但同一次实验一般使用同一个版本的注释及参考基因组类型，所以建索引过程一般只需要进行一次。

* STAR Alignment

  采用与原文献处理方式相同的工具`STAR`进行比对，STAR是一款RNA-seq比对工具，对于一般的转录本剪切位点有比对有较强的的灵敏度，具体的使用方法和问题解决可以参考[官方Manual](https://github.com/alexdobin/STAR/blob/master/doc/*STARmanual*.pdf)。

  这里使用双端比对的方式：

  ```bash
  nohup STAR \
  --runThreadN 18 \
  --outFilterType BySJout \
  --outSAMtype BAM SortedByCoordinate \
  --genomeDir ./hg19 \
  --readFilesIn SRR12129121_1.no_adapt.fastq SRR12129121_2.no_adapt.fastq \
  --outBAMsortingThreadN 4 \
  --outFileNamePrefix SRR12129121. &
  ```

  这是一个比较基础的比对方式，解释：

  * `–-outFilterType`根据splice junction对读段进行过滤，目的是筛去那些匹配不上剪切点的read
  * `--outSAMtype`，以bam输出并按坐标排序，后续软件需要
  * `--readFilesIn`，指定两个文件就是双端比对

* 比对结束后，可以通过检查`SRR12129121.Log.final.out`来检查比对结果：

  ```tex
                                   Started job on |       Jun 05 12:25:54
                               Started mapping on |       Jun 05 12:28:34
                                      Finished on |       Jun 05 12:33:30
         Mapping speed, Million of reads per hour |       355.49
  
                            Number of input reads |       29229536
                        Average input read length |       200
                                      UNIQUE READS:
                     Uniquely mapped reads number |       27271039
                          Uniquely mapped reads % |       93.30%
                            Average mapped length |       199.44
                         Number of splices: Total |       24816097
              Number of splices: Annotated (sjdb) |       24666103
                         Number of splices: GT/AG |       24637797
                         Number of splices: GC/AG |       154235
                         Number of splices: AT/AC |       17637
                 Number of splices: Non-canonical |       6428
                        Mismatch rate per base, % |       0.21%
                           Deletion rate per base |       0.01%
                          Deletion average length |       1.72
                          Insertion rate per base |       0.01%
                         Insertion average length |       1.41
                               MULTI-MAPPING READS:
          Number of reads mapped to multiple loci |       1501442
               % of reads mapped to multiple loci |       5.14%
          Number of reads mapped to too many loci |       4348
               % of reads mapped to too many loci |       0.01%
                                    UNMAPPED READS:
    Number of reads unmapped: too many mismatches |       0
         % of reads unmapped: too many mismatches |       0.00%
              Number of reads unmapped: too short |       449083
                   % of reads unmapped: too short |       1.54%
                  Number of reads unmapped: other |       3624
                       % of reads unmapped: other |       0.01%
                                    CHIMERIC READS:
                         Number of chimeric reads |       0
                              % of chimeric reads |       0.00%
  ```

### 定量

原文采用的方式是使用`R`的`GenomicAlignments`包中的`summarizeOverlaps`函数，该函数可以通过简单的外显子/基因水平计数得到raw counts：

```R
se <- summarizeOverlaps(exbygene, reads, mode="IntersectionNotEmpty")
```

这里我们使用`stringtie`同样可以得到raw counts，方法是首先运行`stringtie`：

```bash
stringtie SRR12129121.Aligned.sortedByCoord.out.bam \
-o SRR12129121.gtf \
-G gencode.v19.annotation.gtf \
-c 0.01 -m 100 -e
```

解释：

* `-c`，对应于每个转录本最少的覆盖度，至少要0.01
* `-m`，对应于transcript最小的比对结果长度，小于100的会被抛弃
* `-e`，只使用注释文件中的转录本，不推测新的

使用`stringtie`自带的脚本`prepDE.py`（位置一般在`<conda目录>/pkgs/stringtie-<版本>/bin`下），可以这样运行：

```bash
python prepDE.py \
-i sample_list.txt \
-g gene_count_matrix.csv \
-t transcript_count_matrix.csv
```

这里需要制定`sample_list.txt`，第一列为样本名称，第二列为定量的GTF文件路径：

```
Control_1 SRR12129121.gtf
```

运行后就得到了raw counts矩阵。

```bash
cat gene_count_matrix.csv | head
gene_id,SRR12129121
ENSG00000223972.5|DDX11L1,5
ENSG00000243485.5|MIR1302-2HG,3
ENSG00000284332.1|MIR1302-2,0
ENSG00000237613.2|FAM138A,0
ENSG00000268020.3|OR4G4P,0
ENSG00000240361.2|OR4G11P,0
ENSG00000186092.7|OR4F5,0
ENSG00000238009.6|ENSG00000238009,75
ENSG00000241860.7|ENSG00000241860,90
```

用`Excel`打开如下：

![excel_data](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623221109.png)

## RNASeq 下游分析

### 差异表达基因分析

本次实验的数据来源是GEO数据库中GSE153637所标识的数据。这是一篇2021年发表在Nature Cancer上，研究RNA结合蛋白在白血病中的作用的文章。

Prieto, C., Nguyen, D.T.T., Liu, Z. et al. Transcriptional control of CBX5 by the RNA-binding proteins RBMX and RBMXL1 maintains chromatin state in myeloid leukemia. Nat Cancer 2, 741–757 (2021).

https://www.nature.com/articles/s43018-021-00220-w#citeas

本次我们使用DESeq2包进行差异表达基因分析，首先我们需要对数据进行预处理。

```R
library(tidyverse)
library(pheatmap)
library(DESeq2)
library(readxl)
library(EnhancedVolcano)
```

```r
rna_seq <- read_excel(
    "RNAseq.xlsx",
    sheet = "Readcounts"
)
```

简单观察数据，可以看到实验中共有三组，control, shRNA1, shRNA2，每组一共四个样本，一共是12个样本。样本中基因的数目都是32866个基因。

```R
print(dim(rna_seq))
print(head(rna_seq))
print(tail(rna_seq))
```

#### 基因ID转换

事实上，在表达量定量一步，我们常常使用的是人类基因组的注释文件(gtf格式)作为输入。这些文件几乎都是使用Ensemble ID作为基因的ID，而在差异表达分析中，我们常常需要使用基因的名字(Gene Symbol)作为分析的基础。因此，我们需要将基因的ID转换为基因的名字。这里我们使用的数据已经经过作者的ID转换，因此我们不需要再进行ID转换了。

Ensemble ID的格式是ENSGxxxxxxxxx.x，其中ENSG后面跟的数字是基因唯一的ID，.号后面的数字是版本号。

其实ID转换的方法有非常多，许多生物信息网站上都有ID转换的功能，在R语言中也可以进行ID转换。例如可以使用ClusterProfiler包中的bitr函数进行ID转换。

```R
library(clusterProfiler)
library(org.Hs.eg.db)
genid <- bitr(rna_seq$Geneid, fromType = "ENSEMBL", toType = "SYMBOL", OrgDb = org.Hs.eg.db)
```

#### 数据预处理

对数据进行一定的预处理，首先提取出基因名字，然后将基因名字作为行名，将样本名字作为列名。然后筛选掉部分不是基因的和表达量非常低的基因。

```R
rna_seq <- rna_seq %>%
    mutate(Gene = ...1) %>%
    dplyr::select(-...1) %>%
    group_by(Gene) %>%
    summarise_all(sum) %>% # 这里的sum是对每一列进行求和，其实这个矩阵应该是已经整理好的不用。
    as.data.frame()

rownames(rna_seq) <- rna_seq$Gene
rna_seq <- rna_seq[, -1] # remove Gene column
print(head(rna_seq))
rna_seq <- rna_seq[-c(1:20), ] # 去掉前20行，这里面是一些不是基因的东西
rna_seq <- rna_seq[rowSums(rna_seq) >= 10, ] # 去掉在所有样本里面表达量小于10的基因
```


从样本名中提取出实验处理分组信息用于DESeq2的差异表达分析

```R 
id <- colnames(rna_seq)
id <- as_tibble(id)
id <- id %>%
    mutate(type = str_extract(value, "(control|shRNA[12])"))
```

```R 
print(id)
```

使用DESeq2进行表达分析，得到差异表达基因的结果，然后对结果进行可视化，得到差异表达基因的火山图和热图。

```R
dds <- DESeqDataSetFromMatrix(
    countData = rna_seq,
    colData = id,
    design = ~type
)
dds$type <- relevel(dds$type, ref = "control")
dds <- DESeq(dds)
```

#### 数据质控

在生物信息学分析中，数据质控是非常重要的一步。在RNAseq下游分析中我们也常常通过一些方法对数据进行质控。首先我们可以用PCA和聚类的方式对数据进行观察，看看数据是否有异常，例如是否有明显的分组，是否有明显的异常样本等等。

首先我们观察被敲除的RBMX基因的表达量，可以看到被敲除的基因的表达量都是非常低的，这也是符合预期的。

```R
plotCounts(dds, gene = "RBMX", intgroup = "type")
```

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623222226.png)

之后我们可以使用PCA的方式对数据进行观察，可以看到数据的分组有点点问题

```R
library(PCAtools)
vst <- rna_seq
meta <- data.frame(row.names = colnames(rna_seq), type = id$type)
p <- pca(vst, metadata=meta, removeVar = 0.05)
biplot(p)
```

 ![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623222259.png) 

```R
res1 <- results(dds, name="type_shRNA1_vs_control")
res2 <- results(dds, name="type_shRNA2_vs_control")
```

使用EnhancedVolcano包绘制差异基因表达的火山图，使用pheatmap包绘制差异基因表达的热图。

```R
EnhancedVolcano(res1,
    lab = rownames(res1),
    x = "log2FoldChange",
    y = "pvalue"
)
EnhancedVolcano(res2,
    lab = rownames(res2),
    x = "log2FoldChange",
    y = "pvalue"
)
```

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623222318.png)
![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623222326.png)

```R
res_filtered <- res2 %>%
    as.data.frame() %>%
    rownames_to_column("Gene") %>%
    filter(padj < 0.05 & abs(log2FoldChange) > 1)
dim(res_filtered)
print(head(res_filtered))
```

```R
differential_genes <- res_filtered$Gene
diff_matrix <- rna_seq[sample(differential_genes, 50), ]
RBMX <- rna_seq["RBMX",]
diff_matrix <- rbind(diff_matrix, RBMX)
pheatmap(diff_matrix, scale = "row")
ggsave("heatmap.png", width = 10, height = 10)
```

```R
diff_matrix <- rbind(diff_matrix, RBMX)
meta <- data.frame(row.names = colnames(diff_matrix), type = id$type)
p <- pca(diff_matrix, metadata=meta, removeVar = 0.05)
biplot(p)
```

### GO富集分析

使用ClusterProfiler包进行GO富集分析，使用enrichplot包绘制GO富集分析的结果。这几个R包都是由南方医科大学余光创课题组开发的，可以在Bioconductor中找到。

https://yulab-smu.top/


```R
library(clusterProfiler)
library(org.Hs.eg.db)
```

使用enrichGO函数进行GO富集分析，使用dotplot函数绘制GO富集分析的结果。

```R
#gene ontology
go_bp <- enrichGO(gene = differential_genes,
                  OrgDb = org.Hs.eg.db,
                  keyType = "SYMBOL",
                  ont = "BP", # BP: biological process, MF: molecular function, CC: celluar component, ALL
                  pAdjustMethod = "BH",
                  pvalueCutoff = 0.05,
                  qvalueCutoff = 0.05)
dotplot(go_bp, title = "GO Biological Pathway", showCategory = 10)

```

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_RNA-seq_20230623222357.png)

# Bioinformatics databases



生物信息数据库集合庞大而繁杂，根据不同的需求，可以通过元数据库（数据库的数据库）来寻找所需要的数据库。本文主要基于ABC（Applied Bioinformatics Course）网站：[链接](http://abc.gao-lab.org/databases.php)，以及CNCB，Database Commons（A catalog of worldwide biological databases）

本文主要以LEB课程上使用过的数据库作为参考，整理总结相关数据库的信息以及使用方法。

## UniProt

### UniProt简介

`UniProt`是Universal Protein的英文缩写，是信息最丰富、资源最广的蛋白质数据库，它由整合Swiss-Prot、TrEMBL和PIR-PSD三大数据库的数据而成。它的数据主要来自于基因测序项目完成后，后续获得的蛋白质序列，同时包含了大量来自文献的蛋白质的生物功能信息。

其子库结构如下：

| 数据库名             | 全名                                      | 用途                                         |
| -------------------- | ----------------------------------------- | -------------------------------------------- |
| UniProtKB/Swiss-Prot | Protein knowledgebase                     | 高质量、手工注释的、非冗余的数据库           |
| UniProtKB/TrEMBL     | Protein knowledgebase                     | 自动翻译蛋白质序列，预测序列，未验证的数据库 |
| UniParc              | Sequence                                  | 非冗余蛋白质序列数据库                       |
| UniRef               | Sequence clusters                         | 聚类序列减小数据库，加快搜索的速度           |
| Proteomes            | Protein sets from fully sequenced genomes | 为全测序基因组物种提供蛋白质组信息           |

Swiss-Prot由Amos Bairoch博士在1986年创建，由瑞士生物信息学研究所开发，EBI和SIB共同协作了Swiss-Prot和TrEMBL数据库。

来自相同基因和相同物种的序列合并到相同的数据库条目中，确定序列之间的差异：可变剪接、自然变异、错误的起始位点、错误的外显子边界、移码、未识别的冲突，科学文献中的注释包括但不限于：

- 蛋白质和基因名称
- 功能
- 特定于酶的信息，例如催化活性，辅因子和催化残基
- 亚细胞定位
- 蛋白质相互作用
- 表达方式
- 重要域和站点的位置和角色
- 离子，底物和辅因子结合位点
- 通过自然遗传变异，RNA编辑，替代剪接，蛋白水解加工和翻译后修饰产生的蛋白质变异形式

### UniProt常用查询

UniProt支持直接使用浏览器进行UniProtKB的查询，可以通过直接输入感兴趣的蛋白质名称来搜索

![image-20230630172214299](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172214299.png)

* 搜索位：搜索感兴趣的基因名、蛋白质（UniProt-ID，唯一）或感兴趣的组织、疾病、功能、家族、物种等等

* 筛选栏：

  * Status：数据库来源，Review的即只显示Swiss-Prot，Unreviewed即只显示TrEMBL
  * Popular Organisms：组织、物种
  * Taxonomy：系统分类，通过系统树筛选
  * Proteins with：属性，包含文献属性以及差异区分属性

* 结果栏：

  * 依次为：UniProt-ID，数据库命名，蛋白质学名，基因名，所属组织以及常用名，长度

    可以通过Customize columns自定义显示结果

* 下载：Download进行下载，下载成`FASTA`文件格式，默认对所有搜索结果进行下载为一个文件

### UniProt下载结果

通过Download键下载，文本以直接`url`生成给用户，下载完成后，打开`FASTA`格式文件：

```text
 >sp|Q01860|PO5F1_HUMAN POU domain, class 5, transcription factor 1 OS=Homo sapiens OX=9606 GN=POU5F1 PE=1 SV=1
  MAGHLASDFAFSPPPGGGGDGPGGPEPGWVDPRTWLSFQGPPGGPGIGPGVGPGSEVWGI
  PPCPPPYEFCGGMAYCGPQVGVGLVPQGGLETSQPEGEAGVGVESNSDGASPEPCTVTPG
  AVKLEKEKLEQNPEESQDIKALQKELEQFAKLLKQKRITLGYTQADVGLTLGVLFGKVFS
  QTTICRFEALQLSFKNMCKLRPLLQKWVEEADNNENLQEICKAETLVQARKRKRTSIENR
  VRGNLENLFLQCPKPTLQQISHIAQQLGLEKDVVRVWFCNRRQKGKRSSSDYAQREDFEA
  AGSPFSGGPVSFPLAPGPHFGTPGYGSPHFTALYSSVPFPEGEAFPPVSVTTLGSPMHSN
```

- **sp**：Swiss-Prot数据库的简称，也就是上面说的验证后的蛋白数据库

- **Q01860**：UniProt ID号

- **PO5F1_HUMAN**：是UniProt 的登录名

- **POU domain, class 5, transcription factor 1**：蛋白质名称

- **OS=Homo sapiens**：OS是Organism简称，Homo sapiens为人的拉丁文分类命名，也就是这是人的蛋白质

- **OX=9606**：Organism Taxonomy，也就是物种分类数据库Taxonomy ID

- **GN=POU5F1**：Gene name，基因名为POU5F1

- **PE=1**：Protein Existence，蛋白质可靠性，对应5个数字，数字越小越可靠：

- - 1：Experimental evidence at protein level
  - 2：Experimental evidence at tranlevel
  - 3：Protein inferred from homology
  - 4：Protein predicted
  - 5：Protein uncertain

- **SV=1**：Sequence Version，序列版本号

### 其他用途

当我们需要将测序得到的蛋白质确定其信息，或查询有关蛋白质的同源或相似蛋白质时，可以使用BLAST工具。当然UniProt本身内置了BLAST工具，在查询时选择对应的蛋白序列，点击BLAST即可在全数据库范围内查询相似或同源的序列：

![image-20230630172226908](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172226908.png)

必须提到的是，除UniProt本身提供的BLAST功能以外，这里不得不提到NCBI的BLASTP工具，可以对未知的蛋白质序列比对到已知的蛋白质数据上：

![image-20230630172234030](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172234030.png)

这里面选择UniProtKB/Swiss-Prot作为比对数据库，就可以将输入的数据向UniProt进行BLAST，输出相应的UniProt条目。



## RefSeq

### RefSeq简介

RefSeq是NCBI制定的参考序列计划，为多种生物提供序列的数据信息及相关资料，用于医学、基因功能和基因功能比较研究。RefSeq数据库中的所有数据是一个非冗余的、提供参考标准的数据，包括染色体、基因组（细胞器、病毒、质粒）蛋白、RNA等。

*与Genbank开放数据库不同的是，Genbank对每个基因都含有许多序列，每天和EMBL和DDBJ交换数据，造成部分的重复；而RefSeq则被设计成每个人类位点的代表序列来去除冗余，由NCBI自行矫正。*

RefSeq的ID命名常用规范：

* `NM`：表示RefSeq的标准序列
* `XM`：预测的蛋白编码序列
* `NR`：非编码蛋白的mRNA序列

具体命名规范可见下表：

![img](https://www.genedenovo.com/uploads/tmp/jidiaoaa_977775msesther.jpg)

### RefSeq使用

RefSeq提供数据的下载传输服务，通过ftp可以下载：

![image-20230630172244249](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172244249.png)

查看历史版本的ftp目录可以找到所需对饮版本的基因组组装结果以及注释信息，可以看到最新版本的`GRCh38.p14`表示38版本的组装结果和第14版注释，共包含3.1Gb个碱基和59,444个基因。

除ftp协议直接访问外，RefSeq数据也可以通过NCBI BLAST进行访问，Entrez（NCBI在线资源检索器）进行访问。

NCBI页提供GDV（Genome Data Viewer），类似于一个IGV浏览器，但其上显示了染色体区域、基因组位置、注释包括可变剪接、基因、转录本、核酸多态性等多个对应数据：

![image-20230630172252573](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172252573.png)

总结而言，RefSeq的主要特性是去冗余性，以及蛋白质和核酸序列的明确对应，对于人工注释带来的包含序列信息和生物标签的数据、以及数据可信度。

## InterPro

### InterPro简介

InterPro数据库提供蛋白质序列的功能分析并归纳为蛋白质家族，以此预测presence of domains和important sites。InterPro使用了先验的模型，整合了不同数据库形成一个整体，以分类蛋白。

我们可以使用InterProScan来参考InterPro进行一些所需的蛋白质模体、结构域的查询。

Interpro 通过整合多个蛋白相关的数据库，提供了一个方便的对蛋白序列进行功能注释的平台，功能注释的内容包括蛋白质家族预测，domain 和 结合位点预测； Interpro 在整合多个数据库的同时，去掉了冗余，提供了一个统一的接口，用来对序列进行功能注释；而且还提供了interproscan 这样的命令行工具，可以方便的对大规模的蛋白序列进行注释，目前， UniprotKB 和 Gene Ontology 就是使用 interproscan 对蛋白序列进行注释的。

### InterPro使用

我们使用InterPro的官网进行数据访问：[链接](http://www.ebi.ac.uk/interpro/)

将序列粘贴到输入框直接进行注释即可，点击Search，等候数据库的事务处理队列执行完毕，既可以查看结果。一个简单的（如图所示）的序列搜索时间大概在2-3分钟左右：

![image-20230630172303232](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172303232.png)

这里很奇怪，使用InterPro的官网案例序列搜索不到相应的蛋白质序列，猜测官网数据可能只是用于数据库搜索的格式范例，但至少我们得知了数据输出的形式包括：

* 序列氨基酸长度
* Protein famliy，蛋白质家族信息，以及与该蛋白质同源异态的蛋白质序列
* InterPro GO条目
* PANTHER GO条目

除使用在线搜索工具以外，还可以使用EBI提供的脚本进行线下注释（本质上仍需要访问在线数据库，将数据结果返回本地），可以通过[链接](http://www.ebi.ac.uk/Tools/Webservices/services/pfa/iprscan5_rest)进行Perl脚本的下载。

输出结果：

```text
RUNNING
FINISHED
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.out.txt
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.log.txt
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.tsv.txt
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.xml.xml
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.htmltarball.html.tar.gz
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.gff.txt
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.svg.svg
Creating result file: iprscan5-R20160605-043400-0109-32295822-es.sequence.txt
```

### InterProScan使用

Interproscan，通过蛋白质结构域和功能位点数据库预测蛋白质功能。是EBI开发的一个集成了蛋白质家族、结构域和功能位点的非冗余数据库。Interproscan整合了一些使用最普及的一些数据库，并应用于功能未知的蛋白进行Interpro注释和GO注释。需要说明：*Proteins that have diverged from a common ancestral gene are known as [homologous](https://www.ebi.ac.uk/training/online/glossary/homologous)*，所以homologous就是祖先gene。

一个**基于Domain**的分析方法如下：

一个蛋白质包含多个Domain，例如包括：

![img](https://img2020.cnblogs.com/i-beta/1719714/202003/1719714-20200313104428576-1114482142.png)

这些具有不同功能的domains，将它们的功能assembly，一同完成一个大的行为：

![img](https://img2020.cnblogs.com/i-beta/1719714/202003/1719714-20200313105400761-1466709477.png)

InterPro通过将不同的Domain序列整合到已有的数据库中，预测可能得结构所具有的功能。

除此之外，InterProScan还可以通过**基于Sequence Feature**的方法进行搜索：例如酶的活性作用位点，基于蛋白质互作的蛋白质序列互作，以及化学修饰活性位点。

运行`iprscan`：

```
bin/iprscan -cli -iprlookup -goterms -format xml -i test.fasta -o test.out
```

`iprscan`的参数说明：

* `-cli`：设定程序在unix命令下运行，如果不设此参数，程序会被当作CGI程序运行。
* `-iprlookup`：结果里显示相应的interpro注释信息。
* `-goterms`：结果里显示相应的GO注释信息，但前面要加上-iprlookup参数。
* `-format`：输出结果的格式，有raw, xml, txt, html(default), ebixml(EBI header on top of xml) gff。
* `-appl`：数据库和扫描方法的的选择。无此参数表示默认选择全部数据库（配置Interpro scan时候设置的数据库）。选择多个数据库则需该参数多次。本地运行Interproscan相比官网运行，有两个数据库不能选择为tmhmm 和 signalp。这两个数据库的选择需要commercial license。
* `-i`：输入文件，InterProScan支持输入蛋白质序列和核酸序列，如果输入核酸系列，程序会将其翻译成蛋白质序列，你可以指定翻译用到的密码表，用下面的-trtable参数，序列格式可以是raw，Fasta或者EMBL。
* `-o`：结果输出文件，如果不选择此参数，结果将输出到标准输出，输出格式可以用下面的-format参数设定。
* `-trtable`：选择核酸翻译蛋白质的密码表，同时可以设定-trlen 参数来控制核酸翻译的转录子长度。
* `-nocre`：不对输入蛋白质序列进行crc64匹配。不加此参数，则默认是会对蛋白质序列开启了crc64匹配。Interpro数据库(memember database)已经包含了大量序列搜索的结果，就是如果你的蛋白序列已经包含在interpro的数据库里面，iprscan会直接给出搜索结果，无需进行本地运算。interpro数据库不包括tmhmm, coil和signalp，所以crc64匹配不到这3个数据库。一般情况下，由于commercial licese而无法使用
  tmhmm和signalp数据库，coil数据库也不会使用。故不使用此参数，加快程序运行速度。
* `-seqtype`：输入的序列类型，蛋白质序列（-seqtype p）（defult）或者（-seqtype n）。
* `-email`：设定一个Email地址，程序运行完毕向信箱发送邮件通知分析完毕。
* `-verbose`：程序运行过程中显示运行的状态。
* `-help`：显示帮助信息。

InterProScan其它附带的重要程序：

```text
meter.pl     reports the progress of a job.有百分之几的chunk已经运行完毕。
converter.pl 将raw的格式转换成其它的格式，比如html,xml，txt等格式。
iterator.pl  运用于逐条去注释序列
```

hmmpfam, hmmscan 和 hmmsearch 能多线程运行。经过测试 PIR superfamily 和 SUPERFAMILY 这两个数据库的应用比较费时，可以设置conf文件夹中的配置文件hmmpir.conf和superfamily.conf，将其中的cpu_opt的值设置高一些。默认是1。
InterProScan不能立马给出结果文件并相继把结果放入到结果文件中。可以采用逐条比对的方法来得到注释结果。可以采用如下的方法来随时终结掉程序，并拿到部分结果。

```bash
bin/iterator.pl -i test.fa -o test.out -c "bin/iprscan -cli -i %infile -iprlookup -goterms -format xml"
```

* 优点：使用本地化的数据库，在断网和计算机资源充足的情况下，能加快注释速度；本地化网页版能同时比对多条序列；本地化能对DNA序列进行interpro注释。
* 缺点：本地化安装InterProScan比较复杂耗时；需要不时更新本地数据库；本地化运行耗费计算资源大；



## KEGG

### KEGG简介

KEGG，全称Kyoto Encyclopedia of Genes and Genomes，是一个从分子水平信息，特别是基因组测序和其他高通量实验技术产生的大规模分子数据库，以了解细胞、有机体和生态系统等生物系统的高级功能和效用的数据库资源。自95年推出第一版以来，进行了陆续的丰富和更新。

### KEGG使用

KEGG提供在线蛋白质功能搜索工具，以缺氧诱导因子HIF家族为例，进行KEGG搜索：

![image-20230630172319572](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172319572.png)

点击KEGG PATH，即可查看通路信息，包括：

* KEGG Pathway的ID，名称
* 介绍，所属的通路家族
* 通路结构图
* 引用的文章，包括PMID，作者、名称以及链接DOI号
* 相关通路

![image-20230630172329965](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172329965.png)



## PlantTFDB

### PlantTFDB简介

由高歌实验组开发的植物转录因子数据库，包含植物转录因子的家族分类规则、基因转录因子全谱、注释、转录因子结合图谱（Binding Motifs），转录因子预测、系统发生树等，设计165个包括拟南芥、水稻、杨树、大豆、玉米、小麦等五种。

提供中文帮助文档：[链接](http://planttfdb.gao-lab.org/download/brief_guidance.pdf)

### PlantTFDB使用

数据库支持通过物种发生树（Browse By Species）和通过转录因子家族（Browse By Families）对转录因子进行搜索，也支持对转录因子核酸序列进行BLAST搜索。

![image-20230630172339142](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/image-20230630172339142.png)

数据库的条目包含转录因子的大量详细注释信息：

* 转录因子的数据库唯一ID
* 所属组织、系统ID、所属转录因子家族
* 蛋白质信息、分子量，基因结构
* Signature Domain，蛋白质结构域
* 蛋白质模体来源，包括Gene3D、SuperFamily、CDD、Pfam等数据库
* 序列信息、三维结构
* 功能简介，来自UniProt等数据库
* 基因注释，来自RefSeq、UniProt、STRING等数据库
* 同源信息

* 相关文献

# ChIP-Seq 数据分析

## 背景

ChIP-seq即 染色质免疫沉淀（Chromatin Immuno-Precipitation）测序，包括染色质免疫沉淀和二代测序两个部分。其中染色质免疫沉淀用于确定蛋白质和DNA的互作情况，主要用于研究转录因子(TF)和组蛋白。

生物体内，蛋白质与DNA存在各种互作，为了研究这些互作的结合位点，可以使用染色质免疫沉淀技术即ChIP。ChIP将甲醛与细胞作用，使细胞内的DNA和蛋白质进行交联形成复合物，随后将细胞裂解，分离染色体，用超声或酶将染色质切割。切割后，使用蛋白质的抗体将蛋白质-DNA片段复合物沉淀下来，随后去交联释放DNA，将DNA进行纯化得到目标片段。 

在已有测序文库的前提下，对得到的DNA片段进行高通量测序，质控后与参考基因组进行比对。由于结合位点上蛋白结合越多，获得的测序片段也就越多，因此在匹配结果上可以看到DNA片段的重合，形成峰(peak)，根据位点上的read数进行作图，这样可以得到ChIP-seq的峰图。随后可以对峰图进行生物阐释，如基因组注释、GO分析、motif查找等。

下图展示了ChIP-seq湿实验部分的大致流程。

![Figure 1](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image001.jpg)

## ChIP-seq实验流程（上游分析）

### 1.1 获取.fastq文件

​    本次实验使用的数据来自对转录因子CTCF的研究，CTCF是一种染色体结合蛋白，能够与基因组上多个位点结合来调控转录。服务器上已经准备了湿实验得到的样本的测序数据。文献数据的下载地址为https://www.encodeproject.org/experiments/ENCSR617IFZ/

### 1.2 fastqc质控

使用fastqc对测序质量进行评价:

fastqc ctcf_chip/ctcf_chip_input_r1.fastq.gz > output/ctcf_chip_input_r1_fastqc.html

打开html文件可以查看多个指标的测序质量，示例如下图所示

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image003.png)

### 1.3 cutadpat去接头

测序结果中有部分接头序列，为避免影响后续分析需要去除，可以使用cutadapt软件。

去除3’端接头:

```
cutadapt -a AACCGGTT -o output.fastq.gz input.fastq.gz
去除5’端接头:
cutadapt -g ADAPTER -o output.fastq.gz input.fastq.gz
```

### 1.4 bowtie2比对，得到sam文件并转化为bam文件

由于需要将读段与全基因组比对，因此需要使用长序列的比对算法，bowtie2是一种合适的方法

首先建立索引

bowtie2-build -f hg38_human.fa hg38_index > map_index

进行比对

bowtie2 -x map_index ctcf_input_r1.fastq.gz ctcf_input_r2.fastq.gz      

\> ctcf_input_map.sam

随后将sam文件转换为bam文件，可以使用samtools

以上流程获得的结果可以在文献数据中获得：

https://www.encodeproject.org/files/ENCFF354GWS/@@download/ENCFF354GWS.bam

### 1.5 MACS软件peak calling

使用MACS2程序可以对比对后的bam文件进行peak calling即峰富集：

macs2 callpeak ENCFF354GWS.bam -f > output

在文件目录下可以得到记录峰值的bam文件和narrowPeak文件

由于存在生物学重复，有多个bed文件，可以用bedtools进行merge

最终得到的bed文件也可以从文献数据中获得：

https://www.encodeproject.org/files/ENCFF285QVL/@@download/ENCFF285QVL.bed.gz

文献还提供了bigBed和narrowPeak文件，可以用IGV软件打开，结果如图所示：

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image005.png)

# Github Pages 建立个人网页

## GitHub Pages 简单介绍

GitHub Pages是GitHub公司提供的一项可用于建立个人网页的免费站点托管服务，它直接从GitHub上的仓库获取HTML、CSS和JavaScript文件，通过自动的构建过程运行文件，然后发布网站。

GitHub Pages提供一个`github.io`域名作为站点托管域名，假如个人拥有其他域名，也可以使用这些域名来托管站点。

GitHub Pages 的一个优势是它允许使用 Jekyll 站点生成器，并直接解析 Markdown 文件来创建网站。GitHub Pages 还提供了较多的默认主题，用户可以在这些主题中选择一个来建立一个漂亮的网页。

## 创建 GitHub Pages 站点

GitHub Pages 提供了几种方式来创建一个站点。首先，可以从一个已经有的存储库来创建一个站点。其次，可以在现有存储库上建立一个站点。我们小组选择通过创建一个新的存储库来创建站点。

首先，创建一个新仓库，以用户名来命名，仓库名为 `Purkinje-cell.github.io`，然后设置使用 README 初始化仓库。之后，单击“创建存储库”，辩可以新建一个存储库用于站点配置。

GitHub Pages 需要一个入口文件来创建站点首页，它会自动查找 `index.html`、`index.md` 或 `README.md` 文件，并根据其内容渲染页面。

通过这种方式，我们小组将之前所有的讨论报告上传至 GitHub 存储库中，然后使用 GitHub Pages 来进行站点创建，并将其设置为 Public 存储库（所有人可见）。然后，我们就可以通过 `Purkinje-cell.github.io` 这一域名，通过各大浏览器进行访问。

以下是存储库的示例图

![](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/scr_GithubPages_20230624155303.png)

## 使用 Jekyll 向 GitHub Pages 站点添加主题

Jekyll 是一个静态站点生成器，内置 GitHub Pages 支持和简化的构建过程。 Jekyll 使用 Markdown 和 HTML 文件，并根据您选择的布局创建完整静态网站。 Jekyll 支持 Markdown 的使用。在 Jekyll 的支持下，我们可以直接使用 Markdown 的语法，方便的创建网页。

Jekyll的官方网站是 [Jekyllcn.cn](jekyllcn.com)，它上面提供了详细的设置Jekyll的渲染方式的文档。我们根据文档和GitHub的指导，选用了jekyll-theme-cayman主题。以下是我们的主页情况。在上传原来讨论报告的过程中，我们还遇到的一个问题是原先讨论报告中插入的图片丢失的问题，后来我们选择了在存储库中添加img文件夹，保存需要用的图片的方式来解决。另外，还可以使用图床的方式，下面做简要的介绍。

## Markdown 图床配置

在使用 Markdown 撰写讨论报告时，我们发现了一个问题：Markdown 对于插入图片的支持比较复杂，需要指定本地图片的路径或网络图片的 URL。因此，我们上传至 Github 的 Markdown 报告中无法显示图片。我们搜索了 Markdown 图片支持的解决方案，发现图床是一种较好的方式。

**图床（Image Hosting)** 是一个在线存储和分享图片的服务。它允许用户将图片上传到图床服务器上，并通过生成的链接或嵌入代码将图片分享给其他人或展示在不同的平台上。图床为用户提供了一个方便、快捷的图片存储和分享方式，不需要担心存储空间和带宽问题。用户可以通过图床服务轻松上传、管理和分享图片，而不必担心占用自己的硬盘空间或消耗大量带宽。这对于网站制作、论坛交流、博客撰写等场景非常有用，因为图片通常占用很大的存储空间和带宽。使用图床服务，用户可以将图片托管到第三方服务器上，让访问者直接从图床服务器获取图片，减轻了原始服务器的压力。

这里我们根据网上的教程, 使用 GitHub 创建一个图床。[如何利用 Github 搭建自己的免费图床？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/347342082)

首先，在 Github 上新建一个仓库 Images（此处命名可以修改），并在 Settings -> Developer Settings -> Personal access tokens 里新创建一个 Token，用于 PicGo 软件直接使用 API 访问 Github。

![image.png](https://raw.githubusercontent.com/Purkinje-cell/Images/master/20230427160824.png)

之后，从 GitHub 上下载 PicGo 软件，并按照教程配置它的 Github 仓库名和分支名，并粘贴之前设置好的 Token。使用 PicGo 控制图片上传的好处是它能够较为方便的设置不同图床的上传，以及在上传后可以自动复制 Markdown 类型的链接到剪贴板，从而减小了工作量。

![image.png](https://raw.githubusercontent.com/Purkinje-cell/Images/master/20230427161524.png)

之后，将 Markdown 中图片的格式从引用本地文件改为引用 GitHub 上的 URL。再将 Markdown 上传到 Github，发现已经能够显示图片。

目前网上有许多免费的图床，如 SM. MS。或者可以使用腾讯云存储服务来进行图床的配置。使用腾讯云的好处是上传速度较快，不会遇到 GitHub 需要 VPN 的问题。