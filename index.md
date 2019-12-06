---
layout: page
title: IOHprofiler
nav_order: 2
---

# Profiling Iterative Optimization Heuristics

**IOHprofiler** is a tool for benchmarking and analyzing and _iterative optimization heuristics_ (IOHs), e.g., Evolutionary Algorithms and Swarm-based Algorithms. It consists of two parts:

* __IOHexperimenter__ for generating benchmarking suites, which produce experiment data, and
* __IOHanalyzer__ for the statistical analysis and visualization of the experiment data.

The common workflow of benchmaking and empirical analysis is shown as follows:
![](/assets/fig/overview.jpg)

[__IOHexperimenter__](IOHexperimenter/) provides,

* a generic framework to generate benchmarking suite for the optimization task you're insterested in,
* a _Pseudo-Boolean Optimization_ ([PBO](/Suites/PBO/)) benchmark suite, containing 23 test problems of the kind $f\colon \\{0,1\\}^d \rightarrow \mathbb{R}$, and
* the integration of 24 _Black-Box Optimization Benchmarking_ ([BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf)) functions on the continuous domain, namely $f\colon \mathbb{R}^d \rightarrow \mathbb{R}$.

[__IOHanalyzer__](IOHanalyzer/) provides:

* a web-based interface to analyze and visualize the empirical performance of IOHs,
* interactive plot,
* statistical evaluation,
* report generation, and
* `R` programming interfaces in the backend.

## Links

* __Project repositories__:
  * Main repository: [https://github.com/IOHprofiler](https://github.com/IOHprofiler)
  * Algorithms: [https://github.com/IOHprofiler/IOHalgorithm](https://github.com/IOHprofiler/IOHalgorithm)
  * Performance data: (for the time being, these are available via the web-interface at [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl), or at [https://github.com/IOHprofiler/IOHdata](https://github.com/IOHprofiler/IOHdata))
* __Documentation__: [Documentation on arXiv (Preliminary, an updated version will be made available soon)](https://arxiv.org/abs/1810.05281)
* __Bug reports__:
  * __IOHanalyzer__: [https://github.com/IOHprofiler/IOHanalyzer/issues](https://github.com/IOHprofiler/IOHanalyzer/issues)
  * __IOHexperimenter__: [https://github.com/IOHprofiler/IOHexperimenter/issues](https://github.com/IOHprofiler/IOHexperimenter/issues)
* __IOHanalyzer Online Service__: [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](mailto:iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)

## Acknowledgments

We thank our colleagues Anne Auger, Dimo Brockhoff, Arina Buzdalova, Maxim Buzdalov, Johann Dréo, Nikolaus Hansen, Pietro S. Oliveto, Ofer Shir, Markus Wagner, and Thomas Weise for various discussions around the benchmarking of iterative optimization heuristics.  

Parts of our work have been inspired by working group 3 of COST Action CA15140 'Improving Applicability of Nature-Inspired Optimisation by Joining Theory and Practice (ImAppNIO)' supported by the European Cooperation in Science and Technology.

Our work has been supported by a public grant as part of the Investissement d'avenir project, reference ANR-11-LABX-0056-LMH, LabEx LMH, in a joint call with the Gaspard Monge Program for optimization, operations research, and their interactions with data sciences.

Furong Ye acknowledges financial support from the China Scholarship Council, CSC No. 201706310143.