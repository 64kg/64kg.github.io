---
title: Analyzing Monte Carlo
date: 2022-08-26 15:05:53
updated: 2022-08-26 15:05:53
categories:
- Science
tags:
- Math
- Statistics
---

Some methods to estimate the accuracy as well as minimum samples needed of Monte Carlo method.

<!-- more -->

In Monte Carlo, we esitmate $\mathbb{E}X$ by sampling i.i.d. $X_1,\dots,X_n$.

Some of the symbols in this article are generic, as follows:

- $\varepsilon$: absolute error
- $\delta$: relative error
- $n$: number of samples

# Estimate an expectation

By Chebyshev inequality:

$$
\Pr(|\bar{X}_n-\mathbb{E}X|\geq \varepsilon) \leq \frac{\operatorname{Var}X}{n\varepsilon^2}
$$

This means that for the failure probability to not exceed $\varepsilon$, the number of samples should be least

$$
n\geq \frac{\operatorname{Var}X}{p_\text{fail}\varepsilon^2}
$$

# Estimate a probability

By Chernoff bound, if $X\sim \text{Bernouli}(p)$, then

$$
\Pr( |\overline{X}_{n} -p|\geqslant \delta p) \leqslant 2\exp\left( -\frac{\delta ^{2}}{3} np\right)
$$

This means that for the failure probability to not exceed $p_\text{fail}$, the number of samples should be least

$$
n\geqslant \frac{3\ln( 2/p_\text{fail} )}{p\delta ^{2}}
$$

This bound contains $p$ which is not always desired. We can drop it by replacing relative error with absolute error.

As long as

$$
n\geqslant \frac{3\ln( 2/p_\text{fail} )}{\varepsilon ^{2}}\left( \geqslant \frac{3\ln( 2/p_\text{fail})}{p\delta ^{2}}\right),
$$

the failure probability wouldn't exceed $p_\text{fail}$.