Let us define the Monte Carlo estimator for the definite integral of given function $f(x)$

Definite integral: $$\int_{a}^{b} f(x) dx$$
 Random variable
$$X_i \sim p(x)$$
Monte Carlo estimator
$$F_N = \frac{1}{N} \sum_{i=1}^{N} \frac{f(X_i)}{p(X_i)}$$