---
export_on_save:
    puppeteer: true # 保存文件时导出 PDF
---
$$
\min_{P,Z,Q,E} \sum_{s=1}^S\|Z_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_s\|_{2,1}+\delta \sum_{s=1}^S\|X_s-XQ_s\|_F^2+\kappa  \sum_{s=1}^S\|P_sX_s-P_sX_sZ_s\|_F^2+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}X_s=X_sZ_s+E_s,P_s^TP_s=I
$$

其中：
$X=[X_1,X_2,\dots,X_S]\in \mathbb{R} ^{D\times N}$,是多模态数据，D为原始数据的特征数，N为所有模态数据的样本总数$ N=\sum^{S}n_s $
$X_s\in\mathbb{R} ^{D\times n_s}$,是每个模态的数据
$P_s\in\mathbb{R} ^{d\times D}$,d为降维之后特征数
$Q_s\in\mathbb{R} ^{N\times n_s}$, $n_s$为单个模态数据的样本数
$Z_s\in\mathbb{R} ^{n_s\times n_s}$
$E_s\in\mathbb{R} ^{D\times n_s}$
引入辅助变量J
$$
\min_{P,Z,Q,E,J} \sum_{s=1}^S\|J_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_s\|_{2,1}\\
+\delta \sum_{s=1}^S\|X_s-XQ_s\|_F^2+\kappa  \sum_{s=1}^S\|P_sX_s-P_sX_sZ_s\|_F^2\\
+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
\text{s.t.}X_s=X_sZ_s+E_s,P_s^TP_s=I,Z=J
$$
$$
\Downarrow 构建增广拉格朗日函数
$$
$$
\mathcal{L} (Z,P,Q,E,J,\Gamma ,\Delta ) \sum_{s=1}^S\|J_s\|_*+\beta \sum_{s=1}^S\|Q_s\|_F^2+\gamma \sum_{s=1}^S\|E_s\|_{2,1}\\
+\delta \sum_{s=1}^S\|X_s-XQ_s\|_F^2+\kappa  \sum_{s=1}^S\|P_sX_s-P_sX_sZ_s\|_F^2\\
+\lambda \sum_{s \not ={t}}\text{HSIC}(Z_s,Z_t)\\
+\frac{\mu}{2}\sum_{s=1}^S\left(\left\lVert X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right\rVert_F^2+\left\lVert Z_s-J_s+\frac{\Delta}{\mu}\right\rVert_F^2 \right) 
$$

一次固定一个模式
$$
\mathcal{L} (Z,P,Q,E,J,\Gamma ,\Delta ) \|J_s\|_*+\beta \|Q_s\|_F^2+\gamma \|E_s\|_{2,1}\\
+\delta \|X_s-XQ_s\|_F^2+\kappa  \|P_sX_s-P_sX_sZ_s\|_F^2\\
+\lambda \sum_{t=1,t \not ={s}}^S\text{HSIC}(Z_s,Z_t)\\
+\frac{\mu}{2}\left(\left\lVert X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right\rVert_F^2+\left\lVert Z_s-J_s+\frac{\Delta}{\mu}\right\rVert_F^2 \right) 
$$
HSIC约束选择内积核函数$K_s=Z_s^TZ_s$
$$
\sum_{t=1,t \not ={s}}^S\text{HSIC}(Z_s,Z_t)=\sum_{t=1,t \not ={s}}^S\text{Tr}(HK_sHK_t)=\sum_{t=1,t \not ={s}}^S\text{Tr}(Z_sHK_tHZ_s^T)=\text{Tr}(Z_sKZ_s^T)
$$
其中：
$$
K=\sum_{t=1,t \not ={s}}^SHK_tH
$$
$$
\begin{aligned}
&\mathcal{L} (Z,P,Q,E,J,\Gamma ,\Delta ) \|J_s\|_*+\beta \|Q_s\|_F^2+\gamma \|E_s\|_{2,1}\\
&+\delta \|X_s-XQ_s\|_F^2+\kappa  \|P_sX_s-P_sX_sZ_s\|_F^2\\
&+\lambda \text{Tr}(Z_sKZ_s^T)\\
&+\frac{\mu}{2}\left(\left\lVert X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right\rVert_F^2+\left\lVert Z_s-J_s+\frac{\Delta}{\mu}\right\rVert_F^2 \right) 
\end{aligned}
$$
其中：
$$
K=\sum_{t=1,t \not ={s}}^SHK_tH
$$
$H=I-\frac{1}{n}11^T$是中心化矩阵，
$$
\Downarrow 更新
$$

