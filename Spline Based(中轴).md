---
tags:
  - mesh
  - Paper
---

# Mesh Generation
Method for circular vessel and generalized to general convex cross.

inputs：a spline-based representation of the centerline curve and a convex parametrization of the contour of the vessel section.
## Single Branch
![[Pasted image 20250714125843.png]]
a cubic spline curve is given to describe the centerline.
$$ \mathbf{s}(t) = \sum_{i} \mathbf{b}_i N_{i,3}(t)$$
for each $t_0 \in [0,1]$ the radius of the vessel cross section centered as $s(t_0)$ is given by $r(t_0)$
the unit tangent: $$ t(t) = \frac{s'(t)}{\| s'(t) \| _2} $$
**rotation minimizing frame (RMF)**:minimize the torsion between two consecutive vessel sections


![[Pasted image 20250923104145.png]]
### The circular vessel section
the manually constructed template map：$$r': \widehat{\Omega} \to \widehat{\Omega}^r$$**grid optimization**


$$ 
s:  \widehat{\Omega}^r \to \widehat{\Omega}^r$$
**grid optimization**

composition of the two maps
$$r: \widehat{\Omega} \to \widehat{\Omega}^r$$

Given the strictly monotone sequence$\{0 = t_0, t_1, \ldots, t_N = 1\}$ ,generate a catalogue of planar geometries $Ω_i$ with radii $r(t_i) > 0$ and corresponding parametrization $x_i^{2D}: \widehat{\Omega}^r \to \Omega_i$

**define the lifted (into $\mathbb{R}^3$) cross sectional maps** $x_i^{3D}: \widehat{\Omega} \to \mathbb{R}^3$

$$\mathbf{x}_i^{3D} = \begin{bmatrix} \mathbf{n}(t_i) & \mathbf{m}(t_i) \end{bmatrix} \mathbf{x}_i^{2D} + \mathbf{s}(t_i)$$
### General convex cross sections
RadóKneser-Choquet theorem：a harmonic planar map $b:\widehat{\Omega} \to \Omega$ is diffeomorphic in $Ω$, provided $Ω$ is convex
$$
i \in \{1, 2\},\ \Delta \mathbf{x}_i = 0 \ \text{in}\ \widehat{\Omega}^\text{r}, \ \text{s.t.}\ \left. \mathbf{x} \right|_{\partial \widehat{\Omega}^\text{r}} = P(\mathbf{F})$$
## Non-planar bifurcations

