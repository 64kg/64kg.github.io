---
title: Collection of quantum papers
date: 2021-09-01 14:17:00
updated: 2022-01-17 21:45:00
categories:
- Science
tags:
- Quantum
- Collection
- Paper
---

Collection of quantum related papers I've read.

<!-- more -->

# VQA

## 2021

[Variational quantum algorithms](https://arxiv.org/abs/2012.09265)

M. Cerezo, et al. | Nature Reviews Physics

Review on variational quantum algorithms. Very useful.

[An efficient quantum algorithm for the time evolution of parameterized circuits](https://doi.org/10.22331/q-2021-07-28-512)

Stefano Barison, et al. | Quantum

Model VQS gradient update rule as an optimization problem.

[Cost Function Dependent Barren Plateaus in Shallow Parametrized Quantum Circuits](https://www.nature.com/articles/s41467-021-21728-w)

M. Cerezo, et al. | Nature Communications

Cost function based analysis of Barren plateaus.

[Qubit-excitation-based adaptive variational quantum eigensolver](https://www.nature.com/articles/s42005-021-00730-0)

Yordan S. Yordanov, et al. | Nature Communication Physics

Adaptation to Qubit-ADAPT-VQE, QEB-ADAPT-VQE, converge fast comparably with fermion-ADAPT-VQE while maintaining shallow CNOT depth.

[Variational quantum algorithm with information sharing](https://doi.org/10.1038/s41534-021-00452-9)

Chris N. Self, et al. | NPJ Quantum Inf.

Scheme for parallel Bayesian optimzation of varational problem (VQE etc.) with similar parameters. Information sharing between similar problems helps reducing iterations in need (~10-50 its, compared to ~100-1000 for gradient descent methods).

[Qubit-ADAPT-VQE: An Adaptive Algorithm for Constructing Hardware-Efficient Ansätze on a Quantum Processor](https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.2.020310)

Tang, H. L. et al. | PRX Quantum

An improved version of [ADAPT-VQE](https://www.nature.com/articles/s41467-019-10988-2), where operator pool is reduced two $2n-2$ Pauli strings (Hamiltonian satisfies time-reversal symmetry). This was proved by showing that the operator sets $\{Z_kY_{k+1},Y_k\}_{1\leq k<n}$ is complete (for real Hamiltonian and real states).

[Contextual Subspace Variational Quantum Eigensolver](https://quantum-journal.org/papers/q-2021-05-14-456/)

William M. Kirby, Andrew Tranter, and Peter J. Love | Quantum

Contextual subspace variational quantum eigensolver (CS-VQE), a hybrid quantum-classical algorithm for approximating the ground state energy of a Hamiltonian. The approximation to the ground state energy is obtained as the sum of two contributions.

## 2020

[Entanglement Devised Barren Plateau Mitigation](https://journals.aps.org/prresearch/abstract/10.1103/PhysRevResearch.3.033090)

Taylor L. Patti et al. | Physical Review Research

It suggests that Barren plateaus may be connected with spread of entanglement (which shall be further checked) and provides several mitigation strategies based on that. Three insights it provides are important: low variance of gradients doesn't equal pool training performance, loss function relating to eigenvector of an observable often trains better (despite low variance), introduction of Langevin noise helps avoid Barren plateaus is both surprising and worth further study.

[Classical Simulation of Noncontextual Pauli Hamiltonians](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.123.200501)

William M. Kirby and Peter J. Love | PhysRevLett

Contextuality is an indicator of non-classicality, and a resource for various quantum procedures. In this paper, we present an efficiently computable test to determine whether or not the objective function for a VQE procedure is contextual.

[Classical Simulation of Noncontextual Pauli Hamiltonians](https://doi.org/10.1103/PhysRevA.102.032418)

William M. Kirby and Peter J. Love | Physical Review A

Noncontextual Pauli Hamiltonians decompose into sets of Pauli terms to which joint values may be assigned without contradiction. We construct a quasi-quantized model for noncontextual Pauli Hamiltonians. Using this model, we give an algorithm to classically simulate noncontextual VQE. We also use the model to show that the noncontextual Hamiltonian problem is NP-complete.

[Is the Trotterized UCCSD Ansatz Chemically Well-Defined?](https://arxiv.org/abs/1910.10329)

Harper R. Grimsley et al. | arXiv

Operator ordering in trotterization of UCCSD has a significant effect (large energy differences between orderings) when there is a significant
amount of electron correlation.

## 2019

[An adaptive variational algorithm for exact molecular simulations on a quantum computer](https://www.nature.com/articles/s41467-019-10988-2)

Grimsley H. R. et al. | Nat. Commun.

The fermion ADAPT-VQE algorithm. The ansatz is constructed by starting from Hartree-Fock state, and adding fermionic operators that produces largest energy gradient.![adapt-vqe](/attach/71ldl.png)

# Quantum chemistry

## 2020
[Quantum computational chemistry](https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.92.015003)

Sam McArdle et al. | Rev. Mod. Phys.

This review provides a comprehensive introduction to both computational chemistry and quantum computing, bridging the current knowledge gap.