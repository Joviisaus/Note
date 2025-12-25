---
tags:
  - Rendering
  - Definition
---
## Radiant Energy and Flux
Radiant energy is the energy of electromagnetic rediation.It is measured in unit of joules,and denoted by $$Q[j = Joule]$$
Definition:**Radiant flux(power)** is the enerty emitted,reflect,transmitted or received,per unit time.$$\Phi \equiv \frac{\mathrm{d}Q}{\mathrm{d}t} \quad [\mathrm{W} = \mathrm{Watt}] \quad [\mathrm{lm} = \mathrm{lumen}]^\star$$
![[Pasted image 20251222011003.png]]
## Radiant Intensity
Definition:The radiant(luminous) intensity is the power per unit **solid angle** emitted by a point light source ![[Pasted image 20251222011313.png]]
$$I(\omega) \equiv \frac{\mathrm{d} \Phi}{\mathrm{d} \omega}
$$ $$
\left[ \frac{\mathrm{W}}{\mathrm{sr}} \right] \left[ \frac{\mathrm{lm}}{\mathrm{sr}} = \mathrm{cd} = \text{candela} \right]$$

### Solid Angles
Ratio of subtended area on sphere to radius squared
- $\Omega = \frac{A}{r^2}$
- Sphere has $4 \pi$ steradians 
![[Pasted image 20251222011417.png]]


$$\begin{aligned}
\mathrm{d}A &= (r\,\mathrm{d}\theta)(r \sin \theta\,\mathrm{d}\phi) \\
&= r^2 \sin \theta\,\mathrm{d}\theta\,\mathrm{d}\phi
\end{aligned}
$$
$$
\mathrm{d}\omega = \frac{\mathrm{d}A}{r^2} = \sin \theta\,\mathrm{d}\theta\,\mathrm{d}\phi$$
Flux on a point light source: $$\begin{aligned}
\Phi &= \int_{S^2} I \, \mathrm{d}\omega \\
&= 4\pi I
\end{aligned}
$$
$$
I = \frac{\Phi}{4\pi}$$
## Irradiance
Defination：The irradiance is the power per(perpendicular/projected) unit area incident on a surface point.
$$E(\mathbf{x}) \equiv \frac{\mathrm{d} \Phi(\mathbf{x})}{\mathrm{d} A}
$$$$
\left[\frac{\mathrm{W}}{\mathrm{m}^{2}}\right] \left[\frac{\mathrm{lm}}{\mathrm{m}^{2}} = \mathrm{lux}\right]$$
## Radiance
Defination:The radiance is the power emitted,reflected,transmitted or received by a surface.per unit solid angle,per projected unit area.
![[Pasted image 20251222234757.png]]
$$L(\mathrm{p}, \omega) \equiv \frac{\mathrm{d}^{2} \Phi(\mathrm{p}, \omega)}{\mathrm{d} \omega \mathrm{d} A \cos \theta} 
\quad \text{\small $\cos \theta$ accounts for projected surface area}
$$

$$
\left[ \frac{\mathrm{W}}{\mathrm{sr} \, \mathrm{m}^{2}} \right] 
\left[ \frac{\mathrm{cd}}{\mathrm{m}^{2}} = \frac{\mathrm{lm}}{\mathrm{sr} \, \mathrm{m}^{2}} = \mathrm{nit} \right]$$
- Irradiance: power per projected area
- **Radiance**: power per unit solid angle & per projected area/Irradiance per solid angle/Intensity per unit area
- Intensity: power per solid angle

> Irradiance & Radiance
> $$E(\mathrm{p}) = \int_{H^{2}} L(\mathrm{p}, \omega) \cos \theta \, \mathrm{d} \omega$$
> ![[Pasted image 20251222235734.png]]

## BRDF (Bidirectional Reflectance Distribution Function)
Radiance from direction $\omega_i$ turns into the power $E$ that $dA$ receives.
Then power $E$ will become the radiance to any other direction $\omega$
![[Pasted image 20251223001153.png]]
Differential **irradiance** incoming: $\mathrm{d} E(\mathrm{p}, \omega) = L_{i}(\mathrm{p}, \omega) \cos \theta \, \mathrm{d} \omega$
Differential **radiance** existing: $dL_r(\omega_r)$

**BRDF** represents how much light is reflected into each outgoing direction $\omega_r$ from each incoming light.
$$f_{r}(\omega_{i} \to \omega_{r}) = \frac{\mathrm{d} L_{r}(\omega_{r})}{\mathrm{d} E_{i}(\omega_{i})} = \frac{\mathrm{d} L_{r}(\omega_{r})}{L_{i}(\omega_{i}) \cos \theta_{i} \, \mathrm{d} \omega_{i}} \quad \left[ \frac{1}{\mathrm{sr}} \right]$$
Reflection Equation:$$L_{r}(\mathrm{p}, \omega_{r}) = \int_{H^{2}} f_{r}(\mathrm{p}, \omega_{i} \to \omega_{r}) L_{i}(\mathrm{p}, \omega_{i}) \cos \theta_{i} \, \mathrm{d} \omega_{i}$$

## The Rendering Equation

$$L_{o}(\mathrm{p}, \omega_{o}) = L_{e}(\mathrm{p}, \omega_{o}) + \int_{\Omega^{+}} L_{i}(\mathrm{p}, \omega_{i}) f_{r}(\mathrm{p}, \omega_{i}, \omega_{o}) (\mathbf{n} \cdot \omega_{i}) \, \mathrm{d} \omega_{i}$$
Simplified：
$$L = E +KL$$
L:Energy
E:Self emmision
K:Reflection operator

$$ L = E+KE+K^2E+ K^3E+...$$
