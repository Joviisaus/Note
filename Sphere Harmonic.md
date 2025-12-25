---
tags:
  - Definition
  - Rendering
---

Assume the function can be written as a product of three independent parts
$$f(r, \theta, \phi) = \underbrace{R(r)}_{\text{Distance}} \cdot \underbrace{\Theta(\theta)}_{\text{Latitude}} \cdot \underbrace{\Phi(\phi)}_{\text{Longitude}}$$
The Sphere Harmonic basises is built to simulate the **Latitude** and **Longitude**. 
$$
f(\theta, \phi) = \sum_{\ell=0}^{\infty} \sum_{m=-\ell}^{\ell} a_{\ell m} Y_{\ell}^m(\theta, \phi)$$
Here $m$ and $l$ means the order and $a$ is the paramenter to be computed for each function. 
$$Y_{\ell}^m(\theta, \phi) = \sqrt{\frac{(2\ell + 1)}{4\pi} \frac{(\ell - m)!}{(\ell + m)!}} P_{\ell}^m(\cos \theta) e^{im\phi}$$
