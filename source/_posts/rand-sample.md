---
title: Random sampling
date: 2021-12-27 14:37:00
updated: 2022-02-19 17:31:00
categories:
- Science
tags:
- Statistics
- Note
---

Basic sampling methods for some interesting and useful distributions.

<!-- more -->

# Standard randomness

The uniform (0, 1) variable provided by almost all languages.

# Normal distribution

Refer to [Wikipedia](https://en.wikipedia.org/wiki/Normal_distribution#Generating_values_from_normal_distribution).

One easy-to-implement "Box–Muller method": Generate two independent random numbers $U$ and $V$ distributed uniformly on (0,1). Then the two random variables $X$ and $Y$
$$
X = \sqrt{-2\ln(U)}\sin(2\pi V), Y = \sqrt{-2\ln(V)}\sin(2\pi U)
$$
will be both normally distributed and independent.

# Haar distribution on n-sphere

Generate n independent normal variables $X_1,X_2,\ldots,X_n$, then $(Y_1,Y_2,\ldots,Y_n)$ where
$$
Y_i = X_i / \sqrt{X_1^2+X_2^2+\ldots+X_n^2}
$$
will be uniformly distributed on a n-sphere.

The intuition is that under any rotation, the covariance matrix will always be identity.

# Arbitrary discrete distribution

Suppose we want to sample a discrete distribution on $\{1,2,\ldots,N\}$, and we hope this distribution to take arbitrary form, e.g., a function $f$. This can be done in $O(\log N)$ time, by dichotomy, if the discrete integral of $f$ can be computed in $O(1)$ time.

```py
def sample_f(N, f):
	F = discrete_int(f)
	l, r = 1, N
	while l < r:
		m = (l + r) // 2
		p = (F(m+1) - F(l)) / (F(r+1) - F(l))
		if bernouli(p) == 0:
			r = m
		else:
			l = m + 1
	return l
```

For example, if $f$ is a polynomial of degree $k$, a simple python implementation:

```py
from random import random

# p: big end probability, L: length
def sample_poly(L: int, ord):
    l, r = 0, L-1
    while l < r:
        m = (l + r) // 2
        p = ((m+1)**ord-l**ord)/((r+1)**ord-l**ord)
        if random() < p:
            r = m
        else:
            l = m + 1
    return l
```

poly 3:

![enter image description here](/attach/ylu4g.png)