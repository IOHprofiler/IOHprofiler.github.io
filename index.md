---
layout: page
title: About
nav_order: 1
---

IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics
============================================

**IOHprofiler** is a new tool for analyzing and comparing iterative optimization heuristics. It consists of two part: __IOHexperimenter__ and __IOHanalyzer__. 

[https://iohprofiler.github.io/IOHexperimeter](__IOHexperimenter__) provides a tool for:
* A framework for straightforward benchmarking of any iterative optimization heuristic
* A suite consisting of 23 pre-made Pseudo-Boolean benchmarking function, with easily accessible methods for adding custom functions and suites 
* Logging methods to effortlesly store benchmarking data in a format compatible with __IOHanalyzer__, with future support for additional data logging options
* (__Soon to come__:) A framework which significantly simplifies algorithm design

__IOHanalyzer__ provides:
* a web-based interface to analyze and visualize the empirical performance of IOHs
* interactive plot
* statistical evaluation
* report generation
* `R` programming interfaces in the backend

## Source
* __source code__ : [https://github.com/IOHprofiler](https://github.com/IOHprofiler)
* __Documentation__: [https://arxiv.org/abs/1810.05281](https://arxiv.org/abs/1810.05281)
* __Wiki page__: [https://iohprofiler.github.io/IOHanalyzer](https://iohprofiler.github.io/IOHanalyzer)
* __Bug reports (IOHanalyzer)__: [https://github.com/IOHprofiler/IOHAnalyzer/issues](https://github.com/IOHprofiler/IOHAnalyzer/issues)
* __Online service (IOHanalyzer)__: [http://iohprofiler.liacs.nl](http://iohprofiler.liacs.nl)
* __General Contact__: [iohprofiler@liacs.leidenuniv.nl](iohprofiler@liacs.leidenuniv.nl)
* __Mailing List__: [https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler](https://lists.leidenuniv.nl/mailman/listinfo/iohprofiler)

## Contact

If you have any questions, comments, suggestions or pull requests, please don't hesitate contacting us <IOHprofiler@liacs.leidenuniv.nl>!

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
