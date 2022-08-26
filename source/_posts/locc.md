---
title: Local quantum operations assisted by classical communication (LOCC)
date: 2022-08-26 15:05:53
updated: 2022-08-26 15:05:53
categories:
- Science
tags:
- Quantum
---

LOCC never increase entropy.

<!-- more -->
# Quantum operation on bipartite system

Consider a bipartite system $\mathcal{H}^{A} \otimes \mathcal{H}^{B}$
shared between Alice and Bob. Catogorize quantum operations into several
classes, where the latter contains the former.

**(**$\Omega _{1}$**, convex hull of local operations)** Convex hull of
all maps of the form $\Lambda ^{A} \otimes \Lambda ^{B}$.

**(**$\Omega _{2}$**, one-way LQCC)** Quantum operation of the form
$$\Lambda ( \rho ) =\sum _{ij}\left(\mathbb{1} \otimes W_{ij}^{B}\right)\left( V_{i}^{A} \otimes \mathbb{1}\right) \rho \left( V_{i}^{A} \otimes \mathbb{1}\right)^{\dagger }\left(\mathbb{1} \otimes W_{ij}^{B}\right)^{\dagger }$$
where $( V_{i})_{i}$ and $( W_{ij})_{j}$ satisfies the completeness
condition.

**(**$\Omega _{3}$**, LQCC)** Quantum operation of the form
$$\Lambda ( \rho ) =\sum _{ij}\left(\mathbb{1} \otimes W_{i_{1} \cdots i_{k}}^{B}\right)\left( V_{i_{1} \cdots i_{k-1}}^{A} \otimes \mathbb{1}\right) \cdots \left(\mathbb{1} \otimes W_{i_{1} i_{2}}^{B}\right)\left( V_{i_{1}}^{A} \otimes \mathbb{1}\right) \rho ( \cdots )^{\dagger }$$
where $( V_{\cdots i})_{i}$ and $( W_{\cdots j})_{j}$ satisfies the
completeness condition.

**(**$\Omega _{4}$**, separable operations)** Quantum operation of the
form
$$\Lambda ( \rho ) =\sum _{i}( V_{i} \otimes W_{i}) \rho ( V_{i} \otimes W_{i})^{\dagger }$$
where $( V_{i} \otimes W_{i})_{i}$ satisfies the completeness condition.

LQCC: Local quantum operations assisted with classical communication

**Lemma 1.** Above operations are closed under tensor product, convex
combinations and composition.

# LQCC on pure states

**Lemma 2.** LQCC on pure state $|\psi \rangle$ can be simulated by
one-way LQCC.

Proof. We first prove that each measurement Bob takes can
be simulated by Alice measuring, followed by local unitary
transformations conditioned on Alice's output. Strictly, let
$\left\{M_{m}^{B}\right\}_{m}$ be measurements Bob takes, we assert that
there exists $\left\{M_{m}^{A}\right\}_{m}$ and corresponding
$\left\{U_{m}^{A} ,U_{m}^{B}\right\}_{m}$ such that for all $m$,
$$\left(\mathbb{1} \otimes M_{m}^{B}\right) |\psi '\rangle =\left( U_{m}^{A} \otimes U_{m}^{B}\right)\left( M_{m}^{A} \otimes \mathbb{1}\right) |\psi '\rangle .$$
The intuition is that Alice and Bob are symmetric when they share a pure
state, in the sense of Schmidt decomposition. For example, let
$|\psi '\rangle =\sum _{i}\sqrt{c_{i}} |a_{i} \rangle |b_{i} \rangle$.
If Bob measure in the basis $\{|b_{i} \rangle \}_{i}$, it would be same
if Alice measure in the basis $\{|a_{i} \rangle \}_{i}$ instead.

In general, Alice and Bob can be swapped through local unitaries:
$$\exists V_{m}^{A} ,V_{m}^{B} ,\text{s.t.} \ \left(\mathbb{1} \otimes M_{m}^{B}\right) |\psi '\rangle =\left( V_{m}^{A} \otimes V_{m}^{B}\right)\left( M_{m}^{B} \otimes \mathbb{1}\right) |\psi '\rangle ^{T} .$$
To see this, we can decompose
$$\left(\mathbb{1} \otimes M_{m}^{B}\right) |\psi '\rangle =\sum _{i}\sqrt{c_{i}} |a_{i} \rangle |b_{i} \rangle$$
and let
$V_{m}^{A} =\sum _{i} |a_{i} \rangle \langle b_{i} |,V_{m}^{B} =\sum _{i} |b_{i} \rangle \langle a_{i} |$.
Again,
$$\exists W^{A} ,W^{B} ,\text{s.t.} \ |\psi '\rangle ^{T} =\left( W^{A} \otimes W^{B}\right) |\psi '\rangle .$$
Combining these two, we get
$$\left(\mathbb{1} \otimes M_{m}^{B}\right) |\psi '\rangle =\left( V_{m}^{A} M_{m}^{B} W^{A} \otimes V_{m}^{B} W^{B}\right) |\psi '\rangle .$$
Now let
$M_{m}^{A} =M_{m}^{B} W^{A} ,U_{m}^{A} =V_{m}^{A} ,U_{m}^{B} =V_{m}^{B} W^{B}$,
we are done, since $W^{A}$ depends on $|\psi '\rangle$ but not $m$,
making $\left\{M_{m}^{A}\right\}_{m}$ a legal measurement.

