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