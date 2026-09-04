---
permalink: /
title: ""
excerpt: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

{% if site.google_scholar_stats_use_cdn %}
{% assign gsDataBaseUrl = "https://cdn.jsdelivr.net/gh/" | append: site.repository | append: "@" %}
{% else %}
{% assign gsDataBaseUrl = "https://raw.githubusercontent.com/" | append: site.repository | append: "/" %}
{% endif %}
{% assign url = gsDataBaseUrl | append: "google-scholar-stats/gs_data_shieldsio.json" %}

<span class='anchor' id='about-me'></span>

I am a second-year PhD student in Systems Biology at Columbia University Irving Medical Center, jointly supervised by [Dr. Bing Ren](https://renlab.sdsc.edu/index.html) and [Dr. David Knowles](https://daklab.github.io/). Before my PhD, I completed my undergraduate training at the School of Life Sciences, Peking University with a major in Bioinformatics.

<span class='anchor' id='news'></span>

# 📰 News

- **2025.11** — 📄 Paper published in ***Cell Mol Immunol*** on T-cell metabolism and SARDH
- **2025.09** — 🎉 Started PhD in Systems Biology at Columbia University Irving Medical Center
- **2025.06** — 🎓 Graduated from Peking University with the **Shen Tong Outstanding Undergraduate Award**
- **2025.05** — 📄 Paper published in ***Cell*** on immune heterogeneity in anti-PD-1 treated NSCLC
- **2024.07** — 📄 Paper published in ***Cell Reports Medicine*** on neoadjuvant immunotherapy in NSCLC
<!-- TODO: Update 2024.XX with the actual publication month for the Cell Reports Medicine paper -->
<!-- TODO: Add more news entries as needed, format: - **YYYY.MM** — 📄/🎉/🎓/🏆 Your news here -->

<span class='anchor' id='research'></span>

# 🔬 Research

My research focuses on developing and applying computational frameworks to decode the multiscale regulatory logic of biological systems in health and disease. Specifically, I am interested in:

1. **Tumor Immune Microenvironment**

   Cancer is highly heterogeneous, and its interactions with the immune system shape progression, metastasis, and response to treatment. During my undergraduate research in Zemin Zhang's group, I used single-cell RNA/TCR sequencing and whole-exome sequencing to study complementary aspects of the tumor microenvironment and mechanisms of immunotherapy resistance. In a phase 2 trial of neoadjuvant sintilimab plus chemotherapy for EGFR-mutant NSCLC ([*Cell Reports Medicine*, 2024](https://doi.org/10.1016/j.xcrm.2024.101615)), we found that infiltration and clonal expansion of CCR8+ Treg<sup>hi</sup>/CXCL13+ Tex<sup>lo</sup> cells mark an immunotherapy-resistant subtype, suggesting a signature for patient stratification. In a single-cell atlas of 234 patients with NSCLC treated with anti-PD-1 ([*Cell*, 2025](https://doi.org/10.1016/j.cell.2025.03.018)), we resolved five tumor immune microenvironment subtypes with distinct therapeutic outcomes. We also identified SARDH in one-carbon metabolism as a T-cell metabolic checkpoint and potential therapeutic target ([*Cell Mol Immunol*, 2025](https://doi.org/10.1038/s41423-025-01331-5)).

2. **Spatiotemporal Resolution of Tissue Structure**

   Cell-type composition captures only part of tissue biology; spatial organization and intercellular signaling determine how tissues develop and function. During my summer research in Xiaojie Qiu's group at Stanford, I learned to use cell-cell interaction (CCI) analysis to move from tissue composition toward interaction and structure. Using 3D MERFISH data from developing mouse hearts, I constructed a three-dimensional CCI atlas, identified differential signaling patterns in cardiac progenitor cells, and characterized the interplay between Wnt and BMP signaling in juxtacardiac field development. I then developed an explainable AI model linking cell-cell communication to heart morphogenesis.

3. **Genomics of Gene Regulation**

   Perturbations reveal gene regulation by exposing causal responses, while sequence-to-function models identify the regulatory information encoded in DNA. In my current research at Columbia, I work on perturbation modeling and transcriptional dynamics. In parallel, I develop sequence-to-function models with latent chromatin states to map DNA sequence to cell-type-specific omics profiles. By benchmarking these models against systems such as AlphaGenome and interpreting their variant-effect predictions, I aim to disentangle gene-regulatory grammar from downstream cellular behavior.

By bridging these dimensions, I aim to transform static molecular snapshots into predictive, dynamical models of complex biological processes.

<!-- → [Read more about my research](/research-descriptions/) -->

# 🎓 Education

- *2025.09 - Present*, Columbia University Irving Medical Center, New York, USA, PhD Student in Systems Biology
- *2021.09 - 2025.06*, Peking University, Beijing, Bachelor of Engineering in Bioinformatics

<span class='anchor' id='publications'></span>

# 📝 Publications

<div class="publication-item" markdown="1">

Chao Zhang, Yuxuan Sun, **Dingcheng Yi**, Benyuan Jiang, Lixu Yan, …, Zemin Zhang, Wenzhao Zhong. Neoadjuvant sintilimab plus chemotherapy in early-stage EGFR-mutant NSCLC: phase 2 trial interim results (NEOTIDE/CTONG2104). ***Cell Reports Medicine***, **2024**

<details class="pub-abstract">
<summary>Abstract</summary>
<div class="abstract-content">
<!-- TODO: Paste abstract here -->
The clinical efficacy of neoadjuvant immunotherapy plus chemotherapy remains elusive in localized epidermal growth factor receptor (EGFR)-mutant non-small cell lung cancer (NSCLC). Here, we report interim results of a Simon's two-stage design, phase 2 trial using neoadjuvant sintilimab with carboplatin and nab-paclitaxel in resectable EGFR-mutant NSCLC. All 18 patients undergo radical surgery, with one patient experiencing surgery delay. Fourteen patients exhibit confirmed radiological response, with 44% achieving major pathological response (MPR) and no pathological complete response (pCR). Similar genomic alterations are observed before and after treatment without influencing the efficacy of subsequent EGFR-tyrosine kinase inhibitors (TKIs) in vitro. Infiltration and T cell receptor (TCR) clonal expansion of CCR8+ regulatory T (Treg)<sup>hi</sup>/CXCL13+ exhausted T (Tex)<sup>lo</sup> cells define a subtype of EGFR-mutant NSCLC highly resistant to immunotherapy, with the phenotype potentially serving as a promising signature to predict immunotherapy efficacy. Informed circulating tumor DNA (ctDNA) detection in EGFR-mutant NSCLC could help identify patients nonresponsive to neoadjuvant immunochemotherapy. These findings provide supportive data for the utilization of neoadjuvant immunochemotherapy and insight into immune resistance in EGFR-mutant NSCLC.
</div>
</details>

</div>

<div class="publication-item" markdown="1">

Zedao Liu, Zhenlin Yang, Junqi Wu, Wenjie Zhang, Yuxuan Sun, Chao Zhang, Guangyu Bai, …, **Dingcheng Yi**, …, Zemin Zhang. A single-cell atlas reveals immune heterogeneity in anti-PD-1-treated non-small cell lung cancer. ***Cell***, **2025**

<details class="pub-abstract">
<summary>Abstract</summary>
<div class="abstract-content">
<!-- TODO: Paste abstract here -->
Anti-PD-(L)1 treatment is standard for non-small cell lung cancer (NSCLC), but patients show variable responses to the same regimen. The tumor immune microenvironment (TIME) is associated with immunotherapy response, yet the heterogeneous underlying therapeutic outcomes remain underexplored. We applied single-cell RNA and TCR sequencing (scRNA/TCR-seq) to analyze surgical tumor samples from 234 NSCLC patients post-neoadjuvant chemo-immunotherapy. Analyses revealed five distinct TIME subtypes with varying major pathological response (MPR) rates. MPR patients had elevated levels of FGFBP2+ NK/NK-like T cells, memory B cells, or effector T cells, while non-MPR patients showed higher CCR8+ Tregs. T cell clonal expansion analyses unveiled heterogeneity in non-MPR patients, marked by varying expansions of Tex-relevant cells and CCR8+ Tregs. Precursor exhausted T cells (Texp cells) correlated with recurrence-free survival, identifying a patient subgroup with reduced recurrence risk despite lack of MPR. Our study dissects TIME heterogeneity in response to chemoimmunotherapy, offering insights for NSCLC management.
</div>
</details>

</div>

<div class="publication-item" markdown="1">

Wen Si, Sijin Cheng, Haiyin He, Yu Zhang, Yuhui Miao, **Dingcheng Yi**, Mengjiao Ni, Anqiang Wang, Hongtao Fan, Yufei Bo, Chang Liu, Zhaode Bu, Linnan Zhu, Zemin Zhang. SARDH in the 1-C metabolism sculpts the T-cell fate and serves as a potential cancer therapeutic target. ***Cell Mol Immunol***, **2025**

<details class="pub-abstract">
<summary>Abstract</summary>
<div class="abstract-content">
<!-- TODO: Paste abstract here -->
T-cell metabolism plays a pivotal role in defining T-cell functional states. Through analysis of a comprehensive pancancer single-cell transcriptional atlas, we identified SARDH, an enzyme involved in one-carbon (1-C) metabolism, as a potential T-cell metabolic checkpoint. SARDH significantly impacts T-cell fate and function, leading to impaired tumor control efficacy. Knocking down SARDH resulted in sarcosine accumulation and reduced consumption of S-adenosylmethionine (SAM), a critical methyl donor for epigenetic modulation, likely due to the shift in glycine-to-sarcosine homeostasis. Deletion of SARDH increased H3K79me2 modification at NF-κB-activating genes, thereby augmenting NF-κB signaling and T-cell function. Additionally, we observed transcriptional dysregulation of 1-C metabolism within tumors across various cancer types, which was often accompanied by increased sarcosine levels. Sarcosine was found to induce SARDH upregulation, suggesting a feedback mechanism for metabolic homeostasis in T cells within tumors. These findings underscore the potential effects and mechanism of targeting 1-C metabolism, particularly SARDH, as an avenue for cancer therapy.
</div>
</details>

</div>

# 🏆 Honors & Awards

| Award | Year |
|-------|------|
| [**Shen Tong Outstanding Undergraduate Award**](https://www.bio.pku.edu.cn/homes/Index/jz_page/96.html), highest honor for undergraduates in the department | 2025 |
| **National Scholarship** | 2024 |
| **Pacemaker to Merit Student** | 2024 |
| **May 4th Scholarship**, highest individual scholarship at Peking University | 2023 |
| **Award for Academic Excellence** | 2022, 2023 |
| **Qin Wanshun-Jin Yunhui Scholarship** | 2022 |

<span class='anchor' id='collaboration'></span>

# 🤝 Open for Collaboration

I am interested in collaborations in two areas:

- **Tumor immune microenvironment (TME)** — Single-cell, spatial, and multi-omic studies of tumor–immune interactions and immunotherapy response
- **Regulatory genomics** — Perturbation and sequence-to-function modeling of gene regulation and variant effects

Please reach out if you are working on related questions or datasets.

📧 [dy2527@cumc.columbia.edu](mailto:dy2527@cumc.columbia.edu) · 💼 [LinkedIn](https://www.linkedin.com/in/dingcheng-yi-29810b370/)

<span class='anchor' id='hobby'></span>

# 🎮 Beyond Research

I love playing the Rubik's Cube — my WCA ID is [2016YIDI01](https://www.worldcubeassociation.org/persons/2016YIDI01) — and diving into geeky pursuits like Arch Linux and Emacs. I'm also a big anime fan — my favorite is *Liz and the Blue Bird*.

<span class='anchor' id='cv'></span>

# 📄 CV

My full CV is available here:

[<i class="fas fa-file-pdf"></i> Download CV (PDF)]({{ site.author.cv | relative_url }}){: .btn .btn--info}

