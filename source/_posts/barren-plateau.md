---
title: Barren Plateau (TBD)
date: 2022-03-26 15:25:53
updated: 2022-03-26 15:25:53
categories:
- Science
tags:
- Quantum
---

VQA may fail when cost function landscape is extremely flat, a phenomenon
called Barren Plateaus. In this note, I list latest progress on this topic.

<!-- more -->

# Introduction

Variational quantum algorithms (VQA) utilize an ansatz (or parameterized
quantum circuits, PQC) $U( \theta )$ to optimize a cost function
$C( \theta )$. The optimization iteration is performed by classical
optimizors such as GD, BFGS, ADAM, etc. Gradient based optimizors have
to query gradients at each iteration, which is done by measuring ansatz.
One possible reason this algorithm fail is cost function landscape being
extremely flat. By Chernov bound it implies unacceptable number of
measurements for evaluating a single gradients. To make it precise, we
say the cost function exhibits barren plateau (BP) for a parameter if
variance of respective partial derivative is exponentially small.

A cost function $C( \theta )$ of a $n$-qudit ansatz $U( \theta )$ is
said to have a barren plateau when training $\theta _{i} \in \theta$, if
$$\underset{\theta }{\operatorname{Var}}[ \partial _{\theta _{i}} C( \theta )] =\frac{1}{\exp( \Omega ( n))}$$

We make a few remarks here:

1.  In some text, BP is more strictly defined with respect to
    $\| \nabla C\|$.

2.  By Chebyshev inequality, the probability of finding a point outside
    of the BP area is exponentially small, which means random parameter
    initialization is basically inapplicable.

We now make some conventions and give some simple insights. We may
assume ansatz to consists of parameterized gates and unparameterized
gates, and each parameterized gate takes the form  $\exp( -i\theta A)$,
where $A$ is a Hermitian. Parameterized gates are assumed to be
periodical and smooth, with period $2\pi$. Unless specially specified,
cost function takes the form
$\operatorname{Tr}\left( OU\rho U^{\dagger }\right)$, and
$\mathbb{E}_{\theta }[ C] =0$.

**Lemma 1** For each $\theta _{i} \in \theta$,
$$\underset{\theta }{\mathbb{E}}[ \partial _{\theta _{i}} C] =0$$

*proof*. We decompose periodical real-valued function $C$ as Fourier
series
$\sum _{\mathbf{n}} c_{\mathbf{n}}\exp( i\theta \cdotp \mathbf{n})$. As
a consequence,

$$\underset{\theta }{\mathbb{E}}[ \partial _{\theta _{i}} C] =\sum _{\mathbf{n}} c_{\mathbf{n}}\underset{\theta }{\mathbb{E}}\left[ e^{i\theta \cdotp \mathbf{n}}\right]( in_{i}) =0
\tag*{$\square$}$$


# Scaling result

# Mitigation algorithms

# Appendix I: Integration on the unitary group

## Monomial integration formula

A general formula[^Col02] of integral of a $( t,t)$-order monomial over the
unitary group $U( d)$ of dimension $d$ can be represented by Weingarten
function
$\operatorname{Wg}$:

$$
\int _{U( d)} U_{i_{1} j_{1}} \cdots U_{i_{t} j_{t}} U_{i'_{1} j'_{1}}^{*} \cdots U_{i'_{t} j'_{t}}^{*} dU=\sum _{\sigma ,\tau \in S( t)} \delta _{i_{1} i'_{\sigma ( 1)}} \cdots \delta _{i_{t} i'_{\sigma ( t)}} \delta _{j_{1} j'_{\tau ( 1)}} \cdots \delta _{j_{t} j'_{\tau ( t)}}\operatorname{Wg}_{d}^{t}\left( \sigma \tau ^{-1}\right)
$$

The exact form of Weingarten function is
$$\operatorname{Wg}_{d}^{t}( \pi ) =\frac{1}{t!^{2}}\sum _{\lambda \vdash t}\frac{\chi ^{\lambda }( 1)^{2} \chi ^{\lambda }( \pi )}{s_{\lambda ,d}( 1)}$$
where the sum is over all integer partition $\lambda$ of $q$,
$\chi ^{\lambda }$ is the character of $S( t)$ indexed by $\lambda$, and
$s_{\lambda ,d}$ is the Schur function.

There are a few remarks on this formula:

1.  Here we follow convention in the quantum mechanics and $U^{*}$
    represents conjugate of $U$, not conjugate transpose. The latter is
    written as $U^{\dagger }$.

2.  The value of Weingarten function only depends on the conjugacy class
    of a permutation, thus the term
    $\operatorname{Wg}_{d}^{t}\left( \sigma \tau ^{-1}\right)$ in (1)
    can be replaced with
    $\operatorname{Wg}_{d}^{t}\left( \sigma ^{-1} \tau \right) =\operatorname{Wg}_{d}^{t}\left( \tau \sigma ^{-1}\right) =\operatorname{Wg}_{d}^{t}\left( \tau ^{-1} \sigma \right)$.

