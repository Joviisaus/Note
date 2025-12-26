---
tags:
  - Rendering
  - Summary
---
## Ray Tracing

To overcome the shorts of Rasterization
- Soft shadow
- Glossy
- Indirect illumination

```image-layout-d
![[Soft shadow.png]]
![[Pasted image 20251221232515.png]]
![[Pasted image 20251221232532.png]]
```

Ray Tracing is accurate,but is very slow (**offline** usually)

### Basic Ray Tracing Algorithm
Scence composed by [[Light Rays]] 

### Ray Casting
1. Generate an image by **Casting one ray per pixel**
2. Check for shadows by **Sending a ray to the light**
> Shadow appears while it cannot be catched by camera and light at the same time(hard shadow)

```image-layout-a
![[Pasted image 20251221233906.png]]
![[Pasted image 20251221234616.png]]
```
Eye ray generate from each pixel and stop at its first vertex,then perform shading calculation to compute color by light source.

Classic Method: [[Whitted-Style]] method.

## Ray Equations
Ray is defined by its origin and a direction vector.
$$ r(t) = o + td \quad 0\le t $$![[Pasted image 20251221235947.png]]

### Ray Intersection With Implicit Surface
Ray: $r(t) = o + td \quad 0\le t$
General implicit surface: $p: f(p) = 0$
Substitute ray equation: $f(o+td) = 0$

### Ray Intersection With Triangle Mesh
A plane is defined by a point $p'$ on the plane and a normal vector $N$. The implicit equation of the plane is: $$f(p) = (p - p') \cdot N = 0$$ Substitute the ray equation $r(t) = o + td$ into the plane equation: $$((o + td) - p') \cdot N = 0$$ Solve for $t$: 1. Distribute the dot product: $(o - p') \cdot N + t(d \cdot N) = 0$ 2. Isolate the term with $t$: $t(d \cdot N) = (p' - o) \cdot N$ 3. Solve: $$t = \frac{(p' - o) \cdot N}{d \cdot N}$$ **Validity Conditions:** If $d \cdot N = 0$, the ray is parallel to the plane (no intersection). For a valid intersection, we usually require $t > 0$. 
### Ray Intersection With Triangle Mesh 
To intersect a ray with a triangle defined by vertices $P_0, P_1, P_2$, we use the **Möller-Trumbore algorithm**. This algorithm solves for the intersection directly using barycentric coordinates without needing to pre-calculate the plane equation. 
#### 1. Parametric Representation 
Any point $P$ inside the triangle can be expressed using barycentric coordinates $(u, v)$: $$P(u, v) = (1 - u - v)P_0 + uP_1 + vP_2$$ Where $u \ge 0, v \ge 0$ and $u + v \le 1$. 
#### 2. Intersection Equation 
Setting the ray equation equal to the triangle parametrization: $$o + td = (1 - u - v)P_0 + uP_1 + vP_2$$ Rearranging the terms into a linear system: $$o - P_0 = t(-d) + u(P_1 - P_0) + v(P_2 - P_0)$$ 
#### 3. Solving the System (Cramer's Rule) Let $e_1 = P_1 - P_0$, $e_2 = P_2 - P_0$, and $s = o - P_0$. The solution for $t, u, v$ is: 
$$\begin{bmatrix} t \\ u \\ v \end{bmatrix} = \frac{1}{(d \times e_2) \cdot e_1} \begin{bmatrix} (s \times e_1) \cdot e_2 \\ (d \times e_2) \cdot s \\ (s \times e_1) \cdot d \end{bmatrix}$$

## Booosting Method
### Bounding Volume
For each parallel, compute $t_{min}$ and $t_{max}$ 
![[Pasted image 20251222003106.png]]
Available time: between $t_{enter} = \max(t_{min})$  and $t_{exit} = \min(t_{max})$ 

### Spatial Partition
![[Pasted image 20251222004750.png]]
### Object Partitions & Bounding Volume Hierachy(BVH)
Subdivide :
1. Choose a dimension to split
2. Heuristic #1:Always choose the longest axis in node
3. Heuristic #2:Split node at location of median object
4. Stop when node contains few elements
![[Pasted image 20251222005410.png]]
## Physically Correct Illuminating manner
To accurately measure the spatial properties of light: [[Basic radiometry]]

### The Rendering Equation

