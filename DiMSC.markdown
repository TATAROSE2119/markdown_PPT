---
marp: true
size: 16:9
paginate: true



style: |
  section {
    font-family: 'Microsoft YaHei', sans-serif;
    font-size: 17px;
    line-height: 1.6;
    position: relative;
  }

  section::before {
    content: "";
    position: absolute;
    top: 0;
    right: 0%;
    left:35%;
    width: 100%;
    height: 100%;
    background-image: url('image/xiaohui.jpg');
    background-position: right;
    background-size: cover;
    background-repeat: no-repeat;
    background-blend-mode: lighten;
    opacity: 0.2;
    z-index: -1;
  }


---

# <!--fit--> 多样性诱导的多视图子空间聚类（DiMSC）
作者：Xiaochun Cao
时间：2015
汇报人：潘岩

---

#### **2. 研究背景与动机**  
- **多视图数据的特点**  
  - 数据从多个视角/特征提取（如颜色、纹理、光流等）。  
  - 不同视图可能包含互补信息，但独立使用时存在信息冗余或不足。互补性：不同视图可能包含不同的信息。例如，颜色特征可能对光照变化敏感，而纹理特征可能对光照变化不敏感。如果能够结合这些视图的互补信息，可以提升聚类性能。
- **现有方法的局限性**  
  - 传统方法（如NaMSC）独立学习视图表示，忽略互补性。每个视图的子空间表示 $Z^{(v)}$ 是通过仅使用该视图的数据来构建的，而不考虑其他视图的信息。
  - 缺乏对视图间依赖性的显式约束。  
- **研究目标**  
  - 提出一种多视图聚类框架，通过HSIC强制视图多样性，提升聚类性能。  

---

#### **3. 相关工作**  
- **多视图聚类方法**  
  - 协同正则化谱聚类（Co-Reg SPC）  
  - 协同训练谱聚类（Co-Train SPC）  
  - 多核学习（MKL）  
- **子空间聚类方法**  
  - 稀疏子空间聚类（SSC）  
  - 低秩表示（LRR）  
  - 平滑表示聚类（SMR）  
- **现有方法的不足**  
  - 未显式约束视图间互补性（仅假设独立性）。  

---

#### **4. 方法概述**  
- **核心思想**  
  - **多样性正则化**：通过HSIC衡量视图间依赖性，强制多样性。  
  - **多视图联合优化**：交替最小化策略更新各视图表示。  

- **目标函数**  
  $$
  \mathcal{O} = \underbrace{\sum_{v} \text{重构误差}}_{\text{保证表示质量}} + \lambda_S \underbrace{\sum_{v} \text{平滑项}}_{\text{局部一致性}} + \lambda_V \underbrace{\sum_{v \neq w} \text{HSIC}}_{\text{多样性约束}}
  $$  
  - **HSIC公式**：  
    $$
    \text{HSIC} = \frac{1}{(n-1)^2} \text{tr}(\mathbf{K}^{(v)} \mathbf{H} \mathbf{K}^{(w)} \mathbf{H})
    $$  
    （Gram矩阵通过$\mathbf{Z}^{(v)^T} \mathbf{Z}^{(v)}$计算）  

- **优化流程**  
  1. 初始化各视图表示（如SMR）。  
  2. **交替优化**：固定其他视图，通过Sylvester方程求解当前视图的$\mathbf{Z}^{(v)}$。  
  3. 迭代至收敛（通常<5次迭代）。  

---

#### **5. 实验与结果**  
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

#### **6. 结论与未来工作**  
- **结论**  
  - HSIC显式约束视图多样性，有效提升聚类性能。  
  - DiMSC在多视图数据（尤其是光照/姿态变化场景）中表现优异。  
- **未来方向**  
  - 扩展至非线性核（如高斯核）。  
  - 探索更高效的多视图联合优化策略。  
---

<style>
  h1 {
    font-size: 90px;
  }
</style>

# 谢谢观看 请批评指正
