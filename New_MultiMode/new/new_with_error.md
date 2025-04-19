---
export_on_save:
    puppeteer: true # 保存文件时导出 PDF
---
# 逐步构建过程
$X_s=XQ_s+E_{1,s}$找出选择矩阵$Q_s$

$$
\min_{Q_s}\sum_{s=1}^S\|Q_s\|_F^2+\sum_{s=1}^S\|E_{1,s}\|_{2,1} \\
\text{s.t.}X_s=XQ_s+E_{1,s}
$$
其中：

$X=[X_1,X_2,\dots,X_S]\in \mathbb{R} ^{D\times N}$,是多模态数据，D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$X_s\in\mathbb{R} ^{D\times n_s}$,是每个模态的数据
$P_s\in\mathbb{R} ^{d\times D}$,d为降维之后特征数
$Q_s\in\mathbb{R} ^{N\times n_s}$,（什么功能的矩阵？） $n_s$为单个模态数据的样本数。
$Z_s\in\mathbb{R} ^{n_s\times n_s}$
$E_s\in\mathbb{R} ^{D\times n_s}$
加入低秩表示投影和HSIC约束
$$
\min_{P,Z,Q,E_1,E_2} \sum_{s=1}^S\|Z_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_{1,s}\|_{2,1}+\delta \sum_{s=1}^S\|E_{2,s}\|_{2,1}\\
+\kappa  \sum_{s=1}^S\|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}X_s=XQ_s+E_{1,s},XQ_s=XQ_sZ_s+E_{2,s},P_s^TP_s=I
$$

引入辅助变量J,Z=J
$$
\min_{P,Z,Q,E_1,E_2,J} \sum_{s=1}^S\|Z_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_{1,s}\|_{2,1}+\delta \sum_{s=1}^S\|E_{2,s}\|_{2,1}\\
+\kappa  \sum_{s=1}^S\|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}X_s=XQ_s+E_{1,s},XQ_s=XQ_sZ_s+E_{2,s},Z_s=J_s,P_s^TP_s=I
$$
构建增广拉格朗日函数
$$
\begin{aligned}
&\mathcal{L} (P,Z,Q,E_1,E_2,J,Y_1,Y_2,Y_3)\\
&=\sum_{s=1}^S\|J_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_{1,s}\|_{2,1}+\delta \sum_{s=1}^S\|E_{2,s}\|_{2,1}\\
&+\kappa  \sum_{s=1}^S\|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
&+\frac{\mu}{2}\|X_s-XQ_s-E_{1,s}+\frac{Y_1}{\mu}\|\\
&+\frac{\mu}{2}\|XQ_s-XQ_sZ_s-E_{2,s}+\frac{Y_2}{\mu}\|\\
&+\frac{\mu}{2}\|Z_s-J_s+\frac{Y_3}{\mu}\|\\
\end{aligned}
$$
HSIC约束选择内积核函数$K_s=Z_s^TZ_s$
$$
\sum_{t=1,t \not ={s}}^S\text{HSIC}(Z_s,Z_t)=\sum_{t=1,t \not ={s}}^S\text{Tr}(HK_sHK_t)=\sum_{t=1,t \not ={s}}^S\text{Tr}(Z_sHK_tHZ_s^T)=\text{Tr}(Z_sKZ_s^T)
$$
其中：$H=I-\frac{1}{n}11^T$是中心化矩阵，$K_t=Z_t^TZ_t,K=\sum_{t=1,t \not ={s}}^SHK_tH$
每次固定一个工况，拉格朗日函数可以重写为：
$$
\begin{aligned}
&\mathcal{L} (P,Z,Q,E_1,E_2,J,Y_1,Y_2,Y_3)\\
&=\|J_s\|_*+\beta \|Q_s\|_F^2+\gamma \|E_{1,s}\|_{2,1}+\delta \|E_{2,s}\|_{2,1}\\
&+\kappa  \|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\lambda \text{Tr}(Z_sKZ_s^T)\\
&+\frac{\mu}{2}\|X_s-XQ_s-E_{1,s}+\frac{Y_1}{\mu}\|\\
&+\frac{\mu}{2}\|XQ_s-XQ_sZ_s-E_{2,s}+\frac{Y_2}{\mu}\|\\
&+\frac{\mu}{2}\|Z_s-J_s+\frac{Y_3}{\mu}\|\\
\end{aligned}
$$
# 更新
## 1. J ...
## 2. Z

