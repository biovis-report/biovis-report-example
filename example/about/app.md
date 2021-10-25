> Author : Yechao Huang
>
> E-mail：[1721070009@fudan.edu.cn](mailto:1721070009@fudan.edu.cn)
>
> Git: <http://choppy.3steps.cn/huangyechao/wgs-germline.git>
>
> Last Updates: 06/02/2018

## 安装指南

```bash
# 激活环境
source activate choppy
# 安装APP
choppy install huangyechao/wgs-germline
```

## APP 概述

#### 应用

本 APP 是基于 Sentieon 软件中全基因组数据分析流程所构建的，整个APP运行是从原始测序数据 的`fastq`文件直到包含变异结果信息的 `vcf` 文件；本 APP 适用的平台为 choppy 平台（http://choppy.3steps.cn），通过choppy 即可安装本 APP 进行使用；本 APP 使用的数据分析是在阿里云的批量计算平台上进行。

#### 使用范围

- wgs-germline
- 二代测序
- 双端测序

#### 局限性

- 结果包含变异信息包括：SNP 和 INDEL，缺少 SV 的结果

- 原始数据信息需要在阿里云端存放


## 流程与参数

### 流程示意图

![wgs germline](http://kancloud.nordata.cn/2019-02-13-wgs-germline.png)

### 参数说明

#### 通用参数

| -t             | 程序运行的线程数               |
| -------------- | ------------------------------ |
| ref_dir        | 参考基因组数据所在的目录       |
| fasta          | 参考基因组的名字               |
| sample         | 生成的结果文件的前缀名         |
| -i             | 上一步骤的结果文件作为输入文件 |
| -o             | 当前步骤的输出结果文件         |
| -k             | Known sites                    |
| dbmills_dir    | dbmills数据所在的目录          |
| dbmills        | dbmills数据的名字              |
| docker         | task运行时使用的docker镜像     |
| cluster_config | 流程运行时使用的服务器的配置   |
| disk_size      | 流程运行时数据盘的大小         |

步骤参数

**BWA-MEM**

```bash
--fastq_1			测序数据的R1端
--fastq_2			测序数据的R2端
-K 10000000			该设置可用于比对过程中使得结果与线程数独立
-o				比对结果输出的文件名
```

**Dedup**

```bash
--score_info		the location and filename of the temporary score output file
--metrics		the location and filename of the dedup metrics results output file
```

**Pre-dedup bam QC**

```bash
GC_SUMMARY_TXT		the location and filename of the GC bias metrics summary results output file
GC_METRIC_TXT		the location and filename of the GC bias metrics results output file
MQ_METRIC_TXT		the location and filename of the mapping quality metrics results output file
QD_METRIC_TXT		the location and filename of the quality/depth metrics results output file
IS_METRIC_TXT		the location and filename of the insertion size metrics results output file
ALN_METRIC_TXT		the location and filename of the alignment metrics results output file
GC_METRIC_PDF		the location and filename of the GC bias metrics report output file
MQ_METRIC_PDF		the location and filename of the mapping quality metrics report output file
QD_METRIC_PDF		the location and filename of the quality/depth metrics report output file
IS_METRIC_PDF		the location and filename of the insertion size metrics report output file
```

**BQSR**

```bash
RECAL_DATA.TABLE	the location and filename of the recalibration table
RECAL_DATA.TABLE.POST	the location and filename of the temporary post recalibration table
RECAL_RESULT.CSV	the location and filename of the temporary recalibration results output file used for plotting
BQSR_PDF		the location and filename of the BSQR results output file
```

**Haplotyper**

```bash
VARIANT_VCF		the location and filename of the variant calling output file
```

### APP输入

本`APP`的输入文件的文件头可以通过 choppy 的 `samples` 命令产生：

```bash
choppy samples wgs-germline-latest --output sample.csv
```

生成的 `sample.csv` 文件包含以下几个参数信息：

`read1` : 测序数据的R1端所在的 OSS 路径

`read2`：测序数据的R2端所在的 OSS 路径

`sample_name` : 结果文件的前缀名

`cluster` ：APP 运行时集群的配置选择

`sample_id` : 对应样本提交时生成的目录名

`disk_size` : APP 运行时所使用的数据盘的大小（GB）

### APP的输出

APP 的每一步的结果会输出到阿里云的 OSS 中，其路径通常为所配置的bucket里面以所定义的项目名内；其结果如下所示：

![](http://kancloud.nordata.cn/2019-02-13-1549784133993.png)

其中主要关注结果文件是否正确输出及是否完整。如果想要获取结果文件可以通过 `choppy` 的 `download` 命令将所获得的结果下载到服务器上。

```bash
usage: choppy download <oss_link> <local_path> [<args>]
```

### APP 输出结果文件

本`APP` 获得的最终结果为 **`vcf`** 文件，对 `vcf` 文件的解读如下：

**VCF** 文件的格式通常由两部分构成，分别为带有 `#` 的表头注释信息以及变异的具体信息

#### 头文件注释信息

包括vcf文件版本、FORMAT、INFO、参考基因组以及执行程序等信息

```bash
##fileformat=VCFv4.2
##FORMAT=<ID=AD,Number=.,Type=Integer,Description="Allelic depths for the ref and alt alleles in the order listed">
##FORMAT=<ID=DP,Number=1,Type=Integer,Description="Read depth">
##FORMAT=<ID=GQ,Number=1,Type=Integer,Description="Genotype quality">
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=PL,Number=G,Type=Integer,Description="The phred-scaled genotype likelihoods rounded to the closest integer">
##INFO=<ID=AC,Number=A,Type=Integer,Description="Allele count in genotypes, for each ALT allele, in the same order as listed">
##INFO=<ID=AF,Number=A,Type=Float,Description="Allele frequency, for each ALT allele, in the same order as listed">
##INFO=<ID=AN,Number=1,Type=Integer,Description="Total number of alleles in called genotypes">
##INFO=<ID=BaseQRankSum,Number=1,Type=Float,Description="Z-score from Wilcoxon rank sum test of Alt Vs. Ref base qualities">
.....
##INFO=<ID=SOR,Number=1,Type=Float,Description="Symmetric Odds Ratio of 2x2 contingency table to detect strand bias">
......
##contig=<ID=1,length=249250621,assembly=hg19>
......
##contig=<ID=X,length=155270560,assembly=hg19>
##contig=<ID=Y,length=59373566,assembly=hg19>
##contig=<ID=M,length=16571,assembly=hg19>
#CHROM  POS     ID      REF     ALT     QUAL    FILTER  INFO    FORMAT  ZC-101
```

解读如下：

```bash
1. CHROM(chromosome):染色体
2. POS  变异位点在参考基因组中的位置
3. ID - identifier: variant的ID。比如在dbSNP中有该SNP的id，则会在此行给出；若没有，则用'.'表示其为一个novel variant。
4. REF - reference base(s):参考碱基，染色体上面的碱基，必须是ATCGN中的一个，N表示不确定碱基
5. ALT - alternate base(s):与参考序列比较发生突变的碱基
6. QUAL - quality: Phred格式(Phred_scaled)的质量值，表 示在该位点存在variant的可能性；该值越高，则variant的可能性越大；计算方法：Phred值 = -10 * log (1-p) p为variant存在的概率; 通过计算公式可以看出值为10的表示错误概率为0.1，该位点为variant的概率为90%。
7. FILTER - _filter status: 使用上一个QUAL值来进行过滤的话，是不够的。GATK能使用其它的方法来进行过滤，过滤结果中通过则该值为”PASS”;若variant不可靠，则该项不为”PASS”或”.”。
8. INFO - additional information:  这一行是variant的详细信息，具体如下：
  #DP-read depth：样本在这个位置的reads覆盖度。是一些reads被过滤掉后的覆盖度。DP4:高质量测序碱基，位于REF或者ALT前后
  #QD：通过深度来评估一个变异的可信度。Variant call confidence normalized by depth of sample reads supporting a variant         
  #MQ：表示覆盖序列质量的均方值RMS Mapping Quality
  #FQ：phred值关于所有样本相似的可能性
  #AC，AF 和 AN：AC(Allele Count) 表示该Allele的数目；AF(Allele Frequency) 表示Allele的频率； AN(Allele Number) 表示Allele的总数目。对于1个diploid sample而言：则基因型 0/1 表示sample为杂合子，Allele数为1(双倍体的sample在该位点只有1个等位基因发生了突变)，Allele的频率为0.5(双倍体的sample在该位点只有50%的等位基因发生了突变)，总的Allele为2； 基因型 1/1 则表示sample为纯合的，Allele数为2，Allele的频率为1，总的Allele为2。
  #MLEAC：Maximum likelihood expectation (MLE) for the allele counts (not necessarily the same as the AC), for each ALT allele, in the same order as listed
  #MLEAF：Maximum likelihood expectation (MLE) for the allele frequency (not necessarily the same as the AF), for each ALT allele, in the same order as listed
  #BaseQRankSum   比较支持变异的碱基和支持参考基因组的碱基的质量，负值表示支持变异的碱基质量值不及支持参考基因组的，正值则相反，支持变异的质量值好于参考基因组的。0表示两者无明显差异。
  #FS  使用F检验来检验测序是否存在链偏好性。链偏好性可能会导致变异等位基因检测出现错误。输出值Phred-scaled p-value，值越大越可能出现链偏好性。
  #InbreedingCoeff    使用似然法检验样本间的近交系数（又或者称为近亲关系）。值越高越可能是近亲繁殖。
  #MQRankSum  比较支持变异的序列和支持参考基因组的序列的质量，负值表示支持变异的碱基质量值不及支持参考基因组的，只针对杂合。正值则相反，支持变异的质量值好于参考基因组的。0表示两者无明显差异。实际应用中一般过滤掉较小的负值。
  #BaseCounts   所有样本在变异位点ATCG的数量
  #ClippingRankSum  同前面两个类似，负值表示支持变异的read有更的的hard-clip碱基，正值表示支持参考基因组的的read有更多的hard-clip。0最好，无论是正值还是负值都表示可能可能存在人为偏差。
  #ReadPosRankSum    检测变异位点是否有位置偏好性（是否存在于序列末端，此时往往容易出错）。最佳值为0，表示变异与其在序列上的位置无关。负值表示变异位点更容易在末端出现，正值表示参考基因组中的等位基因更容易在末端出现。
  #ExcessHet   检测这些样本的相关性，与InbreedingCoeff相似，值越大越可能是错误。
  #LikelihoodRankSum  评价支持变异和ref的序列与best hyplotype的匹配性，0为最佳值。负值表示支持变异的read匹配度不及支持ref的匹配度，正值则相反。值越大表示越可能是出现了错误。
  #HaplotypeScore    分数越高越可能出现错误。Higher scores are indicative of regions with bad alignments, typically leading to artifactual SNP and indel calls.
  #SOR：也是一个用来评估是否存在链偏向性的参数，相当于FS的升级版。The StrandOddsRatio annotation is one of several methods that aims to evaluate whether there is strand bias in the data. It is an updated form of the Fisher Strand Test that is better at taking into account large amounts of data in high coverage situations. It is used to determine if there is strand bias between forward and reverse strands for the reference or alternate allele. The reported value is ln-scaled.
  #IS：插入缺失或部分插入缺失的reads允许的最大数量
  #G3：ML 评估基因型出现的频率
  #HWE：chi^2基于HWE的测试p值和G3
  #CLR：在受到或者不受限制的情况下基因型出现可能性log值
  #UGT：最可能不受限制的三种基因型结构
  #CGT：最可能受限制三种基因型的结构
  #PV4：四种P值得误差，分别是（strand、baseQ、mapQ、tail distance bias）
  #INDEL：表示该位置的变异是插入缺失
  #PC2：非参考等位基因的phred（变异的可能性）值在两个分组中大小不同
  #PCHI2：后加权chi^2，根据p值来测试两组样本之间的联系
  #QCHI2：Phred scaled PCHI2
  #PR：置换产生的一个较小的PCHI2
  #QBD：Quality by Depth，测序深度对质量的影响
  #RPB：序列的误差位置（Read Position Bias）
  #MDV：样本中高质量非参考序列的最大数目
  #VDB：Variant Distance Bias，RNA序列中过滤人工拼接序列的变异误差范围
9. FORMAT 和最后一列sample中的信息是对应的
  #AD 和 DP：AD(Allele Depth)为sample中每一种allele的reads覆盖度,在diploid中则是用逗号分割的两个值，前者对应ref基因型，后者对应variant基因型； DP（Depth）为sample中该位点的覆盖度。
  #GT：样品的基因型（genotype）。两个数字中间用’/'分 开，这两个数字表示双倍体的sample的基因型。0 表示样品中有ref的allele；1表示样品中variant的allele； 2表示有第二个variant的allele。因此： 0/0 表示sample中该位点为纯合的，和ref一致； 0/1 表示sample中该位点为杂合的，有ref和variant两个基因型； 1/1 表示sample中该位点为纯合的，和variant一致。
  #GQ：即第二可能的基因型的PL值，相对于最可能基因型的PL值（其PL=0）而言，大于99时，其信息量已不大，因此大于99的全部赋值99。当GQ值很小时，意味着第二可能基因型与最可能基因型差别不大。
  #GL：三种基因型（RR RA AA）出现的可能性，R表示参考碱基，A表示变异碱基
  #DV：高质量的非参考碱基
  #SP：phred的p值误差线
  #PL：指定的三种基因型的可能性(provieds the likelihoods of the given genotypes)。这三种指定的基因型为(0/0,0/1,1/1)，这三种基因型的概率总和为1。和之前不一致，该值越大，表明为该种基因型的可能性越小。 Phred值 = -10 * log (p) p为基因型存在的概率。
```

#### 变异结果信息

```bash
1       13273   .       G       C       2058.77 .       AC=1;AF=0.500;AN=2;BaseQRankSum=-0.478;ClippingRankSum=0.000;DP=328;ExcessHet=3.0103;FS=6.142;MLEAC=1;MLEAF=0.500;MQ=40.63;MQRankSum=1.924;QD=6.28;ReadPosRankSum=1.637;SOR=0.903       GT:AD:DP:GQ:PL  0/1:217,111:328:99:2087,0,4874
1       13417   .       C       CGAGA   1589.73 .       AC=1;AF=0.500;AN=2;BaseQRankSum=1.638;ClippingRankSum=0.000;DP=627;ExcessHet=3.0103;FS=6.036;MLEAC=1;MLEAF=0.500;MQ=38.27;MQRankSum=0.215;QD=2.58;ReadPosRankSum=0.813;SOR=0.295        GT:AD:DP:GQ:PL  0/1:540,76:616:99:1627,0,24416
1       13418   .       G       A       1506.77 .       AC=1;AF=0.500;AN=2;BaseQRankSum=2.603;ClippingRankSum=0.000;DP=639;ExcessHet=3.0103;FS=9.202;MLEAC=1;MLEAF=0.500;MQ=38.46;MQRankSum=-1.756;QD=2.36;ReadPosRankSum=-2.113;SOR=1.634      GT:AD:DP:GQ:PL  0/1:521,118:639:99:1535,0,14252
```

简单解读：

第一个位点的信息表示：1号染色体上的第13273位，参考基因组上碱基为G突变为C，其质量值为2068.77；Allele的数目为1，频率为0.5，总数目为2，基因型为杂合子；GT:AD:DP:GQ:PL  0/1:217,111:328:99:2087,0,4874 表示其基因型为 0/1 ；reads 的覆盖度在参考基因组中为217，在variant中为111；该位点的覆盖度为328；

### CHANGELOG



### FAQ