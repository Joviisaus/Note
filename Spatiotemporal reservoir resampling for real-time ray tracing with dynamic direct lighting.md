---
tags:
  - Research
  - Rendering
  - Paper
---

## Preliminaries
The Reflect Radiance $L$ of a point $y$ in direction $\overrightarrow{\omega}$  is given by an integral of all light emitting from $A$
$$L(y, \omega) = \int_{A} \rho(y, \overrightarrow{yx} \leftrightarrow \vec{\omega}) L_{e}(x \rightarrow y) G(x \leftrightarrow y) V(x \leftrightarrow y) \mathrm{d} A_{x},$$
for [[BSDF]] $ρ$, emitted radiance $L_e$ , mutual visibility $V$ between $x$ and $y$  , and a geometry term $G$ containing inverse squared distance and cosine terms.

**Importance Sampling(IS)**:Standard Monte Carlo importance sampling
$$\langle L\rangle_{\mathrm{is}}^{N} = \frac{1}{N} \sum_{i=1}^{N} \frac{f(x_{i})}{p(x_{i})} \approx L.$$
here choosing $N$ Samples $x_i$ from a [[PDF]] $p(x_i)$

**Multiple Importance Sampling (MIS)** :Draw samples proportional to individual terms in the integrand $$\langle L\rangle_{\mathrm{mis}}^{M, N}=\sum_{s=1}^{M} \frac{1}{N_{s}} \sum_{i=1}^{N_{s}} w_{s}(x_{i}) \frac{f(x_{i})}{p_{s}(x_{i})}$$
here $\omega_s$ satisfies a partition of unity $\sum_{s=1}^{M} w_{s}(x) = 1$  and $w_{s}(x) = \frac{N_{s} p_{s}(x)}{\sum_{j} N_{j} p_{j}(x)}$ is a popular and provably good choice.

### Resampled Importance Sampling (RIS)
Sample approximately proportional to the  product of some of the terms
Generating $M \ge 1$ candidate samples $x = \{x_1,...,x_M\}$ from a source distribution, and randomly chooses an index $z\in \{1,...M\}$ $$p(z \mid \mathbf{x}) = \frac{\text{w}(x_{z})}{\sum_{i=1}^{M} \text{w}(x_{i})} \quad \text{with} \quad \text{w}(x) = \frac{\hat{p}(x)}{p(x)}$$
A sample of $y \equiv x_z$ used in RIS simulator$$\langle L\rangle_{\mathrm{ris}}^{1, M} = \frac{f(y)}{\hat{p}(y)} \cdot \left( \frac{1}{M} \sum_{j=1}^{M} \text{w}(x_{j}) \right)$$
Repeating RIS multiple times and averaging the results yields an  N-sample RIS estimator $$\langle L\rangle_{\mathrm{ris}}^{N, M} = \frac{1}{N} \sum_{i=1}^{N} \left( \frac{f(y_{i})}{\hat{p}(y_{i})} \cdot \left( \frac{1}{M} \sum_{j=1}^{M} \mathrm{w}(x_{ij}) \right) \right)$$

**Combining RIS with MIS**
the shape of the source PDF $p$ influences both the effective  PDF of $y$ and the speed it converges to $\hat{p}$.

generate the pool of proposals using MIS and use the effective MIS (mixture) PDF as the source PDF in the rest of the RIS procedure.
### Weighted Reservoir Sampling
A family of algorithms for sampling $N$ random elements from a stream $\{ x_1,x_2, x_3,  . . . , x_M \}$
Each element has a weight $\text{w}_i$ 
$$P_{i} = \frac{\mathrm{w}(x_{i})}{\sum_{j=1}^{M} \mathrm{w}(x_{j})}$$
Reservoir sampling algorithms are classified based on whether element $x_i$ may appear multiple times in the output set.Update rule stochastically replaces $x_i$ in the  reservoir with the next sample $x_{m+1}$ with probability.$$\frac{\text{w}(x_{m+1})}{\sum_{j=1}^{m+1} \text{w}(x_{j})}$$
To ensure invariant, other samples' probability needs to be changed $$\frac{\mathrm{w}(x_{i})}{\sum_{j=1}^{m} \mathrm{w}(x_{j})} \left(1 - \frac{\mathrm{w}(x_{m+1})}{\sum_{j=1}^{m+1} \mathrm{w}(x_{j})}\right) = \frac{\mathrm{w}(x_{i})}{\sum_{j=1}^{m+1} \mathrm{w}(x_{j})}$$
