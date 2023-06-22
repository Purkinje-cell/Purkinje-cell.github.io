---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.5
  kernelspec:
    display_name: R
    language: R
    name: ir
---

# RNASeq 下游分析
## 1. 差异表达基因分析

本次实验的数据来源是GEO数据库中GSE153637所标识的数据。这是一篇2021年发表在Nature Cancer上，研究RNA结合蛋白在白血病中的作用的文章。

Prieto, C., Nguyen, D.T.T., Liu, Z. et al. Transcriptional control of CBX5 by the RNA-binding proteins RBMX and RBMXL1 maintains chromatin state in myeloid leukemia. Nat Cancer 2, 741–757 (2021).

https://www.nature.com/articles/s43018-021-00220-w#citeas

本次我们使用DESeq2包进行差异表达基因分析，首先我们需要对数据进行预处理。

```R vscode={"languageId": "r"}
library(tidyverse)
library(pheatmap)
library(DESeq2)
library(readxl)
library(EnhancedVolcano)
```

```R vscode={"languageId": "r"}
rna_seq <- read_excel(
    "RNAseq.xlsx",
    sheet = "Readcounts"
)
```

简单观察数据，可以看到实验中共有三组，control, shRNA1, shRNA2，每组一共四个样本，一共是12个样本。样本中基因的数目都是32866个基因。

```R vscode={"languageId": "r"}
print(dim(rna_seq))
print(head(rna_seq))
print(tail(rna_seq))
```

### (基因ID转换)
事实上，在表达量定量一步，我们常常使用的是人类基因组的注释文件(gtf格式)作为输入。这些文件几乎都是使用Ensemble ID作为基因的ID，而在差异表达分析中，我们常常需要使用基因的名字(Gene Symbol)作为分析的基础。因此，我们需要将基因的ID转换为基因的名字。这里我们使用的数据已经经过作者的ID转换，因此我们不需要再进行ID转换了。

ENSGxxxxxxxxx.x

其实ID转换的方法有非常多，许多生物信息网站上都有ID转换的功能，在R语言中也可以进行ID转换。例如可以使用ClusterProfiler包中的bitr函数进行ID转换。

```R vscode={"languageId": "r"}
#library(clusterProfiler)
#library(org.Hs.eg.db)
#genid <- bitr(rna_seq$Geneid, fromType = "ENSEMBL", toType = "SYMBOL", OrgDb = org.Hs.eg.db)
```

对数据进行一定的预处理，首先提取出基因名字，然后将基因名字作为行名，将样本名字作为列名。然后筛选掉部分不是基因的和表达量非常低的基因。

```R vscode={"languageId": "r"}
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

```R vscode={"languageId": "r"}
print(head(rna_seq))
print(dim(rna_seq))
```

从样本名中提取出实验处理分组信息用于DESeq2的差异表达分析

```R vscode={"languageId": "r"}
id <- colnames(rna_seq)
id <- as_tibble(id)
id <- id %>%
    mutate(type = str_extract(value, "(control|shRNA[12])"))
```

```R vscode={"languageId": "r"}
print(id)
```

使用DESeq2进行表达分析，得到差异表达基因的结果，然后对结果进行可视化，得到差异表达基因的火山图和热图。

```R vscode={"languageId": "r"}
dds <- DESeqDataSetFromMatrix(
    countData = rna_seq,
    colData = id,
    design = ~type
)
dds$type <- relevel(dds$type, ref = "control")
dds <- DESeq(dds)
```

### 数据质控

在生物信息学分析中，数据质控是非常重要的一步。在RNAseq下游分析中我们也常常通过一些方法对数据进行质控。首先我们可以用PCA和聚类的方式对数据进行观察，看看数据是否有异常，例如是否有明显的分组，是否有明显的异常样本等等。

首先我们观察被敲除的RBMX基因的表达量，可以看到被敲除的基因的表达量都是非常低的，这也是符合预期的。

```R vscode={"languageId": "r"}
plotCounts(dds, gene = "RBMX", intgroup = "type")
```

之后我们可以使用PCA的方式对数据进行观察，可以看到数据的分组有点点问题

```R vscode={"languageId": "r"}
library(PCAtools)
vst <- rna_seq
meta <- data.frame(row.names = colnames(rna_seq), type = id$type)
p <- pca(vst, metadata=meta, removeVar = 0.05)
biplot(p)
```

```R vscode={"languageId": "r"}
res1 <- results(dds, name="type_shRNA1_vs_control")
res2 <- results(dds, name="type_shRNA2_vs_control")
```

使用EnhancedVolcano包绘制差异基因表达的火山图，使用pheatmap包绘制差异基因表达的热图。

```R vscode={"languageId": "r"}
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

```R vscode={"languageId": "r"}
res_filtered <- res2 %>%
    as.data.frame() %>%
    rownames_to_column("Gene") %>%
    filter(padj < 0.05 & abs(log2FoldChange) > 1)
dim(res_filtered)
print(head(res_filtered))
```

```R vscode={"languageId": "r"}
differential_genes <- res_filtered$Gene
diff_matrix <- rna_seq[sample(differential_genes, 50), ]
RBMX <- rna_seq["RBMX",]
diff_matrix <- rbind(diff_matrix, RBMX)
pheatmap(diff_matrix, scale = "row")
ggsave("heatmap.png", width = 10, height = 10)
```

```R vscode={"languageId": "r"}
diff_matrix <- rbind(diff_matrix, RBMX)
meta <- data.frame(row.names = colnames(diff_matrix), type = id$type)
p <- pca(diff_matrix, metadata=meta, removeVar = 0.05)
biplot(p)
```

## 2. GO富集分析

使用ClusterProfiler包进行GO富集分析，使用enrichplot包绘制GO富集分析的结果。这几个R包都是由南方医科大学余光创课题组开发的，可以在Bioconductor中找到。

https://yulab-smu.top/

```R vscode={"languageId": "r"}
library(clusterProfiler)
library(org.Hs.eg.db)
```

使用enrichGO函数进行GO富集分析，使用dotplot函数绘制GO富集分析的结果。

```R vscode={"languageId": "r"}
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

```R vscode={"languageId": "r"}
differential_genes
```

```R vscode={"languageId": "r"}
go_bp
```

```R vscode={"languageId": "r"}
Gene Set Enrichment Analysis(GSEA)
```

```R vscode={"languageId": "r"}
res_filtered <- res2 %>%
    as.data.frame() %>%
    rownames_to_column("Gene") %>%
    filter(padj < 0.1 & abs(log2FoldChange) > 0.5)
differential_genes <- res_filtered$Gene
gsea_data <- res2[differential_genes, ] %>%
    as.data.frame() %>%
    rownames_to_column("Gene") %>%
    dplyr::select(Gene, log2FoldChange)
geneList <- gsea_data$log2FoldChange
names(geneList) <- gsea_data$Gene
geneList <- sort(geneList, decreasing = TRUE)
print(head(geneList))
print(length(geneList))
```

```R vscode={"languageId": "r"}
gse_bp <- gseGO(
    geneList = geneList,
    ont = "ALL",
    OrgDb = org.Hs.eg.db,
    keyType = "SYMBOL",
    pAdjustMethod = "BH",
    pvalueCutoff = 0.1,
)

```

```R vscode={"languageId": "r"}

```