3.  The measure of $U( d)$ is the unique normalized Haar measure. In
    this sense, such integrals can be identified as expectation:
    $\int _{U( d)} f( U) dU=\mathbb{E}_{U}[ f( U)]$. It also implies
    that (1) is in fact a $( t,t)$th (or simply $t$th) moment.

4.  The integral vanishes if the order $( t,t')$ is not balanced.

5.  The integral vanishes if row (column) indices and conjugate row
    (column) indices do not pair up.

Packages and softwares:

1.  IntU[^PM17]: symbolic inegration, on Mathematica.

## Useful identities

**First moment**
$$\int _{U( d)} U_{i_{1} j_{1}} U_{i'_{1} j'_{1}}^{*} dU=\frac{\delta _{i_{1} i'_{1}} \delta _{j_{1} j'_{1}}}{d}$$

**Second moment**
$$\begin{gathered}
\int _{U( d)} U_{i_{1} j_{1}} U_{i_{2} j_{2}} U_{i'_{1} j'_{1}}^{*} U_{i'_{2} j'_{2}}^{*} dU\\
=\frac{\delta _{i_{1} i'_{1}} \delta _{i_{2} i'_{2}} \delta _{j_{1} j'_{1}} \delta _{j_{2} j'_{2}} +\delta _{i_{1} i'_{2}} \delta _{i_{2} i'_{1}} \delta _{j_{1} j'_{2}} \delta _{j_{2} j'_{1}}}{d^{2} -1} -\frac{\delta _{i_{1} i'_{1}} \delta _{i_{2} i'_{2}} \delta _{j_{1} j'_{2}} \delta _{j_{2} j'_{1}} +\delta _{i_{1} i'_{2}} \delta _{i_{2} i'_{1}} \delta _{j_{1} j'_{1}} \delta _{j_{2} j'_{2}}}{d\left( d^{2} -1\right)}\end{gathered}$$

**Equivalent form of** (1)
$$\int _{U( d)} U^{\otimes t} \otimes \left( U^{*}\right)^{\otimes t} dU=\sum _{\sigma ,\tau \in S( t)}\operatorname{Wg}_{d}^{t}\left( \sigma \tau ^{-1}\right) |\sigma _{d}^{t} \rangle \langle \tau _{d}^{t} |$$
where $|\pi _{d}^{t} \rangle$ is the non-normalized *permutation state*
$$|\pi _{d}^{t} \rangle =\sum _{i_{1} ,\dotsc ,i_{t} \in [ d]} |i_{\pi ( 1)} ,\dotsc ,i_{\pi ( t)} \rangle |i_{1} ,\dotsc ,i_{t} \rangle$$

# Appendix II: Quantum desgin

## Definition

**Definition 2** A **unitary t-design** is an ensamble of unitaries $\{V_{i} ,p_{i}\}$
which is indistinguishable from Haar measure, by observing up to $t$th
moment. Precisely, for any $( t,t)$-polynomial $P$,
$$\sum _{i} p_{i} P( V_{i}) =\int _{U( d)} P( U) dU$$
or equivalently
$$\sum _{i} p_{i} V_{i}^{\otimes t} \otimes \left( V_{i}^{*}\right)^{\otimes t} =\int _{U( d)} U^{\otimes t} \otimes \left( U^{*}\right)^{\otimes t} dU$$

We remark that $M^{\otimes t} \otimes \left( M^{*}\right)^{\otimes t}$
is refered to as $t$**th moment superoperator**, sending
$\rho ^{\otimes t}$ to $\left( M\rho M^{\dagger }\right)^{\otimes t}$.

Quantum design share the same name with block design. In fact, they are
similar in the sense of randomness. A **block t-design** is an
assignment of blocks ($k$-subsets) in $[ N]$, such that for all
$i=1,2,\dotsc ,t$, every two $i$-tuples lie in the same number of
blocks. It turn out that, if machine A randomly output $k$ different
numbers from $[ N]$ while machine B randomly output a block from a block
t-design, these two machine is indistinguishable if observer is only
allowed to read $t$ elements from outputs.

[^Col02]: Collins, B. (2002). Moments and Cumulants of Polynomial random variables on unitary groups, the Itzykson-Zuber integral and free probability. ArXiv. https://doi.org/10.48550/ARXIV.MATH-PH/0205010
[^PM17]: Puchała, Z., & Miszczak, J. A. (2017). Symbolic integration with respect to the Haar measure on the unitary groups. Bulletin of the Polish Academy of Sciences Technical Sciences, 65(1), 21–27. https://doi.org/10.1515/bpasts-2017-0003