```matlab
kappa*tr((P*X*Q-P*X*Q*Z)*(P*X*Q-P*X*Q*Z)')
+mu/2*tr((X*Q-X*Q*Z-E2+Y2/mu)*(X*Q-X*Q*Z-E2+Y2/mu)')
+mu/2*tr((Z-J+Y3/mu)*(Z-J+Y3/mu)')
+lambda*tr(Z*K*Z')
```
function:
\[
\begin{aligned}
  &f = kappa \mathrm{tr}((P X Q-P X Q Z) (P X Q-P X Q Z)^\top )\\
  &+mu/2 \mathrm{tr}((X Q-X Q Z-E2+Y2/mu) (X Q-X Q Z-E2+Y2/mu)^\top )\\
  &+mu/2 \mathrm{tr}((Z-J+Y3/mu) (Z-J+Y3/mu)^\top )\\&+lambda \mathrm{tr}(Z K Z^\top )
\end{aligned}
\]

gradient:
\[
\begin{aligned}
  \frac{\partial f}{\partial Z}\\
  & = mu (Z-J+1/mu Y3)-2 kappa (P X Q)^\top  (P X Q-P X Q Z)\\
  &-mu (X Q)^\top  (X Q-X Q Z-E2+1/mu Y2)\\
  &+lambda Z K^\top +lambda Z K
\end{aligned}
\]
let =0
$$
\begin{aligned}
Z_{\text{set}\{i\}} = & \left( \mu + 2  \kappa  \left( P_{\text{set}\{i\}}  X  Q_{\text{set}\{i\}} \right)^\top  P_{\text{set}\{i\}}  X  Q_{\text{set}\{i\}} - \mu  \left( X  Q_{\text{set}\{i\}} \right)^\top  X  Q_{\text{set}\{i\}} \right. \\
& \left. + \lambda  \left( K^\top + K \right) \right)^{-1} \\
&  \left( \mu  J_{\text{set}\{i\}} - Y_{3_{\text{set}\{i\}}} + 2  \kappa  \left( P_{\text{set}\{i\}}  X  Q_{\text{set}\{i\}} \right)^\top  P_{\text{set}\{i\}}  X  Q_{\text{set}\{i\}} \right. \\
& \left. - \mu  \left( X  Q_{\text{set}\{i\}} \right)^\top  X  Q_{\text{set}\{i\}} + \mu  \left( X  Q_{\text{set}\{i\}} \right)^\top  E_{2_{\text{set}\{i\}}} - Y_{2_{\text{set}\{i\}}} \right)
\end{aligned}
$$

## 3. P ...

## 4. Q 
$$
\min_Q \beta \|Q_s\|_F^2+\kappa  \|P_sXQ_s-P_sXQ_sZ_s\|_F^2+\frac{\mu}{2}\|X_s-XQ_s-E_{1,s}+\frac{Y_1}{\mu}\|_F^2\\
+\frac{\mu}{2}\|XQ_s-XQ_sZ_s-E_{2,s}+\frac{Y_2}{\mu}\|_F^2
$$
function:
\[
\begin{aligned}
  &f = beta \mathrm{tr}(Q Q^\top )+kappa \mathrm{tr}((P X Q-P X Q Z) (P X Q-P X Q Z)^\top )\\
  &+mu/2 \mathrm{tr}((X Q-X Q Z-E2+Y2/mu) (X Q-X Q Z-E2+Y2/mu)^\top )\\
  &+mu/2 \mathrm{tr}((X_s-X Q-E1+Y1/mu) (X_s-X Q-E1+Y1/mu)^\top )\\
\end{aligned}
\]

gradient:
\[
\begin{aligned}
  &\frac{\partial f}{\partial Q} = 2 beta Q\\
  &+2 kappa (P X)^\top  (P X Q-P X Q Z)-2 kappa (P X)^\top  (P X Q-P X Q Z) Z^\top \\
  &+mu X^\top  (X Q-X Q Z-E2+1/mu Y2)- mu X^\top  (X Q-X Q Z-E2+1/mu Y2) Z^\top \\
  &-mu X^\top  (Xs-X Q-E1+1/mu Y1)
\end{aligned}
\]

## 5. E1

## 6. E2