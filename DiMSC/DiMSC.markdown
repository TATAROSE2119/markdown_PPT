---
marp: true
size: 16:9
paginate: true



style: |
  section {
    font-family: 'Microsoft YaHei', sans-serif;
    font-size: 19px;
    line-height: 1.5;
    }
  section::before {
    content: "";
    position: absolute;
    top: 0;
    right: 0;
    left: 60;
    width: 120%;
    height: 120%;
    background: url('https://github.com/TATAROSE2119/markdown_PPT/blob/main/DiMSC/img/image.png?raw=true') no-repeat right;
    background-size: contain;
    opacity: 0.1; /* 设置背景图片透明度 */
    pointer-events: none;
    }

---

# 工作汇报

---

# <!--fit--> 多样性诱导的多视图子空间聚类（DiMSC）
作者：Xiaochun Cao
时间：2015
汇报人：潘岩


---


#### **1. 研究背景与动机**  
- **多视图数据的特点**  
  - 数据从多个视角/特征提取（如颜色、纹理、光流等）。  
  - 不同视图可能包含互补信息，但独立使用时存在信息冗余或不足。
  - 互补性：不同视图可能包含不同的信息。例如，颜色特征可能对光照变化敏感，而纹理特征可能对光照变化不敏感。如果能够结合这些视图的互补信息，可以提升聚类性能。
- **朴素方法的局限性**  
  - 论文中先提出了一种朴素方法（如NaMSC）独立学习视图表示，NaMSC方法的目标是将多个视图的数据表示整合到一个共同的子空间中，以便进行聚类。每个视图都使用独立的子空间表示来表示数据，但是它们并未通过任何机制来确保不同视图之间的互补性。
  $$
     \min_{Z^{(v)}} \| X^{(v)} - X^{(v)} Z^{(v)} \|_F^2 + \alpha^{(v)} \Omega(Z^{(v)})
  $$
  其中，$\Omega(Z^{(v)})$ 是一个平滑正则项，强制子空间表示满足分组效果。
  $$
  \Omega(Z^{(v)}) = \frac{1}{2} \sum_{i=1}^{n} \sum_{j=1}^{n} w_{ij}^{(v)} \| z_i^{(v)} - z_j^{(v)} \|^2_2 = \text{tr}(Z^{(v)} L^{(v)} Z^{(v)^T})
  $$

  - 缺乏对视图间依赖性的显式约束。  

---


#### **2. 方法概述**  
- **核心思想**  
  - **多样性正则化**：通过HSIC衡量视图间依赖性，强制多样性。  
  - **多视图联合优化**：交替最小化策略更新各视图表示。  

- **目标函数**  
  $$
  \mathcal{O} = \underbrace{\sum_{v} \text{重构误差}}_{\text{保证表示质量}} + \lambda_S \underbrace{\sum_{v} \text{平滑项}}_{\text{局部一致性}} + \lambda_V \underbrace{\sum_{v \neq w} \text{HSIC}}_{\text{多样性约束}}
  $$  

  $$
  O(Z^{(1)}, \dots, Z^{(V)}) = \sum_{v=1}^{V} \left( \| X^{(v)} - X^{(v)} Z^{(v)} \|_F^2 \right) + \sum_{v=1}^{V} \alpha^{(v)} \text{tr}(Z^{(v)} L^{(v)} Z^{(v)^T}) + \sum_{v \neq w} \lambda_V \text{HSIC}(Z^{(v)}, Z^{(w)})
  $$

  其中，各项的具体含义如下：

  1. **重构误差项**：
   $$
   \| X^{(v)} - X^{(v)} Z^{(v)} \|_F^2
   $$
   度量了每个视图的重构误差，确保学习到的子空间表示 $Z^{(v)}$ 能够尽可能地重构原始数据 $X^{(v)}$。
---
- 
  2. **平滑正则项**：
   $$
   \alpha^{(v)} \text{tr}(Z^{(v)} L^{(v)} Z^{(v)^T})
   $$
   它通过图拉普拉斯矩阵 $L^{(v)}$ 强制相似的样本在子空间表示中保持接近。此项的作用是保持数据点之间的结构一致性，确保同一类的数据点在聚类过程中被归到同一子空间。

  3. **多样性约束项（HSIC项）**：
   $$
   \sum_{v \neq w} \lambda_V \text{HSIC}(Z^{(v)}, Z^{(w)})
   $$
   该项是论文中提出的关键部分，利用 **Hilbert-Schmidt独立性准则（HSIC）** 来强制不同视图之间的多样性。

