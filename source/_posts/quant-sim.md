---
title: Quantum simulation
date: 2021-08-07 11:28:00
updated: 2021-08-30 10:43:00
categories:
- Science
tags:
- Quantum
- Note
- Paper
---

Simulating quantum mechanics by quantum computer has gained much attention. This review summarized several digital quantum simulation approaches, including product formula, quantum walk, varational principles and algebraic methods.

<!-- more -->

# Introduction

Simulating quantum mechanics, e.g. solving the Schrödinger equation on time-independent Hamiltonian $H$:
$$
i\hbar \frac{d}{dt} |\psi \rangle =H|\psi \rangle 
$$
is known to be a computationally hard problem due to *exponential explosion* back in 1980s. Approximation methods such as Monte Carlo algorithm ([Suzuki, 1993](https://www.worldscientific.com/worldscibooks/10.1142/2262)) only works on well-behaved problems, which discourages practical usage.

The notion of simulating qauntum mechanics by *quantum computer* was envisioned by Feynman ([1982](https://link.springer.com/article/10.1007/BF02650179)). In fact, quantum computer was shown to have universal capability ([Lloyd, 1996](https://science.sciencemag.org/content/273/5278/1073)). However, as for simulating a problem-specific machine (e.g., a simple system with partial control that mimics a larger system in an analog way) rather than a universal quantum computer may be sufficient. Therefore practical qauntum simulation may come to reality way before a univeral quantum computer.

Recently quantum simulation has gained more and more attention due to its potential applications in, e.g., condensed-matter physics, high-energy physics, atomic physics, quantum chemistry, and cosmology, as well as due to the advances in the coherent manipulation of quantum systems that promise practical quantum simulation in near future.

# Digital & Analog simulation

One approach to simulate quantum system is based on the well-known circuit model, which is usually referred to as *digital quantum simulation*. Digital quantum simulation consists of 3 steps:
1. initial state preperation: encode the wave function using computational basis;
2. evolution unitary implementation: by application of a sequence of single- or two qubits gates;
3. final measurement: decode and extract information.

Note that the advantage of quantum simulator over classical can only be exhibited when each of the three steps is realized efficiently, i.e., using polynomial resources. Actually these are not easy tasks.

Another approach is *analog quantum simulation*, where one quantum system mimics another. This can be done if there exists a mapping $f$ between the system and the simulator:

$$
\begin{aligned}
|\psi _{sim}( 0) \rangle  & =f|\psi _{sys}( 0) \rangle \\
|\psi _{sys}( t) \rangle  & =f^{-1} |\psi _{sim}( t) \rangle \\
H_{sim} & =fH_{sys} f^{-1} .
\end{aligned}
$$

See Georgescu et al. ([2014](https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.86.153))'s review for more on this topic.

The following sections summarize several methods for implementing evoluntion unitary in digital quantum simulation.


# Product formula

The product formula approach is to approximate $e^{-iHt}$ in two steps:
1. Hamiltonian decompostion: decompose $H = \sum_{l=1}^{L}H_l$, where each $H_l$ is easy to simulate;
2. Hamiltonian recombination: combine $e^{-iH_lt}$s to get an approximation of $e^{-iHt}$.

The algorithm of this type has circuit complexity $ploy(t,1/\varepsilon)$.

TODO: improvements: https://arxiv.org/abs/1907.11679

## Hamiltonian decomposition

**Local Hamiltonian**. A large class of Hamiltonians (e.g., Hubbard, Ising and Heisenberg model) can be decomposed as a sum of local interactions, as considered by Lloyd ([1996](https://science.sciencemag.org/content/273/5278/1073)). Each local interaction acts non-trivially on a small subsystem and can thus be simulated more easily. For example, in many physical model the Hamiltonian can be written as a sum of Pauli strings, and a $k$-local Pauli string can be simulated using $2k$ $CNOT$s, a $Rz$ that acts on an ancilla qubit, and several other primitive single qubit gates (see, e.g., [Georgescu's review](https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.86.153)).

**Sparse Hamiltonian**. Simulating local Hamiltonians was later generalized by  Aharonov et al. ([2003](https://dl.acm.org/doi/10.1145/780542.780546)) into simulating spase Hamiltonians. In this setting, the Hamiltonian $H$ is accessed by an oracle, e.g., an oracle $f$ that on inputing a row index $i$ and an positive interger $j$, outputs the column index and the value of the $j$-th non-zero element in the $i$-th row. In a quantum circuit, this oracle is realized by a unitary $U_f$ such that $U_f|i,j,0\rangle = |i,j,f(i,j)\rangle$.

To decompose sparse Hamiltonians, it is common to consider the *graph of the Hamiltonian*. A Hamiltonian can be thought of as an adjacency matrix of a graph. The undirected graph formed by connecting two vertices if and only if the edge between them has nonzero weight (possibly complex weight) is called the graph of the Hamiltonian. Note that the graph of a $d$-*sparse* Hamiltonian (each row and column has at most $d$ nonzero elements) has maximum degree $d$.

Some graphs are easy to simulate. For example, graphs with maximum degree one such as Pauli strings. Berry et al. ([2006](https://arxiv.org/abs/quant-ph/0508139)) decompose a $d$-sparse Hamiltonian of dimension $N$ into $6d^2$ 1-sparse Hamiltonians, each can be simulated by making $O(\log^*N)$ queries.

Galaxies (forest of stars) are also easy to simulate. Childs et al. ([2010](http://arxiv.org/abs/1003.3683v2)) decompose the $d$-sparse Hamiltonian into $6d$ galaxies, and each galaxy can be simulated by making $O(d+\log^*N)$ queries.

Here the function $\log^*$ is defined by $\log^*N=0$ if $0<N\leq 1$ and $\log^*N=1+\log^*\log N$ if $N>1$.

## Hamiltonian recombination

Suppose $H = \sum_{l=1}^{L}H_l$ and each $e^{-iH_lt}$ can be approximated with high precision. The evolution of $H$ cannot be obtained by multiplying each $e^{-iH_lt}$ unless $\{H_l\}$ commutes.

The *Trotter-Suzuki decompostion* gives an approximation of the evolution unitary by product of evolution of each local interaction. The *Suzuki formula* of the $n$-th order $S_m$ gives the $m$-th order approximation of the exponential:
$$
\begin{aligned}
S_{1}( \lambda ) & :=\prod _{l=1}^{L} e^{i\lambda H_{l}} =e^{i\lambda H} +O\left( \lambda ^{2}\right) ,\\
S_{2}( \lambda ) & :=\prod _{l=1}^{L} e^{i\frac{\lambda }{2} H_{l}}\prod _{k=L}^{1} e^{i\frac{\lambda }{2} H_{k}} =e^{i\lambda H} +O\left( \lambda ^{3}\right) .
\end{aligned}
$$

The first order formula $S_1(\lambda)$ is also known as *Trotter formula* or *Lie product formula*. Suzuki proved in his thesis ([1991](https://aip.scitation.org/doi/10.1063/1.529425)) that the $(2k-1)$-th order formula is equivalent to the $2k$-th formula, i.e.,
$$
S_{2k-1}(\lambda) = S_{2k}(\lambda) + O(\lambda ^{2k+1}) .
$$

Suzuki also gives a recursive method to define higher order formula, i.e., using a composition of low order formulas with controllable coefficients to cancel high order terms, much similar to Runge-Kutta method in numerical analysis. One way (given by Suzuki) to define the $2k$-th formula is
$$
S_{2k}( \lambda ) :=S_{2k-2}( p_{k} \lambda )^{2} S_{2k-2}((1-4p_k) \lambda) S_{2k-2} ( p_{k} \lambda )^{2},\ \text{where}\ p_{k} =\left( 4-4^{\frac{1}{2k-1}}\right)^{-1} .
$$

Finally, the evolution at time $t$ can thus be approximated by dividing $t$ into $r$ steps:
$$
e^{itH} =S_{m}\left(\frac{t}{r}\right)^{r} +O\left(\frac{t^{m+1}}{r^{m}}\right) .
$$

The number of exponentials in a $2k$-th order Trotter-Suzuki decompositon is $O(L5^kt(t/\varepsilon)^{1/2k})$. This bound is made precise by Berry et al. ([2006](https://arxiv.org/abs/quant-ph/0508139)) to be
$$
N_{exp} \leqslant 5^{2k} L^{2} \| H\| t\left(\frac{L\| H\| t}{\varepsilon }\right)^{1/2k}
$$
where $\Vert H\Vert$ is the spectral norm of $H$.

Several attempts on improving Trotter-Suzuki decomposition have been made.

Zhang et al. ([2020](https://arxiv.org/abs/2011.05283)) proposed an adaptive version of product formula, where each evolution term in
$$
\prod _{l=1}^{L} e^{i\lambda H_{l}}
$$
is selected in a heuristic way, thus reducing circuit depth. Suppose the current selected terms are $\{O_1,\cdots,O_n\}\subseteq \{H_l\}$, then the approximation error
$$
\begin{aligned}
\varepsilon  & =\| e^{-iH\delta t} |\psi ( t) \rangle -\prod _{j=1}^{n} e^{-iO_{j} \lambda _{j} \delta t} |\psi ( t) \rangle \| \\
 & =\delta t\left( \langle H^{2} \rangle +\sum _{jj'} A_{jj'} \lambda _{j} \lambda _{j'} -2\sum _{j} C_{j} \lambda _{j}\right) +O\left( \delta t^{2}\right)
\end{aligned}
$$
where $A_{jj'} =Re( \langle O_{j} O_{j'} \rangle ), C_{j} =Re( \langle HO_{j} \rangle )$ can be efficiently evaluated.

In the thesis of Jones et al. ([2019](https://arxiv.org/pdf/1904.01336.pdf)), the coefficients in Suzuki formula is optimised using evolutionary strategies.

# Quantum walk

The best known algorithm for simulating sparse Hamiltonian is based on *Childs quantum walk* ([2009](https://link.springer.com/article/10.1007/s00220-009-0930-1), [2012](https://dl.acm.org/doi/10.5555/2231036.2231040)), which extends the *Szegedy walk* ([2004](http://arxiv.org/abs/quant-ph/0401053)). Compared to product formula based algorithms whose query complexity is nearly linear with regard to $t$ and $poly(1/\varepsilon)$ in terms of $\varepsilon$, the best algorithm based on quantum walk achieves query complexity $O(\tau + \log(1/\varepsilon)/\log\log(\varepsilon))$, where $\tau:=d\| H\| _{\max} t$ depends on sparsity $d$, the largest element of $H$ and time $t$.

## Query lower bound

Berry et al. ([2014](https://dl.acm.org/doi/abs/10.1145/3313276.3316386)) proved a query lower bound scaling in error as $\varepsilon$ of $\Omega(\log\varepsilon/\log\log\varepsilon)$. In another [thesis](ttps://arxiv.org/abs/1501.01715) a year later, they proved an extension of the *no fast foward theorem* ([2007](http://dx.doi.org/10.1007/s00220-006-0150-x)) that gives a query complexity scaling in $\tau$ (instead of $t$ in the no fast forward theorem) as $\Omega (\tau)$.

The optimal additive lower bound $\Omega(\tau + \log\varepsilon/\log\log\varepsilon)$ was finally achieved by Low et al. ([2017](https://arxiv.org/abs/1606.02685)).

## Childs quantum walk

Quantum walk is defined analogous to classical random walk, or Markov chains. A quantum walk with $N$ states is specified by a $N\times N$ Hermitian $H$. The *continious quantum walk* is obtained by the schrödinger equation. The *discrete quantum walk*, however, can only be defined on an enlarged state space. Specificly, the Hilbert space $\mathbb{C}^N$ is extended to $\mathbb{C}^{2N\times 2N}$.

The Hamiltonian $H$ of sparsity $d$ is accessed by two oracle $O_{H} ,O_{F}$, where $O_{H}$ accepts $( j,k) \in [ N] \times [ N]$ and outputs $H_{jk}$, and $O_{F}$ accepts $( j,l) \in [ N] \times [ d]$ and output the $l$th nonzero element in the $j$th row of $H$. With $O( 1)$ query to $O_{H} ,O_{F}$, one can implement an isometry $T$ such that
$$
T|j\rangle = |j\rangle \otimes \left(\sqrt{\frac{\varepsilon }{\| H\| _{1}}}\sum _{k=1}^{N}\sqrt{H_{jk}^{*}} |k\rangle |0\rangle +\sqrt{1-\frac{\varepsilon \sigma _{j}}{\| H\| _{1}}} |\zeta _{j} \rangle |1\rangle \right) ,
$$
where $\varepsilon \in ( 0,1]$ is a parameter that can be made small to obtain a *lazy quantum walk*, $\sqrt{H_{jk}^{*}}$ is chosen such that $\sqrt{H_{kj}^{*}}\left(\sqrt{H_{jk}^{*}}\right)^{*} =H_{jk}$, $|\zeta _{j} \rangle$ is some superposition of $|k\rangle$, $\sigma _{j} :=\sum _{k=1}^{M} |H_{jk} |$, $\| H\| _{1} :=\max_{j} \sigma _{j}$.

A swap $S$ such that $S|j\rangle |k\rangle =|k\rangle |j\rangle$ can be implemented easily. Note that $T|j\rangle$ is choosen such that
$$
\langle j|T^{\dagger } ST|k\rangle =\frac{\varepsilon }{\| H\| _{1}} H_{jk}
$$

The quantum walk operator is defined to be
$$
W:=iS\left( 2TT^{\dagger } -I\right).
$$

## Quantum simulation by quantum walk

Let $\lambda$ be an eigenvalue of $H$, and $|\lambda \rangle$ the corresponding eigenvector. $T$ maps $|\lambda \rangle$ into the combination of two eigenvectors of $W$:
$$
T|\lambda \rangle =\frac{|\lambda _{+} \rangle +|\lambda _{-} \rangle }{\sqrt{2}} ,
$$
and $W|\lambda _{\pm } \rangle =e^{i\theta _{\pm }} |\lambda _{\pm } \rangle$,
$$
\theta _{\pm } =\pm \arcsin\left(\frac{\lambda }{d\| H\| _{\max}}\right) +( 1\mp 1)\frac{\pi }{2} .
$$

Quantum simulation by quantum walk can be summarized as the process below:
$$
|\lambda \rangle \xrightarrow{T}\frac{|\lambda _{+} \rangle +|\lambda _{-} \rangle }{\sqrt{2}}\xrightarrow{U_h} \frac{e^{ih( \theta _{+})} |\lambda _{+} \rangle +e^{ih( \theta _{-})} |\lambda _{-} \rangle }{\sqrt{2}}\rightarrow e^{-i\lambda t} |\lambda \rangle
$$
with function $h(\theta)=-\tau\sin\theta=-d\| H\| _{\max} t\sin \theta$.

## Implementing function $h$

Berry et al. ([2015](https://arxiv.org/abs/1501.01715)) used *Linear Combination of Unitaries (LCU)* algorithm to obtain a linear combination of $W^1,W^2,\cdots,W^N$, which approximates $h$. This method has a major drawback: success probability decays with $N$. Moreover, the query complexity is $O(\tau \log\varepsilon/\log\log\varepsilon)$ which is not optimal.

Low et al. ([2017](https://arxiv.org/abs/1606.02685)) proposed *Quantum Signal Processing* algorithm and achieved query lower bound $O(\tau + \log\varepsilon/\log\log\varepsilon)$ without success probabilty decay. In general, given a odd periodic function $h: (\pi,\pi]\rightarrow(\pi,\pi]$, the quantum signal processing algorithms transform an unitary
$$
W=\sum_\lambda e^{i\theta_\lambda}|\lambda\rangle\langle\lambda|\rightarrow V=\sum_\lambda e^{ih(\theta_\lambda)}|\lambda\rangle\langle\lambda|.
$$
with precision $\varepsilon$ and success probability $1-O(\varepsilon)$. The query complexity of the signal processing algorithm depends on the Fourier decomposition of $h$.

# Variational methods

*Varational Quantum Algorithm (VQA)* use low depth quantum circuits as a subroutine in a larger classical optimisation and have been applied broadly, including to binary optimisation problems, training machine learning models, and obtaining energy spectra. VQA is a quantum analog of classical machine learning methods, as it employs parametrized quantum circuits on quantum computers and leverages classical optimization toolbox for parameter optimizations. Cerezo et al. ([2020](https://arxiv.org/abs/2012.09265)) reviewed VQA and its various applications.

https://doi.org/10.1038/s41534-021-00452-9 intro-p2

*Varational Quantum Simulation (VQS)* essentially approximates evolution unitary $e^{-iHt}$ by a parametrized quantum circuit (or an *ansatz*) where the evolution of parameters in time $t$ is determined by classical varational principles. Concretely, a first-order ode of parameters, whose coefficients can be efficiently determined by Hadamard test, is induced by varational principles. Yuan et al. ([2018](https://arxiv.org/abs/1812.08767)) studied the performance of different varational principles on different system (pure or mixed) and different evolution (real time and imaginary time).

One obstacle is the lack of knowledge on contructing ansatz, which drastically affects the performance of simulation. In the inital work of Li et al. ([2017](https://journals.aps.org/prx/abstract/10.1103/PhysRevX.7.021050)), the algorithm error is evaluated to be
$$
O\left(\left(\sqrt{\Delta_{\max}^{(2)}} + \sqrt{\delta t\Delta_{\max}^{(3)}}\right)T\right)
$$
where $\Delta^{(2)}$ and $\Delta^{(3)}$ depend on the ansatz and $H$, thus a poorly designed ansatz leads to an linear error scaling in time $T$ with large coefficient. 

Beside being sufficiently expressive, an ansatz must meet other practical concerns including hardware limitations (restricted qubit connectivity and practical limits on circuit depth due to experimental noise) as well as optimisation considerations (the number of optimisation parameters must not be too large)

# Algebra methods

## Fixed depth simulation by Cartan decomposition

TODO.