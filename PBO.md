---
layout: page
title: IOHproblem
permalink: /IOHproblem/
--- 

At this moment, **IOHproblem** consisted of the _Pseudo-Boolean Optimization_ (PBO) problem set, which contains 25 test problems of the kind $f\colon \\{0,1\\}^d \rightarrow \mathbb{R}$.

## Pseudo-Boolean Optimization (PBO) Problem Set

While we are interested in covering different types of fitness landscapes, we care much less about their actual embedding, and mainly seek to understand algorithms that are invariant under the problem representation. In the context of pseudo-Boolean optimization $f:\\{0,1\\}^n \to \mathbb{R}$, a well-recognized approach to request representation invariance is to demand that an algorithm shows the same or similar performance on any instance mapping each bit string $x \in \\{0,1\\}^n$ to the function value $f(\sigma(x \oplus z))$, where $z$ is an arbitrary bit string of length $n$, $\oplus$ denotes the bit-wise XOR function, and $\sigma(y)$ is to be read as the string $(y_{\sigma(1)},\ldots,y_{\sigma(n)})$ in which the entries are swapped according to the permutation $\sigma:[n] \to [n]$. Using these transformations, we obtain from one particular problem $f$ a whole set of instances $\\{ f(\sigma(\cdot \oplus z)) \mid z \in \\{0,1\\}^n, \sigma \text{ permutation of } [n] \\}$, all of which have fitness landscapes that are pairwise isomorphic. To allow future comparisons with non-ranking-based algorithms, objectives of instances are shifted by a multiplicative and an additive offset. That is, instead of receiving the values $f(\sigma(x\oplus z))$, only the transformed values $a f(\sigma(x\oplus z)) + b$ are made available to the algorithms.

Practically, for the PBO suite, problems with instance 1 in **IOHexperimenter**  is the basic instance of each problem. For other instances the $\oplus$ and $\sigma$ transformations are separated. More precisely, instances 2-50 are obtained from instance 1 by a \"$\oplus z$\" rotation with a randomly chosen $z \in \\{0,1\\}^n$, and random fitness offsets $a\in [\frac{1}{5},5]$, $b \in [-1000,1000]$. For instances 51-100 there is no \"$\oplus z$\" rotation, but the strings are permuted by a randomly chosen $\sigma$ and the ranges for the random fitness offset are chosen as for instances 2-6. For each function and each dimension the values of $z$, $\sigma$, $a$, and $b$ are fixed per each instance, but different functions of the same dimensions may have different $z$ and $\sigma$ transformations.

Description of problems of PBO suite is below. To add new test problems or create new benchmark suite, please follow the [Section 4.4](/IOHexperimenter/extension/).

### F1: OneMax (Hamming Distance）

It asks to optimize $OM:\\{0,1\\}^n \rightarrow [0..n], x \mapsto \sum_{i=1}^n{x_i}$. The problem has a very smooth and non-deceptive fitness landscape. Due to the well-known coupon collector effect, it is relatively easy to make progress when the function values are small, and the probability to obtain an improving move decreases considerably with increasing function value.

### F2: LeadingOnes

The problem asks to maximize the function $LO:\\{0,1\\}^n \to [0..n], x\mapsto \max \\{i \in [0..n] \mid \forall {j} \le {i}: x_j=1\\} = \sum_{i=1}^n{\prod_{j=1}^i{x_i}}$, which counts the number of initial ones.

### F3: A Linear Function with Harmonic Weights

The problem is a linear function $f:\\{0,1\\}^n \to \mathbb{R}, x \mapsto \sum_{i} i x_i$ with harmonic weights.

### F4 - F17: The W-model

