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
