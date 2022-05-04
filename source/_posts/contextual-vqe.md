---
title: Contextual subspace VQE
date: 2021-10-16 17:40:00
updated: 2021-10-17 19:34:00
categories:
- Science
tags:
- Quantum
- Paper
- Note
---

Summary of contextual subspace VQE, how it divide a Hamiltonian into classical and non-classical parts, how to evaluate the classical part, and how to reduce qubit requirements for the non-classical part. This note also contains some of my own thoughts on it.

<!-- more -->

# Introduction

This is a sequential work of William M. Kirby, Peter J. Love, et al. [[1]](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.123.200501)[[2]](https://arxiv.org/abs/2002.05693)[[3]](https://quantum-journal.org/papers/q-2021-05-14-456/).

# Mermin–Peres magic square

$$
\begin{bmatrix}
IZ & ZI & ZZ\\
XI & IX & XX\\
-XZ & -ZX & YY
\end{bmatrix}
$$

# Measurement contextuality

This section is a summary of [[1]](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.123.200501).

$\mathcal{S}$: set of observables (paulis)

$\mathcal{Z}$: observables (in $\mathcal{S}$) which commute universally in $\mathcal{S}$

$\overline{\mathcal{S}}$: closure under inference

**Theorem**. $\mathcal{S}$ is **noncontextual** iff there is a *consistent* valuation, iff the *compability graph* of $\mathcal{S} \backslash \mathcal{Z}$ consists of disjoint cliques, i.e., $\mathcal{S} =\mathcal{Z} \cup \mathcal{C}_{1} \cup \cdots \cup \mathcal{C}_{N}$

E.g., $\mathcal{S} =\{IZ,ZI,IX,XZ\}$, compability graph:![enter image description here](/attach/stjcc.png)

# Noncontextual Hamiltonian

This section is a summary of [[2]](https://arxiv.org/abs/2002.05693).

$\mathcal{S} =\mathcal{Z} \cup \mathcal{C}_{1} \cup \cdots \cup \mathcal{C}_{N}$.

-   At least *one*, but not necessarily *all*, valuation is consistent.

(E.g., $\mathcal{S} =\{IX,XI,XX\}$)

-   Claim: number of consistent valuations is a power of 2.

(One minimum generator set: $\mathcal{G} \cup \mathcal{A}$, where $\mathcal{A}$ consists of exactly one element $A_{i}$ from each $\mathcal{C}_{i}$, and $\mathcal{G}$ is a minimum generator set of $\mathcal{Z} \cup \{A_{i} C_{i} |1\leqslant i\leqslant N,C_{i} \in \mathcal{C}_{i}\}$)

-   *Ontos* vs *epistēmē*.

-   Not necessarily all distribution of valuations (epistemic state) is possible (correponds to a valid ontic state).

(E.g., $\mathcal{S} =\{X,Z\}$)

-   A valid set of epistemic states: $E:=\left\{(\vec{q} ,\vec{r}) \in \{\pm 1\}^{|G|} \times \mathbb{R}^{N} ||\vec{r} |=1\right\}$, each $(\vec{q} ,\vec{r})$ defines a joint distribution:

$$P_{(\vec{q} ,\vec{r})}( g_{1} ,\cdots ,a_{1} ,\cdots ) =\prod \delta _{g_{i} ,q_{i}}\prod \frac{1}{2} |a_{i} +r_{i} |$$
($\langle G_{i} \rangle =q_{i} ,\langle A\rangle =+1$, where
$A( r) :=\sum r_{i} A_{i}$)

-   $E$ contains every eigenstate of the original $H$.

# Contextual subspace VQE

This section is a summary of [[3]](https://quantum-journal.org/papers/q-2021-05-14-456/).

$\mathcal{S} =\mathcal{S}_{nc} \cup \mathcal{S}_{c}$.

-   Eigenstates of $H$ are not necessarily contained in $E$.

-   Howerver, by searching in ground epistemic state of $\mathcal{S}_{nc}$ (a subspace of the Hilbert space), one reduces qubits.

(Rotate $G_{i}$s into single qubit $Z$ gates and $A$ into a single qubit Pauli gate, thus fixing some qubits and restricting $\mathcal{S}_{c}$ to fewer qubits. Do VQE on restricted $\mathcal{S}_{c}$, the approximation ground energy is $E_{c} +E_{nc}$)

-   Reduce $\mathcal{S}_{nc}$ to introduce more quantum correction (thus achieve higher accuracy), until a full VQE is recovered.

![enter image description here](/attach/t1mp0.png)

![enter image description here](/attach/3ign3.png)

# How contextual can a Hamiltonian be

A Hamiltonian can be arbitrarily contextual, i.e., the proportion of a non-contextual subset can be arbitrarily small.

**Claim 1:** Every graph can be the compatibility graph of a set of Pauli strings.

Encode a graph into Pauli strings by induction. Initially set the encoding of every vertex to be an $I$-string of length $n$ (number of vertices). Pick a vertex, set the first operator of its encoding to be $X$. For every other vertex, set the first operator to be $Z$ iff there's no edge between them.

**Claim 2:** Disjoint-cliques subgraph (DCS) can be arbitrarily small, compared to the original graph.

Construct an interval graph as follows. For fixed positive integer $d$,
$$V=\bigcup _{k=0}^{d-1}\left\{\left(\frac{i}{2^{k}} ,\frac{i+1}{2^{k}}\right) |0\leqslant i< 2^{k}\right\}$$
For example,
![enter image description here](/attach/qot79.png)
It can be shown that $n=|V|=d2^{d-1}$, yet the maximum DCS has size $2^{d} -1$.