$$L_{o}(\mathrm{p}, \omega_{o}) = L_{e}(\mathrm{p}, \omega_{o}) + \int_{\Omega^{+}} L_{i}(\mathrm{p}, \omega_{i}) f_{r}(\mathrm{p}, \omega_{i}, \omega_{o}) (\mathbf{n} \cdot \omega_{i}) \, \mathrm{d} \omega_{i}$$
Simplified：
$$L = E +KL$$
L:Energy
E:Self emission
K:Reflection operator

$$ L = E+KE+K^2E+ K^3E+...$$

## Path Tracing
Solve the Rendering Equation by [[Monte Carlo Integration]]
```python
# Path Tracing Shading Function
def shade(p, wo):
    # 1. Randomly sample a direction based on a PDF
    wi = sample_direction(pdf(w))
    
    # 2. Trace a ray from point p in direction wi
    ray_r = trace_ray(p, wi)
    
    # 3. Intersection Logic
    if ray_r.hit_light():
        # Direct illumination contribution
        return (L_i * f_r * cosine) / pdf(wi)
        
    elif ray_r.hit_object(at=q):
        # Recursive indirect illumination (Global Illumination)
        return (shade(q, -wi) * f_r * cosine) / pdf(wi)
```

Distributed Ray Tracing if $N != 1$  
Tracy **More than 1 path** through each pixel and average its radiance.
![[Screenshot 2025-12-27 at 01.06.38.png]]
When to stop:
**Russian Roulette**
1. Obtain a shading result $L_o$
2. Manually set a probability $p(0<p<1)$
3. Return the shading result by $\frac{L_o}{p}$
4. With probability $1-p$,get $0$
5. Expect is $E = p\frac{L_o}{p}+p0 = L_o$

```python
def shade(p, wo):
    # --- Part 1: Russian Roulette Termination ---
    # Manually specify a probability P_RR
    P_RR = 0.8  
    # Randomly select ksi in a uniform distribution in [0, 1]
    ksi = random_uniform(0, 1)
    
    # If the random value exceeds the threshold, terminate the ray
    if ksi > P_RR:
        return 0.0

    # --- Part 2: Ray Casting and Sampling ---
    # Randomly choose ONE direction wi based on a PDF
    wi = sample_direction(pdf(w))
    # Trace a ray r starting at p in direction wi
    ray_r = trace(p, wi)

    # --- Part 3: Contribution Calculation ---
    # Case A: Ray hits a light source directly
    if ray_r.hit_light():
        return (L_i * f_r * cosine) / pdf(wi) / P_RR

    # Case B: Ray hits another object at point q (Recursive Step)
    else if ray_r.hit_object(at=q):
        # Recursively call shade and weight the result by P_RR
        return (shade(q, -wi) * f_r * cosine) / pdf(wi) / P_RR
```

**Transfer the rendering equation**
Project $\omega_i$ to $A$. 
$$L_o(x, \omega_o) = \int_{\Omega^+} L_i(x, \omega_i) f_r(x, \omega_i, \omega_o) \cos \theta \, d\omega_i 

= \int_{A} L_i(x, \omega_i) f_r(x, \omega_i, \omega_o) \frac{\cos \theta \cos \theta'}{\|x' - x\|^2} \, dA$$
![[Screenshot 2025-12-27 at 01.27.55.png]]
```python
def shade(p, wo):
    # --- 1. Russian Roulette Termination ---
    # Manually specify a probability P_RR (e.g., 0.8)
    P_RR = 0.8 
    
    # Randomly select a value between 0 and 1
    ksi = random_uniform(0, 1)
    
    # If the random value is greater than P_RR, stop the recursion
    if ksi > P_RR:
        return 0.0

    # --- 2. Sampling and Ray Casting ---
    # Randomly choose ONE direction wi based on a PDF
    wi = sample_hemisphere(pdf)
    
    # Trace a ray from point p in direction wi
    ray = trace_ray(p, wi)

    # --- 3. Contribution Calculation ---
    # Case A: The ray hits a light source directly
    if ray.hit_light():
        # Calculate contribution using the Rendering Equation
        # We divide by P_RR to keep the estimate unbiased
        return (L_i * f_r * cos_theta) / pdf(wi) / P_RR

    # Case B: The ray hits another object at point q
    elif ray.hit_object():
        q = ray.intersection_point
        # Recursively call shade for the next bounce
        return (shade(q, -wi) * f_r * cos_theta) / pdf(wi) / P_RR 
```
### Compared to [[Whitted-Style]] Ray Tracing
[[Whitted-Style]]:
- Always stop at diffuse surfaces.
- Always perform specular reflections/refractions.

Glossy material cannot be computed correctly.