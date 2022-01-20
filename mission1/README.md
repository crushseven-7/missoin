# 文章架构思路  
mission文章分三步走对健康及肺结节样本进行分析。整体分析思路见下图。文章将分为三个部分对健康与肺结节样本进行分析。  
![分析思路](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_workflow.png "分析流程")  
## step1  
首先对收集的健康样本与肺结节样本的5hmC数据进行差异与建模分析。健康样本共计408例，随机抽取良性肺结节，AIS,MIA,IA肺结节患者各400例作为肺结节对照。下图是根据genebody定量结果进行的tsne展开。二维展开发现样本地域间可能存在潜在的batch。但是根据样本亚型结果显示，MIA与IA两种最恶性肿瘤与健康区分较为明显，良性与AIS样本间区分不为明显。因此无法确认是否出现批次效应。**是否可以增加上海健康样本20-50例进行进一步确认？**  
![tsne展开](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_1tsne_plot.png "tsne展开")  
后续使用DESeq2进行了数据差异分析。筛选log2(FC)>0.1 & q<10^-20 差异基因358个进行富集分析。KEGG共有三条通路富集结果。胞内吞作用与抑制肿瘤转移，炎症感染等症状密切相关。同时血小板激活可解释肺损伤过程。因此差异结果符合健康与肺结节样本分组。  
![KEGG](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_1kegg.png "KEGG")  
后续进行了三种组学的建模分析。genebody组学对358个差异基因进行了特征筛选最终保留了182个特征进行模型训练。建模使用svmRadial，最终模型训练集0.9347测试集0.9315。fragmentation。endmotif。三组学模型的testA AUC见下图。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_1combineAUC.png "AUC")
