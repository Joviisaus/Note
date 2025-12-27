# BFGS
## Compare with the Newton method
- Newton method: compute Hessian matrix
- BFGS:approximate using the gradiante

## Method
To minimize $f(x)$  ,BFGS approximates the **Newton Step** $Δ x$ in each step $k$,let $B_k$ be the approximation of the $H^{-1}$ in each step $k$.
1. Step displacement:$$s_k = x_{k+1}-x_k$$
2. Gradient displacement:$$ y_k = \nabla f_{k+1} - \nabla f_k$$
Update matrix $B$ in $$B_{k+1}y_k = s_k$$

# "Limited-Memory" (L)
