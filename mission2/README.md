# mission文章  
文章拟使用实性结节（包含鳞癌，细胞瘤以及IA等各类恶性）和良性结节样本创建二分类模型，并查看预测结果。  
目前表格内有三中心来源样本，其中上海肺科样本共计412例（109例良性样本与303例恶性样本）；遵义共计23例（12例良性样本与11例恶性样本）；宁波42例（良恶性分别21例）。总计恶性335，良性142。建模8/2划分训练集验证集。后续进行建模分析。  
## train test分选  
142例良性样本，28例作为测试集，114例作为训练集。335例恶性实性肺结节样本，67例测试集，268例训练集。下图可见对于年龄测试集训练集均实现分层抽样。  
![年龄抽样](https://github.com/crushseven-7/missoin/blob/main/pic/sample_age.png "年龄抽样")  
## sldingwindow  
使用8kb nonverlap窗口进行定量。删除端粒中心粒，删除样本中reads长度极值（99.9%）。最终共计333949个特征。去除掉定量结果全部为0的特征最终剩余330013个window进行筛选。logCPM+limma筛选5X内P值小与0.1的差异基因。最终使用弹性网选定16个marker，在测试集中AUC为0.64。AUC可见在打分中建部分样本的预测效果较差，根据比较详细病理未发现规律。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_slidingwindow.png "AUC")  
## genebody  
在sldingwindow效果不佳后尝试了genebody定量方法。genebody区间共计57921个特征。去除掉全部为0的基因后剩余53172个genebody进行后续筛选。logCPM+limma筛选5X内P值小与0.001的差异基因。最终弹性网选定28个marker进行模型训练。AUC最终优于sldingwindow单提升效果不大。查看测试集中预测错误样本，与slidingwindow之间未发现显著关联。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_genebody.png "AUC")
## endmotif 
之前的研究表明cfDNA存在偏好性末端motif。因此这里鉴别出每一条cfDNA 5'端4-mer end motif（一共4^4=256种），并以其出现频率作为特征进行建模。
在训练集中，一共筛选出31种良恶性差异的4-mer end-motif。通过svmLinear对这31种4-mer end-motif的频率进行训练。结果表明：在训练集中AUC=0.8,测试集AUC=0.729，不存在过拟合。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/end_motif_5hmC_batchInf_fdr0.005_svmLinear.png "AUC")  
## fragment ratio  
使用8kb nonoverlap滑动窗口去除端粒中心粒与low mapbality区域后分别对60-150短序列以及150-220长序列分别定量并根据delfi方法对定量结果进行GC矫正。矫正后得到每个窗口短reads/长reads比值作为特征。进行10X较差验证，最终筛选78个特征进行弹性网模型训练。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_ratio.png "AUC") 
## CNA  
CNA分析使用QDNAseq包对样本dedup bam文件直接分析。使用软件包内置10kb滑动窗口作为定量区间，去除black-list区间并经过GC矫正后得到样本在每个窗口的copy number分值。之后会对分值进行标准化以及去除极端值。最后使用DNAcopy对分值进行最终标准化。使用最终打分分值作为特征进行建模。最终挑选68个特征使用弹性网建模。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_CNA.png "AUC")   
# 模型整合  
后续进行模型整合工作。剔除slidingwindow模型，共计四种组学模型进行整合。四种模型打分作为特征建模效果差于两模型打分整合。符合预期，由于genebody与ratio的train AUC过高导致模型学习深度降低，但是在测试集中无法更好的泛化。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_two_score.png "AUC")  
后续对特特征整合进行整合。由于四个组学共计筛选出266个特征，因此直接对特征融合会产生严重的过拟合。首先使用boruta包进行了降维，后对特征融合。并尝试了不同的组合。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_combine_feature.png "AUC")
