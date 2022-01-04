# mission文章  
文章拟使用实性结节（包含鳞癌，细胞瘤以及IA等各类恶性）和良性结节样本创建二分类模型，并查看预测结果。  
目前表格内有三中心来源样本，其中上海肺科样本共计412例（109例良性样本与303例恶性样本）；遵义共计23例（12例良性样本与11例恶性样本）；宁波42例（良恶性分别21例）。总计恶性335，良性142。建模8/2划分训练集验证集。后续进行建模分析。  
## train test分选  
142例良性样本，28例作为测试集，114例作为训练集。335例恶性实性肺结节样本，67例测试集，268例训练集。下图可见对于年龄测试集训练集均实现分层抽样。  
![年龄抽样](https://github.com/crushseven-7/missoin/blob/main/pic/sample_age.png "年龄抽样")  
## sldingwindow  
使用8kb nonverlap窗口进行定量。删除端粒中心粒，删除样本中reads长度极值（99.9%）。最终共计333949个特征。去除掉定量结果全部为0的特征最终剩余330013个window进行筛选。最终使用弹性网选定16个marker，在测试集中AUC为0.64。  
![AUC](https://github.com/crushseven-7/missoin/blob/main/pic/mission2_slidingwindow.png "AUC")
## endmotif  
## fragment ratio  
## CNA  
## CRAG  
