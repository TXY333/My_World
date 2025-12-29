<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

  

### 公式组成与符号解释

  

**背景：** 本组公式描述的是图像与latent code的双向对比学习损失，两者分别通过特征提取器（CNN、Transformer）和投影头，得到高维embedding，然后在batch内进行配对、归一化和概率化处理。

  

***

  

#### 1. 图像到latent code方向损失（\$ \mathcal{L}_i^{(I \to w)} \$）

  

$$

\mathcal{L}_i^{(I \to w)} = -\log\left[ \frac{\exp\left(\langle h_I(I_i), h_w(w_i) \rangle / t \right)}{\sum_{k=1}^{S} \exp\left(\langle h_I(I_i), h_w(w_k) \rangle / t \right)} \right]

$$

  

- **\$ h_I(I_i) \$：** 第i个图像经过特征提取和投影头得到的embedding。

- **\$ h_w(w_i) \$：** 第i个latent code经过特征提取和投影头得到的embedding。

- **\$ \langle h_I(I_i), h_w(w_i) \rangle \$：** 两embedding的余弦相似度（衡量它们的接近程度）。

- **分子：** 是同batch里的i配对（正样本），经过温度t归一化后的相似度。

- **分母：** 是当前图像embedding与所有batch内latent code embedding（包括正负样本）的归一化权重之和。

- **t：** 可学习的温度参数，调节分布的尖锐程度。

- **损失：** 使正样本相似度最大化，负样本相似度最小化。

  
  

#### 2. latent code到图像方向损失（\$ \mathcal{L}_i^{(w \to I)} \$）

  

$$

\mathcal{L}_i^{(w \to I)} = -\log\left[ \frac{\exp\left(\langle h_w(w_i), h_I(I_i) \rangle / t \right)}{\sum_{k=1}^{S} \exp\left(\langle h_w(w_i), h_I(I_k) \rangle / t \right)} \right]

$$

  

- 这一步是交换anchor和样本方向，目的是让latent code also能聚合到对应图像特征。

- 本质同上，但每个latent code当anchor，正样本是对应图像，其余batch内图像都是负样本。

  
  

#### 3. 总对比损失（\$ \mathcal{L}_align \$）

  

$$

\mathcal{L}_\text{align} = \frac{1}{S} \sum_{i=1}^{S} \left( \lambda \mathcal{L}_i^{(I \to w)} + (1-\lambda) \mathcal{L}_i^{(w \to I)} \right)

$$

  

- **S：** batch内对数。

- **\$ \lambda \$：** 损失权重（如文中为0.5），平衡两个方向的贡献，一般取0.5即两者平均。

- 对每对样本，既让图像特征聚合到对应latent code，也让latent code聚合到对应图像特征。

- 总损失是所有pair损失的平均。

  

***

  

### 工作方式总结

  

1. **正向和反向双重对比。** 每个图像和latent code配对，同时作为anchor，分别和所有同batch其他样本对比。

2. **归一化为概率后用对数损失。** 最大化正确配对的概率，最小化错误配对概率，让两空间对齐。

3. **梯度推动。** 若正样本不够近，负样本过近，梯度自动推动特征更紧密、区分度更强。

4. **温度t调节软max分布。** t小分布更尖锐，更强调难区分的负样本。

  

**这样优化后，模型既能让图像与latent code embedding高度语义一致，也能增强跨模态特征的鲁棒性和泛化能力。**

  

如果你需要详细的反向传播推导或代码实现分析，也可以继续提问！

<span style="display:none">[^1]</span>

  

<div align="center">⁂</div>

  

[^1]: image.jpg