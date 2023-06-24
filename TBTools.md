# TBTools

作者：朱亦文

LEB G03 小组讨论报告

组长：易鼎程

执笔：朱亦文

主题：TBtools软件的使用

内容：

**TBtools简介** 

TBtools是一个着重界面化、图形化的软件，内置了许多的生物信息工具。TBtools将Linux命令转化为简单的界面交互，让没有写代码经验的同学也能轻松获得生物信息数据；此外它集成了众多功能，能让使用者连续地完成一套工作。

TBtools使用指南 https://www.yuque.com/cjchen/hirv8i

**TBtools功能**

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image002.jpg)

官方使用指南的TBtools功能一览

分为五个板块：序列工具，Blast比对，GO和KEGG分析，作图，以及其他工具

**TBtools应用：建立系统发育树**

**![img](C:/Users/ydc2020/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)**

以这篇文献的数据为例：文献研究的是PR-1基因族，是小麦基因组中病程相关的一系列基因，在NCBI上可以找到其对应的基因accession ID为HQ541961至HQ541981，相应的氨基酸序列ID为AEH25616至AEH25636，共42个序列，如果从NCBI手动下载较为麻烦，这里用TBtools的序列下载功能。

**序列下载**

TBtools-Sequence Toolkit菜单下找到NCBI Sequence Fetch，其中NCBI Sequence Downloads可以从NCBI上下载序列。

下载序列一共只需三步：在左侧输入accession ID list，右侧选择文件格式（默认fasta格式），最后选择输出文件地址并start即可，非常方便。TBtools会自动将序列合并到一个文件。

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image006.png)

得到文件如下所示

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image008.png)

**建立ML树**

使用TBtools的搜索功能，搜索tree，找到One Step Build a ML Tree

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image010.png)

选择下载的氨基酸序列，设置输出地址，点击start开始建树

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image012.png)

得到简单的系统发育树

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image014.jpg)

在输出目录下还能够查看树文件(.treefile)和多序列比对文件(.fa)

![img](https://obsidian-image-1312618184.cos.ap-beijing.myqcloud.com/image/clip_image016.png)
