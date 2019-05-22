---
layout: default
title: Home
nav_order: 1
---
IOHprofiler: A Benchmarking and Profiling Tool for Iterative Optimization Heuristics
============================================

IOHprofiler is a new tool for analyzing and comparing iterative optimization heuristics.
Given as input algorithms and problems written in C, C++, or Python*, it provides as output an in-depth statistical evaluation of the algorithms’ fixed-target and fixed-budget running time distributions. In addition to these performance evaluations, IOHprofiler also allows to track the evolution of algorithm parameters, making our tool particularly useful for the analysis, comparison, and design of (self-)adaptive algorithms.

IOHprofiler is a ready-to-use software. It consists of two parts: IOHExperimenter, which generates the running time data; and IOHAnalyzer, which produces the summarizing comparisons and statistical evaluations. Currently IOHExperimenter is built on the [COCO](https://github.com/numbbo/coco) software, but a new C++ based version is developing and will be released soon.

[This code] implements the experimentation tool of IOHprofiler. 
For the analyzer part, please visit [IOHAnalyzer page](https://github.com/IOHprofiler/IOHAnalyzer).