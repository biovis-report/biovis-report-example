## Meta Data and Clinical Information

### Description

Primary tumor tissue and blood samples were obtained from 504 consecutive female Chinese patients with TNBC treated at Fudan University Shanghai Cancer Center (FUSCC). According to the quality check metrics, total 465 patients were available in the final cohort. Briefly, 279 had whole exome sequencing (WES) data on primary tumor tissue and paired blood samples, 401 had copy-number alteration (CNA) data and 360 had RNA sequencing (RNA-seq) data on primary tumor tissue. Here we show the data type and the clinical data of 495 patients.

### Clinical Data Table

@datatable-js(dataUrl='./example/data/data_clinical_patient.csv', enableItemSearch=True)

### Pivot Table for Clinical Data

@pivot-table-js(dataUrl='./example/data/data_clinical_patient.csv', enableLocal=True)

### View Relationship across Clinical Variables

@scatter-plot-r(dataFile="./example/data/data_clinical_patient.csv", dataType='csv', xAxis='RFS_time_Days', yAxis='HRD')