Iteratively, we convert each measurement Bob takes into Alice's. Since
multiple measurements can be chained into a single one, and
$\left\{U_{m}^{A} M_{m}^{A}\right\}_{m}$ is legal measurement, each LQCC
$\Lambda$ on $|\psi \rangle$ can be converted to
$$\Lambda ( |\psi \rangle \langle \psi |) =\sum _{m}\left(\mathbb{1} \otimes U_{m}^{B}\right)\left( M_{m}^{A} \otimes \mathbb{1}\right) |\psi \rangle \langle \psi |\left( M_{m}^{A} \otimes \mathbb{1}\right)^{\dagger }\left(\mathbb{1} \otimes U_{m}^{B}\right)^{\dagger }$$
which is a one-way LQCC.

# LQCC and entropy: Nielson theorem

**Definition 1.** (Majorization) Let $( p_{i})_{i=1}^{m_{1}}$ and
$( q_{i})_{i=1}^{m_{2}}$ be two probability distributions with
probabilities arranged in decreasing order, i.e.,
$p_{1} \geqslant p_{2} \geqslant \dotsc \geqslant p_{m_{1}}$ and
similarly for $( q_{i})_{i}$. Then we will say that $( q_{i})_{i}$
majorizes $( p_{i})_{i}$, in symbols $( q_{i})_{i} \succ ( p_{i})_{i}$,
if for $k\leqslant \min\{m_{1} ,m_{2}\}$ we have
$$\sum _{i=1}^{k} q_{i} \geqslant \sum _{i=1}^{k} p_{i} .$$
We define
majorization for density operators (by eigenvalues) and pure states (by
Schmidt decompostion) similarly.

**Lemma 3.** $\rho \prec \sum _{i} p_{i} U_{i} \rho U_{i}^{\dagger }$,
where $\sum _{i} p_{i} =1$.

Proof. Rayleigh quotient achieves its
maximum at sum of largest eigenvalues. Let $n$ be the dimension of
$\rho$ and $k\in [ 1,n]$. The sum of largest $k$ eigenvalues of
$\sum _{i} p_{i} U_{i} \rho U_{i}^{\dagger }$ is
$$\begin{aligned}
              & \underset{V\in \mathbb{C}^{n\times k} ,\operatorname{rank} V=k}{\max}\operatorname{Tr}\left(\frac{V^{\dagger }\left(\sum _{i} p_{i} U_{i} \rho U_{i}^{\dagger }\right) V}{V^{\dagger } V}\right)              \\
    =         & \underset{V\in \mathbb{C}^{n\times k} ,\operatorname{rank} V=k}{\max}\sum _{i} p_{i}\operatorname{Tr}\left(\frac{V^{\dagger } U_{i} \rho U_{i}^{\dagger } V}{V^{\dagger } V}\right)                           \\
    \leqslant & \sum _{i} p_{i}\underset{V_{i} \in \mathbb{C}^{n\times k} ,\operatorname{rank} V_{i} =k}{\max}\operatorname{Tr}\left(\frac{V_{i}^{\dagger } U_{i} \rho U_{i}^{\dagger } V_{i}}{V_{i}^{\dagger } V_{i}}\right) \\
    =         & \sum _{i} p_{i}\underset{V\in \mathbb{C}^{n\times k} ,\operatorname{rank} V=k}{\max}\operatorname{Tr}\left(\frac{V^{\dagger } \rho V}{V^{\dagger } V}\right)                                                  \\
    =         & \underset{V\in \mathbb{C}^{n\times k} ,\operatorname{rank} V=k}{\max}\operatorname{Tr}\left(\frac{V^{\dagger } \rho V}{V^{\dagger } V}\right)\end{aligned}$$
which is no larger than the sum of largest $k$ eigenvalues of $\rho$.

