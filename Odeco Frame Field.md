---
tags:
  - Paper
  - mesh
aliases:
  - Designing 3D Anisotropic Frame Fields with Odeco Tensors
---
## Odeco Tensor Representation
Defined on:
- a tetrahedral mesh $\Omega = (\mathcal{V}, \mathcal{T})$ 
- Boundary domian is $\partial \Omega$ 
- Odeco tensor field $f$ defined on vertices $\mathcal{V}$ 
- $f$ denoted as $\Theta = \theta_1, \cdots, \theta_{|\mathcal{V}|}$ and $\Lambda = \lambda_1, \cdots, \lambda_{|\mathcal{V}|},$
- $\theta_i = (\theta_i^x, \theta_i^y, \theta_i^z) \in \mathbb{R}^3$ : Euler angles to control oriention
- $\lambda_i = (\lambda_i^x, \lambda_i^y, \lambda_i^z) \in \mathbb{R}^3$ : stretching ratios to control anisotropy

### Odeco Polynomial
To build a **one-to-one correspondence** between $T = \sum_{i=1}^{n} \lambda_i v_i^{\otimes d} \in S^d(\mathbb{R}^n)$ and homogeneous polynomials $f$ of degree $d$ in $n$ variables $x = (x_1,...,x_n)$ .

$T$ is odeco if and only if the coefficients $u$ of $f$ satisfy $u^TA_mu = 0$ for a finite set of symmetric $A_m$.  

>Approve:
>1. Necessity:
>	 Consider $f(x) = \sum u_{d_1, \dots, d_n} \prod_{i=1}^n x_i^{d_i}$ , for each $0<i<n$ ,compute $\frac{\partial f}{\partial x_i}$ and transfer the result into matrix $M_i$  must satisfy $$\forall j \neq i \quad M_iM_j-M_jM_i = 0$$ which means each partial derivatives commutes. For each element in $M_iM_j-M_jM_i$ it can be transfered into $u^TA_mu$, so that finite symmertric $A_m$ satisfies $u^TA_mu = 0$
>2. Suffeciecy:
>	

### Interior Odeco Tensor
Decompose each odeco tensor $f_i$ into two components: one for the orientation $\theta_i$ and one for the strstching $\lambda_i$
$$f(\theta_i, \lambda_i) = \underbrace{e^{\theta_i^x L_x} e^{\theta_i^y L_y} e^{\theta_i^z L_z}}_{\text{orientation}} \underbrace{\hat{f}(\lambda_i)}_{\text{stretching}}, \quad \forall i \in \Omega$$
Here $L_x, L_y, L_z \in \mathbb{R}^{15 \times 15}$ are the angular momentum  operator,and $\hat{f}$ is an odeco tensor.

$\hat{f}(\lambda_i)$ is computed by projection odeco polynomial $h = \lambda_i^x x^4 + \lambda_i^y y^4 + \lambda_i^z z^4$ to the [[Sphere Harmonic]] basics as $$\hat{f}(\lambda_i) = B\lambda_i$$
where $B \in \mathbb{R}^{15 \times 3}$ is a constant matrix.

![[Pasted image 20251226204203.png]]

### Boundary Odeco Tensor
Only rotate around $\vec{n}_i$ , expression can be simplified as $$f(\theta_i, \lambda_i) = f(\theta_i^z, \lambda_i) = R_i e^{\theta_i^z L_z} \hat{f}(\lambda_i), \quad \forall i \in \partial\Omega$$
## Compute a 3D anisotropic odeco tensor  field (AOTF)
### Formulation

$$\begin{align*}
\min_{\Theta, \Lambda} E_T &= E_s(\Theta, \Lambda) + \psi E_\Lambda(\Lambda), \\
E_s(\Theta, \Lambda) &= \sum_{i,j \in \Omega} w_{ij} \| f(\theta_i, \lambda_i) - f(\theta_j, \lambda_j) \|_2^2, \\
E_\Lambda(\Lambda) &= \sum_{i \in \Omega} \| \lambda_i - \lambda_i^{In} \|_2^2,
\end{align*}$$

where the total energy $E_T$ consists of a Dirichlet energy $E_s$ for measuring the smothness of the field. $E_{\Lambda}$ denotes a soft penalty term to align user-specified stretching ratios $\Lambda^{In}$ ,$\psi$ default as 50.

### Optimization
warm start  **L-BFGS** 
[[Euler Integration]]