#### step 1 J
$$
J_{k+1}=\Theta (\frac{1}{\mu})\left(Z+\frac{\Delta}{\mu}\right) 
$$
#### step 2 Z
$$
\min_{Z_s} \kappa  \|P_sX_s-P_sX_sZ_s\|_F^2
+\lambda \text{Tr}(Z_sKZ_s^T)
+\frac{\mu}{2}\left(\left\lVert X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right\rVert_F^2+\left\lVert Z_s-J_s+\frac{\Delta}{\mu}\right\rVert_F^2 \right)
$$
范数转化成tr的形式
$$
\begin{aligned}
&\min_{Z_s} \kappa \text{Tr}((P_sX_s-P_sX_sZ_s)(P_sX_s-P_sX_sZ_s)^T)+\lambda \text{Tr}(Z_sKZ_s^T)\\
 &+\frac{\mu}{2}\left(\text{Tr}\left(\left(X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right) \left(X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right)^T\right) +\text{Tr}\left(\left(Z_s-J_s+\frac{\Delta}{\mu}\right) \left(Z_s-J_s+\frac{\Delta}{\mu}\right)^T\right) \right)
\end{aligned}
$$
求导
function:
$$
  f = kappa\cdot \mathrm{tr}((P\cdot X-P\cdot X\cdot Z)\cdot (P\cdot X-P\cdot X\cdot Z)^\top )+lambda\cdot \mathrm{tr}(Z\cdot K\cdot Z^\top )\\
  +mu/2\cdot (\mathrm{tr}((X-X\cdot Z-E+Lambda/2)\cdot (X-X\cdot Z-E+Lambda/2)^\top )+\mathrm{tr}((Z-J+Delta/2)\cdot (Z-J+Delta/2)^\top ))
$$

gradient:
$$
  \frac{\partial f}{\partial Z} = lambda\cdot Z\cdot K^\top -2\cdot kappa\cdot (P\cdot X)^\top \cdot (P\cdot X-P\cdot X\cdot Z)\\
  +lambda\cdot Z\cdot K-mu\cdot X^\top \cdot (X-X\cdot Z-E+1/2\cdot Lambda)+mu\cdot (Z-J+1/2\cdot Delta)
  \\=0
$$
$\rightarrow $
$$
(2\kappa A^TA+\mu X^TX+\mu I)Z+ Z\lambda(K^T+K)=2\kappa A^TA+\mu X^TX-\mu X^T(E-\Lambda/2)+\mu J-\mu /2 \Delta
$$
```matlab
Z = sylvester(A, B, C);
```
#### step 3 P

$$
\min_{P_s} \kappa  \|P_sX_s-P_sX_sZ_s\|_F^2 \\
=\min_{P_s} \kappa\text{Tr}(PX(Z-I)(Z-I)^TX^TP^T)
$$
转化为求特征值
$$
X(\kappa(Z-I)(Z-I))X^Tp=\xi p
$$
选前lower_dim个向量拼接成$P_s^T$

#### step 4 Q
$$
\min_{Q_s} \beta \|Q_s\|_F^2+\delta \|X_s-XQ_s\|_F^2
$$
求导=0
$$
Q_s=(\beta I+\delta X^TX)^{-1}\delta X^TX_s
$$


#### step 5 E
$$
\min_{E_s}\gamma \|E_s\|_{2,1}+\frac{\mu}{2}\left\lVert X_s-X_sZ_s-E_s+\frac{\Gamma}{\mu}\right\rVert_F^2
$$
 \( V = X_s - X_SZ_s + \frac{\Gamma}{\mu} \)
 \[
[E^*]_{:,i} =
\begin{cases}
\left[ V_{:,i} \right]_2 - \alpha & \text{if } \| V_{:,i} \|_2 > \alpha \\
0 & \text{otherwise}
\end{cases} 
\]
where \( [E^*]_{:,i} \) is the column of the optimal solution \( E^* \).

#### step 5 others

$$
\Gamma _{k+1}=\Gamma_{k}+\mu_k \left( X - X Z_{k+1}  - E_{k+1} \right)\\
\Delta _{k+1} = \Delta_{k} + \mu_k \left( Z_{k+1} - J_{k+1} \right)\\
\mu_{k+1} = \min(\rho \mu_k, \mu_{\max})
$$