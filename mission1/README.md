# 文章架构思路  
mission文章分三步走对健康及肺结节样本进行分析。整体分析思路见下图。文章将分为三个部分对健康与肺结节样本进行分析。  
![分析思路](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_workflow.png "分析流程")  
## step1  
首先对收集的健康样本与肺结节样本的5hmC数据进行差异与建模分析。健康样本共计408例，随机抽取良性肺结节，AIS,MIA,IA肺结节患者各400例作为肺结节对照。下图是根据genebody定量结果进行的tsne展开。二维展开发现样本地域间可能存在潜在的batch。但是根据样本亚型结果显示，MIA与IA两种最恶性肿瘤与健康区分较为明显，良性与AIS样本间区分不为明显。因此无法确认是否出现批次效应。##是否可以增加上海健康样本20-50例进行进一步确认？##  
![tsne展开](https://github.com/crushseven-7/missoin/blob/main/pic/mission1_1tsne_plot.png "tsne展开")
