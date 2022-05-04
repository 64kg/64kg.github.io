---
title: Generalized Rayleigh quotient
date: 2021-10-23 20:31:00
updated: 2022-02-15 19:41:00
categories:
- Science
tags:
- Math
- Note
---

Rayleigh quotient appears a lot in machine learning, quantum mechanics etc. This note details Rayleigh quotient and simple proofs as well as introduce some of its applications.

<!-- more -->

# Generalized Rayleigh quotient

In mathematics, the **Rayleigh quotient** for a given complex Hermitian matrix $M$ and nonzero vector $x$ is defined as ([Wiki](https://en.wikipedia.org/wiki/Rayleigh_quotient)):
$$
R( M,w) =\frac{w^{\dagger } Mw}{w^{\dagger } w}
$$

In this note, we aim at a generalized Rayleigh quotient, where vector ensemble $V$ (instead of $x$) is considered:
$$
R( M,V) =\operatorname{tr}\left(\frac{V^{\dagger } MV}{V^{\dagger } V}\right)
$$

Remark that here quotient of matrices is well defined as $\operatorname{tr}(AB^{-1})=\operatorname{tr}(B^{-1}A)$. Also, if the denominator is $V^{\dagger } PV$ instead of $V^{\dagger } V$ where $P\succ 0$, then a standard version can be recovered by transforming $V$ to $QV$ and $M$ to $\left( Q^{\dagger }\right)^{-1} MQ^{-1}$, where $P=Q^{\dagger } Q$.

In order to calculate the lower bound (resp. upper bound) of classical Rayleigh quotient, the easiest approach is to use Lagrange multipliers. Unfortunately, this method cannot generalized to $R(M,V)$. Nevertheless, as one may expect, generalized Rayleigh quotient depends on eigenvalues of $M$ just like its classical counterpart.

**Theorem.** *For* $n\times n$ *Hermitian* $M$ *and* $n\times k$ *vector ensemble* $V$, *generalized Rayleigh quotient is bounded by*
$$
\lambda _{1} +\cdots +\lambda _{k} \leqslant R( M,V) \leqslant \lambda _{n-k+1} +\cdots +\lambda _{n}
$$
*The lower bound (resp. upper bound) is achieved when* $V$ *is the ensemble of eigenvectors of corresponding eigenvalues.*

Proof. First notice that we can assume $V$ is an ensemble of standard orthogonal vectors. If not, there exists unitary $U$ such that $V^{T} V=U^{\dagger } DU$ where $D$ is a positive diagonal matrix. By properties of matrix trace,
$$
\operatorname{tr}\left(\frac{V^{\dagger } MV}{V^{\dagger } V}\right) =\operatorname{tr}\left(\frac{D^{-1/2} UV^{\dagger } MVU^{\dagger } D^{-1/2}}{D^{-1/2} UV^{\dagger } VU^{\dagger } D^{-1/2}}\right)
$$
Now $VU^{\dagger } D^{-1/2}$ is an ensemble of standard orthogonal vectors.
By the same reasoning one can assume Hermitian $M$ to be diagonal. Thus
$$
R( M,V) =\operatorname{tr}\left( V^{\dagger } MV\right) =\sum _{j} \lambda _{j}\sum _{i} |v_{ij} |^{2}
$$
Let $r_{j} =\sum _{i} |v_{ij} |^{2}$, then $r_{1} ,\dotsc r_{n}$ are numbers in $[ 0,1]$ and sums to $k$. Thus $\sum _{j} r_{j} \lambda _{j}$ can be viewed as a (restricted) convex combination of $\{\lambda _{j}\}_{j=1}^{n}$, which takes its lower bound (resp. upper bound) at $\lambda _{1} +\cdots +\lambda _{k}$ (resp. $\lambda _{n-k+1} +\cdots +\lambda _{n}$). That the corresponding $V$ is an ensemble of eigenvectors is easily checked.

# Applications

## Fisher's linear discriminant

$$
\min J_{F}( w) =\frac{w^{T} S_{b} w}{w^{T} S_{w} w}
$$

## Normalized cut

$$
\begin{array}{l} \min J( V_{1} ,\dotsc ,V_{K}) =\frac{1}{2}\sum _{k=1}^{K}\frac{w\left( V_{k} ,\overline{V_{k}}\right)}{d( V_{k})}\\ \Leftrightarrow \min J( Z) =\frac{1}{2}\operatorname{tr}\left(\frac{Z^{T} LZ}{Z^{T} DZ}\right) \end{array}
$$
where $L=D-W$ is the Laplace matrix.