---  
    
   - **HSIC的定义**
    假设有两个随机变量 $X$ 和 $Y$，它们分别取值于两个不同的空间 $\mathcal{X}$ 和 $\mathcal{Y}$，并且我们有样本数据 $\{(x_i, y_i)\}$ 来估计它们的联合分布。通过映射这两个变量到适当的**再生希尔伯特空间**（RKHS），HSIC可以定义为这两个变量的**交叉协方差算子**的Hilbert-Schmidt范数。具体地，HSIC的定义如下:
    
  $$
    \text{HSIC}(p_{XY}, \mathcal{H}_X, \mathcal{H}_Y) = \|C_{XY}\|^2_{\text{HS}}
    $$
    
  其中，$C_{XY}$ 是通过核函数构建的交叉协方差算子，$\| \cdot \|_{\text{HS}}$ 是Hilbert-Schmidt范数。

---
  
  - **如何计算HSIC**

    由于直接计算HSIC通常涉及到概率密度函数的估计，实际上我们通常使用**经验版本**的HSIC来估算样本数据中的依赖性。对于给定的样本数据集 $\{(x_i, y_i)\}_{i=1}^n$，其经验估计形式为：

    $$
    \text{HSIC}(Z, \mathcal{H}_X, \mathcal{H}_Y) = \frac{1}{(n-1)^2} \text{tr}(K_X H K_Y H)
    $$

    其中：

    - $K_X$ 和 $K_Y$ 是通过核函数分别计算的 $X$ 和 $Y$ 的Gram矩阵（即相似度矩阵），分别对应于数据集中的 $X$ 和 $Y$。
    - $H = I - \frac{1}{n} \mathbf{1}$ 是居中矩阵，$\mathbf{1}$是一个所有元素都为1的列向量。

    通过这个经验版本的HSIC，我们可以计算样本数据中两个视图（或者变量）之间的依赖性。

---
  - **HSIC在DiMSC中**
    **实际上就是将不同的视图使用内积核函数隐式的投影到再生希尔伯特空间，然后根据两个视图在这个空间中的内积判断相似性。**

    $$
    \sum_{w=1, w \neq v}^V \text{HSIC}(Z^{(v)}, Z^{(w)}) = \text{tr}(Z^{(v)} H K Z^{(v)^T})
    $$

    这个项的作用是**惩罚**视图之间的依赖性，即如果两个视图的表示 $Z^{(v)}$ 和 $Z^{(w)}$之间存在强烈的依赖性（例如冗余信息较多），HSIC将返回一个较大的值，优化过程中通过减小HSIC的值来减少视图间的冗余，提升其互补性，从而提高聚类效果。

- **优化流程**  
  1. 初始化各视图表示（如SMR）。  
  2. **交替优化**：固定其他视图，通过Sylvester方程求解当前视图的$\mathbf{Z}^{(v)}$。  
  3. 迭代至收敛（通常<5次迭代）。  

---


#### **3. 实验与结果**  
- **数据集**  
  - Yale（人脸，光照/表情变化）  
  - Extended YaleB（大范围光照变化）  
  - ORL（姿态/表情变化）  
  - Notting-Hill（视频人脸聚类）  

- **对比方法**  
  - Single\_best, FeatConcate, Co-Reg SPC, Co-Train SPC, NaMSC等。  

- **评价指标**  
  - NMI, ACC, AR, F-score, Precision, Recall。  
- **关键结果**  
  - **DiMSC全面领先**：在NMI、ACC等指标上显著优于基线（例：Extended YaleB的NMI提升4.1%）。  
  - **多样性有效性**：HSIC项显著减少冗余（对比NaMSC的相似性矩阵可视化）。  
  - **高效收敛**：5次迭代内收敛。  

---


<style>
  h1 {
    font-size: 90px;
  }
</style>

# 谢谢观看 请批评指正