**Theorem 1.** (Nielson's theorem) $|\psi \rangle$ can be converted into
$|\phi \rangle$ by LQCC iff $|\phi \rangle$ majorizes $|\psi \rangle$.

Proof. (Only if) By Lemma 2, there exists measurement
$\left\{M_{m}^{A}\right\}_{m}$ and corresponding unitaries
$\left\{U_{m}^{B}\right\}_{m}$ such that
$$\sum _{m}\left(\mathbb{1} \otimes U_{m}^{B}\right)\left( M_{m}^{A} \otimes \mathbb{1}\right) |\psi \rangle \langle \psi |\left( M_{m}^{A} \otimes \mathbb{1}\right)^{\dagger }\left(\mathbb{1} \otimes U_{m}^{B}\right)^{\dagger } =|\phi \rangle \langle \phi |.$$
It's easily checked that there exists $\{p_{m}\}_{m}$ with
$\sum _{m} p_{m} =1$ such that for all $m$,
$$\left(\mathbb{1} \otimes U_{m}^{B}\right)\left( M_{m}^{A} \otimes \mathbb{1}\right) |\psi \rangle \langle \psi |\left( M_{m}^{A} \otimes \mathbb{1}\right)^{\dagger }\left(\mathbb{1} \otimes U_{m}^{B}\right)^{\dagger } =p_{m} |\phi \rangle \langle \phi |.$$
Tracing out $\mathcal{H}^{B}$, we need to prove
$\rho _{\phi } \succ \rho _{\psi }$. For each $m$ we have
$$M_{m}^{A} \rho _{\psi }\left( M_{m}^{A}\right)^{\dagger } =p_{m} \rho _{\phi }$$
By polar decomposition, there exists unitary $U_{m}$ such that
$$M_{m}^{A}\sqrt{\rho _{\psi }} =\sqrt{p_{m} \rho _{\phi }} U_{m} .$$
Thus
$$\rho _{\psi } =\sum _{m}\sqrt{\rho _{\psi }}\left( M_{m}^{A}\right)^{\dagger } M_{m}^{A}\sqrt{\rho _{\psi }} =\sum _{m} p_{m} U_{m}^{\dagger } \rho _{\phi } U_{m} .$$
That $\rho _{\phi } \succ \rho _{\psi }$ follows from Lemma 3.

(If) We show that (one-way) LQCC can change two of Schmidt coefficients
of a pure state. Thus, after a sequence of LQCC $|\psi \rangle$ can be
converted to $|\psi '\rangle$ whose Schimdt coefficients coincides with
$|\phi \rangle$'s. After that, one can convert $|\psi '\rangle$ to
$|\phi \rangle$ by two local unitaries.

Let $|\psi \rangle =\sum _{i}\sqrt{p_{i}} |a_{i} \rangle |b_{i} \rangle$
and suppose we want to change $p_{1} ,p_{2}$ to $q_{1} ,q_{2}$
($p_{1} +p_{2} =q_{1} +q_{2}$). Let
$P=\sum _{i\geqslant 3} |a_{i} \rangle \langle a_{i} |$. We define our
LQCC to be
$$\Lambda ( \rho ) =\sum _{i=1}^{2}( M_{i} \otimes U_{i}) \rho ( M_{i} \otimes U_{i})^{\dagger }$$
where
$$\begin{aligned}
    M_{1} & :=\alpha \left( P+\sqrt{\frac{q_{1}}{p_{1}}} |a_{1} \rangle \langle a_{1} |+\sqrt{\frac{q_{2}}{p_{2}}} |a_{2} \rangle \langle a_{2} |\right) , \\
    U_{1} & :=\mathbb{1} ,                                                                                                                                 \\
    M_{2} & :=\beta \left( P+\sqrt{\frac{q_{2}}{p_{1}}} |a_{2} \rangle \langle a_{1} |+\sqrt{\frac{q_{1}}{p_{2}}} |a_{1} \rangle \langle a_{2} |\right) ,  \\
    U_{2} & :=\sum _{i\geqslant 3} |b_{i} \rangle \langle b_{i} |+|b_{1} \rangle \langle b_{2} |+|b_{2} \rangle \langle b_{1} |.\end{aligned}$$
The construction is such that
$M_{i} \otimes U_{i} |\psi \rangle \varpropto |\psi '\rangle$ btw. The
coefficients $\alpha ,\beta$ can be determined by the constraint
$M_{1}^{\dagger } M_{1} +M_{2}^{\dagger } M_{2} =\mathbb{1}$.

Given that the spetral of maximally entangled states is
$\left\{d^{-1} ,d^{-1} ,\dotsc \right\}$ and the spetral of separable
states is $\{1,0,0,\dotsc \}$, LQCC never increases entanglement,
intuitively. In fact, any state can be transformed from maximally
entangled states, or to separable states, by LQCC.

**Lemma 4.** For any density operator
$\rho \in \mathcal{T}\left(\mathcal{H}^{A} \otimes \mathcal{H}^{B}\right)$,
maximally entangled state $|\Psi \rangle$, and separable state
$|\phi ^{A} \rangle |\phi ^{B} \rangle$, there exists LQCC
$\Lambda _{1} ,\Lambda _{2}$ such that
$$|\Psi \rangle \xrightarrow{\Lambda _{1}} \rho \xrightarrow{\Lambda _{2}} |\phi ^{A} \rangle |\phi ^{B} \rangle$$

Proof. The existence of $\Lambda _{1}$ is obvious by Theorem 1 and Lemma 1.
$\Lambda _{2}$ can be defined to be

$$\Lambda _{2}( \rho ) =\sum _{ij}\left( |\phi ^{A} \rangle \langle \psi _{i}^{A} |\otimes |\phi ^{B} \rangle \langle \psi _{j}^{B} |\right) \rho \left( |\phi ^{A} \rangle \langle \psi _{i}^{A} |\otimes |\phi ^{B} \rangle \langle \psi _{j}^{B} |\right)$$
