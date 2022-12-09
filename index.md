---
layout: page
title: IOHprofiler
nav_order: 2
---

# Profiling Iterative Optimization Heuristics

**IOHprofiler**, a benchmarking platform for evaluating the performance of _iterative optimization heuristics_ (IOHs), e.g., Evolutionary Algorithms and Swarm-based Algorithms. We aim to the integrate various elements of the entire benchmarking pipeline, ranging from problem (instance) generators and modular algorithm frameworks over automated algorithm configuration techniques and feature extraction methods to the actual experimentation, data analysis, and visualization. It consists of the following major components:

* [__IOHexperimenter__](IOHexp/) for generating benchmarking suites, which produce experiment data,
* [__IOHanalyzer__](IOHanalyzer/) for the statistical analysis and visualization of the experiment data,
* [__IOHproblem__](IOHproblem/) for providing a collection of test functions.
* [__IOHdata__](IOHdata) for hosting the benchmarking data sets from __IOHexperimenter__ as well as other platforms, e.g., [_BBOB/COCO_](https://github.com/numbbo/coco) and [_Nevergrad_](https://github.com/facebookresearch/nevergrad).
* [__IOHalgorithm__](IOHalgorithm) for efficient implemention of various classic optimization algorithms.
  
The composition of **IOHprofiler** and the coordinations of its components are depicted below:
![](/assets/fig/overview.png)

[__IOHexperimenter__](IOHexp/) provides,

* A generic framework to generate benchmarking suite for the optimization task you're insterested in.
* A _Pseudo-Boolean Optimization_ ([PBO](/IOHproblem)) benchmark suite, containing 25 test problems of the kind $f\colon \\{0,1\\}^d \rightarrow \mathbb{R}$.
* The integration of 24 _Black-Box Optimization Benchmarking_ ([BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf)) functions on the continuous domain, namely $f\colon \mathbb{R}^d \rightarrow \mathbb{R}$.

[__IOHanalyzer__](IOHanalyzer/) provides:

* Performance analysis in both a <i>fixed-target</i> and <i>fixed-budget</i> perspective
* A web-based interface to interactively analyze and visualize the empirical performance of IOHs.
* Statistical evaluation of algorithm performance.
* `R` programming interfaces in the backend for even more customizable analysis.

## Documentations

In addition to this wiki page, there are a number of (more) detailed documentations to explore:

* __General__: [https://arxiv.org/abs/1810.05281](https://arxiv.org/abs/1810.05281)
* __IOHanalyzer__: [https://dl.acm.org/doi/abs/10.1145/3510426](https://dl.acm.org/doi/abs/10.1145/3510426)
* __IOHexperimenter__: [https://arxiv.org/abs/2111.04077](https://arxiv.org/abs/2111.04077)

## Links

* __Project repositories__:
  * Main repository: [https://github.com/IOHprofiler](https://github.com/IOHprofiler)
  * Algorithms: [https://github.com/IOHprofiler/IOHalgorithm](https://github.com/IOHprofiler/IOHalgorithm)
  * Performance data: (for the time being, these are available via the web-interface at [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl), or at [https://github.com/IOHprofiler/IOHdata](https://github.com/IOHprofiler/IOHdata))
* __IOHanalyzer Online Service__: [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl)
* __Bug reports__:
  * __IOHanalyzer__: [https://github.com/IOHprofiler/IOHanalyzer/issues](https://github.com/IOHprofiler/IOHanalyzer/issues)
  * __IOHexperimenter__: [https://github.com/IOHprofiler/IOHexperimenter/issues](https://github.com/IOHprofiler/IOHexperimenter/issues)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](mailto:iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)