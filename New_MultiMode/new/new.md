---
export_on_save:
    puppeteer: true # 保存文件时导出 PDF
    
---

$$
\min_{P,E,Z,Q}\sum_{s=1}^S\|Z_s\|_*+\sum_{s=1}^S\lambda\|E_s\|_{2,1}+\sum_{s=1}^S\gamma\|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}XQ_s=XQ_sZ_s+E_s,P_s^TP_s=I
$$

$$
\min_{P,Z,Q}\sum_{s=1}^S\|Q_s\|_F^2+\sum_{s=1}^S\|X_s-XQ_s\|_F^2++\sum_{s=1}^S\gamma\|P_sX_s-P_sX_sZ_s\|_F^2\\
\text{s.t.}X_s=X_sZ_s+E_s,P_s^TP_s=I
$$

其中：
$X=[X_1,X_2,\dots,X_S]\in \mathbb{R} ^{D\times N}$,是多模态数据，D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$X_s\in\mathbb{R} ^{D\times n_s}$,是每个模态的数据
$P_s\in\mathbb{R} ^{d\times D}$,d为降维之后特征数
$Q_s\in\mathbb{R} ^{N\times n_s}$, $n_s$为单个模态数据的样本数
$Z_s\in\mathbb{R} ^{n_s\times n_s}$
$E_s\in\mathbb{R} ^{D\times n_s}$
$$
\downarrow 堆叠
$$
$$
\min_{P,E,Z,Q}\|Z\|_*+\lambda\|E\|_{2,1}+\gamma\|PXQ-PXQZ\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}XQ=XQZ+E,P^TP=I
$$
$X\in \mathbb{R} ^{D\times N}$,D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$P=[P_1;P_2;\dotsb;P_s] \in\mathbb{R} ^{(S\times d)\times D}$
$Q=[Q_1,Q_2,\dotsb,Q_s]\in\mathbb{R} ^{N\times \sum n_s}$, $n_s$为单个模态数据的样本数
$Z=\text{diag}[Z_1,Z_2,\dotsb,Z_s]\in\mathbb{R} ^{\sum n_s\times \sum n_s}$
$E=[E_1,E_2,\dotsb,E_s]\in\mathbb{R} ^{D\times \sum n_s}$
$$
\downarrow 优化
$$
$$
\min_{P,E,Z,Q,J}\|J\|_*+\lambda\|E\|_{2,1}+\gamma\|PXQ-PXQZ\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}XQ=XQZ+E,Z=J,P^TP=I
$$
增广拉格朗日函数
$$
\mathcal{L} (P,E,Z,Q,J,\Lambda,\Pi)=\|J\|_*+\lambda\|E\|_{2,1}+\gamma\|PXQ-PXQZ\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
+\left\langle \Lambda,XQ-XQZ-E \right\rangle +\frac{\mu}{2}\left\lVert XQ-XQZ-E\right\rVert _F^2\\
+\left\langle \Pi,Z-J \right\rangle +\frac{\mu}{2}\left\lVert Z-J\right\rVert _F^2 
$$
$$
\downarrow 化简
$$
$$
\mathcal{L} (P,E,Z,Q,J,\Lambda,\Pi)=\|J\|_*+\lambda\|E\|_{2,1}+\gamma\|PXQ-PXQZ\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
+\left\langle \Lambda,XQ-XQZ-E \right\rangle +\left\langle \Pi,Z-J \right\rangle \\
+\frac{\mu}{2}\left(\left\lVert XQ-XQZ-E\right\rVert _F^2+\left\lVert Z-J\right\rVert _F^2\right)  
$$

$$
\downarrow 化简
$$
$$
\mathcal{L} (P,E,Z,Q,J,\Lambda,\Pi)=\|J\|_*+\lambda\|E\|_{2,1}+\gamma\|PXQ-PXQZ\|_F^2+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
+\frac{\mu}{2}\left(\left\lVert XQ-XQZ-E+\frac{\Lambda}{\mu}\right\rVert _F^2+\left\lVert Z-J+\frac{\Pi}{\mu}\right\rVert _F^2\right)  
$$
$X\in \mathbb{R} ^{D\times N}$,D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$P=[P_1;P_2;\dotsb;P_s] \in\mathbb{R} ^{(S\times d)\times D}$
$Q=[Q_1,Q_2,\dotsb,Q_s]\in\mathbb{R} ^{N\times \sum n_s}$, $n_s$为单个模态数据的样本数
$Z=\text{diag}[Z_1,Z_2,\dotsb,Z_s]\in\mathbb{R} ^{\sum n_s\times \sum n_s}$
$E=[E_1,E_2,\dotsb,E_s]\in\mathbb{R} ^{D\times \sum n_s}$

$$
\mathcal{L} (P_s,E_s,Z_s,Q_s,J_s,\Lambda_s,\Pi_s)=\sum_{s=1}^S\|J_s\|_*+\sum_{s=1}^S\lambda\|E_s\|_{2,1}+\sum_{s=1}^S\gamma\|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\sum_{s=1s ,\not ={t}}^S\beta \text{HSIC}(Z_s,Z_t)\\
+\sum_{s=1}^S\frac{\mu}{2}\left(\left\lVert XQ_s-XQ_sZ_s-E_s+\frac{\Lambda_s}{\mu}\right\rVert _F^2+\left\lVert Z_s-J_s+\frac{\Pi_s}{\mu}\right\rVert _F^2\right)  
$$

2. Z
   
3. P
$$
\min_P \gamma\|PXQ-PXQZ\|_F^2
$$
1. Q
$$
\min_Q \gamma\|PXQ-PXQZ\|_F^2+\frac{\mu}{2}\left\lVert XQ-XQZ-E+\frac{\Lambda}{\mu}\right\rVert _F^2
$$