The W-model comprises 4 basic transformations, each coming with different instances. We use $W(\cdot,\cdot,\cdot,\cdot)$ to denote the configuration chosen in our benchmark set.

 - Reduction of dummy variables $W(k,\ast,\ast,\ast)$: a reduction mapping each string $(x_1, \ldots, x_n)$ to a substring $(x_{i_1}, \ldots, x_{i_k})$ for randomly chosen, pairwise different $i_1,\ldots, i_k \in [n]$.
 - Neutrality $W(\ast,\mu,\ast,\ast)$: The bit string $(x_1,\ldots,x_n)$ is reduced to a string$(y_1,\ldots,y_m)$ with $m:=\frac{n}{\mu}$, where $\mu$ is a parameter of the transformation. For each $i \in [m]$ the value of $y_i$ is the majority of the bit values in a size-$\mu$ substring of $x$. More precisely, $y_i=1$ if and only if there are at least $\frac{\mu}{2}$ ones in the substring $(x_{(i-1)\mu+1},x_{(i-1)\mu+2},\ldots,x_{i\mu})$. When $\frac{n}{\mu} \notin \mathbb{N}$, the last bits of $x$ are simply copied to$y$. In our assessment, we regard only the case $\mu=3$.
 - Epistasis $W(\ast,\ast,\nu,\ast)$ The idea of epistasis is to introduce local perturbations to the bit strings. To this end, a string $x=(x_1,\ldots,x_n)$ is divided into subsequent blocks of size $\nu$. Using a permutation $e_{\nu}:\\{0,1\\}^{\nu} \to \\{0,1\\}^{\nu}$, each substring $(x_{(i-1)\nu+1},\ldots,x_{i\nu})$ is mapped to another string $(y_{(i-1)\nu+1},\ldots,y_{i\nu})=e_{\nu}((x_{(i-1)\nu+1},\ldots,x_{i\nu}))$. The permutation $e_{\nu}$ is chosen in a way that Hamming-1 neighbors $u,v \in \\{0,1\\}^{\nu}$ are mapped to strings of Hamming distance at least $\nu-1$. In our evaluation, we use $\nu=4$ only.
 - Fitness perturbation $W(\ast,\ast,\ast,r)$. With this transformation we can determine the ruggedness and deceptiveness of a function. Unlike the previous transformations, this perturbation operates on the function values, not on the bit strings. To this end, a *ruggedness* function $r:\\{f(x) \mid {x} \in \\{0,1\\}^n \\}=:V \to {V}$ is chosen. The new function value of a string $x$ is then set to $r(f(x))$ , so that effectively the problem to be solved by the algorithm becomes $r \circ f$. We use the following three ruggedness functions.
  - $r_1:[0..s] \to [0..\lceil{s/2}\rceil+1$ with $r_1(s)= \lceil {s/2} \rceil +1$ and $r_1(i)=\lfloor {i/2} \rfloor+1$ for $i<s$ and even s, and $r_1(i)=\lceil {i/2} \rceil+1$ for $i<s$ and odd $s$. This function maintains the order of the search points
  - $r_2:[0..s] \to [0..s]$ with $r_2(s)=s$, $r_2(i)=i+1$ for $i \equiv {s  {mod}  2}$ and $i<s$, and $r_2(i)=\max\\{i-1,0\\}$ otherwise. This function introduces moderate ruggedness at each fitness level. 
  - $r_3:[0..s] \to [0..s]$ with $r_3(s)=s$ and $r_3(s-5j+k)=s-5j+(4-k)$ for all $j \in {[s/5]}$ and $k {\in} [0..4]$ and $r_3(k)=s - (5\lfloor {s/5} \rfloor - 1 )- k$ for $k \in [0..s - 5\lfloor {s/5} \rfloor -1]$. With this function the problems become quite deceptive, since the distance between two local optima implies a difference of $5$ in the function values. 

F4-F17 present superpositions of individual W-model transformations to the OneMax (F1) and the LeadingOnes (F2) problem. Precisely, F4-F17 are

* F4: $\text{OneMax} + W([n/2],1,1,id)$
* F5: $\text{OneMax} + W([0.9n],1,1,id)$
* F6: $\text{OneMax} + W([n],\mu=3,1,id)$
* F7: $\text{OneMax} + W([n],1,\nu=4,id)$
* F8: $\text{OneMax} + W([n],1,1,r_1)$
* F9: $\text{OneMax} + W([n],1,1,r_2)$
* F10: $\text{OneMax} + W([n],1,1,r_3)$
* F11: $\text{LeadingOnes} + W([n/2],1,1,id)$
* F12: $\text{LeadingOnes} + W(0.9n,1,1,id)$
* F13: $\text{LeadingOnes} + W([n],\mu=3,1,id)$
* F14: $\text{LeadingOnes} + W([n],1,\nu=4,id)$
* F15: $\text{LeadingOnes} + W([n],1,1,r_1)$
* F16: $\text{LeadingOnes} + W([n],1,1,r_2)$
* F17: $\text{LeadingOnes} + W([n],1,1,r_3)$

### F18: Low Autocorrelation Binary Sequences (LABS)

The Low Autocorrelation Binary Sequences (LABS) problem poses a non-linear objective function over a binary sequence space, with the goal to maximize the reciprocal of the sequence's autocorrelation:

 $ \frac{n^2}{2E(S)} \text{ with } E(S) = \sum_{k=1}^{n-1}\left(\sum_{i=1}^{n-k}s_i\cdot s_{i+k}\right)^2$

where the sequence is of length n $S:=\left(s_1,\ldots,s_n\right)$ with $s_i=\pm 1$ . To obtain a pseudo-Boolean problem, we use the straightforward interpretation $s_i=2x_i-1$ for all $i \in [n]$. 

### F19-F21: The Ising Model

The classical Ising model considers a set of spins placed on a regular lattice $G=([n],E)$, where each edge $(i,j) \in {E}$ is associated with an interaction strength $J_{ij}$. Given a configuration of $n$ spins, $S:=\left(s_1,\ldots,s_n\right)$, this problem poses a quadratic function, representing the system's energy and depending on its structure $J_{ij}$. Assuming zero external magnetic fields and using $s_i=2x_i-1$ we obtain the following pseudo-Boolean maximization problem:

$[ISING:] \sum\limits_{\\{i,j\\} \in {E}} \left[x_{i}x_{j} - \left(1-x_{i} \right)\left(1-x_{j} \right) \right] $

In PBO we consider three instances: the one-dimensional ring (F19), the two-dimensional torus (F20), and thetwo-dimensional triangular (F21).

### F22: Maximum Independent Vertex Set

Given a graph $G=([n],E)$, an independent vertex set is a subset of vertices where no two vertices are linked by an edge. A maximum independent vertex set (MIVS) is defined as an independent subset Given a graph $V^{\prime} \subset [n]$ having largest possible size.

$[MIVS:] \sum_i x_i \;   \textrm{s.t.} \; \sum_{i < j} x_i x_j e_{ij} = 0~$.

### F23: N-Queens Problem

The N-queens problem (NQP) is defined as the task to place N queens on an ${N}\times{N}$ chessboard in such a way that they cannot attack each other.

Selection of upper problems is suggested in the paper [Benchmarking discrete optimization heuristics with IOHprofiler](https://doi.org/10.1145/3319619.3326810).