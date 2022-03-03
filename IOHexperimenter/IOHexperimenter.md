---
layout: page
title: IOHexperimenter
has_children: true
permalink: /IOHexp/
description: Experimenting IOHs
---

**Experimenter** for **I**terative **O**ptimization **H**euristics (IOHs), built natively in* `C++`.

* **Documentation**: [https://iohprofiler.github.io/IOHexperimenter](https://iohprofiler.github.io/IOHexperimenter)
* **Publication**: [https://arxiv.org/abs/2111.04077](https://arxiv.org/abs/2111.04077)

**IOHexperimenter** *provides*:

* A framework to ease the benchmarking of any iterative optimization heuristic
<!-- * Continuous and discrete benchmarking problems -->
* [Pseudo-Boolean Optimization (PBO)](https://iohprofiler.github.io/IOHproblem/) problem set (25 pseudo-Boolean problems)
* Integration of the well-known [Black-black Optimization Benchmarking (BBOB)](https://github.com/numbbo/coco) problem set (24 continuous problems)
* Interface for adding new problems and suite/problem set
* Advanced logging module that takes care of registering the data in a seamless manner
* Data format is compatible with [IOHanalyzer](https://github.com/IOHprofiler/IOHanalyzer)

**IOHexperimenter** is available for:

* `C++` manual can be found [here](https://iohprofiler.github.io/IOHexp/Cpp/)
* `Python`: please see [here](https://github.com/IOHprofiler/IOHexperimenter/tree/master/ioh) for details user manual
* or as a [pip package](https://pypi.org/project/ioh); [Wiki Page](https://iohprofiler.github.io/IOHexp/python/).