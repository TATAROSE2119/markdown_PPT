$$
\min_{P,E,Z,Q}\sum_{s=1}^S\left(\|Z_s\|_*+\lambda\|E_s\|_{2,1}+\gamma\|P_sXQ_s-P_sXQ_sZ_s\|_F^2\right)+\beta \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}XQ_s=XQ_sZ_s+E_s,P_s^TP_s=I
$$
其中：
$X\in \mathbb{R} ^{D\times N}$,D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
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