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
Expressing $B_k$ as a sequence of operators applied to an initial simple identity matrix $I$.
## Going Forward
1. $z = \gamma_k q$ 
2. for $k-m<i<k-1,i++$
	1. $\beta_i = \rho_i(y_i^Tz)$
	2. $z = z+s_i(\alpha_i-\beta)$
3. $p_k = -z$
## Going Backward
1. set $q = \nabla f_k$ 
2. for $k-m<i<k-1,i--$
	1. $\alpha_i = \rho_i(s_i^Tq)$ , $\rho_i = \frac{1}{y_i^Ts_i}$
	2. $q = q- \alpha_i y_i$
