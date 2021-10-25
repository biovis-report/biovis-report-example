<!--note C:\Users\YY\Documents\working\choppy_report\report will be automatically updated. donot put useful files there!  -->

<!-- http://127.0.0.1:8080/ -->

## **Chinese Triple-Negative Breast Cancer Cohort**

### **Description**

Triple negative breast cancers (TNBC) were defined as lack of  expression of the estrogen receptor (ER) or the progesterone (PR) and no ERBB2/Her2 amplification, comprising 10%-20% of all breast cancers. This kind of breast were frequently diagnosed in younger patients and liable to be aggressive. Patients diagnosed with TNBC had a higher rate of distant recurrence and a worse 5-year prognosis. The recent findings based on omics data had accelerated the interpretation of  TNBC. Here, we introduce the Chinese Triple-Negative Breast Cancer Cohort, an exceedingly  important supplement for TNBC researching community, published by Yi-zhou Jiang, et al.

---

### Meta Data and Clinical Information

Primary tumor tissue and blood samples were obtained from 504 consecutive female Chinese patients with TNBC treated at Fudan University Shanghai Cancer Center (FUSCC). According to the quality check metrics, total 465 patients were available in the final cohort. Briefly, 279 had whole exome sequencing (WES) data on primary tumor tissue and paired blood samples, 401 had copy-number alteration (CNA) data and 360 had RNA sequencing (RNA-seq) data on primary tumor tissue. Here we show the data type and the clinical data of 495 patients.

---

@data-table-js(dataUrl='/Users/choppy/Downloads/report_templates/data/meta_and_clinical_data_table_short_c8.csv', enableItemSearch=True)

---

@pivot-table-js(dataUrl='/Users/choppy/Downloads/report_templates/data/meta_and_clinical_data_table_short_c8.csv', enableLocal=True)

---

@upset-r()

---

@boxplot-r()

---

### **View Summary**

- example
- **View Relationship across Clinical Variables**

---

###  Genomic Alternations

- **Somatic SNP and Indel**

Among the TNBCs with WES data, 25,713 somatic mutations were identified, comprising 24,025 single-nucleotide variants (SNVs) and 1,688 insertions or deletions (INDELs). These tumors harbored a median of 46 nonsynonymous SNVs and 4 INDELs, which are similar to the results for the TCGA TNBC cohort.

- **Non-synonymous mutation frequency by mRNA subtype**

---

@data-table-js(dataUrl='/Users/choppy/Downloads/report_templates/data/somatic_freq_top.csv')

----

### Mutation frequency

@stack-barplot-r(dataFile='/Users/choppy/Downloads/report_templates/data/Somatic_Mutationfrequency_top20.rds', dataType='rds', xAxis='Hugo_Symbol', yAxis='Number',labelAttr='Variant_Classification',barPos='stack')

---

### Somatic SNP and Indel
- example
- 需求：也需要单个基因的展示
- Germline SNP and Indel
- maf

---
@muts-needle-r()
---

### Copy number Alterations

MYC was the gene most frequently affected by somatic CNAs (gained in 81% of patient samples), with frequent gains in E2F3 (55%), IRS2 (49%), CCNE1 (47%), EGFR (47%), NFIB (44%), CCND1 (44%), and MYB (41%) and frequent losses in CHD1 (lost in 71% of samples), PTEN (58%), RB1 (54%), and CDKN2A (43%).

---

### Gene Expression

The tumors samples were classified into 4 separate subtypes using unsupervised k-means clustering based on the top 2,000 most differentially expressed coding genes. These four subtypes consisted of the following: (1) a luminal androgen receptor (LAR) subtype (23%) characterized by androgen receptor signaling; (2) an immunomodulatory (IM) subtype (comprising 24% of tumors) with high immune cell signaling and cytokine signaling gene expression; (3) a basal-like and immune-suppressed (BLIS) (39%) subtype characterized by upregulation of cell cycle, activation of DNA repair, and downregulation of immune response genes; and (4) a mesenchymal-like (MES) subtype (15%) enriched in mammary stem cell pathways.

---

@heatmap-r(dataFile='/Users/choppy/Downloads/report_templates/data/tnbcexp_20_m0sd1_heatmap.rds', dataType='rds')

---

### **heatmap**

@heatmap-d3(dataFile='/Users/choppy/Downloads/report_templates/data/tnbcexp_20_m0sd1_heatmap.rds', dataType='rds')

---

### **per gene expression**
The description for gene expression matrix.

---

@group-boxplot(dataFile='/Users/choppy/Downloads/report_templates/data/tnbcexpmelt1_tp53.rds', dataType='rds',xAxis='Group', yAxis='Value')

---

@group-boxplot(dataFile='/Users/choppy/Downloads/report_templates/data/tnbcexpmelt_top19.rds', dataType='rds',xAxis='Group', yAxis='Value')

---

### **Reference**

Jiang et al., Genomic and Transcriptomic Landscape of Triple-Negative Breast Cancers: Subtypes and Treatment Strategies, Cancer Cell (2019), https://doi.org/10.1016/j.ccell.2019.02.001
