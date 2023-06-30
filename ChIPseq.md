# LEB G03 小组讨论报告-ChIP seq

组长：易鼎程

执笔：朱亦文

主题：ChIP-seq与实践

内容：ChIPseq数据分析

## ChIP-seq背景

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

 