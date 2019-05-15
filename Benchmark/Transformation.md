---
layout: page
title: Transformation
parent: Benchmark
nav_order: 1
permalink: /Benchmark/Transformation/
--- 

## Transformation

Instead of testing one particular problem $f$ only, the user can choose to run experiments on several problem instances that are obtained from $f$ through a set of transformations. In its most general form,IOHprofiler currently offers to return to the algorithm the values as $af(\sigma (x {\oplus} z)) + b$, where
* $a$ is a multiplicative shift of the function value,
* $b$ is an additive shift of the function value,
* ${\oplus}z: \{0,1\}^n \to \{0,1\}^n, (x_1, ... ,x_n) \mapsto ((x_1 + z_1) {mod} 2, ... ,(x_n + z_n) {mod} 2)$ is an XOR-shift of the search point,
* $\mathcal{S}^n \to \mathcal{S}^n, (x_1, ... ,x_n) \mapsto (x_{\sigma(1)}, ... ,x_{\sigma(n)})$ is a permutation of the search point. Note here that, in abuse of notation, we identify the permutation ${\sigma}: [1..n] \to [1..n]$ with the here-defined re-ordering of the bit string.

In practical, we applied $af(x {\oplus} z) + b$ to instance 2-50, and $af(\sigma (x)) + b$ to instance 51-100. $a \in [\frac{1}{5},5]$ and $b \in [-1000,1000]$ are randomly generated.