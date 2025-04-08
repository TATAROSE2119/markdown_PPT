---
export_on_save:
    puppeteer: true # 保存文件时导出 PDF
    
---

$$
\min_{P,Z,Q,E}\alpha \sum_{s=1}^S\|Z_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_s\|_{2,1}+\delta \sum_{s=1}^S\|X_s-XQ_s\|_F^2+\kappa  \sum_{s=1}^S\|P_sX_s-P_sX_sZ_s\|_F^2+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}X_s=X_sZ_s+E_s,P_s^TP_s=I
$$

其中：
$X=[X_1,X_2,\dots,X_S]\in \mathbb{R} ^{D\times N}$,是多模态数据，D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$X_s\in\mathbb{R} ^{D\times n_s}$,是每个模态的数据
$P_s\in\mathbb{R} ^{d\times D}$,d为降维之后特征数
$Q_s\in\mathbb{R} ^{N\times n_s}$, $n_s$为单个模态数据的样本数
$Z_s\in\mathbb{R} ^{n_s\times n_s}$
$E_s\in\mathbb{R} ^{D\times n_s}$