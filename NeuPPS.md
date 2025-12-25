---
tags:
  - mesh
  - Paper
---

给定的任意拓扑都可以学习出一套精准可以对齐的参数化表达
## Neural Piecewise Parametric Surfaces
将带有Patch分布的 3D模型，构建2-mainifold feature complex $C$ 嵌入 D 维特征空间的映射（D取88)
![[Pasted image 20251201162105 1.png]]
对于 $C_i$ ,由点进行均值差值 $z(u)$ 表示

$z(u)$ :将参数提升至$D$维空间

$$ f_{\theta}:z \in R^D \rightarrow x \in R ^3$$
## Training
![[Pasted image 20251202150532 1.png]]
在每个顶点定义一个高维向量$Z$，首先全局参数化，流形上每个空间获得一个$u$,多边形的MVC也在这个参数化平面上构建

定义 Loss：
$$ \mathcal{L}_{\text{mesh}} = \sum_{i} \mathcal{L}_{\text{patch}}^{i}, $$
$$ \mathcal{L}_{\text{patch}} = \mathcal{L}_{\text{surface}} + \lambda_{\text{normal}} \mathcal{L}_{\text{normal}} + \lambda_{\text{param}} \mathcal{L}_{\text{param}}, \tag{2} $$
其中
$$\mathcal{L}_{\text{surface}} = \int_{\mathbf{u} \in \Omega_{i}} \| \mathbf{x}(\mathbf{u}) - \mathbf{p}_{\mathbf{u}} \|_{\text{L1}},$$
$$ \mathcal{L}_{\text{normal}} = \int_{\mathbf{p} \in \mathcal{M}_{i}} \| \mathbf{n}(\mathbf{x}) - \mathbf{n}(\mathbf{p}) \|_{\text{L1}}, $$
$$\mathcal{L}_{\text{param}} = \int_{\mathbf{p} \in M_{i}} \| \partial \mathbf{x} / \partial u - \partial \mathbf{p} / \partial u \|_{\text{L2}} + \| \partial \mathbf{x} / \partial v - \partial \mathbf{p} / \partial v \|_{\text{L2}}.$$
参数化方法：Progressive Parameterization (PP) algorithm，没有提及怎么做无缝，只讲了每个块参数化后保证在多边形上

## Patch Segmantation
1. 与拓扑圆盘拓扑同胚
2. 角点得是流形拓扑