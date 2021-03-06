# Intelligent Sensor Prognostics Health Management

## 故障诊断
- 首先尝试LSTM时序模型，捕获故障模式的长时间依赖限于性能，提出递进方案
- 终端，首先调试RF、GBDT树模型，平滑但维护、可解释性差。改用LR，添加离散化业务规则特征，特征选择后还存在多重共线性，引入L2正则改善
- 服务器，优化第一层大卷积核的WDCNN，自动提取周期数据的短时、位移无关特征，调节超参数使最后池化层在输入信号的感受野大于一个周期。由于线上噪声与变环境负载，极小batch训练、并对第一层卷积核中的权值Dropout
- 针对样本不均衡，overlap增强、调整loss中的类别权重、使用focalloss、保存多元高斯分布检测的异常数据用于故障样本生成 

## 生命周期管理
- 考虑样本量少且获取成本高，设计增量学习的集成模型，首先由总体样本估计威布尔累积失效分布函数，得到初始寿命模型。
- 线上评估回归效果，选择对增量数据多项式回归，更新回归系数，解决寿命分布复杂、状态易突变问题。为降低结果的方差，类似bagging思想，将各参数回归结果融合

## 模型迭代
- Spark平台进行特征工程及异常数据的半监督聚类标注

## few-shot learning
