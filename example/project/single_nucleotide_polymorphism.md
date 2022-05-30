## Genomic Alterations

### Non-synonymous mutation frequency by mRNA subtype

Among the TNBCs with WES data, 25,713 somatic mutations were identified, comprising 24,025 single-nucleotide variants (SNVs) and 1,688 insertions or deletions (INDELs). These tumors harbored a median of 46 nonsynonymous SNVs and 4 INDELs, which are similar to the results for the TCGA TNBC cohort.

@datatable-js(dataUrl='./example/data/somatic_freq_top.csv')

### Mutation frequency

@stack-barplot-r(dataFile='./example/data/Somatic_Mutationfrequency_top20.rds', dataType='rds', xAxis='Hugo_Symbol', yAxis='Number',labelAttr='Variant_Classification',barPos='stack')
