---
export_on_save:
    puppeteer: true # 保存文件时导出 PDF
    
---
目前为止多工况过程监测的模型都是建立在训练数据和测试数据都有相同的mode种类和数量\
解决的问题：训练数据中的所有模式都会共同影响投影矩阵P的学习，当测试数据的mode种类少于训练数据的mode种类时，比如：测试数据中只包含模式1（mode1）和模式2（mode2），但训练数据中有模式1（mode1）和模式2（mode2）模式3（mode3），在由训练数据学习出的位于$P$矩阵集合$P=[P_1;P_2;P_3]$中的$P_1$和$P_2$，在训练过程会受到模式3的影响。
## 加入自适应重构损失项--在线更新（学习）机制
加入自适应重构损失项，并且在原来的离线建模和在线监控过程中间加入在线学习过程，在线学习过程根据自适应重构损失项来选择并调整当前测试数据所对应mode的$P_{mode}$,调整过程是增量更新过程。\
自适应重构损失项的具体形式$f(\cdot)$待定
$$
\mathcal{L}_{\text{recon}}^{\text{adaptive}}=f(P_s,w_s,..)
$$
$$
\mathcal{L}_{\text{recon}}^{\text{adaptive}}=\sum^S_{s=1} w_s\|f(P_s,w_s,...)\|
$$
## 引入动态权重 $w_s$ 对每个模式的重构误差进行加权 

这样，对于测试数据中未出现的模式，其对应的 $w_s$ 将被动态降低（甚至趋于零），使得这些模式对整体重构误差的贡献减弱。

$w_s$的设计：\
**对于每个模式s，可以预先计算该模式的代表性向量$\mu_s$**，$\mu_s$可以是训练数据中每个模式的均值或者聚类中心。\
采用点积或者余弦相似度
$$
s_s=f(\mathbf{x_{new}},\mu_s)
$$
可以在 softmax 前加入一个门限 $\tau$：
$$
w_s = 
\begin{cases}
\frac{\exp(\lambda s_s)}{\sum_{s=1}^{S} \exp(\lambda s_s)}, & \text{if } 
s_s \ge \tau \\
0, & \text{if } s_s < \tau
\end{cases}
$$
这样，当相似度低于阈值时，模式 $s$ 的权重 $w_s$ 将为零，从而忽略该模式对重构损失的贡献。
## **在线学习增量更新模型参数**
1. **增量更新**：  
通过在线学习算法对模型参数进行更新。在线学习意味着在每次获得新数据后，不是重新训练整个模型，而是通过增量更新已有模型：
- 对于每个模式 $s$，更新投影矩阵 $P_s$ ，通过梯度下降或其他优化方法：
     $$
     P_{s,excat} \leftarrow P_{s,original} - \eta \frac{\partial \mathcal{L}_{\text{recon}}^{\text{adaptive}}}{\partial P_s}
     $$
     其中，$\eta$ 是学习率，$\frac{\partial \mathcal{L}_{\text{recon}}^{\text{adaptive}}}{\partial P_s}$ 是重构损失对$P_s$ 的梯度。第一次迭代使用$P_{s,original}$。每得到一个$\mathbf{x_{new}}$，都只增量更新一次。

1. **更新其他模型参数 ???**：  
   除了投影矩阵 $P_s$，可能还需要根据目标函数的其他部分更新模型的其它参数

![alt text](image-1.png)