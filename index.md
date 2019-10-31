---
layout: page
title: About
nav_order: 1
---

IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics
============================================

**IOHprofiler** is a new tool for analyzing and comparing iterative optimization heuristics. It consists of two part: __IOHexperimenter__ and __IOHanalyzer__. 

[__IOHexperimenter__](IOHexperimenter/) provides easy-to-use benchmarking functionality, including:
* A framework for straightforward benchmarking of any iterative optimization heuristic
<!-- * A suite consisting of 23 pre-made Pseudo-Boolean benchmarking function, with easily accessible methods for adding custom functions and suites  -->
* Generic benchmarking procedure using suites, with two pre-installed suites: [PBO](Benchmark/) for pseudo-boolean optimization and [BBOB](https://coco.gforge.inria.fr/downloads/download16.00/bbobdocfunctions.pdf) for continuous.
* Logging methods to effortlesly store benchmarking data in a format compatible with __IOHanalyzer__, with future support for additional data logging options
* (__Soon to come__:) A framework which significantly simplifies algorithm design

[__IOHanalyzer__](IOHanalyzer/) provides an intuitive tool for performance analysis, including:
* A web-based interface (based on `R`) to analyze and visualize the empirical performance of IOHs
* Interactive plotting of performance data from fixed-target and fixed-budget perspectives
* Rigorous statistical evaluation
* (__Beta__:) Automatic report generation

## Source
* __Base repository__ : [IOHprofilers GitHub Profile](https://github.com/IOHprofiler)
* __Documentation__: [Arxiv Verion of "IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics"](https://arxiv.org/abs/1810.05281)
* __Bug reports__: [IOHanalyzer](https://github.com/IOHprofiler/IOHanalyzer/issues) or [IOHexperimenter](https://github.com/IOHprofiler/IOHexperimenter/issues)
* __Online service (IOHanalyzer)__: [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](mailto:iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [IOHprofiler mailing list](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)

## Citing

When using IOHprofiler and parts thereof, please kindly cite this work as

Carola Doerr, Hao Wang, Furong Ye, Sander van Rijn, Thomas Bäck: <i>IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics</i>, arXiv e-prints:1810.05281, 2018.

```bibtex
@ARTICLE{IOHprofiler,
  author = {Carola Doerr and Hao Wang and Furong Ye and Sander van Rijn and Thomas B{\"a}ck},
  title = {IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics},
  journal = {arXiv e-prints:1810.05281},
  archivePrefix = "arXiv",
  eprint = {1810.05281},
  year = 2018,
  month = oct,
  keywords = {Computer Science - Neural and Evolutionary Computing},
  url = {https://arxiv.org/abs/1810.05281}
}
```

## Collection of work using __IOHprofiler__
### Full papers:
- Furong Ye, Carola Doerr, and Thomas Bäck. <i>Interpolating Local and Global Search by Controlling the Variance of Standard Bit Mutation</i>. CEC’19 [Wednesday, 5:00 pm in Special Session CEC-21: Theory of Bio-Inspired Computation]
- Carola Doerr, Furong Ye, Sander van Rijn, Hao Wang, Thomas Bäck. <i>Towards a theory-guided benchmarking suite for discrete black-box optimization heuristics: profiling (1 + λ) EA variants on OneMax and LeadingOnes</i>. GECCO’18
- Naama Horesh, Thomas Bäck, Ofer M. Shir. <i>Predict or Screen Your Expensive Assay? DoE vs. Surrogates in Experimental Combinatorial Optimization</i>. GECCO’19
- Nguyen Dang, Carola Doerr. <i>Hyper-Parameter Tuning for the (1+(λ, λ)) GA</i>. GECCO’19

### Workshop papers:
- Carola Doerr, Furong Ye, Naama Horesh, Hao Wang, Ofer M. Shir, and Thomas Bäck. <i>Benchmarking Discrete Optimization Heuristics with IOHprofiler</i>. GECCO’19
- Borja Calvo, Ofer M. Shir, Josu Ceberio, Carola Doerr, Hao Wang, Thomas Bäck, and Jose A. Lozano. <i>Bayesian Performance Analysis for Black-Box Optimization Benchmarking</i>. GECCO’19
- Ivan Ignashov, Arina Buzdalova, Maxim Buzdalov, and Carola Doerr. <i>Illustrating the Trade-Off between Time, Quality, and Success Probability in Heuristic Search</i>. GECCO’19
- Nathan Buskulic and Carola Doerr. <i>Maximizing Drift is Not Optimal for Solving OneMax</i>. GECCO’19

