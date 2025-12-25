---
tags:
  - Rendering
  - Definition
---

**Probability Density Function (PDF)** :In rendering, it is the mathematical tool we use to handle the transition from a continuous integral to a discrete summation.

## Common PDF examples :
- **Uniform Sampling:** $p(x)=\frac{1}{A}$​ (Every point on a light source has an equal chance).
- **Cosine Sampling:** $p(x)=\frac{cosθ}{\pi}​$ (Samples are concentrated where the [[BSDF]] is strongest).
- **Light Sampling:** $p(x)$ is proportional to the intensity of the light source $L_e$​.