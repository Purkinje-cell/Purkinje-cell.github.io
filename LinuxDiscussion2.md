### Linux 生物技术基础课程小组讨论报告

组：LEB G03
次：第二次
组长：易鼎程
执笔：易鼎程

### 1. 时间

2023.3.16 下午 3 点-5 点

### 2. 方式

- 线下小组讨论

### 3. 主题

- PAM 和 BLOSUM 打分矩阵的构建
- `BLAST` 本地化安装和使用测试
- 国家生物中心新版 BLAST 网站使用测试

### 4. 内容

#### PAM 和 BLOSUM 打分矩阵的构建

- PAM：PAM（Point accepted mutation）矩阵是利用氨基酸点突变概率构建的蛋白质替换打分矩阵，它假设氨基酸的取代速率与分歧时间无关，而只与取代前后的两个氨基酸有关。PAM 矩阵描述了在给定时间内两个氨基酸之间发生替换的概率，这个时间被称为 PAM 距离。例如，PAM250 矩阵描述了在 250 个 PAM 距离内两个氨基酸之间发生替换的概率。
- BLOSUM：BLOSUM (BLOcks SUbstitution Matrix) 矩阵是基于氨基酸 blocks 的替换比例来计算单个氨基酸的替换分值。例如，BLOSUM62 矩阵是使用序列一致性高于 62%的序列来进行构建的。
- PAM 和 BLOSUM 打分矩阵的区别
	- PAM 后缀数字越大，用于表征两个序列的进化距离越远。而 BLOSUM 正好相反
	- PAM 矩阵的构建是对全长序列进行全局比对，而 BLOSUM 则是进行局部比对

#### `BLAST` 本地化安装和使用测试

按照 PPT 上的使用方法在服务器上安装 `BLAST` 软件

##### 在讨论中发现 PPT 上存在一些问题

###### 1. 某些箭头的标注有误，如下图中过滤期望值高于 1 的阳性结果应该指的是 evalue 那里
![](./img/scr_LinuxDiscussion2_20230624162117.png)
###### 2. PSI-BLAST 的命令为 `psiblast`，后面没有 p。而且 uniport 拼错了
![wq](./img/scr_LinuxDiscussion2_20230624162127.png)

- 按 PPT 上的方式对 ZMTF_PEP. FASTA 文件建立索引之后可以看到新生成了三个分别以 `.phr`, `.pin`, `.psq` 结尾的文件，我们希望研究它们各自的功能是什么，经过查询我们知道了：
	- `.phr` 文件：Protein Hash Recovery 文件，用于快速查询蛋白质序列的索引文件。 
	- `.pin` 文件：Protein Index 文件，也是用于加速蛋白质序列查询的索引文件，但它使用了不同的索引算法，适用于更大的序列数据集。 
	- `.psq`文件：Protein Sequence文件，是保存蛋白质序列本身的文件。

![](./img/scr_LinuxDiscussion2_20230624162142.png)
#### 国家生物中心新版 BLAST 网站的使用测试

##### 发现存在 BUG：使用 PAM250 打分矩阵会导致报错 No result 

如图所示使用 PAM250 矩阵，会导致结果中没有找到的匹配序列
![](./img/scr_LinuxDiscussion2_20230624162156.png)

使用 BLOSUM62/PAM70 矩阵可以找到相似序列，但其他的计分矩阵都会报错 No result


### 5. 问题

* `未解决` ：玉米转录因子蛋白质序列和编码区核苷酸序列数据在服务器上已经有了（ZMTF_PEP. FASTA/ZMTF_CDS. FASTA），但不清楚是怎么得到的？
	* 目前我们在 Uniprot 上面直接搜索，只能找到约 50 条记录
![](./img/scr_LinuxDiscussion2_20230624162228.png)
	 - 后来在 PlantTFDB 上我们找到了这些转录因子的数据，但是不清楚具体得到这些数据的方式。
![](./img/scr_LinuxDiscussion2_20230624162214.png)
玉米数据集，
