## Gene Expression

### Description

The tumors samples were classified into 4 separate subtypes using unsupervised k-means clustering based on the top 2,000 most differentially expressed coding genes. These four subtypes consisted of the following: (1) a luminal androgen receptor (LAR) subtype (23%) characterized by androgen receptor signaling; (2) an immunomodulatory (IM) subtype (comprising 24% of tumors) with high immune cell signaling and cytokine signaling gene expression; (3) a basal-like and immune-suppressed (BLIS) (39%) subtype characterized by upregulation of cell cycle, activation of DNA repair, and downregulation of immune response genes; and (4) a mesenchymal-like (MES) subtype (15%) enriched in mammary stem cell pathways.

### Per Gene Expression

The description for gene expression matrix.

@grouped-boxplot-r(dataFile='./example/data/tnbcexpmelt1_tp53.rds', dataType='rds',xAxis='Group', yAxis='Value')


@grouped-boxplot-r(dataFile='./example/data/tnbcexpmelt_top19.rds', dataType='rds',xAxis='Group', yAxis='Value')

### Heatmap

@heatmap-d3-r(dataFile='./example/data/tnbcexp_20_m0sd1_heatmap.rds', dataType='rds')
