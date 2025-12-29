## 潜变量是什么？
![[Pasted image 20250924152949.png]]
**潜变量（Latent variables**：是不能被直接精确测量，但可通过数学模型从可观察变量（显变量）间接推断的变量。
real world:N Dimensions 
world in eyes: 3 d
![[Pasted image 20250924153547.png]]
对于人脑来说：图中的分簇很明显，但是将这一信息使计算机理解？
 ![[Pasted image 20250924162518.png]]


### 高斯混合模型 
GMM：为何“求和”可写成“连乘”的等价联合形式）

给定隐变量 $\(Z\in\{1,\dots,K\}\)$，其先验为 $\(\pi_k=P(Z=k)\)$，且$ $\(\sum_{k=1}^K \pi_k=1,\ \pi_k\ge 0\)$。边缘分布为
$$
P(X)=\sum_{z} P(Z=z)\,P(X\mid Z=z)
=\sum_{k=1}^{K}\pi_k\,\mathcal N\!\big(x\mid \mu_k,\Sigma_k\big).
$$

用 one-hot 向量表示隐变量
$$\[
z=(z_1,\dots,z_K),\quad z_k\in\{0,1\},\quad \sum_{k=1}^K z_k=1,
\]
其中事件 \(Z=k\) 等价于 \(z_k=1\)。则联合分布可以写成“选择性乘积”的形式（只有被选中的分量保留）：
$$
$$P(X,z)=\prod_{k=1}^{K}\Big(\pi_k\,\mathcal N(x\mid \mu_k,\Sigma_k)\Big)^{z_k}.
$$

直观解释：由于 \(z\) 是 one-hot，乘积中仅有一个指数 \(z_k=1\) 的项被“激活”，其余 \(z_j=0\) 的项变为 \(1\)，因此该乘积等价于“在求和里选中某一项”。对数形式便于优化：
$$
\log P(X,z)=\sum_{k=1}^{K} z_k\Big(\log \pi_k+\log \mathcal N(x\mid \mu_k,\Sigma_k)\Big).
$$

将联合分布边缘化即可回到求和表达：
$$
P(X)=\sum_{z}P(X,z)=\sum_{k=1}^{K}\pi_k\,\mathcal N(x\mid \mu_k,\Sigma_k).
$$
![[Pasted image 20250924165100.png]]

### 基于隐变量的图像生成模型
![[Pasted image 20250924165530.png]]
![[Pasted image 20250924171257.png]]
![[Pasted image 20250924171818.png]]
## 潜变量生成模型：编码/采样流程与潜空间要求

### 两种路径
- **训练/重建**：$$(x \xrightarrow{q_\phi(z\mid x)} z \xrightarrow{p_\theta(x\mid z)} \hat{x})  
  （在 (q_\phi(z\mid x))$$ 中对 $(z)$ 采样或取均值）
- **纯生成**：$$(z \sim p(z) \xrightarrow{p_\theta(x\mid z)} x)$$  
  （不需要编码器）

### 潜空间的两个性质
- **连续性（Continuity）**：潜空间中 \(z\) 的小变化应引起图像 \(x\) 的平滑变化。
- **完备性（Coverage/Completeness）**：先验 $(p(z))$ 上**几乎所有**采样到的 $(z)$ 都能被解码为有意义的图像。


> 要点：编码器 \(q_\phi(z\mid x)\) 用于**推断/重建**；**纯生成**时只需从 \(p(z)\) 采样后经解码器生成。
